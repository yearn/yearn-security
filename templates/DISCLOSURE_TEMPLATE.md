# Incident Disclosure Template

Use this template when creating a new incident disclosure. Copy this file to `disclosures/YYYY-MM-DD.md` and fill in the sections.

---

# Incident disclosure - YYYY-MM-DD

## Summary

<!-- Provide a brief overview (2-4 bullet points) covering:
- What happened and when
- Which systems/vaults were affected
- The financial impact (if any)
- Current status and resolution
-->

- [Brief description of the incident]
- [Affected systems/vaults]
- [Financial impact]
- [Resolution status]

## Background

<!-- Provide context about the affected protocols, systems, or components. Include:
- What is the affected protocol/system?
- Brief technical description
- Any relevant audit history
- Links to documentation
-->

[Background context here]

## Details of Incident

<!-- Describe the technical details of what happened:
- Which contracts/strategies were affected?
- What was the root cause?
- How was the vulnerability exploited (if applicable)?
-->

### Affected Components

| Component | Address | Description |
| --------- | ------- | ----------- |
| [Name]    | [0x...] | [Description] |

### Root Cause Analysis

[Detailed technical analysis of what went wrong]

### Financial Impact

| Asset | Amount Lost | Value (USD) | Notes |
| ----- | ----------- | ----------- | ----- |
| [Token] | [Amount] | $[Value] | [Notes] |

**Total Impact**: $[Total]

## Details of Fix

<!-- Describe the remediation steps taken:
- Immediate actions taken
- Long-term fixes implemented
- How similar incidents will be prevented
-->

### Immediate Response

[Steps taken immediately after discovery]

### Long-term Remediation

[Permanent fixes and improvements]

### Prevention Measures

[Steps to prevent similar incidents]

## Timeline of Events

<!-- Chronological list of events with timestamps (UTC) -->

| Time (UTC) | Event |
| ---------- | ----- |
| YYYY-MM-DD HH:MM | [Event description] |
| YYYY-MM-DD HH:MM | [Event description] |
| YYYY-MM-DD HH:MM | [Event description] |

## Third-Party Disclosure

<!-- Document any disclosures made to third parties -->

As per Yearn's security process document [[N]](#References), [disclosure details here].

## Lessons Learned

<!-- What was learned from this incident? -->

1. [Lesson 1]
2. [Lesson 2]
3. [Lesson 3]

## References

<!-- Numbered list of references -->

1. [Reference title](URL)
2. [Reference title](URL)
3. [Yearn's Security Process](https://github.com/yearn/yearn-security/blob/master/SECURITY.md)

---

## Appendix: Disclosure Checklist

Before publishing, ensure:

- [ ] All affected parties have been notified
- [ ] Fixes have been deployed and verified
- [ ] Financial impact is accurately calculated
- [ ] Timeline is complete and accurate
- [ ] Technical details have been reviewed for accuracy
- [ ] No sensitive information is included that could enable further exploits
- [ ] References are valid and accessible
- [ ] Document has been reviewed by at least two team members
