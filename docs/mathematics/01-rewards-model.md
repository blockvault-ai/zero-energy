<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

> This document captures the mathematical core of the Official Staking System (Sections 6.2 and 6.3).
> Position and withdrawal rules are in `tokenomics/03-staking.md`.

## 6.2 Dynamic rewards model

The system **abandons any interpretation of a guaranteed fixed ROI**. The reward rate results from two components that are added together, each with its own ceiling.

### Temporal component (position age)

Sigmoid curve over the time the position has been deposited:

$$ r_{personal} = \frac{a \cdot t}{\sqrt{t^2 + c}} $$

| Variable | Definition | Reference value |
| --- | --- | --- |
| $t$ | time in staking | — |
| $a$ | component ceiling (personal APR cap) | **5 %** |
| $b$ | offset | **0** (removed) |
| $c$ | slope parameter | **1,323 days²** |

With $b = 0$, the curve starts at $t=0$ and grows monotonically toward $a$, rewarding permanence without locking anyone in. The reference parameters are:

|  |  |
| --- | --- |
| **Time in staking** | **Proportion of ceiling reached** |
| 1 day | ~2.7 % |
| 7 days | ~18.9 % |
| 21 days | ~50.0 % |
| 30 days | ~63.6 % |
| 90 days | ~92.7 % |
| 1 year | ~99.5 % |

These values follow `t / sqrt(t² + c)` with `c = 1,323 days²`, and are therefore fully reproducible.

### Usage component (supply-based)

The second component ties the rate to real usage: it rises with the amount of ZREC staked relative to the circulating ZDAO supply. The higher the staking participation and the lower the ZDAO in circulation, the higher the rate, up to its ceiling. This is **anti-dilutive**: as more ZDAO enters circulation, the emission rate falls.

$$ r_{usage} = r_{usage,max} \cdot \min\!\left(1,\; \frac{ZREC_{staked}}{totalSupply_{ZDAO} \cdot totalSupplyFactor}\right) $$

| Variable | Definition |
| --- | --- |
| $r_{usage,max}$ | usage component ceiling (≤ half of MAX_EMISSION_RATE) |
| $ZREC_{staked}$ | total ZREC deposited in staking |
| $totalSupply_{ZDAO}$ | circulating ZDAO supply (minted minus burned) |
| $totalSupplyFactor$ | governance parameter (10%–100%): percentage of `totalSupply` used as the denominator. Larger factor ⇒ smaller rate |

The denominator uses the **circulating ZDAO supply**, not a fixed reference magnitude: because ZDAO is mintable/burnable, the reference moves with the actual economy. No fixed reserve or schedule is involved.

### Accrual

The reward is the **integral of the instantaneous rate over the deposit period**, not the final rate applied to the whole period. Because the personal sigmoid has a closed-form antiderivative, the accrual is computed exactly without numerical integration:

$$ reward = amount \cdot \left( a\cdot\left[\sqrt{T^2 + c} - \sqrt{c}\right] + r_{usage}\cdot T \right) / \left(1\ year\right) $$

where $T$ is the total elapsed time of the position. The sigmoid contribution `a·[√(T²+c) − √c]` is the exact integral `∫ r_personal(t) dt` from `0` to `T` (with `b = 0`, so the antiderivative `a·√(t²+c)` evaluates to `a·√c` at `t=0`). This avoids the double counting that would result from multiplying the final instantaneous rate by the whole elapsed period.

The combined rate is capped: $r_{personal} + r_{usage} \le MAX\_EMISSION\_RATE$.

**Only the ZDAO actually accrued are minted.** There is no fixed reserve and no surplus allocation to a liquidity address: ZDAO not needed for rewards are simply never minted. This keeps emission strictly proportional to usage.

## 6.3 Issuance: on-demand, bounded by MAX_SUPPLY

There is **no fixed reserve and no decreasing schedule**. ZDAO is minted **on demand**, only when a reward is legitimately accrued, and is bounded by a single hard limit:

**1. Absolute ceiling — invariant.** `totalSupply(ZDAO) ≤ 1,000,000,000`. Verified on every mint (Section 2.1).

**2. Rate ceilings.** The emission rate itself is bounded by `MAX_EMISSION_RATE` (personal + usage components), a governance parameter, not a monetary amount.

Because ZDAO is mintable/burnable, the circulating supply contracts when ZDAO is burned (Section 2.6), restoring capacity. Emission therefore tracks real usage — staking participation and circulating supply — rather than a predetermined calendar or reserve.

**Why a rate ceiling is necessary.** ZREC grows without limit as certified production increases. A percentage rate applied over a growing staked base produces growing issuance; the `MAX_EMISSION_RATE` cap, combined with the anti-dilutive usage component (which falls as ZDAO supply grows), bounds that risk without resorting to fixed monetary ceilings.
