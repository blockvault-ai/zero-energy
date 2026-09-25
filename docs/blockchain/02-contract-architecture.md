<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# 9. CONTRACT ARCHITECTURE

Modular infrastructure on Base, with decoupled responsibilities.

|  |  |
| --- | --- |
| **Contract** | **Main responsibilities** |
| ZERO DAO (ZDAO) | ERC-20, 8 decimals, mintable/burnable. `MAX_SUPPLY` enforced against `totalSupply`. Minting restricted to authorized addresses. Burning restores minting capacity |
| ZERO REC (ZREC) | ERC-20, 3 decimals, single fungible asset. Minting from Claim (Presale and Merits) and from Energy Issuance. Differentiated Genesis / Energy accounting |
| Claim | Verification of Merkle proofs against the published root. Resolution of the TOKEN ZERO minting right (ZDAO/ZREC choice) with single consumption. Delivery of ZDAO (Presale) or minting of ZREC (Presale and Merits) with automatic staking for Merits positions |
| Energy Issuance | Verification of the oracle's validation. Uniqueness registration by batch identifier. Batch state and provisionality window. Minting and distribution |
| Staking | Individualized positions. Linear-in-time reward model with a per-year halving rate (100 % year 1, 50 % year 2, …). Withdrawal rules differentiated by position type. Holds a cumulative `stakingMinted` counter capped at the 500M reserve and bounded by the decreasing curve |
| Energy Certification | Burning of ZREC destined for compensation. Issuance of the NFT with its purpose. Non-transferability of the Genesis Offset certificates |
| Governance | Proposals and votes. Parameters within range. Appointment and revocation of the Committee. Execution subject to timelock |
| Treasury | Multisig custody. Reception of the energy ZREC fraction. Exclusive execution of approved operations |
| Energy Oracles | Registry of authorized oracles. Signed authorizations with timestamp and expiry. Auditable registry of validations |

**Data acquisition layer.** Off-chain infrastructure (Section 4.4). It is not part of the contract set and **holds no permission whatsoever over them**. Its substitution or unavailability does not affect the integrity of assets already issued.

The architecture must allow incorporating new contracts, services, and integrations without replacing the core contracts.
