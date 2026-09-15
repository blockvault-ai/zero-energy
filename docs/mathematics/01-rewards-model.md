<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

> This document captures the mathematical core of the Official Staking System (Sections 6.2 and 6.3).
> Position and withdrawal rules are in `tokenomics/03-staking.md`.

## 6.2 Dynamic rewards model

The system **abandons any interpretation of a guaranteed fixed ROI**. The reward accrues **linearly in time**, with an annual rate that **halves each year**: a position held for a full year earns **100 % of its principal**; a second full year earns **50 %** of the principal; a third **25 %**; and so on.

### Temporal component (position age)

The reward accrues **linearly** within each staking year. The annual reward rate, by year of permanence, is:

$$ r_{n} = \frac{100\,\%}{2^{\,n-1}} $$

|  |  |
| --- | --- |
| **Staking year** | **Annual rate** |
| Year 1 | 100 % |
| Year 2 | 50 % |
| Year 3 | 25 % |
| Year 4 | 12.5 % |
| Subsequent | halving each year |

A position earns its current year's rate **proportionally to the time actually staked within that year**. There is no sigmoid, no back-loading, and no preferential weighting of early or late permanence: the rate is flat within each year.

### Accrual

The total reward is the sum, over each year of permanence, of the year's rate applied to the time staked in that year:

$$ reward = amount \cdot \sum_{n \ge 1} r_{n} \cdot \frac{\;t_{n}\;}{Y} $$

where $Y = 365$ days and $t_{n}$ is the portion of year $n$ that the position was staked (in days, capped at $Y$). Concretely:

- $T = 0$ → $reward = 0$.
- $T = 365$ days → $reward = amount$ (**100 % of principal**).
- $T = 2$ years → $reward = 1.5 \cdot amount$ (**150 %**, i.e. 100 % + 50 %).
- $T = 3$ years → $reward = 1.75 \cdot amount$ (**175 %**).
- A half-year position → $reward = 0.5 \cdot amount$ (**50 %**).

The accrual is therefore **piecewise linear** in time: a flat annual rate, halved at each yearly boundary.

> **Why this form.** Linearity makes the reward transparent and predictable at any point in time, without a normalization constant or a slope parameter to tune. The halving schedule (`100 % → 50 % → 25 % …`) follows the **same halving pattern** as the annual issuance ceiling of Section 6.3, so the reward rate and the issuance limits decline together.

**No auto-compounding.** There is no separate growth factor and no compounding: the linear accrual alone carries the entire reward.

**Only the ZDAO actually accrued are minted**, within the reserve and year ceilings of Section 6.3. There is no surplus allocation to a liquidity address: ZDAO not needed for rewards are simply never minted.

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
