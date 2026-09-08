<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# ANNEX A — TECHNICAL REFERENCE

Reference implementation of the two mechanisms with the greatest economic impact. **This is not production code**: it lacks access control, full integration, and security checks, and must undergo independent audit.

**A.1 ZDAO supply guardian** (Sections 2.1 and 2.2)

```solidity
uint256 constant MAX_SUPPLY = 1_000_000_000e8; // ZDAO uses 8 decimals
uint256 constant STAKING_RESERVE = 500_000_000e8;

uint256 public cumulativeMinted; // only grows: burning does NOT reduce it
uint256 public stakingMinted; // minted by the staking contract

/// @notice Remaining minting capacity under the absolute ceiling.
function remainingMintCapacity() public view returns (uint256) {
    return MAX_SUPPLY - cumulativeMinted;
}

/// @notice Staking reserve pending to be minted.
function pendingStakingReserve() public view returns (uint256) {
    return STAKING_RESERVE - stakingMinted;
}

/// @dev Invariants I-1 and I-3. Applied on EVERY mint.
function _mint(address to, uint256 amount) internal override {
    cumulativeMinted += amount; // never decreases
    require(cumulativeMinted <= MAX_SUPPLY, "max supply exceeded");
    super._mint(to, amount);
}

/// @dev Burning destroys tokens but does NOT restore minting capacity.
function burn(uint256 amount) external {
    _burn(msg.sender, amount); // totalSupply drops, cumulativeMinted does NOT
}

/// @dev Only the staking contract mints against the reserve.
function mintStakingReward(address to, uint256 amount) external onlyStaking {
    stakingMinted += amount;
    require(stakingMinted <= STAKING_RESERVE, "staking reserve exceeded");
    _mint(to, amount);
}
```

Note that `burn()` does not touch `cumulativeMinted`: that single omission is what keeps the ceiling a real limit. `mintStakingReward` additionally enforces the 500M staking reserve.

**A.2 Staking reward accrual** (Section 6.2)

```solidity
struct Position {
    uint256 amount; // ZREC deposited (3 decimals)
    uint256 startDate; // start of the age computation
    uint256 released; // only Merits positions
    PositionType kind; // FREE | MERITS_LOCKED
}

/// @notice Sigmoid curve of the temporal component.
/// y = a·(t-b) / sqrt((t-b)^2 + c)
function personalRate(uint256 elapsed) public view returns (uint256) {
    uint256 uk = uint256(elapsed); // sigmoidB = 0 (offset removed)
    return sigmoidA * uk / sqrt(uk * uk + sigmoidC);
}

/// @notice Usage component (supply-based), linear in staked ZREC
/// relative to circulating ZDAO supply. Anti-dilutive.
function usageRate() public view returns (uint256) {
    uint256 zdaoTs = zdao.totalSupply(); // circulating, minted minus burned
    uint256 target = zdaoTs * totalSupplyFactor / SCALE;
    if (totalStaked >= target) return maxUsageRate;
    return maxUsageRate * totalStaked / target;
}

/// @notice Accrual. Exact integral of the instantaneous rate over the period.
/// The personal sigmoid (b = 0) has a closed-form antiderivative:
///   ∫ a·t/sqrt(t²+c) dt = a·sqrt(t²+c)
/// so the accrued reward is computed exactly without numerical integration.
function accrued(Position storage p) internal view returns (uint256) {
    uint256 elapsed = block.timestamp - p.startDate;
    uint256 personal = _personalIntegral(elapsed);
    uint256 usage = usageRate() * elapsed;
    uint256 raw = p.amount * (personal + usage) / (YEAR * SCALE);
    return raw; // no period cap: bounded by MAX_EMISSION_RATE and MAX_SUPPLY
}

/// @notice Exact integral of the personal sigmoid from 0 to elapsed.
///   a·( sqrt(T²+c) − sqrt(c) ), with b = 0.
function _personalIntegral(uint256 elapsed) internal view returns (uint256) {
    uint256 sT = sqrt(elapsed * elapsed + sigmoidC);
    uint256 s0 = sqrt(sigmoidC); // sqrt(c): base of the antiderivative at t=0
    if (sT <= s0) return 0;
    return sigmoidA * (sT - s0);
}
```

Implementation notes:

1. Invariants are verified with checks that revert the transaction, not through administrative configuration.
2. `totalSupply` only changes on mint and burn. Burning restores capacity; `MAX_SUPPLY` is the immutable ceiling.
3. Each contribution creates an independent position with its own age computation.
4. Normalization between ZREC's 3 decimals and ZDAO's 8 decimals must be resolved with integer arithmetic and explicit rounding rules.
5. `usageRate()` reads the circulating `totalSupply` of ZDAO; `totalSupplyFactor` and the rate ceilings are the governance parameters of Section 11.2.
