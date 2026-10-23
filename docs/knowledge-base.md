<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# KNOWLEDGE BASE — TOPIC INDEX

Single entry point to every topic in the specification. Each line points to the file and the exact heading (`#anchor`) where the topic is developed.

> Anchors follow GitHub slug rules: lowercase, punctuation removed, spaces → hyphens.
> Example: `§6.2 Dynamic rewards model` → `mathematics/01-rewards-model.md#62-dynamic-rewards-model`.

## 1. General architecture — `business-logic/01-general-architecture.md`

| Topic | Link |
| --- | --- |
| What ZERO Energy is (DePIN) | [§1.1](business-logic/01-general-architecture.md#11-what-zero-energy-is) |
| Proof of Energy (PoE) | [§1.2](business-logic/01-general-architecture.md#12-proof-of-energy-poe) |
| The two assets (ZDAO vs ZREC) | [§1.3](business-logic/01-general-architecture.md#13-the-two-assets) |
| Protocol layers | [§1.4](business-logic/01-general-architecture.md#14-protocol-layers) |
| Complete operating cycle | [§1.5](business-logic/01-general-architecture.md#15-complete-operating-cycle) |

## 2. ZERO DAO (ZDAO) — `tokenomics/01-zero-dao.md`

| Topic | Link |
| --- | --- |
| Supply magnitudes (`MAX_SUPPLY`, `totalSupply`, `stakingMinted`) | [§2.1](tokenomics/01-zero-dao.md#21-supply-magnitudes) |
| Burn restores capacity (not the staking cap) | [§2.2](tokenomics/01-zero-dao.md#22-burning-returns-minting-capacity-to-the-token-but-not-to-the-staking-cap) |
| Allocation limits | [§2.3](tokenomics/01-zero-dao.md#23-allocation-limits) |
| Fungibility | [§2.4](tokenomics/01-zero-dao.md#24-fungibility) |
| Functions (governance, incentive) | [§2.5](tokenomics/01-zero-dao.md#25-functions) |
| Burn mechanism (threshold) | [§2.6](tokenomics/01-zero-dao.md#26-burn-mechanism) |

## 3. ZERO REC (ZREC) — `tokenomics/02-zero-rec.md`

| Topic | Link |
| --- | --- |
| Standard and decimals (1 ZREC = 1 MWh) | [§3.1](tokenomics/02-zero-rec.md#31-standard-and-decimals) |
| Fungibility — a single token | [§3.2](tokenomics/02-zero-rec.md#32-fungibility-a-single-token) |
| Three origins (Presale / Merits / Energy) | [§3.3](tokenomics/02-zero-rec.md#33-three-origins) |
| Presale ZREC | [§3.3 A](tokenomics/02-zero-rec.md#a-presale-zrec) |
| Merits ZREC | [§3.3 B](tokenomics/02-zero-rec.md#b-merits-zrec) |
| Energy ZREC | [§3.3 C](tokenomics/02-zero-rec.md#c-energy-zrec) |
| Differentiated accounting | [§3.4](tokenomics/02-zero-rec.md#34-differentiated-accounting) |
| Economic value | [§3.5](tokenomics/02-zero-rec.md#35-economic-value) |
| Genesis Offset Program | [§3.6](tokenomics/02-zero-rec.md#36-genesis-offset-program) |
| Energy Certification NFT | [§3.7](tokenomics/02-zero-rec.md#37-energy-certification-nft) |

## 4. Proof of Energy — `infra/01-proof-of-energy.md`

| Topic | Link |
| --- | --- |
| What the protocol certifies | [§4.1](infra/01-proof-of-energy.md#41-what-the-protocol-certifies) |
| Objectives of the validation layer | [§4.2](infra/01-proof-of-energy.md#42-objectives-of-the-validation-layer) |
| Oracle model and decentralization | [§4.3](infra/01-proof-of-energy.md#43-oracle-model-and-progressive-decentralization) |
| Data acquisition (universal connector) | [§4.4](infra/01-proof-of-energy.md#44-data-acquisition) |
| Installation registration and verification | [§4.5](infra/01-proof-of-energy.md#45-installation-registration-and-verification) |
| Energy batches | [§4.6](infra/01-proof-of-energy.md#46-energy-batches) |
| Complete validation flow | [§4.7](infra/01-proof-of-energy.md#47-complete-validation-flow) |

## 5. Genesis phase — `business-logic/02-genesis-phase.md`

| Topic | Link |
| --- | --- |
| Presale (TOKEN ZERO) | [§5.1](business-logic/02-genesis-phase.md#51-presale-token-zero) |
| TGE and Claim window | [§5.2](business-logic/02-genesis-phase.md#52-token-generation-event-tge-and-claim-window) |
| Claim choices | [§5.3](business-logic/02-genesis-phase.md#53-claim-options) |
| Presale branch (TOKEN ZERO → ZDAO or ZREC) | [§5.4](business-logic/02-genesis-phase.md#54-presale-branch-zdao-or-zrec) |
| Merits branch (ZREC with vesting) | [§5.5](business-logic/02-genesis-phase.md#55-merits-branch-zrec-with-vesting) |
| Official Claim System (Merkle) | [§5.6](business-logic/02-genesis-phase.md#56-official-claim-system) |

## 6. Official Staking System

### Mechanics and rules — `tokenomics/03-staking.md`

| Topic | Link |
| --- | --- |
| Technical basis | [§6.1](tokenomics/03-staking.md#61-technical-basis) |
| Position types (free vs Merits) | [§6.4](tokenomics/03-staking.md#64-position-types) |
| Rewards for Merits positions | [§6.5](tokenomics/03-staking.md#65-rewards-of-merits-positions) |
| Withdrawal (requested vs immediate) | [§6.6](tokenomics/03-staking.md#66-withdrawal) |

### Mathematical model — `mathematics/01-rewards-model.md`

| Topic | Link |
| --- | --- |
| Dynamic rewards model (linear + halving) | [§6.2](mathematics/01-rewards-model.md#62-rewards-model) |
| Time component (position age) | [§6.2 Time](mathematics/01-rewards-model.md#time-component-position-age) |
| Accrual | [§6.2 Accrual](mathematics/01-rewards-model.md#accrual) |
| Issuance limits (reserve + halving) | [§6.3](mathematics/01-rewards-model.md#63-issuance-limits) |

## 7. ZERO DEX — `business-logic/03-zero-dex.md`

| Topic | Link |
| --- | --- |
| Nature (Dashboard) | [§7.1](business-logic/03-zero-dex.md#71-nature) |
| Functionalities | [§7.2](business-logic/03-zero-dex.md#72-functionalities) |
| Fees (rate and split) | [§7.3](business-logic/03-zero-dex.md#73-fees) |

## 8. Governance — `blockchain/01-governance.md`

| Topic | Link |
| --- | --- |
| DAO and Protocol Committee | [§8.1](blockchain/01-governance.md#81-dao-and-protocol-committee) |
| Administrative permissions | [§8.2](blockchain/01-governance.md#82-administrative-permissions) |
| Timelock | [§8.3](blockchain/01-governance.md#83-timelock) |
| Emergency pause | [§8.4](blockchain/01-governance.md#84-emergency-pause) |
| Upgradability | [§8.5](blockchain/01-governance.md#85-contract-upgrades) |
| Progressive decentralization (milestones) | [§8.6](blockchain/01-governance.md#86-progressive-decentralization-by-milestones) |
| State publication | [§8.7](blockchain/01-governance.md#87-publication-of-status) |

## 9. Contract architecture — `blockchain/02-contract-architecture.md`

| Topic | Link |
| --- | --- |
| Contract responsibilities (full table) | [§9](blockchain/02-contract-architecture.md#9-contract-architecture) |

## 10. Security — `blockchain/03-security.md`

| Topic | Link |
| --- | --- |
| Audit | [§10.1](blockchain/03-security.md#101-audit) |
| Vectors considered | [§10.2](blockchain/03-security.md#102-vectors-considered) |
| Treasury and operational security | [§10.3](blockchain/03-security.md#103-treasury-and-operational-security) |
| Continuous improvement | [§10.4](blockchain/03-security.md#104-continuous-improvement) |

## 11. Invariants and parameters — `blockchain/04-invariants-parameters.md`

| Topic | Link |
| --- | --- |
| Invariants (I-1 … I-11) | [§11.1](blockchain/04-invariants-parameters.md#111-invariants) |
| Configurable parameters | [§11.2](blockchain/04-invariants-parameters.md#112-configurable-parameters) |

## 12. General provisions — `business-logic/04-general-provisions.md`

| Topic | Link |
| --- | --- |
| Purpose and interpretation | [§12.1](business-logic/04-general-provisions.md#121-purpose-and-interpretation) |
| Evolution | [§12.2](business-logic/04-general-provisions.md#122-evolution) |
| Transparency | [§12.3](business-logic/04-general-provisions.md#123-transparency) |
| Vision | [§12.4](business-logic/04-general-provisions.md#124-vision) |

## Annexes

| Topic | Link |
| --- | --- |
| Technical reference (supply guardian, accrual) | [Annex A](mathematics/02-technical-reference.md#annex-a--technical-reference) |
| Change log | [Annex B](annexes/01-change-log.md#annex-b--change-log) |
| Changes vs original source | [Changes](changes/CHANGES.md) |
| Staking implementation reference | [Implementation](implementation/README.md) |

## Cross-cutting concepts (quick lookup)

| Concept | Primary link |
| --- | --- |
| `MAX_SUPPLY` (1,000,000,000 ZDAO) | [§2.1](tokenomics/01-zero-dao.md#21-supply-magnitudes) |
| Staking reserve (500,000,000 ZDAO) | [§2.3](tokenomics/01-zero-dao.md#23-allocation-limits) · [§6.3](mathematics/01-rewards-model.md#63-issuance-limits) |
| Decreasing halving curve (250M → 125M → …) | [§6.3](mathematics/01-rewards-model.md#63-issuance-limits) |
| Linear reward with per-year halving | [§6.2](mathematics/01-rewards-model.md#62-rewards-model) |
| `stakingMinted` counter | [§2.1](tokenomics/01-zero-dao.md#21-supply-magnitudes) |
| Merits schedule (6m cliff + 2%/month) | [§5.5](business-logic/02-genesis-phase.md#55-merits-branch-zrec-with-vesting) |
| Decimals (ZDAO 8, ZREC 3) | [§3.1](tokenomics/02-zero-rec.md#31-standard-and-decimals) |
| Burn Execution Threshold (10,000 ZDAO) | [§2.6](tokenomics/01-zero-dao.md#26-burn-mechanism) |
| ZERO DEX fee (0.30 %) | [§7.3](business-logic/03-zero-dex.md#73-fees) |
| Committee mandate (24 months) | [§8.1](blockchain/01-governance.md#81-dao-and-protocol-committee) |
| Timelock periods | [§8.3](blockchain/01-governance.md#83-timelock) |
