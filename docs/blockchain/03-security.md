<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# 10. SECURITY

## 10.1 Audit

All contracts must undergo independent audit before production, expressly verifying the invariants of Section 11. Every relevant modification requires an additional audit prior to deployment. The adaptation of the staking model (Section 6.1) requires its own audit: it does not inherit that of the reference contract.

## 10.2 Vectors considered

|  |  |  |
| --- | --- | --- |
| **Vector** | **Measure** | **Section** |
| Repeated Claim after transferring ZDAO | The choice resides in the off-chain allocation, with single consumption | 5.3 |
| Duplication of the allocation using several wallets | Terminal state per allocation | 5.3 |
| Regeneration of issuance capacity through burning | `totalSupply` decreases; MAX_SUPPLY remains the ceiling | 2.2 |
| Evasion of the Merits schedule | The lock lives in the position's withdrawal rules | 5.5 and 6.4 |
| Double minting by retrying a batch | Batch identifier in the uniqueness registry | 4.6 |
| Reuse of already-processed production | Irreversible FINALIZED state | 4.6 |
| Double accounting against external certificates | Prior exclusivity verification | 4.3 |
| Double use of a certificate | Purpose recorded and non-transferability | 3.6 |
| Inflated production declaration | Plausibility checks | 4.5 |
| Nonexistent or oversized installation | Triple verification of nominal power | 4.5 |
| Discretionary authorization of minting | Merkle tree published before the Claim | 5.6 |
| Cumulative deviation from rounding | Mandatory carryover of residues | 4.6 |
| Accelerated emission / supply dilution | MAX_EMISSION_RATE and anti-dilutive usage component as ceiling | 6.2 and 6.3 |

## 10.3 Treasury and operational security

The DAO Treasury operates under a multisig scheme; no Committee member can dispose of the funds individually; every operation complies with the authorization rules and the timelock; all are recorded on-chain.

Operational measures: separation of responsibilities, permission control, permanent registry of operations, cryptographic validation of critical actions, protection against replay attacks and double executions, and continuous monitoring.

## 10.4 Continuous improvement

Governance may incorporate new audits, infrastructure upgrades, improvements in permission management, new cryptographic mechanisms, and incident response procedures.

**The protocol does not currently introduce collateral requirements for the oracles.** A security deposit does not demonstrate that an energy datum is true: it only introduces an economic penalty if someone deceives. For ZERO Energy, the technical verification chain — installation, meter, measurement, period, validation, uniqueness — described in Section 4 is more decisive. Governance may in the future incorporate economic guarantee or penalty mechanisms for certain validators, without turning them into a protocol requirement now.
