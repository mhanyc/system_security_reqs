# WTF (What's this for?)

Vibrant continues to invest in securing data critical to help seekers; and increasingly we are learning that we write software.

This repository contains Vibrant's development of software security requirements.  It is maintained by the InfoSec team.

## Security Requirements

Please consider these a starting point for understanding security within the context of modern software.

>[!NOTE]
> As read on the cover of the Guide . . . DON'T PANIC.  This is only a draft. :people_hugging:

## Usage

At least initially, this documentation will be used by InfoSec to measure systems against consensus best practice (informed by NIST).

As we understand need and appetite, we will work with you (dear reader) to craft [SMART](https://en.wikipedia.org/wiki/SMART_criteria) requirements for your specific system development needs. (together :love_letter:) 

Here's how we do this.

### Validation

InfoSec uses the full ASVS, unedited, as our [validation requirements](./validation_requirements/INDEX.md) for all applications.  If we encounter software not matching the ASVS model, we will adapt.

>[!TIP]
> This tries to answer the question "How well is my system doing compared to best practice?"

### System Development

You may use a [maturity-calibrated](./system_requirements/INDEX.md) set of requirements as a way to reach Vibrant's "next step" assurance level.

>[!TIP]
> This tries to answer the question "What do I have to do to be compliant with Vibrant standards?"

### Origins

InfoSec chose the latest release of [OWASP Application Security Verification Standard](https://github.com/OWASP/ASVS) as the inspiration for Vibrant's security requirements, for a variety of reasons.  Here are a few:

- it maps directly to NIST guidelines
- it offers tiers of requirements, allowing for variation in maturity and risk management
- it represents consensus best-practice within a global application security community

We currently fork the [actual ASVS](./ASVS_docs/README.md) into this repository (periodically) for quick & easy reference.

## References

- [Glossary of terms](./glossary.md)
