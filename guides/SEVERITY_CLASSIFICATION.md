# Vulnerability Severity Classification Guide

This document provides guidelines for classifying the severity of security vulnerabilities discovered in Yearn protocols and related systems.

## Severity Levels

### 🔴 Critical

**Definition**: Vulnerabilities that can lead to direct loss of funds, protocol takeover, or irreversible damage to the protocol.

**Characteristics**:
- Direct theft or permanent freezing of user funds
- Unauthorized minting of protocol tokens
- Complete compromise of protocol governance
- Manipulation of oracle prices leading to significant fund drainage
- Reentrancy attacks enabling fund theft
- Flash loan attacks draining protocol reserves

**Response Time**: Immediate (within hours)

**Examples**:
- Unchecked external calls allowing reentrancy
- Missing access controls on privileged functions
- Integer overflow/underflow leading to fund theft
- Unprotected `selfdestruct` or `delegatecall`
- Price oracle manipulation vulnerabilities

**Bug Bounty Range**: $100,000 - $1,000,000+

---

### 🟠 High

**Definition**: Vulnerabilities that can lead to significant financial loss under specific conditions or compromise important protocol functionality.

**Characteristics**:
- Theft of unclaimed yield or rewards
- Temporary freezing of funds (recoverable)
- Griefing attacks with significant gas costs to users
- Bypassing time-locks or multi-sig requirements
- Unauthorized state changes affecting user positions
- Denial of service to critical protocol functions

**Response Time**: Within 24-48 hours

**Examples**:
- Front-running opportunities for profit extraction
- Incorrect reward distribution logic
- Sandwich attack vulnerabilities
- Missing slippage protection
- Flawed liquidation logic

**Bug Bounty Range**: $25,000 - $100,000

---

### 🟡 Medium

**Definition**: Vulnerabilities that cause limited financial impact or can be exploited only under unusual circumstances.

**Characteristics**:
- Minor yield leakage
- Temporary denial of service to non-critical functions
- Gas optimization issues causing excessive costs
- Incorrect event emissions affecting off-chain systems
- Edge cases in accounting logic

**Response Time**: Within 1 week

**Examples**:
- Rounding errors causing minor discrepancies
- Missing input validation (non-exploitable)
- Inefficient loops causing high gas costs
- Incorrect `view` function return values
- Minor access control issues

**Bug Bounty Range**: $5,000 - $25,000

---

### 🟢 Low

**Definition**: Vulnerabilities with minimal impact that don't directly affect funds or core protocol functionality.

**Characteristics**:
- Best practice violations
- Informational findings
- Code quality improvements
- Minor documentation issues
- Non-critical optimization opportunities

**Response Time**: Next scheduled release

**Examples**:
- Missing `indexed` parameters in events
- Unnecessary state variable declarations
- Suboptimal gas usage in non-critical paths
- Missing NatSpec documentation
- Inconsistent naming conventions

**Bug Bounty Range**: $500 - $5,000

---

## Impact Categories

### Direct Impact on Funds

| Severity | Fund Impact |
| -------- | ----------- |
| Critical | >$1,000,000 or >10% of TVL |
| High     | $100,000 - $1,000,000 or 1-10% of TVL |
| Medium   | $10,000 - $100,000 or 0.1-1% of TVL |
| Low      | <$10,000 or <0.1% of TVL |

### Likelihood Assessment

| Likelihood | Description |
| ---------- | ----------- |
| Almost Certain | Can be triggered by any user at any time |
| Likely | Requires specific conditions but easily achievable |
| Possible | Requires unusual market conditions or timing |
| Unlikely | Requires multiple unlikely conditions |
| Rare | Theoretical only, extremely difficult to trigger |

### Severity Matrix

|                  | **Almost Certain** | **Likely** | **Possible** | **Unlikely** | **Rare** |
| ---------------- | ------------------ | ---------- | ------------ | ------------ | -------- |
| **Critical**     | Critical           | Critical   | High         | High         | Medium   |
| **High**         | Critical           | High       | High         | Medium       | Medium   |
| **Medium**       | High               | Medium     | Medium       | Low          | Low      |
| **Low**          | Medium             | Low        | Low          | Low          | Info     |

---

## Attack Vector Categories

### Smart Contract Vulnerabilities

| Category | Examples | Typical Severity |
| -------- | -------- | ---------------- |
| Reentrancy | External calls before state updates | Critical |
| Access Control | Missing `onlyOwner` modifiers | Critical/High |
| Oracle Manipulation | Spot price usage, TWAP bypass | Critical/High |
| Integer Issues | Overflow, underflow, precision loss | High/Medium |
| Logic Errors | Incorrect calculations, edge cases | High/Medium |
| Front-running | Sandwich attacks, MEV extraction | High/Medium |
| Gas Griefing | Unbounded loops, storage abuse | Medium |
| DoS | Block gas limit, external dependency | Medium/Low |

### DeFi-Specific Vulnerabilities

| Category | Description | Typical Severity |
| -------- | ----------- | ---------------- |
| Flash Loan Attacks | Instant capital for manipulation | Critical |
| Price Manipulation | Oracle or pool price attacks | Critical |
| Sandwich Attacks | Front/back-running trades | High |
| Liquidation Exploits | Unfair or blocked liquidations | High |
| Yield Extraction | Stealing rewards/yields | High |
| LP Token Attacks | Share price manipulation | High |
| Governance Attacks | Vote manipulation, proposal hijack | Critical/High |
| Bridge Exploits | Cross-chain message spoofing | Critical |

---

## Reporting Guidelines

When reporting a vulnerability, include:

1. **Severity Assessment**: Your proposed severity level with justification
2. **Impact Analysis**: Detailed description of potential damage
3. **Likelihood Assessment**: How easily can this be exploited?
4. **Affected Components**: Specific contracts, functions, or systems
5. **Proof of Concept**: Working PoC or detailed exploitation steps
6. **Suggested Fix**: Recommended remediation approach

### Escalation Criteria

A finding may be escalated if:
- The impact is greater than initially assessed
- Multiple vulnerabilities can be chained together
- New attack vectors are discovered
- Market conditions change (e.g., increased TVL)

### De-escalation Criteria

A finding may be de-escalated if:
- Mitigating factors exist (e.g., admin intervention possible)
- The vulnerability requires unrealistic conditions
- Impact is lower than initially assessed
- Existing safeguards provide partial protection

---

## References

- [Immunefi Vulnerability Classification](https://immunefi.com/severity-updated/)
- [OWASP Risk Rating Methodology](https://owasp.org/www-community/OWASP_Risk_Rating_Methodology)
- [SWC Registry - Smart Contract Weakness Classification](https://swcregistry.io/)
- [Yearn Bug Bounty Program](https://immunefi.com/bug-bounty/yearnfinance/information/)
