# Changes — Consolidated Log

All changes applied to the specification, in a single table.

| Ref | Contradiction / Fact | Change |
| --- | --- | --- |
| 1 | "ZREC Genesis" existed as a distinct token type obtained via the "Genesis Right" | Removed. ZREC is a single fungible asset |
| 2 | "Genesis Right" was a special right with single-consumption mechanics | Removed. Merits conversion is a plain choice resolved on the off-chain record |
| 3 | No launch milestone was defined | Introduced the **TGE**: fixes the reference date `t0` and opens the 6-month Claim window |
| 4 | Presale delivered ZDAO only (no ZREC option) | Presale now delivers **ZDAO or ZREC** at the participant's choice, both free of Vesting |
| 5 | Merits participants chose between ZDAO (free) or ZREC (vesting) | Merits convert **directly to ZREC only** (cliff 6 months + 2 %/month, remaining in staking producing free ZDAO) |
| 6 | "Genesis Offset Program" / "Genesis Offset NFT" / "Genesis Issuance" | Renamed to **Foundational Offset Program** / **Foundational Offset NFT** / **Foundational Issuance**; the Foundational Offset NFT is an instance of the Energy Certification NFT with `purpose = Foundational Offset` |
| 7 | Invariants I-4/I-5 (Genesis Right) existed | Removed; the decreasing issuance curve is no longer an invariant |
| 8 | Supply metric was `CumulativeMinted` (never decreased) | Replaced by circulating `totalSupply`. Burning now **restores minting capacity**; `MAX_SUPPLY` remains the immutable ceiling |
| 9 | §2.6 said burning does **not** regenerate capacity, contradicting §2.2 | §2.6 corrected: burning reduces `totalSupply` and **restores minting capacity** |
| 10 | Fixed 500M staking reserve and decreasing emission curve (250/125/62.5/31.25M) | Removed. Emission is **on-demand**, bounded by `MAX_SUPPLY` and `MAX_EMISSION_RATE` |
| 11 | Allocation table expressed fixed amounts ("Allocation rights") | Renamed to "Allocation limits"; Presale/Merits linked to what is actually raised/claimed; the 50% headroom is a dynamic buffer |
| 12 | Reward model had a "global component" with an undefined reference magnitude | Replaced by a **usage component** (supply-based, anti-dilutive): `r_usage = max · ZREC_staked / (totalSupply_ZDAO · totalSupplyFactor)` |
| 13 | Reward accrual had a double time dependence (final rate × full period) | Reformulated as the exact integral of the instantaneous rate (closed-form antiderivative) |
| 14 | Sigmoid offset `b` nonzero caused an over-subtraction bug in the antiderivative (`√(b²+c)` vs `√c`) | Fixed with `b = 0`; antiderivative is `a·(√(T²+c) − √c)` |
| 15 | Sigmoid parameters `a, b, c` had no concrete values | Published: `a = 5 %`, `b = 0`, `c = 1,323 days²`; reference table updated to exact values |
| 16 | ZREC "circulating supply" was ambiguous; ZDAO "locked positions" undefined | Clarified: ZDAO has no locked positions, so `totalSupply` equals circulating supply |
| 17 | ZREC minting origins omitted Presale in contracts | §8.2 and §9 now include Presale ZREC minting via Claim |
| 18 | "Accelerated depletion of the staking reserve" vector was obsolete | Renamed to "Accelerated emission / supply dilution" |
| 19 | "Foundational Issuance compensated" vs "offset" terminology mixed | Unified to "Foundational Issuance offset" |
| 20 | ZERO DEX fee rate was undefined | Defined: initial 0.30 %, range 0.05 % – 1.00 % |
| 21 | Burn trigger mechanism was unspecified | Specified: automatic via keeper/transaction once the segregated accumulation reaches the threshold |
| 22 | "500 kWh/h" invalid unit | Corrected to "500 kW" |
| 23 | Docs version still read "v2.0"; §11 described as "single table" | Updated to v3.3; §11 described as three tables (invariants / parameters / pending) |
| 24 | Presale raised amount presented as a fact | Marked as a theoretical maximum (full subscription assumed) |
| 25 | No staking implementation reference existed | Added `implementation/` folder adapting EasyStaking as the base |
