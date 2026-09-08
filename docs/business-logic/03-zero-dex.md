<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# 7. ZERO DEX

## 7.1 Nature

**ZERO DEX is the ecosystem's main Dashboard and decentralized interface**, not merely an exchange or a set of liquidity pools. It constitutes the layer through which users interact with the protocol's on-chain infrastructure.

It must be expressly distinguished:

ZERO DEX = Dashboard and interface of the ecosystem
Contracts, pools, routers, liquidity protocols = underlying technology layer

ZERO DEX may interact with its own, external or aggregated infrastructure.

## 7.2 Functionalities

They are integrated progressively: exchange of ZDAO, ZREC and compatible assets; access to liquidity pools; ZREC staking and position management; reward claiming; interaction with governance; energy production and Proof of Energy information; visualization of ZREC and Energy Certification NFTs; producer and installation information; protocol metrics and Treasury information; DeFi functionalities and future applications built on ZERO Energy.

**Restriction.** The market operates exclusively on a spot basis. Trading with leverage, margin or derivative instruments on the ecosystem's assets is not offered. Incorporating any functionality of that nature requires an express DAO decision and a prior review of the architecture.

## 7.3 Fees

Operations generate a **fee on the traded volume**, collected in the input asset and consolidated periodically.

**Fee rate: 0.30 % on volume (initial), configurable by governance within the range 0.05 % – 1.00 %** (Section 11.2).

The fee is split among the following destinations:

|  |  |  |
| --- | --- | --- |
| **Destination** | **Initial** | **Configurable range** |
| DAO Treasury | 40 % | 20 % – 50 % |
| Liquidity providers | 30 % | 20 % – 50 % |
| ZDAO acquisition and burn | 20 % | 0 % – 30 % |
| Development and infrastructure | 10 % | 0 % – 20 % |

The sum must at all times equal 100 %; the contract rejects any configuration that does not comply. The fraction destined for burn is accumulated and executed according to the Burn Execution Threshold of Section 2.6.
