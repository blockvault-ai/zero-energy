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
| I-4 | The Genesis Right is consumed only once and does not reside in the token | 5.3 |
| I-5 | Once the foundational window is closed, the Genesis Right is irreversibly disabled | 5.2 |
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
| ZERO DEX fee | on volume | 0.05% – 1.00% | 7.3 |
| Fee split | 40/30/20/10 | see table 7.3 | 7.3 |
| Committee mandate | 24 months | renewable | 8.1 |
| Reward curve parameters | pending implementation | with safety limits | 6.2 |
| Staking withdrawal periods and fee | pending implementation | with safety limits | 6.6 |

## 11.3 Pending decisions

They are expressly declared rather than replaced by assumptions.

|  |  |  |
| --- | --- | --- |
| **Question** | **Blocks** | **Section** |
| Treatment of unclaimed allocations when the window closes | Closure of Milestone 1 | 5.2 |
| Reference magnitude of the global component of staking | Implementation of staking | 6.2 |
| Adjustment mechanism when the accrual exceeds the period ceiling | Implementation of staking | 6.3 |
| Definitive values of withdrawal periods and fee | Implementation of staking | 6.6 |
| Objective criteria for Milestones 4, 5, and 6 | Decentralization | 8.6 |
| Definitive requirements for an external oracle | Milestone 3 | 4.3 |
