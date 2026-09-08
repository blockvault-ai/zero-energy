# Implementation — Staking Architecture

Documentation-only reference for the ZERO Energy staking implementation, adapted from the [EasyStaking contracts](https://github.com/xdaichain/easy-staking-contracts) (xdaichain) as the conceptual base.

## Source reference

**EasyStaking** (Ethereum Mainnet) is an emission-accruing staking protocol. Its core mechanics:

| EasyStaking mechanic | Description |
| --- | --- |
| Emission model | `personal` (sigmoid over deposit age, max 7.5%) + `general` (supply-based linear, max 7.5%), combined cap 15% (`MAX_EMISSION_RATE`) |
| Personal component | `a·(t−b)/sqrt((t−b)²+c)`, sigmoid over staking time |
| Supply-based component | `max · totalStaked / (totalSupply · totalSupplyFactor)`, anti-dilutive |
| Accrual | `amount · rate · time / YEAR`, minted by the staking contract (IERC20Mintable) |
| Surplus | The gap between the max rate and the user's share is sent to a Liquidity Provider address |
| Governance | `set*` parameters (fee, sigmoid a/b/c, totalSupplyFactor, LP address) with a 7-day update delay |
| Withdrawal | Timed (no fee) and instant (fee) |

## Adaptations for ZERO Energy

The ZERO Energy staking diverges from EasyStaking in the following ways (documented in Sections 6.2 and 6.3):

| EasyStaking | ZERO Energy |
| --- | --- |
| Deposited token == reward token (STAKE) | Deposited token is **ZREC**, reward token is **ZDAO** (different assets, decimals 3 vs 8) |
| `general` component uses `totalSupply` of STAKE | `usage` component uses **circulating `totalSupply` of ZDAO** as denominator, with ZREC staked as numerator |
| Mints the max rate and sends surplus to LP | **No LP**: only the actually accrued reward is minted |
| Fixed token supply assumptions | ZDAO is **mintable/burnable**; capacity is measured against circulating `totalSupply` |
| `MAX_EMISSION_RATE` = 15% fixed | `MAX_EMISSION_RATE` is a governance parameter (initial 10%, range 0–15%) |

## Key implementation concerns

1. **Decimal normalization.** ZREC uses 3 decimals, ZDAO uses 8. All reward math must normalize both scales with safe integer arithmetic and explicit rounding (see Section 6.2 and Annex A.2).
2. **Closed-form accrual.** The personal sigmoid's antiderivative `a·sqrt((t−b)²+c)` allows exact accrual without numerical integration, avoiding the double-time-dependence bug.
3. **Anti-dilution.** The usage component falls as ZDAO supply grows, bounding emission without a fixed reserve or schedule.
4. **Audit.** The EasyStaking adaptation does **not** inherit the original audit coverage; it requires independent testing and audit before deployment (Section 6.1).
