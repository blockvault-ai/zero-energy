<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# ANNEX B — CHANGE LOG

Changes in version 2.0 relative to 1.2, for individual review by governance.

**Architecture changes**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 1 | Claim and Genesis are unified into a single 6-month window. The Claim → ZDAO → later-conversion model disappears | 5.2 |
| 2 | The mint-and-burn operation is removed. The ZDAO of the Genesis Right never come to exist on-chain | 3.3 and 5.4 |
| 3 | The Genesis Right resides in the off-chain allocation, not in the token, and is consumed a single time | 5.3 |
| 4 | The Vesting contract is removed. The Merits lockup lives in the withdrawal rules of the staking position | 5.5 |
| 5 | Merits now resolve directly into ZREC Genesis with automatic staking: 6-month lockup and 2% monthly for 50 months | 5.5 |
| 6 | New dynamic staking model (temporal component + global component), taking EasyStaking as a conceptual reference | 6 |
| 7 | ZERO DEX is now defined as the Dashboard and main interface of the ecosystem | 7 |
| 8 | DePIN and Proof of Energy are incorporated as the architectural framework, with express delimitation from the Base consensus | 1.1 and 1.2 |
| 9 | Official nomenclature with tickers ZDAO and ZREC | Preliminary note |

**Economic changes**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 10 | The cap operates again on `CumulativeMinted`. Burning does NOT regenerate issuance capacity. Reverts the circulating-supply formulation of v1.2 | 2.1 and 2.2 |
| 11 | The halving by consumption is replaced by the decreasing annual curve as the cap of the dynamic model | 6.3 |
| 12 | Strategic Emissions is set at 200M (150M liquidity + 50M contingencies, indicative) | 2.3 |
| 13 | Complete Presale table: five tranches with minimum investment, available in parallel | 5.1 |
| 14 | Official decimals: ZDAO 8, ZREC 3, with a minimum unit of 1 kWh | 2 and 3.1 |
| 15 | Initial Burn Execution Threshold of 10,000 ZDAO, governable | 2.6 |

**Changes in the energy layer**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 16 | The restriction on energy fed to the grid is removed. Generation is certified, regardless of destination | 4.1 |
| 17 | The collateral requirement for oracles is removed | 10.4 |
| 18 | The initial per-producer issuance limit is omitted | 3.3 |
| 19 | Hybrid batch closure: 24 hours or 1 MWh | 4.6 |
| 20 | 72-hour provisionality window and FINALIZED state | 4.6 |
| 21 | Triple verification of the nominal power of the installations | 4.5 |

**Governance changes**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 22 | Protocol Committee mandate: 24 months | 8.1 |
| 23 | Decentralization milestones are defined by objective metrics, not dates, and remain open | 8.6 |

**Structure and length changes**

The document goes from 13 to 12 sections and substantially reduces its length without losing normative content. Sections that repeated already-exposed material are removed: the complete ecosystem flow (now a single diagram in 1.5), the enumeration of fundamental principles (replaced by the invariants table in 11.1), and the objectives subsections of each component. The Claim System is integrated into the foundational phase and the data acquisition layer into the Proof of Energy section. Each concept is defined once and the rest of the document references it.
