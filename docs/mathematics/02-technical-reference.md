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

Note that `burn()` does not touch `cumulativeMinted`: that single omission is what turns the ceiling into a real limit.

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
    int256 k = int256(elapsed) - sigmoidB;
    if (k < 0) return 0;
    uint256 uk = uint256(k);
    return sigmoidA * uk / sqrt(uk * uk + sigmoidC);
}

/// @notice Global component. The reference is NOT the max supply of ZREC:
/// ZREC has no ceiling. See pending decision in 6.2.
function globalRate() public view returns (uint256) {
    if (totalStaked >= stakingReference) return maxGlobalRate;
    return maxGlobalRate * totalStaked / stakingReference;
}

/// @notice Accrual. Only what is actually accrued is minted:
/// the surplus up to the ceiling is NOT issued.
function accrued(Position storage p) internal view returns (uint256) {
    uint256 elapsed = block.timestamp - p.startDate;
    uint256 rate = personalRate(elapsed) + globalRate();
    uint256 raw = p.amount * rate * elapsed / (YEAR * SCALE);
    return _applyPeriodCap(raw); // decreasing ceiling of Section 6.3
}
```

Implementation notes:

1. Invariants are verified with checks that revert the transaction, not through administrative configuration.
2. `cumulativeMinted` and `stakingMinted` only increase. No burn operation modifies them.
3. Each contribution creates an independent position with its own age computation.
4. Normalization between ZREC's 3 decimals and ZDAO's 8 decimals must be resolved with integer arithmetic and explicit rounding rules.
5. `stakingReference` and `_applyPeriodCap` correspond to the pending decisions of Section 11.3.
