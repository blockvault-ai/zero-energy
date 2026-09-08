# Changes — Consolidated Log

All changes applied to the specification, in a single table.

| Ref | Contradiction / Fact | Change | Reference |
| --- | --- | --- | --- |
| 1 | "ZREC Genesis" existed as a distinct token type obtained via the "Genesis Right" | Removed. ZREC is a single fungible asset | [§3.2](../tokenomics/02-zero-rec.md#32-fungibility--a-single-token) |
| 2 | "Genesis Right" was a special right with single-consumption mechanics | Removed. Merits conversion is a plain choice resolved on the off-chain record | [§5.3](../business-logic/02-foundational-phase.md#53-claim-choices) |
| 3 | No launch milestone was defined | Introduced the **TGE**: fixes the reference date `t0` and opens the 6-month Claim window | [§5.2](../business-logic/02-foundational-phase.md#52-token-generation-event-tge-and-claim-window) |
| 4 | Presale delivered ZDAO only (no ZREC option) | Presale now delivers **ZDAO or ZREC** at the participant's choice, both free of Vesting | [§5.4](../business-logic/02-foundational-phase.md#54-presale-branch-zdao-or-zrec) |
| 5 | Merits participants chose between ZDAO (free) or ZREC (vesting) | Merits convert **directly to ZREC only** (cliff 6 months + 2 %/month, remaining in staking producing free ZDAO) | [§5.5](../business-logic/02-foundational-phase.md#55-merits-branch-zrec-with-vesting) |
| 6 | "Genesis Offset Program" / "Genesis Offset NFT" / "Genesis Issuance" | Renamed to **Foundational Offset Program** / **Foundational Offset NFT** / **Foundational Issuance**; the Foundational Offset NFT is an instance of the Energy Certification NFT with `purpose = Foundational Offset` | [§3.6](../tokenomics/02-zero-rec.md#36-foundational-offset-program) |
| 7 | Invariants I-4/I-5 (Genesis Right) existed | Removed. The decreasing issuance curve is an invariant | [§11.1](../blockchain/04-invariants-parameters.md#111-invariants) |
| 8 | Supply metric was ambiguous (totalSupply vs cumulative) | `CumulativeMinted` is the growing counter that records every minting. Burning does **not** restore minting capacity | [§2.1](../tokenomics/01-zero-dao.md#21-supply-magnitudes) |
| 9 | Contradiction: §2.6 said burning regenerates capacity vs §2.2 | §2.6 corrected: burning reduces `totalSupply` but **does not regenerate issuance capacity** | [§2.6](../tokenomics/01-zero-dao.md#26-burn-mechanism) |
| 10 | Staking had no fixed bound | Staking is bounded by a **500M reserve** and a **decreasing halving curve** as an annual ceiling | [§6.3](../mathematics/01-rewards-model.md#63-issuance-limits) |
| 11 | Allocation table expressed fixed amounts ("Allocation rights") | Renamed to "Allocation limits"; Presale/Merits linked to what is actually raised/claimed; Staking backed by an immutable 500M reserve | [§2.3](../tokenomics/01-zero-dao.md#23-allocation-limits) |
| 12 | Reward model had a "global component" with an undefined reference magnitude | Replaced by a **usage component** (supply-based, anti-dilutive): `r_usage = max · ZREC_staked / (totalSupply_ZDAO · totalSupplyFactor)` | [§6.2](../mathematics/01-rewards-model.md#usage-component-supply-based) |
| 13 | Reward accrual had a double time dependence (final rate × full period) | Reformulated as the exact integral of the instantaneous rate (closed-form antiderivative) | [§6.2 Accrual](../mathematics/01-rewards-model.md#accrual) |
| 14 | Sigmoid offset `b` nonzero caused an over-subtraction bug in the antiderivative (`√(b²+c)` vs `√c`) | Fixed with `b = 0`; antiderivative is `a·(√(T²+c) − √c)` | [Annex A.2](../mathematics/02-technical-reference.md#annex-a--technical-reference) |
| 15 | Sigmoid parameters `a, b, c` had no concrete values | Published: `a = 5 %`, `b = 0`, `c = 1,323 days²`; reference table updated to exact values | [§6.2 Temporal](../mathematics/01-rewards-model.md#temporal-component-position-age) |
| 16 | ZREC "circulating supply" was ambiguous; ZDAO "locked positions" undefined | Clarified: ZDAO has no locked positions, so `totalSupply` equals circulating supply | [§2.1](../tokenomics/01-zero-dao.md#21-supply-magnitudes) |
| 17 | ZREC minting origins omitted Presale in contracts | §8.2 and §9 now include Presale ZREC minting via Claim | [§8.2](../blockchain/01-governance.md#82-administrative-permissions) |
| 18 | "Accelerated depletion of the staking reserve" vector was mislabeled | Renamed to reflect the current ceiling: decreasing curve and 500M reserve | [§10.2](../blockchain/03-security.md#102-vectors-considered) |
| 19 | "Foundational Issuance compensated" vs "offset" terminology mixed | Unified to "Foundational Issuance offset" | [§8.7](../blockchain/01-governance.md#87-state-publication) |
| 20 | ZERO DEX fee rate was undefined | Defined: initial 0.30 %, range 0.05 % – 1.00 % | [§7.3](../business-logic/03-zero-dex.md#73-fees) |
| 21 | Burn trigger mechanism was unspecified | Specified: automatic via keeper/transaction once the segregated accumulation reaches the threshold | [§2.6](../tokenomics/01-zero-dao.md#26-burn-mechanism) |
| 22 | "500 kWh/h" invalid unit | Corrected to "500 kW" | [§4.5](../infra/01-proof-of-energy.md#45-installation-registration-and-verification) |
| 23 | Docs version still read "v2.0"; §11 described as "single table" | Updated to v3.3; §11 described as three tables (invariants / parameters / pending) | [§11](../blockchain/04-invariants-parameters.md#11-invariants-and-parameters) |
| 24 | Presale raised amount presented as a fact | Marked as a theoretical maximum (full subscription assumed) | [§5.1](../business-logic/02-foundational-phase.md#51-presale) |
| 25 | No staking implementation reference existed | Added `implementation/` folder adapting EasyStaking as the base | [implementation](../implementation/README.md) |
| 26 | No fixed supply counter existed | Added `CumulativeMinted`: every minting records how much has been minted; capacity = `MAX_SUPPLY − CumulativeMinted` | [§2.1](../tokenomics/01-zero-dao.md#21-supply-magnitudes) |
| 27 | No annual emission halving existed | Added the **halving curve**: 250M (year 1) → 125M → 62.5M → 31.25M, halving each year, as a **ceiling** (not an obligation) | [§6.3](../mathematics/01-rewards-model.md#63-issuance-limits) |
| 28 | No fixed staking reserve existed | Added the **500M staking reserve** as an invariant (I-3) | [§2.3](../tokenomics/01-zero-dao.md#23-allocation-limits) |
| 29 | Curve not classified as an invariant | The decreasing issuance curve is classified as **I-4** (absolute annual ceiling) | [§11.1](../blockchain/04-invariants-parameters.md#111-invariants) |
