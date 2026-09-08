<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

> This document captures the mathematical core of the Official Staking System (Sections 6.2 and 6.3).
> Position and withdrawal rules are in `tokenomics/03-staking.md`.

## 6.2 Dynamic rewards model

The system **abandons any interpretation of a guaranteed fixed ROI**. The reward rate results from two components that are added together, each with its own ceiling.

### Temporal component (position age)

Sigmoid curve over the time the position has been deposited:

$$ r_{personal} = \frac{a \cdot (t - b)}{\sqrt{(t - b)^2 + c}} $$

| Variable | Definition |
| --- | --- |
| $t$ | time in staking |
| $a$ | component ceiling |
| $b$ | offset |
| $c$ | slope parameter |

$$ r_{personal} = 0 \quad \text{if} \quad (t - b) < 0 $$

The curve grows quickly at first and then flattens: it rewards permanence without locking anyone in. With reference parameters, the behavior is:

|  |  |
| --- | --- |
| **Time in staking** | **Proportion of ceiling reached** |
| 1 day | ~3 % |
| 7 days | ~19 % |
| 21 days | ~50 % |
| 30 days | ~63 % |
| 90 days | ~93 % |
| 1 year | ~99 % |

### Global component (participation in the system)

Depends on the aggregate level of participation: the higher the deposited volume relative to a reference magnitude, the higher the rate, up to its ceiling.

$$ r_{global} = r_{max} \cdot \min\!\left(1,\; \frac{ZREC_{staked}}{reference}\right) $$

> **Mandatory adaptation.** The reference model computes this magnitude over the maximum supply of the deposited token. **ZREC has no maximum supply**: it grows with certified energy production. Therefore the reference **cannot be MAX_SUPPLY** and must be defined specifically for the ZREC economy — for example, a magnitude of ZREC eligible for staking or a mathematically suitable equivalent. The concrete parameter remains pending definitive implementation.

### Accrual

$$ reward = amount \cdot (r_{personal} + r_{global}) \cdot \frac{time}{1\ year} $$

**Only the ZDAO actually accrued are minted.** ZDAO not needed for rewards are not minted. This is a deliberate deviation from the reference model, which always mints to the maximum and allocates the surplus to a liquidity address; ZERO Energy **does not adopt that distribution**, because it would consume the reserve at the maximum rate regardless of actual participation.

## 6.3 Issuance limit

Two limits operate cumulatively on the result of the previous formulas.

**1. Absolute reserve — invariant.** 500,000,000 ZDAO. It cannot be allocated to treasury, team, liquidity, Merits, campaigns, strategic emissions, or any purpose other than staking.

**2. Decreasing maximum curve.** Issuance ceiling per period, as an upper-bound reference:

|  |  |
| --- | --- |
| **Period** | **Maximum issuance** |
| Year 1 | up to 250,000,000 ZDAO |
| Year 2 | up to 125,000,000 ZDAO |
| Year 3 | up to 62,500,000 ZDAO |
| Year 4 | up to 31,250,000 ZDAO |
| Subsequent | following the decreasing curve |

> These values are **maximum ceilings, not issuance obligations**. If accrued rewards are below the available maximum, the difference is **not minted automatically**. The document must not describe these amounts as figures that will necessarily enter circulation.

**Why the ceiling is necessary.** ZREC grows without limit as certified production increases. A percentage rate applied over a growing base produces growing issuance: without a ceiling, the greater the protocol's energy success, the sooner the rewards reserve would be exhausted. The decreasing curve bounds that risk.

⬜ **Pending definitive implementation:** the adjustment mechanism when aggregate accrual exceeds the period ceiling. The anticipated criterion is pro-rata reduction among active positions.
