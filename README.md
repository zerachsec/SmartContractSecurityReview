# Smart Contract Security Review

This repository documents my journey learning **smart contract security auditing** and **vulnerability research**.
It contains security reviews of vulnerable smart contracts where I analyze the codebase, identify vulnerabilities, and document findings with proof-of-concept tests and recommended mitigations.

The goal of this repository is to build practical experience in:

* Smart contract auditing
* Vulnerability discovery
* Security research
* Writing professional audit reports

---

# Repository Structure

Each folder in this repository represents a **security review of a specific smart contract project**.

```text
SmartContractSecurityReview
│
├── 00-PasswordStoreAudit
├── 01-PuppyRaffleAudit
├── 02-DSCEngineAudit
├── 03-RaffleAudit
```

Each audit folder typically contains:

* `README.md` – Full security review report
* `findings` – Documented vulnerabilities
* `tests` – Proof-of-concept exploit tests
* `notes` – Additional audit notes and observations

---

# Audit Methodology

During each security review I follow a structured auditing approach:

1. Understand the protocol design
2. Identify the attack surface
3. Review contract logic and state variables
4. Check for common vulnerabilities:

   * Access control issues
   * Reentrancy
   * Storage exposure
   * Logic errors
   * Unsafe external calls
5. Write proof-of-concept tests to validate vulnerabilities
6. Document findings and mitigation strategies

---

# Tools Used

The following tools are commonly used during the auditing process:

* **Foundry** – Smart contract testing framework
* **Anvil** – Local Ethereum development node
* **Cast** – Blockchain interaction CLI
* **Manual Code Review**

---

# Purpose

This repository serves as a **security research portfolio** where I document my progress in learning smart contract auditing and security analysis.

All audits in this repository are conducted for **educational and research purposes**.

---

# Disclaimer

The findings documented in this repository are part of security practice and learning exercises. These reviews should not be considered complete professional audits.
