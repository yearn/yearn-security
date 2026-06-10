# Subdomain Takeover Vulnerability

## Description
A subdomain takeover vulnerability was discovered in the ybribe.yearn.fi subdomain. The CNAME record for this subdomain pointed to a Vercel deployment that no longer existed, allowing an attacker to claim the subdomain.

## Remediation
The CNAME record for ybribe.yearn.fi has been removed to prevent subdomain takeover.

## Recommendations
* Regularly review DNS configurations to ensure that all records are up-to-date and point to valid deployments.
* Implement a process to monitor for and respond to subdomain takeover vulnerabilities.