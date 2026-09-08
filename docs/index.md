<!-- Origen: ZERO-ENERGY-Especificacion-Tecnica-v2.0.md -->

# PRELIMINARY NOTE

This document is the functional specification of the ZERO Energy protocol and the single reference source for three audiences: the team implementing the smart contracts, the auditors verifying them, and the governance approving their evolution.

Three reading rules:

* Every assertion must be verifiable against the deployed code. Where the document describes a limit, that limit exists as a check in the contract.
* The **invariants** cannot be modified by any mechanism. The **parameters** allow configuration by governance within the indicated ranges. Section 11 collects all of them in dedicated tables (invariants in 11.1, parameters in 11.2, pending decisions in 11.3).
* When a decision is not made, the document says so. A gap is not replaced by an assumption.

**Nomenclature.** The assets are identified as **ZERO DAO (ZDAO)** and **ZERO REC (ZREC)**. After the first mention in each section, the tickers are used. Formulas, tables, diagrams, and contract specifications always use ZDAO and ZREC.
