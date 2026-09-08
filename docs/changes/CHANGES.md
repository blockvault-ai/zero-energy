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
| 7 | Invariants I-4/I-5 (Genesis Right) existed | Removed; the decreasing issuance curve is no longer an invariant | [§11.1](../blockchain/04-invariants-parameters.md#111-invariants) |
| 8 | Supply metric was `CumulativeMinted` (never decreased) | Replaced by circulating `totalSupply`. Burning now **restores minting capacity**; `MAX_SUPPLY` remains the immutable ceiling | [§2.1](../tokenomics/01-zero-dao.md#21-supply-magnitudes) |
| 9 | §2.6 said burning does **not** regenerate capacity, contradicting §2.2 | §2.6 corrected: burning reduces `totalSupply` and **restores minting capacity** | [§2.6](../tokenomics/01-zero-dao.md#26-burn-mechanism) |
| 10 | Fixed 500M staking reserve and decreasing emission curve (250/125/62.5/31.25M) | Removed. Emission is **on-demand**, bounded by `MAX_SUPPLY` and `MAX_EMISSION_RATE` | [§6.3](../mathematics/01-rewards-model.md#63-issuance-on-demand-bounded-by-max_supply) |
| 11 | Allocation table expressed fixed amounts ("Allocation rights") | Renamed to "Allocation limits"; Presale/Merits linked to what is actually raised/claimed; the 50% headroom is a dynamic buffer | [§2.3](../tokenomics/01-zero-dao.md#23-allocation-limits) |
| 12 | Reward model had a "global component" with an undefined reference magnitude | Replaced by a **usage component** (supply-based, anti-dilutive): `r_usage = max · ZREC_staked / (totalSupply_ZDAO · totalSupplyFactor)` | [§6.2](../mathematics/01-rewards-model.md#usage-component-supply-based) |
| 13 | Reward accrual had a double time dependence (final rate × full period) | Reformulated as the exact integral of the instantaneous rate (closed-form antiderivative) | [§6.2 Accrual](../mathematics/01-rewards-model.md#accrual) |
| 14 | Sigmoid offset `b` nonzero caused an over-subtraction bug in the antiderivative (`√(b²+c)` vs `√c`) | Fixed with `b = 0`; antiderivative is `a·(√(T²+c) − √c)` | [Annex A.2](../mathematics/02-technical-reference.md#annex-a--technical-reference) |
| 15 | Sigmoid parameters `a, b, c` had no concrete values | Published: `a = 5 %`, `b = 0`, `c = 1,323 days²`; reference table updated to exact values | [§6.2 Temporal](../mathematics/01-rewards-model.md#temporal-component-position-age) |
| 16 | ZREC "circulating supply" was ambiguous; ZDAO "locked positions" undefined | Clarified: ZDAO has no locked positions, so `totalSupply` equals circulating supply | [§2.1](../tokenomics/01-zero-dao.md#21-supply-magnitudes) |
| 17 | ZREC minting origins omitted Presale in contracts | §8.2 and §9 now include Presale ZREC minting via Claim | [§8.2](../blockchain/01-governance.md#82-administrative-permissions) |
| 18 | "Accelerated depletion of the staking reserve" vector was obsolete | Renamed to "Accelerated emission / supply dilution" | [§10.2](../blockchain/03-security.md#102-vectors-considered) |
| 19 | "Foundational Issuance compensated" vs "offset" terminology mixed | Unified to "Foundational Issuance offset" | [§8.7](../blockchain/01-governance.md#87-state-publication) |
| 20 | ZERO DEX fee rate was undefined | Defined: initial 0.30 %, range 0.05 % – 1.00 % | [§7.3](../business-logic/03-zero-dex.md#73-fees) |
| 21 | Burn trigger mechanism was unspecified | Specified: automatic via keeper/transaction once the segregated accumulation reaches the threshold | [§2.6](../tokenomics/01-zero-dao.md#26-burn-mechanism) |
| 22 | "500 kWh/h" invalid unit | Corrected to "500 kW" | [§4.5](../infra/01-proof-of-energy.md#45-installation-registration-and-verification) |
| 23 | Docs version still read "v2.0"; §11 described as "single table" | Updated to v3.3; §11 described as three tables (invariants / parameters / pending) | [§11](../blockchain/04-invariants-parameters.md#11-invariants-and-parameters) |
| 24 | Presale raised amount presented as a fact | Marked as a theoretical maximum (full subscription assumed) | [§5.1](../business-logic/02-foundational-phase.md#51-presale) |
| 25 | No staking implementation reference existed | Added `implementation/` folder adapting EasyStaking as the base | [implementation](../implementation/README.md) |
| 25 | No staking implementation reference existed | Added `implementation/` folder adapting EasyStaking as the base |
