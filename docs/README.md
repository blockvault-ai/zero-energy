# ZERO Energy — Technical Specification v3.3

Documentation modularized by domain. Original source: `ZERO-ENERGY-Especificacion-Tecnica-v2.0.md`. This `docs/` tree is the authoritative, up-to-date version (v3.3); the root source file remains historical.

## Domains

| Domain | Description |
| --- | --- |
| [business-logic](business-logic/) | General architecture, Genesis phase, ZERO DEX, provisions |
| [tokenomics](tokenomics/) | ZDAO, ZREC, staking system |
| [mathematics](mathematics/) | Rewards model and technical reference |
| [blockchain](blockchain/) | Governance, contracts, security, invariants |
| [infra](infra/) | Proof of Energy and Energy Oracles |
| [annexes](annexes/) | Change log |

## Preliminary note

See [index.md](index.md).

## Structure

```
docs/
├── README.md
├── index.md                       # Preliminary note
├── business-logic/
│   ├── 01-general-architecture.md
│   ├── 02-genesis-phase.md
│   ├── 03-zero-dex.md
│   └── 04-general-provisions.md
├── tokenomics/
│   ├── 01-zero-dao.md
│   ├── 02-zero-rec.md
│   └── 03-staking.md
├── mathematics/
│   ├── 01-rewards-model.md
│   └── 02-technical-reference.md
├── blockchain/
│   ├── 01-governance.md
│   ├── 02-contract-architecture.md
│   ├── 03-security.md
│   └── 04-invariants-parameters.md
├── infra/
│   └── 01-proof-of-energy.md
└── annexes/
    └── 01-change-log.md
```

## Section correspondence

| Original section | File |
| --- | --- |
| Preliminary note | `index.md` |
| 1. General architecture | `business-logic/01-general-architecture.md` |
| 2. ZERO DAO | `tokenomics/01-zero-dao.md` |
| 3. ZERO REC | `tokenomics/02-zero-rec.md` |
| 4. Proof of Energy | `infra/01-proof-of-energy.md` |
| 5. Genesis phase | `business-logic/02-genesis-phase.md` |
| 6.1 Staking (base) + 6.4–6.6 | `tokenomics/03-staking.md` |
| 6.2–6.3 Mathematical model | `mathematics/01-rewards-model.md` |
| 7. ZERO DEX | `business-logic/03-zero-dex.md` |
| 8. Governance | `blockchain/01-governance.md` |
| 9. Contract architecture | `blockchain/02-contract-architecture.md` |
| 10. Security | `blockchain/03-security.md` |
| 11. Invariants and parameters | `blockchain/04-invariants-parameters.md` |
| 12. General provisions | `business-logic/04-general-provisions.md` |
| Annex A | `mathematics/02-technical-reference.md` |
| Annex B | `annexes/01-change-log.md` |
