<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# 11. INVARIANTS AND PARAMETERS

This section replaces any enumeration of principles scattered throughout the document. It gathers in a single place what cannot change and what can, with its range.

## 11.1 Invariants

They cannot be modified by any governance mechanism or by contract upgrades. They are implemented so that their verification does not depend on upgradable code.

|  |  |  |
| --- | --- | --- |
|  | **Invariant** | **Section** |
| I-1 | `CumulativeMinted` of ZDAO never exceeds 1,000,000,000 | 2.1 |
| I-2 | Burning does not reduce `CumulativeMinted` nor regenerate issuance capacity | 2.2 |
| I-3 | 500,000,000 ZDAO remain reserved exclusively for the Official Staking System and only the staking contract can mint them | 2.3 and 6.3 |
| I-4 | The decreasing issuance curve (250/125/62.5/31.25M) is an absolute annual ceiling | 6.3 |
| I-5 | A foundational allocation is consumed only once; the choice between ZDAO and ZREC resides in the off-chain record, never in the token | 5.3 |
| I-6 | ZREC is a single fungible asset; the restrictions live in the positions, never in the token | 3.2 |
| I-7 | No unit of energy can sustain more than one issuance nor more than one certificate | 4.2 |
| I-8 | A batch in FINALIZED state cannot be reused | 4.6 |
| I-9 | No minting may be authorized by a non-reproducible criterion | 4.5 |
| I-10 | The DAO is the supreme authority; the Committee only exercises delegated competences | 8.1 |

## 11.2 Configurable parameters

Modifiable by governance within the indicated ranges and subject to the timelock of Section 8.3.

|  |  |  |  |
| --- | --- | --- | --- |
| **Parameter** | **Initial value** | **Range** | **Section** |
| Energy Issuance split | 50% producer | 30% – 70% | 3.3 |
| Batch closure — time | 24 hours | configurable | 4.6 |
| Batch closure — energy | 1 MWh | configurable | 4.6 |
| Provisionality window | 72 hours | configurable downward | 4.6 |
| Burn Execution Threshold | 10,000 ZDAO | configurable | 2.6 |
| ZERO DEX fee | 0.30 % | 0.05% – 1.00% | 7.3 |
| Fee split | 40/30/20/10 | see table 7.3 | 7.3 |
| Committee mandate | 24 months | renewable | 8.1 |
| MAX_EMISSION_RATE (staking) | 10 % APR | 0% – 15% | 6.2 |
| Usage component ceiling | 5 % APR | 0% – 7.5% | 6.2 |
| Personal component ceiling (a) | 5 % APR | 0% – 7.5% | 6.2 |
| Reward sigmoid offset (b) | 0 | fixed | 6.2 |
| Reward sigmoid slope (c) | 1,323 days² | with safety limits | 6.2 |
| totalSupplyFactor | 50 % | 10% – 100% | 6.2 |
| Staking withdrawal periods and fee | pending implementation | with safety limits | 6.6 |

## 11.3 Pending decisions

They are expressly declared rather than replaced by assumptions.

|  |  |  |
| --- | --- | --- |
| **Question** | **Blocks** | **Section** |
| Treatment of unclaimed allocations when the window closes | Closure of Milestone 1 | 5.2 |
| Definitive values of withdrawal periods and fee | Implementation of staking | 6.6 |
| Objective criteria for Milestones 4, 5, and 6 | Decentralization | 8.6 |
| Definitive requirements for an external oracle | Milestone 3 | 4.3 |
