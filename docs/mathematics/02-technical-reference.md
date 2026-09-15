<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# ANNEX A — TECHNICAL REFERENCE

Reference implementation of the two mechanisms with the greatest economic impact. **This is not production code**: it lacks access control, full integration, and security checks, and must undergo independent audit.

**A.1 ZDAO supply guardian** (Sections 2.1 and 2.2)

```solidity
/// @notice ZDAO token (ERC-20 mintable/burnable). MAX_SUPPLY lives here.
uint256 constant MAX_SUPPLY = 1_000_000_000e8; // ZDAO uses 8 decimals

/// @dev Invariant I-1. Applied on EVERY mint: totalSupply <= MAX_SUPPLY.
function _mint(address to, uint256 amount) internal override {
    require(totalSupply() + amount <= MAX_SUPPLY, "max supply exceeded");
    super._mint(to, amount);
}

/// @dev ERC-20 burn: reduces totalSupply, restoring minting capacity.
function burn(uint256 amount) external {
    _burn(msg.sender, amount);
}
```

Note that in the token, capacity is measured against `totalSupply` (minted minus burned): burning ZDAO directly restores minting capacity under `MAX_SUPPLY`, following the standard mintable/burnable ERC-20 pattern.

```solidity
/// @notice Staking contract. stakingMinted lives here (NOT in the token).
uint256 constant STAKING_RESERVE = 500_000_000e8;

uint256 public stakingMinted; // only grows: burning does NOT reduce it

/// @notice Staking reserve pending to be minted.
function pendingStakingReserve() public view returns (uint256) {
    return STAKING_RESERVE - stakingMinted;
}

/// @dev Only the staking contract mints against the reserve.
/// stakingMinted is cumulative and is NOT reduced when ZDAO is burned.
function mintStakingReward(address to, uint256 amount) external onlyStaking {
    stakingMinted += amount;
    require(stakingMinted <= STAKING_RESERVE, "staking reserve exceeded");
    zdao.mint(to, amount);
}
```

**A.2 Staking reward accrual** (Section 6.2)

```solidity
struct Position {
    uint256 amount; // ZREC deposited (3 decimals)
    uint256 startDate; // start of the age computation
    uint256 released; // only Merits positions
    PositionType kind; // FREE | MERITS_LOCKED
}

uint256 constant YEAR = 365 days;
uint256 constant RATE_YEAR_1 = 1e18; // 100 % per year (18-decimals fixed point)

/// @notice Annual rate for a given year of permanence, halving each year:
/// year 1 → 100 %, year 2 → 50 %, year 3 → 25 %, ...
function annualRate(uint256 yearIndex) public pure returns (uint256) {
    // yearIndex is 1-based; r = 100 % / 2^(yearIndex - 1)
    return RATE_YEAR_1 / (2 ** (yearIndex - 1));
}

/// @notice Accrual, linear in time with a per-year halving rate.
/// The reward is the sum over each staking year of the year's rate applied
/// to the time actually staked within that year:
///   reward = amount · Σ_n  r_n · t_n / YEAR
/// where t_n is the portion of year n the position was staked.
function accrued(Position storage p) internal view returns (uint256) {
    uint256 elapsed = block.timestamp - p.startDate;
    uint256 reward = 0;
    uint256 remaining = elapsed;
    uint256 year = 1;

    while (remaining > 0 && year <= MAX_YEARS) {
        uint256 t = remaining > YEAR ? YEAR : remaining;
        reward += p.amount * annualRate(year) / 1e18 * t / YEAR;
        remaining -= t;
        year += 1;
    }
    return reward;
}
```

Implementation notes:

1. Invariants are verified with checks that revert the transaction, not through administrative configuration.
2. `totalSupply` only changes on mint and burn. Burning restores capacity; `MAX_SUPPLY` is the immutable ceiling.
3. Each contribution creates an independent position with its own age computation.
4. Normalization between ZREC's 3 decimals and ZDAO's 8 decimals must be resolved with integer arithmetic and explicit rounding rules.
5. `accrued()` returns exactly `amount` after one full year, `1.5 × amount` after two, and so on; the per-year rate halves with no slope or offset parameter (Section 6.2).
