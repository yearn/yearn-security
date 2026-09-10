# Bug Bounty Submission Template

Use this template when submitting a vulnerability through Yearn's bug bounty program.

---

## Submission Details

**Submission Date**: YYYY-MM-DD
**Platform**: [ ] Immunefi [ ] Sherlock [ ] Direct Disclosure
**Contact Email**: [Your email]
**PGP Key**: [Link or fingerprint if using encrypted email]

---

## Executive Summary

<!-- One paragraph overview of the vulnerability -->

[Brief description of the vulnerability and its potential impact]

---

## Vulnerability Details

### Classification

**Severity**: [ ] Critical [ ] High [ ] Medium [ ] Low
**Category**: [e.g., Reentrancy, Access Control, Oracle Manipulation]
**CWE ID**: [If applicable, e.g., CWE-841]

### Affected Components

| Contract | Address | Network | Version |
| -------- | ------- | ------- | ------- |
| [Name]   | [0x...] | [Mainnet/Testnet] | [Git commit or version] |

### Affected Functions

```solidity
// List the specific functions affected
function vulnerableFunction() external;
function relatedFunction() internal;
```

---

## Technical Description

### Root Cause

<!-- Detailed explanation of why this vulnerability exists -->

[Explain the technical root cause]

### Attack Vector

<!-- Step-by-step description of how this vulnerability can be exploited -->

1. [Step 1]
2. [Step 2]
3. [Step 3]
4. ...

### Impact Analysis

<!-- What are the consequences of this vulnerability being exploited? -->

**Financial Impact**: $[Estimated amount or percentage of TVL]
**Affected Users**: [Number or category of users at risk]
**Protocol Impact**: [How does this affect the protocol's operation?]

---

## Proof of Concept

### Prerequisites

- [Required setup, e.g., mainnet fork, specific block number]
- [Required tools, e.g., Foundry, Hardhat]
- [Any token holdings or positions needed]

### Environment Setup

```bash
# Commands to set up the testing environment
git clone [repo]
cd [repo]
forge install
```

### PoC Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "forge-std/Test.sol";

contract ExploitPoC is Test {
    // Contract addresses
    address constant TARGET = 0x...;
    
    function setUp() public {
        // Fork mainnet at specific block
        vm.createSelectFork(vm.envString("ETH_RPC_URL"), BLOCK_NUMBER);
    }
    
    function testExploit() public {
        // PoC code here
        
        // Log the impact
        console.log("Funds stolen: ", amount);
    }
}
```

### Running the PoC

```bash
# Commands to run the PoC
forge test --match-test testExploit -vvv
```

### Expected Output

```
# Expected test output showing the exploit succeeds
[PASS] testExploit() (gas: XXXXX)
Logs:
  Funds stolen: 1000000000000000000000
```

---

## Suggested Remediation

### Immediate Mitigation

<!-- Short-term fixes that can be deployed quickly -->

[Describe immediate steps to reduce risk]

### Recommended Fix

<!-- Long-term solution to address the root cause -->

```solidity
// Code showing the recommended fix
function fixedFunction() external {
    // Fixed implementation
}
```

### Alternative Solutions

<!-- Other possible approaches to fix the issue -->

1. [Alternative 1]
2. [Alternative 2]

---

## Additional Information

### Discovery Context

<!-- How was this vulnerability discovered? -->

[Explain how you found this vulnerability]

### Related Issues

<!-- Are there similar issues in other contracts? -->

[Any related vulnerabilities or patterns to look for]

### References

<!-- Relevant links, previous exploits, or documentation -->

1. [Reference 1](URL)
2. [Reference 2](URL)

---

## Disclosure Preferences

- [ ] I consent to my name being published in the disclosure
- [ ] I prefer to remain anonymous
- [ ] I would like to be credited as: [Alias or name]

---

## Checklist

Before submitting, please verify:

- [ ] The vulnerability is in scope per the bug bounty program
- [ ] This is not a known issue or duplicate
- [ ] The PoC successfully demonstrates the vulnerability
- [ ] Impact assessment is realistic and supported by evidence
- [ ] The submission includes all necessary details to reproduce
- [ ] Contact information is valid and secure

---

## Notes for Yearn Team

<!-- Any additional context for the security team -->

[Additional notes here]
