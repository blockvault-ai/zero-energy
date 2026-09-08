# Change v3.2 — Dynamic mintable/burnable emission

**Date:** 2026-09-08

## Note

The previous model used fixed monetary amounts for emission (500M staking reserve, decreasing curve 250/125/62.5/31.25M, `CumulativeMinted` that never decreased). This is replaced by a **dynamic** model: emission and rewards are driven by usage and supply, not by predetermined amounts. Only limits remain; tokens are mintable and burnable to apply the mechanics.

## Rules of the new model

- ZDAO is **mintable** (staking accrual, governed issuances) and **burnable** (fees, offset).
- The supply metric is the circulating `totalSupply`; burning **restores minting capacity**.
- `MAX_SUPPLY` (1,000M ZDAO) remains the single immutable ceiling.
- Emission rate = **personal** (sigmoid over deposit age) + **usage** (supply-based, anti-dilutive: `ZREC_staked / (totalSupply_ZDAO · totalSupplyFactor)`).
- No fixed reserve, no decreasing schedule, no LP surplus: only the accrued reward is minted.

## Reference to the changes (by section)

| Section | File | Change |
| --- | --- | --- |
| §2.1 | [01-zero-dao.md](../tokenomics/01-zero-dao.md) | `CumulativeMinted` → circulating `totalSupply`; ZDAO mintable/burnable |
| §2.2 | [01-zero-dao.md](../tokenomics/01-zero-dao.md) | Burning regenerates capacity; MAX_SUPPLY stays the ceiling |
| §2.3 | [01-zero-dao.md](../tokenomics/01-zero-dao.md) | "Allocation rights" → "Allocation limits"; staking has no fixed reserve |
| §6.2 | [01-rewards-model.md](../mathematics/01-rewards-model.md) | "Global component" → "Usage component" (supply-based, anti-dilutive) |
| §6.3 | [01-rewards-model.md](../mathematics/01-rewards-model.md) | Fixed reserve + decreasing curve removed → on-demand, MAX_SUPPLY-bound |
| §6.4–6.6 | [03-staking.md](../tokenomics/03-staking.md) | Reward minting on-demand, no 500M reserve |
| §8.2, §8.7 | [01-governance.md](../blockchain/01-governance.md) | Reward minting permission and state publication updated |
| §9 | [02-contract-architecture.md](../blockchain/02-contract-architecture.md) | ZDAO + Staking contract responsibilities updated |
| §10.2 | [03-security.md](../blockchain/03-security.md) | Burning and reserve vectors updated |
| §11.1 | [04-invariants-parameters.md](../blockchain/04-invariants-parameters.md) | I-1/I-2/I-3 reformulated; I-10 (curve) removed |
| §11.2 | [04-invariants-parameters.md](../blockchain/04-invariants-parameters.md) | New params: MAX_EMISSION_RATE, usage ceiling, totalSupplyFactor |
| §11.3 | [04-invariants-parameters.md](../blockchain/04-invariants-parameters.md) | Resolved pendings removed |
| Annex A | [02-technical-reference.md](../mathematics/02-technical-reference.md) | Supply guardian and accrual code rewritten |
| Annex B | [01-change-log.md](../annexes/01-change-log.md) | v3.2 block added |
| New | [implementation/README.md](../implementation/README.md) | EasyStaking adaptation reference |
