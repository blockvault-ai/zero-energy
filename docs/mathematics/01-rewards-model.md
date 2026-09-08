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

The denominator uses the **circulating ZDAO supply**, not a fixed reference magnitude: because ZDAO is mintable/burnable, the reference moves with the actual economy. The overall issuance is nonetheless bounded by the 500M reserve and the decreasing curve of Section 6.3.

### Accrual

The reward is the **integral of the instantaneous rate over the deposit period**, not the final rate applied to the whole period. Because the personal sigmoid has a closed-form antiderivative, the accrual is computed exactly without numerical integration:

$$ reward = amount \cdot \left( a\cdot\left[\sqrt{T^2 + c} - \sqrt{c}\right] + r_{usage}\cdot T \right) / \left(1\ year\right) $$

where $T$ is the total elapsed time of the position. The sigmoid contribution `a·[√(T²+c) − √c]` is the exact integral `∫ r_personal(t) dt` from `0` to `T` (with `b = 0`, so the antiderivative `a·√(t²+c)` evaluates to `a·√c` at `t=0`). This avoids the double counting that would result from multiplying the final instantaneous rate by the whole elapsed period.

The combined rate is capped: $r_{personal} + r_{usage} \le MAX\_EMISSION\_RATE$.

**Only the ZDAO actually accrued are minted**, within the reserve and year ceilings of Section 6.3. There is no surplus allocation to a liquidity address: ZDAO not needed for rewards are simply never minted. This keeps emission strictly proportional to usage.

## 6.3 Issuance limits

Two limits operate cumulatively on the result of the previous formulas, alongside the `MAX_SUPPLY` ceiling.

**1. Absolute staking reserve — invariant.** 500,000,000 ZDAO. It cannot be allocated to treasury, team, liquidity, Merits, campaigns, strategic issuances, or any purpose other than the Official Staking System. Only the staking contract can mint against it, and only when a reward has been legitimately accrued.

**2. Decreasing maximum curve — halving.** Issuance ceiling per year, as an upper-bound reference:

|  |  |
| --- | --- |
| **Year** | **Maximum issuance** |
| Year 1 | up to 250,000,000 ZDAO |
| Year 2 | up to 125,000,000 ZDAO |
| Year 3 | up to 62,500,000 ZDAO |
| Year 4 | up to 31,250,000 ZDAO |
| Subsequent | halving each year |

The first year's ceiling is 250,000,000 ZDAO, halving each subsequent year. These values are **maximum ceilings, not issuance obligations**: if accrued rewards are below the available ceiling for a year, the difference is **not minted automatically**. The document must not describe these amounts as figures that will necessarily enter circulation.

**Why the ceiling is necessary.** ZREC grows without limit as certified production increases. A percentage rate applied over a growing staked base produces growing issuance; without a ceiling, the greater the protocol's energy success, the sooner the rewards reserve would be exhausted. The decreasing curve bounds that risk.

> **Mechanism.** The staking contract tracks the ZDAO minted per year against the curve. If aggregate accrual in a year would exceed that year's ceiling, accrual is reduced pro-rata among active positions so that the year's total never exceeds the ceiling. The unused portion of a year's ceiling is not carried over.

Only the ZDAO actually accrued (and within both ceilings) are minted; there is no LP surplus. Emission therefore tracks real usage — staking participation and circulating supply — bounded by the reserve, the halving curve, and `MAX_SUPPLY`.
