<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# 8. GOVERNANCE AND PROGRESSIVE DECENTRALIZATION

## 8.1 DAO and Protocol Committee

The **DAO is the supreme governance authority**. The **Protocol Committee** is an operational body with delegated competences, **not an authority above the DAO**.

**Initial mandate of the Committee: 24 months.** One year is insufficient to manage energy infrastructure; periods of four or five years concentrate too much power. Twenty-four months allow for work and accountability.

The Committee is subject to renewal, early revocation through the governance mechanisms, replacement for justified cause, transparency obligations, accountability, limits on its competences, and supervision by the DAO.

**The Committee cannot** mint tokens, modify parameters outside of range, or dispose of the DAO Treasury without prior approval.

## 8.2 Administrative permissions

The protocol recognizes exclusively the following permissions. No other permission exists over the core contracts.

|  |  |  |  |
| --- | --- | --- | --- |
| **Permission** | **Holder** | **Scope** | **Destination** |
| Reward minting | Staking contract | On-demand, bounded by MAX_SUPPLY | Permanent |
| Minting via Claim | Claim contract | Allocations from the published root | Extinguishes when the window closes |
| Foundational ZREC minting | Claim contract | Presale and Merits allocations resolved to ZREC | Extinguishes when the window closes |
| Energy minting of ZREC | Energy Issuance contract | Finalized batches | Permanent |
| Oracle operation | ZERO Energy | Signature of validations | Opens to third parties (Milestone 3) |
| Strategic minting | Governance | Strategic Issuances | Subject to the supply invariant |
| Parameter configuration | Governance | Ranges of Section 11 | Permanent |
| Contract upgrades | Governance | Core contracts | Renounced (Milestone 5) |
| Emergency pause | Protocol Committee | Temporary suspension | Renounced (Milestone 5) |

**No minting permission falls on an address controlled by individuals.** Permissions are held by contracts, whose behavior is verifiable.

## 8.3 Timelock

Every execution arising from the governance contract is subject to a mandatory waiting period, during which the pending transaction is publicly queryable.

|  |  |
| --- | --- |
| **Operation** | **Minimum period** |
| Parameters within range | 48 hours |
| DAO Treasury operations | 72 hours |
| Strategic Issuances | 7 days |
| Modification of the oracle set | 7 days |
| Upgrade of core contracts | 14 days |

Its purpose is to guarantee that no decision affecting the assets or the logic of the protocol can be executed without participants having time to learn about it and react.

## 8.4 Emergency pause

The Committee may temporarily suspend operations in the face of a security incident, with strict limits: **it does not allow minting, transferring, or disposing of any asset**; maximum duration of 72 hours with automatic lifting; extension subject to DAO approval; public justification within 24 hours.

## 8.5 Upgradability

The core contracts are deployed with upgrade capability during the initial phases, to allow correcting defects detected in audit or production. Every upgrade requires DAO approval, a 14-day timelock, prior audit, and publication of the proposed code throughout the waiting period.

**No upgrade may modify an invariant of Section 11.** The invariants are implemented so that their verification does not depend on upgradable code. The upgrade capability is extinguished at Milestone 5.

## 8.6 Progressive decentralization by milestones

**Decentralization is not associated with calendar dates.** A protocol does not decentralize because a given date arrives. Each milestone is defined through **objective and verifiable conditions**, and certain conditions must be met simultaneously, so that no one can artificially accelerate the process to obtain power or unlock a capability.

Variables that may be considered jointly: number of independent producers, certified energy, diversity of data sources, number and distribution of validators, maturity of the Energy Oracles, TVL, maturity and audits of the contracts, activity and distribution of participation in governance, and operational security.

**The concrete criteria remain open and will be defined through governance** as the protocol evolves.

Each milestone entails an **irreversible renunciation** of a control capability: once executed, it cannot be reverted by any mechanism, including contract upgrades.

|  |  |  |
| --- | --- | --- |
| **Milestone** | **Renunciation** | **Verification** |
| 1. Closure of the foundational window | Claim is disabled | The functions revert unconditionally |
| 2. Consolidation of issuance | The ZDAO minting permissions are extinguished except staking and strategic | Public list of authorized addresses |
| 3. Independence of validation | ZERO Energy ceases to control energy validation | Public list of oracles and their accredited independence |
| 4. Maturity of the ecosystem | Criteria defined by governance | Published metrics |
| 5. Immutability | Upgrading contracts and the emergency pause are renounced | Administrative address without capacity for action |
| 6. Full governance | The Committee loses all capacity for initiative | Proposals originate only from the governance contract |

> Until the execution of **Milestone 3**, energy validation depends on infrastructure operated by ZERO Energy. The document expressly declares this in Section 4.3 rather than presenting as decentralized a mechanism that is not yet so.

## 8.7 State publication

The protocol publishes permanently and queryably: the milestone reached and the transaction of each renunciation executed; the list of addresses with active minting permission; the pending transactions in the timelock; the composition of the Committee and of the oracle set, with indication of their operator; `totalSupply`, and remaining issuance capacity; the state of staking; the number of registered installations and the accumulated certified energy; and the percentage of Foundational Issuance offset.

> The degree of decentralization of a protocol is not declared: it is demonstrated through the list of capabilities that have been renounced in a verifiable manner.
