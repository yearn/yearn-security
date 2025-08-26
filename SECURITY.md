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

### First Step: Bug Bounty Platforms

All security disclosures should **first** be submitted through one of Yearn’s official bug bounty platforms:

* **Immunefi**: [https://immunefi.com/bug-bounty/yearnfinance/information/](https://immunefi.com/bug-bounty/yearnfinance/information/)
* **Sherlock**: [https://audits.sherlock.xyz/bug-bounties/30](https://audits.sherlock.xyz/bug-bounties/30)

These platforms provide structured communication, payout management, and a transparent process for both researchers and Yearn.

### Directly to Yearn

If—for some reason—the contract in scope is not listed on these bug bounty platforms, or if you prefer to reach out directly, you can refer to the [Contact Information](#Contact-Information)


### Contact Information

For all security-related issues, Yearn maintains the following primary points of contact:

| Contact | Public key                                                                 | Email                                         |
| ------- | -------------------------------------------------------------------------- | --------------------------------------------- |
| tapir   | [PGP](https://github.com/yearn/yearn-security/blob/master/keys/tapir.asc)  | [yvtapir@gmail.com](mailto:yvtapir@gmail.com) |
| Spalen  | [PGP](https://github.com/yearn/yearn-security/blob/master/keys/spalen.asc) | [spalen@proton.me](mailto:spalen@proton.me)   |




## Sending Disclosures

When Yearn becomes aware of security issues affecting other projects, we may inform those projects on a best-effort basis.

When we fix a security issue in Yearn that also affects neighboring projects, we intend to engage in responsible disclosures with them as described in the adopted [standard](https://github.com/RD-Crypto-Spec/Responsible-Disclosure), subject to the deviations explained [below](#deviations-from-the-standard).

---

## Bilateral Responsible Disclosure Agreements

*Yearn does not currently maintain any established bilateral disclosure agreements.*


## Giving Details

After you receive a response to your initial contact, you should disclose the details of the vulnerability to the potentially affected party, along with reasons you think they may be affected and any advice you have on how to fix the issue. Once you have made initial contact, you are expected to provide all the technical details about the vulnerability within two working days of receiving acknowledgement of your initial contact.

Your report should include:

* A PGP encrypted email to maintainers (if you are not using an official bug bounty platform)
* Description of suspected vulnerability
* Steps to reproduce the issue
* Your email address and a secure mechanism to contact you
* Your name and/or colleagues if you wish to be later recognized
* Optionally a patch and/or suggestions to resolve the vulnerability

> **Note:** If your report was submitted via an official bug bounty program (e.g. Immunefi, Sherlock), then the secure submission process and communication channel provided by the platform replaces the need for direct PGP-encrypted communication.

## Setting Dates

After you have provided details of the issue to your neighbors, you both should agree on a date to release updates and a date to publicize the details of the issue. Issues that require only a small change should correspond with a date relatively soon after giving details - somewhere between 30 and 60 days. Issues that require a more significant investment by both parties might result in a date set up to 90 days in the future.

Parties involved in the disclosure should have the expectation of negotiating publishing dates with all of their neighbors that share the affected code, even if it turns out that those neighbors are not vulnerable for some other reason not known at the time.

It is up to each organization to determine their level of vulnerability to any disclosed issue, and rate issues accordingly. In general though, spend bugs should be considered very high priority - users losing access to their funds is precisely what this standard is intended to address.

It is up to each organization to notify their users of the availability of a fix and migrate their user base to the fixed version.

> **Note:** If the vulnerability was reported through an official bug bounty program, timelines and coordination will follow the program’s rules of engagement. If instead the report was made directly (e.g. via PGP-encrypted email), then the same expectations around setting responsible disclosure timelines still apply and should be mutually agreed upon between the reporter and the affected parties.



## Deviations from the Standard

The standard describes reporters of vulnerabilities including full details of an issue, in order to reproduce it. This is necessary for proving that a reported security issue is real, reproducible, and impactful—allowing the development team to accurately prioritize and resolve it.

However, in the case of counterfeiting or fund-stealing bugs affecting Yearn, we may choose not to include full technical details in advance reports to partners before a coordinated release, provided we are confident that they are not vulnerable.


## More Information

Additional security-related information about the Yearn project, including disclosures, signatures, and PGP public keys, can be found in the [yearn-security](https://github.com/yearn/yearn-security) repository.


## Credits

Parts of this document were inspired by [Grin's security policy](https://github.com/mimblewimble/grin/blob/master/SECURITY.md).

