# Obol's Security Process

This document describes the Security Process for Obol Network, including vulnerability disclosures and its [Bug Bounty program](#bug-bounty-program). We are committed to conduct our Security Process in a professional and civil manner. Public shaming, under-reporting, or misrepresentation of vulnerabilities will not be tolerated.

To submit a finding, please follow the steps outlined in receiving disclosures [section](#receiving-disclosures).

## Responsible Disclosure Standard

Obol follows a community [standard](https://github.com/RD-Crypto-Spec/Responsible-Disclosure#the-standard) for responsible disclosure in cryptocurrency and related software. This document is a public commitment to following the standard.

This standard provides detailed information for:

- [Initial Contact](https://github.com/RD-Crypto-Spec/Responsible-Disclosure#initial-contact): how to establish initial contact with Obol's security team.
- [Giving Details](https://github.com/RD-Crypto-Spec/Responsible-Disclosure#giving-details): what details to include with your vulnerability disclosure after having received a response to your initial contact.
- [Setting Dates](https://github.com/RD-Crypto-Spec/Responsible-Disclosure#setting-dates): how to agree on timelines for releasing updates and making details of the issue public.

Any expected deviations and necessary clarifications around the standard are explained in the following sections.

## Receiving Disclosures

### Directly to Obol

Obol is committed to working with researchers who submit security vulnerability notifications to us, to resolve those issues on an appropriate timeline, and to perform a coordinated release, giving credit to the reporter if they would so like.

Please submit issues to **all** of the following main points of contact for
security related issues according to the
[initial contact](https://github.com/RD-Crypto-Spec/Responsible-Disclosure#initial-contact)
and [giving details](https://github.com/RD-Crypto-Spec/Responsible-Disclosure#giving-details)
guidelines.

For all security related issues, Obol has the following main points of contact:

| Contact                | Public key                                                                                                   | Email                             | Keybase                                         |
| ---------------------- | ------------------------------------------------------------------------------------------------------------ | --------------------------------- | ----------------------------------------------- |
| obol security              |[PGP](https://github.com/ObolNetwork/obol-security/blob/main/keys/obol security (B6D735C5) – Public.asc) | security at obol.tech                ||
| eth2devops              |[PGP](https://github.com/ObolNetwork/obol-security/blob/main/keys/eth2devops.asc) | eth2devops at obol.tech                |[@eth2devops](https://keybase.io/eth2devops/chat)|


Include all contacts in your communication, PGP encrypted to all parties.

You can also reach out informally over keybase encrypted chat to one or more of the contacts as per the details above.

## Sending Disclosures

In the case where we become aware of security issues affecting other projects that has never affected Obol, our intention is to inform those projects of security issues on a best effort basis.

In the case where we fix a security issue in Obol that also affects the following neighboring projects, our intention is to engage in responsible disclosures with them as described in the adopted [standard](https://github.com/RD-Crypto-Spec/Responsible-Disclosure), subject to the deviations described in the deviations [section](#deviations-from-the-standard) of this document.

## Bilateral Responsible Disclosure Agreements

_Obol does not currently have any established bilateral disclosure agreements._

## Bug Bounty Program
Obol's bug bounty policy is published [here](https://docs.obol.tech/docs/sec/bug-bounty)

## Deviations from the Standard

The standard describes reporters of vulnerabilities including full details of an issue, in order to reproduce it. This is necessary for instance in the case of an external researcher both demonstrating and proving that there really is a security issue, and that security issue really has the impact that they say it has - allowing the development team to accurately prioritize and resolve the issue.

In the case of a counterfeiting or fund-stealing bug affecting Obol, however, we might decide not to include those details with our reports to partners ahead of coordinated release, as long as we are sure that they are not vulnerable.

## More Information

Additional security-related information about the Obol project including disclosures, signatures and PGP public keys can be found in the [obol-security](https://github.com/ObolNetwork/obol-security) repository.

## Credits

Parts of this document were inspired by [Grin's security policy](https://github.com/mimblewimble/grin/blob/master/SECURITY.md).
