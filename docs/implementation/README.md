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
| `general` component uses `totalSupply` of STAKE | No supply component; the reward is **linear in time with a per-year halving rate** (100 % year 1, 50 % year 2, …) |
| Mints the max rate and sends surplus to LP | **No LP**: only the actually accrued reward is minted |
| Fixed token supply assumptions | ZDAO is **mintable/burnable**; capacity is measured against `totalSupply` (burn restores capacity under `MAX_SUPPLY`); a **500M staking reserve** and a **decreasing halving curve** bound staking issuance via the cumulative `stakingMinted` counter |
| `MAX_EMISSION_RATE` = 15% fixed | No emission-rate cap; the linear-in-time accrual with a per-year halving rate, plus the reserve and the annual halving ceiling, are the only bounds |

## Key implementation concerns

1. **Decimal normalization.** ZREC uses 3 decimals, ZDAO uses 8. All reward math must normalize both scales with safe integer arithmetic and explicit rounding (see Section 6.2 and Annex A.2).
2. **Closed-form accrual.** The linear accrual `reward = amount · Σ_n r_n · t_n / YEAR` (with `r_n = 100 % / 2^(n-1)`) requires no numerical integration and no slope parameter.
3. **Linear rewards with halving.** The reward is **linear in time** — a flat annual rate that halves at each yearly boundary (100 % year 1, 50 % year 2, …). Staking issuance is bounded by a 500M reserve and a decreasing halving curve (Section 6.3), tracked by a cumulative `stakingMinted` counter that burning does not rewind (the ERC-20 `MAX_SUPPLY`, by contrast, is restored on burn).
4. **Audit.** The EasyStaking adaptation does **not** inherit the original audit coverage; it requires independent testing and audit before deployment (Section 6.1).
