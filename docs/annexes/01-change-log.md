<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# ANNEX B — CHANGE LOG

## Changes in version 3.8 (relative to 3.7)

**Linear reward with per-year halving**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 1 | The reward model is changed from a back-loaded temporal sigmoid to **linear accrual in time**: a flat annual rate that halves each year (100 % year 1, 50 % year 2, 25 % year 3, …), applied proportionally to the time staked within each year | 6.2 |
| 2 | The sigmoid slope parameter `c` is **removed**; the annual reward rate is no longer configurable and follows the immutable halving schedule | 6.2 and 11.2 |
| 3 | Annex A.2 accrual rewritten to the piecewise-linear sum `reward = amount · Σ_n r_n · t_n / YEAR` | Annex A.2 |

---

## Changes in version 3.7 (relative to 3.6)

**100 %/year back-loaded reward**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 1 | The reward is a single **temporal sigmoid normalized to deliver exactly 100 % of the principal over one year** (`reward = amount · (√(T²+c) − √c) / (√(Y²+c) − √c)`) | 6.2 |
| 2 | The `mint-growth factor` (`f_growth`) is **removed**; the back-loading is carried solely by the sigmoid's `c` parameter | 6.2 |
| 3 | `MAX_EMISSION_RATE`, the personal ceiling `a`, and the growth parameters `K`/`n` are **removed**; the only bounds are the 500M reserve, the annual halving, and `MAX_SUPPLY` | 11.2 |
| 4 | `totalSupplyFactor` is also removed | 11.2 |

---

## Changes in version 3.6 (relative to 3.5)

**Reward model based on `stakingMinted`**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 1 | The `usage` component (ZREC_staked / totalSupply) is **removed** and replaced by a **mint-growth factor** `f = x² / (1 + x²)` with `x = stakingMinted / K` that scales the reward up as the reserve is consumed | 6.2 |
| 2 | The temporal sigmoid is multiplied by the growth factor: `r_effective = a·t/√(t²+c) · f_growth` | 6.2 |
| 3 | New governance parameters: mint-growth half-growth `K` (250M) and steepness `n` (2); `totalSupplyFactor` is removed | 11.2 |

---

## Changes in version 3.5 (relative to 3.4)

**Separation of token and staking minting counters**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 1 | `MAX_SUPPLY` lives in the ZDAO ERC-20 and bounds `totalSupply`; minting capacity is `MAX_SUPPLY − totalSupply` | 2.1 and 2.2 |
| 2 | Burning ZDAO **restores minting capacity** under `MAX_SUPPLY`, following the standard ERC-20 mintable/burnable capability; any holder may burn their own ZDAO | 2.2 and 2.6 |
| 3 | The staking cap is tracked by `stakingMinted`, a cumulative counter held in the staking contract (capped at 500M); burning does **not** restore it | 2.3, 6.3 and Annex A.1 |
| 4 | The global supply counter `CumulativeMinted` is replaced by the ERC-20 `totalSupply`; `stakingMinted` is the only monotonic record | 2.1 |

---

## Changes in version 3.4 (relative to 3.3)

**Supply and issuance caps**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 1 | The supply metric is `CumulativeMinted` (a growing counter). Burning **does not** regenerate issuance capacity | 2.1 and 2.2 |
| 2 | A fixed **500M staking reserve** is established as an invariant; only the staking contract can mint against it | 2.3 and 6.3 |
| 3 | The **decreasing maximum curve (halving)** is established as an annual ceiling: 250M / 125M / 62.5M / 31.25M, halving each year (a ceiling, not an obligation) | 6.3 |
| 4 | Every minting is recorded in `CumulativeMinted` to compute how much has been minted at any time | 2.1 and Annex A.1 |

---

## Changes in version 3.3 (relative to 3.2)

**Consistency and corrections**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 1 | Burn mechanism corrected: ZDAO burned **restores minting capacity** (aligned with Section 2.2) | 2.6 |
| 2 | Reward sigmoid fixed with `b = 0`; the accrual antiderivative is corrected to `a·(√(T²+c) − √c)` | 6.2 and Annex A.2 |
| 3 | Concrete sigmoid parameters published: `a = 5%`, `b = 0`, `c = 1,323 days²`; reference table updated to exact values | 6.2 and 11.2 |
| 4 | ZREC minting origins clarified: Presale ZREC is minted via Claim alongside Merits ZREC | 8.2 and 9 |
| 5 | ZDAO "circulating supply" clarified: no locked ZDAO positions, so `totalSupply` equals circulating supply | 2.1 |
| 6 | The 50% MAX_SUPPLY headroom (formerly the 500M reserve) is documented as a dynamic buffer | 2.3 |
| 7 | Security vector renamed: "Accelerated depletion of the staking reserve" → "Accelerated emission / supply dilution" | 10.2 |

---

## Changes in version 3.2 (relative to 3.1)

**Economic changes — dynamic mintable/burnable emission**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 1 | The supply metric changes from `CumulativeMinted` to the circulating `totalSupply`. Burning now **restores minting capacity**; `MAX_SUPPLY` remains the absolute ceiling | 2.1 and 2.2 |
| 2 | The allocation table expresses **maximum limits**, not fixed amounts. Presale and Merits are linked to what is actually raised/claimed | 2.3 |
| 3 | The fixed 500M staking reserve and the decreasing emission curve (250/125/62.5/31.25M) are **removed** | 6.3 |
| 4 | The reward model gains a **usage component** (supply-based, anti-dilutive) driven by ZREC staked relative to circulating ZDAO supply; the "global component" is replaced | 6.2 |
| 5 | Emission is **on-demand**: only the accrued reward is minted, no LP surplus | 6.2 |
| 6 | New governance parameters: `MAX_EMISSION_RATE`, usage ceiling, `totalSupplyFactor` | 11.2 |
| 7 | A documentation-only `implementation/` folder is added, adapting EasyStaking as the staking base | implementation |

---

## Changes in version 3.1 (relative to 3.0)

**Architecture changes**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 1 | The **Presale delivers ZDAO or ZREC** at the participant's choice, both free of Vesting (no lock, no mandatory staking stay) | 5.4 |
| 2 | ZREC origins are expanded from two to **three**: Presale ZREC, Merits ZREC, and Energy ZREC | 3.3 |

---

## Changes in version 3.0 (relative to 2.0)

**Architecture changes**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 1 | The concept of **ZREC Genesis is removed**. ZREC is a single fungible asset with two accounting origins: Merits (foundational) and Energy (certified production) | 3.2 and 3.3 |
| 2 | The **Genesis Right is removed**. The Merits choice between ZDAO and ZREC is a plain choice at claim time, resolved on the off-chain allocation | 5.3 |
| 3 | A **Token Generation Event (TGE)** is defined as the launch milestone that fixes the reference date and opens the 6-month Claim window | 5.2 |
| 4 | The **Presale delivers ZDAO only**; the ZREC option (former Option B) is removed | 5.4 |
| 5 | **Merits participants choose** between ZDAO (free) or ZREC (cliff 6 months + 2 % monthly, remaining in staking and producing free ZDAO) | 5.5 |
| 6 | The **Genesis Offset Program** is renamed **Foundational Offset Program**; the Genesis Offset NFT is an instance of the Energy Certification NFT with `purpose = Foundational Offset` | 3.6 and 3.7 |
| 7 | Invariants I-4/I-5 (Genesis Right) are removed; the decreasing issuance curve is classified as an invariant | 11.1 |

**Economic changes**

|  |  |  |
| --- | --- | --- |
|  | **Change** | **Section** |
| 8 | The staking accrual is reformulated as the **exact integral** of the instantaneous rate (closed-form antiderivative of the sigmoid), removing the double time dependence | 6.2 |
| 9 | The ZERO DEX fee rate is defined (initial 0.30 %, range 0.05 % – 1.00 %); the burn trigger mechanism is specified | 7.3 and 2.6 |

---

## Changes in version 2.0 relative to 1.2

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
