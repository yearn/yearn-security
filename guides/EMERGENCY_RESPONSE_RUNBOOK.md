# Security Emergency Response Runbook

This document outlines the standard operating procedures for responding to security incidents affecting Yearn protocols.

---

## Incident Classification

### Severity Levels

| Level | Description | Response Time | Team Required |
| ----- | ----------- | ------------- | ------------- |
| **P0 - Critical** | Active exploit, funds at immediate risk | < 15 minutes | Full war room |
| **P1 - High** | Exploitable vulnerability discovered | < 1 hour | Core security team |
| **P2 - Medium** | Potential vulnerability, unclear exploitability | < 24 hours | Security lead + 1 |
| **P3 - Low** | Minor issue, no immediate risk | < 1 week | Security lead |

---

## P0 - Critical Incident Response

### Immediate Actions (First 15 minutes)

#### 1. Alert and Mobilize
- [ ] Create war room channel (private)
- [ ] Page on-call security responders
- [ ] Notify core team leads
- [ ] Document incident start time

#### 2. Initial Assessment
- [ ] Confirm the incident is real (not a false alarm)
- [ ] Identify affected contracts and addresses
- [ ] Estimate current and potential financial impact
- [ ] Determine if exploit is ongoing

#### 3. Immediate Containment
- [ ] Pause affected contracts (if guardian/emergency role exists)
- [ ] Remove keeper permissions from affected strategies
- [ ] Contact affected third-party protocols
- [ ] Block known attacker addresses (if applicable)

### Short-term Actions (First 1-4 hours)

#### 4. Deep Analysis
- [ ] Trace all exploit transactions
- [ ] Identify root cause
- [ ] Determine full scope of affected systems
- [ ] Identify any remaining vulnerable funds

#### 5. Communication
- [ ] Draft internal situation report
- [ ] Prepare public acknowledgment (if needed)
- [ ] Coordinate with affected partners
- [ ] Prepare user communication

#### 6. Remediation Planning
- [ ] Develop fix or mitigation strategy
- [ ] Prepare emergency governance proposal (if needed)
- [ ] Test fix on testnet/fork
- [ ] Prepare deployment plan

### Recovery Actions (4-24 hours)

#### 7. Deploy Fix
- [ ] Execute fix with multi-sig
- [ ] Verify fix is effective
- [ ] Re-enable affected systems gradually
- [ ] Monitor for any new issues

#### 8. Post-Incident
- [ ] Complete internal incident report
- [ ] Prepare public disclosure
- [ ] Conduct post-mortem meeting
- [ ] Update security processes as needed

---

## War Room Procedures

### Communication Channels

| Channel | Purpose | Access |
| ------- | ------- | ------ |
| Private Telegram/Discord | Real-time coordination | Core responders only |
| Shared Doc | Running incident log | Core responders |
| Public Twitter | Official communications | Approved spokesperson only |

### Roles and Responsibilities

| Role | Responsibilities |
| ---- | ---------------- |
| **Incident Commander** | Overall coordination, decision-making, external communication |
| **Technical Lead** | Root cause analysis, fix development, deployment |
| **Communications Lead** | Draft messages, coordinate disclosures, handle media |
| **Ops Lead** | Multi-sig coordination, keeper management, guardian actions |
| **Scribe** | Document all actions and decisions in real-time |

### Decision Framework

**Before taking action, confirm:**
1. What are we trying to achieve?
2. What are the risks of this action?
3. What are the risks of NOT taking this action?
4. Do we have multi-sig signers available?
5. Is this reversible?

---

## Contact Directory

### Internal Contacts

| Role | Primary | Backup |
| ---- | ------- | ------ |
| Security Lead | [Contact] | [Contact] |
| Smart Contract Lead | [Contact] | [Contact] |
| DevOps Lead | [Contact] | [Contact] |
| Communications | [Contact] | [Contact] |

### External Contacts

| Entity | Purpose | Contact |
| ------ | ------- | ------- |
| Chainalysis | Transaction tracing | [Contact] |
| Immunefi | Bug bounty coordination | [Contact] |
| Legal Counsel | Legal guidance | [Contact] |
| Key Protocols | Partner protocols | [Various] |

### Multi-sig Signers

| Network | Signers Required | Available Signers |
| ------- | ---------------- | ----------------- |
| Ethereum | X of Y | [List] |
| Arbitrum | X of Y | [List] |
| Optimism | X of Y | [List] |

---

## Emergency Actions

### Pause Contracts

```solidity
// Guardian pause function
function pause() external onlyGuardian {
    _pause();
}
```

**Networks and Guardian Addresses:**
| Network | Guardian Address | Multi-sig Required |
| ------- | ---------------- | ------------------ |
| Ethereum | 0x... | No (EOA) |
| Arbitrum | 0x... | No (EOA) |

### Disable Keepers

```bash
# Remove keeper from strategy
# This prevents automated harvests and rebalances
```

### Emergency Withdrawals

Only if absolutely necessary and with governance approval:
```solidity
// Emergency withdrawal function
function emergencyWithdraw() external onlyGovernance {
    // ...
}
```

---

## Post-Incident Checklist

### Within 24 Hours
- [ ] Complete incident timeline
- [ ] Finalize root cause analysis
- [ ] Verify all systems are stable
- [ ] Publish initial public statement

### Within 1 Week
- [ ] Complete detailed post-mortem
- [ ] Publish full disclosure
- [ ] Implement process improvements
- [ ] Update monitoring and alerts

### Within 1 Month
- [ ] Review and update this runbook
- [ ] Conduct team debrief
- [ ] Schedule additional security reviews if needed
- [ ] Update bug bounty scope if needed

---

## Monitoring and Alerts

### Key Metrics to Monitor

| Metric | Alert Threshold | Action |
| ------ | --------------- | ------ |
| Unusual withdrawals | > $X in Y minutes | Investigate immediately |
| Failed transactions spike | > X% increase | Check for DoS attack |
| Oracle price deviation | > X% from reference | Pause affected vaults |
| TVL sudden drop | > X% in Y hours | Investigate cause |
| New contract interactions | Unknown contracts | Review for exploits |

### Alert Channels

- PagerDuty for P0/P1
- Telegram/Discord for all
- Email for P2/P3

---

## Templates

### Initial War Room Message

```
🚨 INCIDENT ALERT - [SEVERITY]

Time: [UTC Timestamp]
Summary: [Brief description]

Affected:
- [Contract/System 1]
- [Contract/System 2]

Estimated Impact: $[Amount]
Status: [Investigating/Contained/Resolved]

Next Steps:
1. [Action 1]
2. [Action 2]

Responders: @[name1] @[name2]
```

### Public Acknowledgment Template

```
We are aware of an issue affecting [system/vault].

We are actively investigating and will provide updates.

User funds are [safe/at risk/affected].

Please do not [action to avoid] until further notice.

Updates will be posted here and on [other channel].
```

---

## References

- [SECURITY.md](../SECURITY.md) - Security policy
- [SEVERITY_CLASSIFICATION.md](SEVERITY_CLASSIFICATION.md) - Severity guidelines
- [DISCLOSURE_TEMPLATE.md](../templates/DISCLOSURE_TEMPLATE.md) - Disclosure format
