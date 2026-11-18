# Yearn's Security Process

This document describes the Security Process for Yearn Finance, including vulnerability disclosures and its bug bounty programs. We are committed to conducting our Security Process in a professional and civil manner. Public shaming, under-reporting, or misrepresentation of vulnerabilities will not be tolerated.

To submit a finding, please follow the steps outlined in the [receiving disclosures section](#receiving-disclosures).


## Responsible Disclosure Standard

Yearn follows a community [standard](https://github.com/RD-Crypto-Spec/Responsible-Disclosure#the-standard) for responsible disclosure in cryptocurrency and related software. This document is a public commitment to following the standard.

This standard provides detailed information for:

* [Initial Contact](https://github.com/RD-Crypto-Spec/Responsible-Disclosure#initial-contact): how to establish initial contact with Yearn's security team.
* [Giving Details](https://github.com/RD-Crypto-Spec/Responsible-Disclosure#giving-details): what details to include with your vulnerability disclosure after having received a response.
* [Setting Dates](https://github.com/RD-Crypto-Spec/Responsible-Disclosure#setting-dates): how to agree on timelines for releasing updates and making details of the issue public.

Any expected deviations and necessary clarifications around the standard are explained in the following sections.


## Receiving Disclosures

### Official Bug Bounty Platforms

Reports covered by Yearn's bug bounty programs must be submitted **only** through the
appropriate official platform:

* **Yearn core contracts**: [Immunefi](https://immunefi.com/bug-bounty/yearnfinance/information/)
* **Yearn strategies**: [Sherlock](https://audits.sherlock.xyz/bug-bounties/30)

Review each program's current scope and rules before submitting. Reports within a program's
scope must use that platform's submission and communication channels, rather than email.

These platforms provide structured communication, payout management, and a transparent process for both researchers and Yearn.

### Directly to Yearn

Email reports are accepted only for vulnerabilities that demonstrate a direct attack against Yearn
on an attack surface outside the scope of **both** programs above. Include the affected asset,
a reproducible attack path, and an explanation of why the report falls outside both programs' scope.
Send qualifying reports to the contacts listed in [Contact Information](#contact-information).


### Contact Information

For direct reports meeting the requirements above, Yearn maintains the following points of contact:

| Contact | Email                                     |
| ------- | ----------------------------------------- |
| Tapir   | [yvtapir@gmail.com](mailto:yvtapir@gmail.com) |
| Spalen  | [spalen@proton.me](mailto:spalen@proton.me)   |
| Moto    | [mmotogascar@proton.me](mailto:motmotogascar@proton.me) |

Yearn prefers [age recipients](keys/age-recipients.txt) for direct encrypted
communication with active security triage contacts.

## Sending Disclosures

When Yearn becomes aware of security issues affecting other projects, we may inform those projects on a best-effort basis.

When we fix a security issue in Yearn that also affects neighboring projects, we intend to engage in responsible disclosures with them as described in the adopted [standard](https://github.com/RD-Crypto-Spec/Responsible-Disclosure), subject to the deviations explained [below](#deviations-from-the-standard).

---

## Bilateral Responsible Disclosure Agreements

*Yearn does not currently maintain any established bilateral disclosure agreements.*


## Giving Details

After you receive a response to your initial contact, you should disclose the details of the vulnerability to the potentially affected party, along with reasons you think they may be affected and any advice you have on how to fix the issue. Once you have made initial contact, you are expected to provide all the technical details about the vulnerability within two working days of receiving acknowledgement of your initial contact.

Your report should include:

* For qualifying direct reports, an email encrypted with the [age recipients](keys/age-recipients.txt)
* Description of suspected vulnerability
* Steps to reproduce the issue
* Your email address and a secure mechanism to contact you
* Your name and/or colleagues if you wish to be later recognized
* Optionally a patch and/or suggestions to resolve the vulnerability

> **Note:** For reports submitted through Immunefi or Sherlock, use the platform's secure submission
> process and communication channel instead of encrypted email.

## Setting Dates

After you have provided details of the issue to your neighbors, you both should agree on a date to release updates and a date to publicize the details of the issue. Issues that require only a small change should correspond with a date relatively soon after giving details - somewhere between 30 and 60 days. Issues that require a more significant investment by both parties might result in a date set up to 90 days in the future.

Parties involved in the disclosure should have the expectation of negotiating publishing dates with all of their neighbors that share the affected code, even if it turns out that those neighbors are not vulnerable for some other reason not known at the time.

It is up to each organization to determine their level of vulnerability to any disclosed issue, and rate issues accordingly. In general though, spend bugs should be considered very high priority - users losing access to their funds is precisely what this standard is intended to address.

It is up to each organization to notify their users of the availability of a fix and migrate their user base to the fixed version.

> **Note:** For vulnerabilities reported through an official bug bounty program, timelines and
> coordination follow the program's rules of engagement. For qualifying email reports outside both
> programs' scope, responsible disclosure timelines should be mutually agreed upon between the
> reporter and the affected parties.


## Deviations from the Standard

The standard describes reporters of vulnerabilities including full details of an issue, in order to reproduce it. This is necessary for proving that a reported security issue is real, reproducible, and impactful—allowing the development team to accurately prioritize and resolve it.

However, in the case of counterfeiting or fund-stealing bugs affecting Yearn, we may choose not to include full technical details in advance reports to partners before a coordinated release, provided we are confident that they are not vulnerable.


## More Information

Additional security-related information about the Yearn project, including disclosures, signatures, and PGP public keys, can be found in the [yearn-security](https://github.com/yearn/yearn-security) repository.


## Credits

Parts of this document were inspired by [Grin's security policy](https://github.com/mimblewimble/grin/blob/master/SECURITY.md).
