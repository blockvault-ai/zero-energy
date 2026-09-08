<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# 6. OFFICIAL STAKING SYSTEM

**ZREC** is deposited and **ZDAO** is received. It is the only mechanism through which ZDAO rewards are minted, on demand and bounded by `MAX_SUPPLY`.

## 6.1 Technical base

The architecture takes as its **conceptual reference** the EasyStaking model deployed on xDaiChain, adapted to the ZERO Energy economy.

> **The adapted implementation does not retain the audit coverage of the original contract.** Any modification or new implementation must undergo independent testing and security audit before deployment.

Structural difference from the reference model: in EasyStaking the deposited asset and the reward asset are the same token. **In ZERO Energy they are different**, which requires explicitly defining the relationship between the two and the normalization between their decimal scales (3 in ZREC, 8 in ZDAO).

## 6.4 Position types

The system uses **individualized positions**, each with its own amount, entry date, age, accumulated rewards, type, status, and withdrawal availability. Each contribution constitutes an independent position with its own age computation.

|  |  |  |
| --- | --- | --- |
|  | **Free staking** | **Merits staking** |
| Applicable to | Presale ZREC, energy ZREC, market ZREC | ZREC from Merits conversion |
| Entry | Voluntary | Automatic during the Claim |
| Lock | No | Initial 6 months |
| Availability | General withdrawal rules | 2 % monthly for 50 months |
| Rewards | General model | General model, identical |

**Both types use the same ZREC.** No distinct versions of the token are created to represent provenance (Section 3.2). The difference lies **exclusively in the principal withdrawal rules**.

## 6.5 Rewards for Merits positions

Merits ZREC **generate rewards from the start of their position, including the six-month lock period**, under the same general economic model. There is no differentiated formula.

The ZDAO generated as rewards:

* come exclusively from on-demand reward minting, not from the Merits Program;
* **do not inherit the lock** of the ZREC that generated them;
* are free once distributed.

## 6.6 Withdrawal

Two modalities for free positions and for the available portion of Merits positions:

|  |  |  |
| --- | --- | --- |
| **Modality** | **Procedure** | **Cost** |
| Requested | Request, waiting period, and execution window | No fee |
| Immediate | Direct execution | Fee on the withdrawn amount |

The timeframes and the fee are governance-configurable parameters within the limits of Section 11. The immediate withdrawal fee is allocated to the mechanism provided by the protocol.

Merits positions cannot withdraw principal through either channel until they have reached their availability according to the 5.5 schedule.
