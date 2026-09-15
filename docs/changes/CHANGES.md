# Changes — Consolidated Log

Every difference between the current `docs/` tree (authoritative) and the original source `ZERO-ENERGY-Especificacion-Tecnica-v2.0.md` (historical), in a single table.

| Ref | Original (`v2.0`) | Current (`docs/`) | Reference |
| --- | --- | --- | --- |
| 1 | Supply metric is `CumulativeMinted` (a counter that only grows) | `MAX_SUPPLY` lives in the ERC-20 and bounds `totalSupply`; a separate cumulative `stakingMinted` counter lives in the staking contract | [§2.1](../tokenomics/01-zero-dao.md#21-supply-magnitudes) |
| 2 | Burning never regenerates emission capacity (`CumulativeMinted` does not decrease) | Burning reduces `totalSupply` and **restores minting capacity under `MAX_SUPPLY`**, but does **not** restore the staking reserve | [§2.2](../tokenomics/01-zero-dao.md#22-burning-restores-minting-capacity-token-but-not-the-staking-cap) |
| 3 | Allocation table expresses fixed amounts ("Asignación") | Allocation table expresses **maximum limits**; Presale/Merits are linked to what is actually raised/claimed | [§2.3](../tokenomics/01-zero-dao.md#23-allocation-limits) |
| 4 | ZDAO "circulating supply" is `totalSupply` minus locked positions | ZDAO has **no locked positions**: `totalSupply` equals circulating supply | [§2.1](../tokenomics/01-zero-dao.md#21-supply-magnitudes) |
| 5 | ZREC has **two** origins (Genesis, Energy) | ZREC has **three** origins (Presale, Merits, Energy) | [§3.3](../tokenomics/02-zero-rec.md#33-three-origins) |
| 6 | A special "Genesis Right" (`Derecho Genesis`) governs the Genesis conversion | No special right: a plain **Claim choice** with terminal consumption, resolved on the off-chain record | [§5.3](../business-logic/02-genesis-phase.md#53-claim-choices) |
| 7 | Only a single 6-month "Claim and Genesis window" | A **TGE** milestone is introduced, fixing the reference date `t0` and opening the 6-month Claim window | [§5.2](../business-logic/02-genesis-phase.md#52-token-generation-event-tge-and-claim-window) |
| 8 | Phase/window/allocation/issuance use "foundational" (`fundacional`), with "Genesis" reserved for the Right and the Offset Program | "Genesis" used uniformly: **Genesis phase**, Genesis window, Genesis allocation, Genesis issuance, Genesis Offset | [§5](../business-logic/02-genesis-phase.md) |
| 9 | Reward model is a temporal **sigmoid + global component** (`tasa_personal + tasa_global`) | Reward model is **linear in time with a per-year halving rate** (100 % year 1, 50 % year 2, …) | [§6.2](../mathematics/01-rewards-model.md#62-dynamic-rewards-model) |
| 10 | ZERO DEX fee rate was undefined (only the destination split) | Fee rate defined: initial **0.30 %**, range 0.05 % – 1.00 % | [§7.3](../business-logic/03-zero-dex.md#73-fees) |
| 11 | Burn trigger "fires when the threshold is reached" (unspecified executor) | Burn is triggered automatically by the **fee-consolidation routine (keeper or any user's transaction)** | [§2.6](../tokenomics/01-zero-dao.md#26-burn-mechanism) |
| 12 | "500 kWh/h" invalid unit | Corrected to **500 kW** | [§4.5](../infra/01-proof-of-energy.md#45-installation-registration-and-verification) |
| 13 | Invariants: I-1 `CumulativeMinted`, I-2 burn never regenerates, I-4 Genesis Right, I-5 window irreversibility | Invariants reformulated: I-1 `totalSupply`, I-2 burn restores (not staking), I-4 decreasing curve as annual ceiling, I-5 allocation single consumption; **I-11** linear halving reward rate added | [§11.1](../blockchain/04-invariants-parameters.md#111-invariants) |
| 14 | "Reward curve parameters" listed as a configurable parameter | Removed: the annual reward rate is **immutable** (halving schedule), no curve parameter remains | [§11.2](../blockchain/04-invariants-parameters.md#112-configurable-parameters) |
| 15 | Pending decision: "reference magnitude of the global staking component" | Removed: there is no global component in the linear model | [§11.3](../blockchain/04-invariants-parameters.md#113-pending-decisions) |
| 16 | Section 11 described as "a single table" | Section 11 described as **three tables** (invariants / parameters / pending) | [§11](../blockchain/04-invariants-parameters.md#11-invariants-and-parameters) |
| 17 | Annual-ceiling adjustment mechanism "pending implementation" | Adjustment mechanism specified: **pro-rata reduction among active positions** | [§6.3](../mathematics/01-rewards-model.md#63-issuance-limits) |
| 18 | Version `2.0` | Version `3.x` (this `docs/` tree is authoritative) | [README](../README.md) |
