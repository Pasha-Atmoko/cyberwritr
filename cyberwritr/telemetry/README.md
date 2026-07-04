### Overview

To help make Cyberwritr better for everyone, we collect anonymized data that helps us understand how to better improve our AI security agent for our users, guide the addition of new features, and fix common errors and bugs. This feedback loop is crucial for improving Cyberwritr's capabilities and user experience.

We use [PostHog](https://posthog.com), an open-source analytics platform, for data collection and analysis. Our telemetry implementation is fully transparent - you can review the [source code](https://github.com/usestrix/cyberwritr/blob/main/cyberwritr/telemetry/posthog.py) to see exactly what we track.

### Telemetry Policy

Privacy is our priority. All collected data is anonymized by default. Each session gets a random UUID that is not persisted or tied to you. Your code, scan targets, vulnerability details, and findings always remain private and are never collected.

### What We Track

We collect only very **basic** usage data including:

**Session Errors:** Duration and error types (not messages or stack traces)\
**System Context:** OS type, architecture, Cyberwritr version\
**Scan Context:** Scan mode (quick/standard/deep), scan type (whitebox/blackbox)\
**Model Usage:** Which LLM model is being used (not prompts or responses)\
**Aggregate Metrics:** Vulnerability counts by severity

### What We **Never** Collect

- Usernames, or any identifying information
- Scan targets, file paths, target URLs, or domains
- Vulnerability details, descriptions, or code
- LLM requests and responses

### How to Opt Out

Telemetry in Cyberwritr is entirely **optional**:

```bash
export CWRITR_TELEMETRY=0
```

You can set this environment variable before running Cyberwritr to disable **all** telemetry.
