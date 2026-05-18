# Smart Contract Security Audit Checklist

A comprehensive checklist for auditing Yearn smart contracts and strategies. Use this as a guide during security reviews to ensure thorough coverage.

---

## Pre-Audit Setup

- [ ] Obtain all relevant source code and dependencies
- [ ] Verify contract addresses and deployed bytecode matches source
- [ ] Review documentation and specifications
- [ ] Understand the intended functionality and invariants
- [ ] Identify external dependencies and integrations
- [ ] Set up local testing environment

---

## 1. Access Control

### Owner/Admin Privileges
- [ ] All privileged functions have appropriate access control modifiers
- [ ] Owner/admin addresses are properly initialized
- [ ] Ownership transfer follows a two-step process
- [ ] Multi-sig requirements are enforced where appropriate
- [ ] Timelocks are implemented for sensitive operations

### Role-Based Access
- [ ] Roles are clearly defined and documented
- [ ] Role assignment/revocation is properly restricted
- [ ] No privilege escalation paths exist
- [ ] Emergency roles have appropriate limitations

### Questions to Ask
- Who can call each function?
- What happens if the owner key is compromised?
- Are there functions that should be restricted but aren't?

---

## 2. Reentrancy

### External Calls
- [ ] State changes occur BEFORE external calls (CEI pattern)
- [ ] Reentrancy guards are used where appropriate
- [ ] Cross-function reentrancy is considered
- [ ] Read-only reentrancy attacks are mitigated
- [ ] Callback functions are protected

### Patterns to Check
```solidity
// BAD: State change after external call
function withdraw(uint amount) external {
    token.transfer(msg.sender, amount);  // External call
    balances[msg.sender] -= amount;       // State change
}

// GOOD: Check-Effects-Interactions
function withdraw(uint amount) external {
    balances[msg.sender] -= amount;       // State change
    token.transfer(msg.sender, amount);  // External call
}
```

---

## 3. Integer Arithmetic

### Overflow/Underflow
- [ ] Solidity 0.8+ is used OR SafeMath is implemented
- [ ] Unchecked blocks are reviewed for safety
- [ ] Multiplication before division to prevent precision loss
- [ ] Division by zero is prevented
- [ ] Type casting is safe (no truncation)

### Precision Issues
- [ ] Decimal handling is consistent across tokens
- [ ] Rounding direction favors the protocol where appropriate
- [ ] Accumulated rounding errors don't compound
- [ ] Share/asset conversions are properly handled

### Common Issues
- [ ] No phantom overflow in calculations
- [ ] Large number multiplication won't overflow
- [ ] Price calculations maintain precision

---

## 4. Oracle Security

### Price Feed Validation
- [ ] Oracle data is fresh (staleness checks)
- [ ] Multiple price sources for critical operations
- [ ] Fallback mechanisms for oracle failure
- [ ] TWAP used instead of spot prices where appropriate
- [ ] Chainlink heartbeat and deviation thresholds checked

### Manipulation Resistance
- [ ] No reliance on manipulable on-chain prices
- [ ] Flash loan resistance for price-dependent operations
- [ ] Sandwich attack mitigations in place
- [ ] Minimum liquidity/volume requirements

### Questions to Ask
- What happens if the oracle returns stale data?
- Can an attacker manipulate the price within a single transaction?
- Are there arbitrage opportunities due to price delays?

---

## 5. Token Handling

### ERC-20 Compatibility
- [ ] Non-standard tokens handled (USDT, BNB, etc.)
- [ ] Fee-on-transfer tokens considered
- [ ] Rebasing tokens handled correctly
- [ ] Return values checked (safe transfer patterns)
- [ ] Zero address validation

### Token Approvals
- [ ] Approvals reset to zero before setting new values
- [ ] Infinite approvals are documented and justified
- [ ] Approval race conditions prevented
- [ ] Permit functions properly validated

### Token Amounts
- [ ] Decimal differences between tokens handled
- [ ] Dust amounts don't cause issues
- [ ] Maximum supply assumptions are documented

---

## 6. Flash Loan Resistance

### Vulnerable Operations
- [ ] Governance voting is time-locked or snapshot-based
- [ ] Collateral ratios use time-weighted averages
- [ ] LP token pricing is flash-loan resistant
- [ ] Share price calculations are atomic-resistant

### Protections
- [ ] Deposit/withdraw in same block restrictions
- [ ] Minimum lock periods for sensitive operations
- [ ] Virtual price mechanisms for LP tokens

---

## 7. External Integrations

### Protocol Dependencies
- [ ] External protocol risks documented
- [ ] Failure modes of dependencies handled
- [ ] Admin key risks of dependencies assessed
- [ ] Upgrade risks of dependencies considered

### Contract Interactions
- [ ] External contract addresses are validated
- [ ] Return data from external calls is validated
- [ ] Failed external calls handled appropriately
- [ ] Gas limits set for external calls where needed

---

## 8. Gas and DoS

### Unbounded Operations
- [ ] No unbounded loops over dynamic arrays
- [ ] Batch operations have size limits
- [ ] Storage arrays have maximum length caps
- [ ] Pagination available for large datasets

### Gas Griefing
- [ ] No expensive operations in view functions called by state-changing functions
- [ ] No external call gas manipulation vectors
- [ ] Refund mechanisms are safe

### Block Gas Limit
- [ ] Critical operations complete within reasonable gas limits
- [ ] No operations that could exceed block gas limit

---

## 9. Initialization

### Constructors and Initializers
- [ ] Initializers can only be called once
- [ ] All state variables properly initialized
- [ ] No uninitialized storage pointers
- [ ] Proxy implementation contracts initialized or disabled

### Deployment
- [ ] Constructor arguments validated
- [ ] Initial state is safe (no frontrunning deployment)
- [ ] Initialization order is correct

---

## 10. Upgradability (if applicable)

### Proxy Patterns
- [ ] Storage layout is upgrade-safe
- [ ] No storage collisions between proxy and implementation
- [ ] Initializers used instead of constructors
- [ ] Implementation contract is properly secured

### Upgrade Process
- [ ] Upgrade authority is properly restricted
- [ ] Upgrade includes timelock/multisig
- [ ] Rollback mechanisms exist
- [ ] State migration is handled

---

## 11. Governance and Time Locks

### Voting
- [ ] Vote manipulation resistance
- [ ] Quorum requirements are reasonable
- [ ] Proposal threshold prevents spam
- [ ] Execution delay allows for review

### Time Locks
- [ ] Sensitive operations have appropriate delays
- [ ] Emergency functions have proper restrictions
- [ ] No timelock bypass mechanisms

---

## 12. Events and Logging

### Event Coverage
- [ ] All state changes emit events
- [ ] Events have appropriate indexed parameters
- [ ] Events contain sufficient information for off-chain tracking
- [ ] No sensitive data in events

---

## 13. Error Handling

### Reverts and Requires
- [ ] Meaningful error messages
- [ ] Custom errors used for gas efficiency (Solidity 0.8.4+)
- [ ] No silent failures
- [ ] Try/catch used appropriately for external calls

---

## 14. Economic Considerations

### Incentive Analysis
- [ ] No profitable griefing attacks
- [ ] Fee structures are fair
- [ ] No arbitrage at protocol expense
- [ ] Liquidation incentives are balanced

### Market Conditions
- [ ] Extreme market scenarios considered
- [ ] Depegging scenarios handled
- [ ] Low liquidity conditions addressed
- [ ] Protocol works at different scales

---

## 15. Code Quality

### Solidity Best Practices
- [ ] Latest stable Solidity version used
- [ ] Compiler warnings resolved
- [ ] No deprecated patterns
- [ ] Consistent code style

### Documentation
- [ ] NatSpec comments present
- [ ] Complex logic explained
- [ ] Assumptions documented
- [ ] Invariants stated

### Testing
- [ ] Unit test coverage >90%
- [ ] Integration tests exist
- [ ] Fuzz testing performed
- [ ] Formal verification (if applicable)

---

## Post-Audit

- [ ] All findings documented and categorized
- [ ] Fixes verified for all critical/high findings
- [ ] Re-audit performed for significant changes
- [ ] Audit report published to repository

---

## References

- [SWC Registry](https://swcregistry.io/)
- [Consensys Smart Contract Best Practices](https://consensys.github.io/smart-contract-best-practices/)
- [Solidity Security Considerations](https://docs.soliditylang.org/en/latest/security-considerations.html)
- [Trail of Bits Building Secure Contracts](https://github.com/crytic/building-secure-contracts)
- [OpenZeppelin Security Guidelines](https://docs.openzeppelin.com/contracts/4.x/api/security)
