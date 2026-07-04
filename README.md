<div align="center">

# Cyberwritr

### AI-powered autonomous security testing agent — powered by Kimi.

<a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache%202.0-3b82f6?style=flat-square" alt="License"></a>
<img src="https://img.shields.io/badge/python-3.12+-4b8bbe?style=flat-square" alt="Python 3.12+">
<img src="https://img.shields.io/badge/LLM-Kimi%20(Moonshot)-000000?style=flat-square" alt="Kimi">

</div>

---

## What is Cyberwritr?

Cyberwritr runs autonomous AI agents that test your applications the way a real
attacker would: it executes your code in an isolated Docker sandbox, probes for
vulnerabilities, and validates each finding with a working proof-of-concept —
so you get exploitable issues, not scanner noise.

It is built for developers and security teams who want fast, accurate security
testing without the overhead of manual pentesting or the false positives of
static analysis.

By default, Cyberwritr uses **Kimi (Moonshot AI)** as its reasoning engine,
routed through LiteLLM — but any OpenAI-compatible or LiteLLM-supported model
works.

## Key capabilities

- **Real exploit validation** — findings come with working PoCs, not guesses
- **Full pentesting toolkit** — recon, exploitation, and validation out of the box
- **Multi-agent orchestration** — specialized agents that collaborate like a red team
- **Sandboxed execution** — all activity runs inside an isolated Docker container
- **Developer-first CLI** — actionable findings with remediation guidance
- **CI/CD friendly** — headless mode with non-zero exit on findings

## Requirements

- **Python 3.12+**
- **Docker** (running)
- A **Kimi / Moonshot API key** (or any other supported LLM key)

## Quick start

```bash
# 1. Clone
git clone https://github.com/Pasha-Atmoko/cyberwritr.git
cd cyberwritr

# 2. Install
pipx install .        # or: uv sync && uv run cwritr ...

# 3. Configure Kimi (copy .env.example -> .env, or export directly)
export CWRITR_LLM="openai/kimi-k2.5"
export LLM_API_BASE="https://api.moonshot.ai/v1"
export LLM_API_KEY="sk-your-moonshot-key"

# 4. Run your first scan
cwritr --target ./app-directory
```

> First run automatically pulls the sandbox Docker image. Results are saved to
> `cyberwritr_runs/<run-name>`.

## Configuration

All settings are read from `CWRITR_*` environment variables (or `~/.cyberwritr/cli-config.json`).

| Variable | Description | Default |
| --- | --- | --- |
| `CWRITR_LLM` | Model string (LiteLLM format) | — (required) |
| `LLM_API_KEY` | API key for the model provider | — (required) |
| `LLM_API_BASE` | API base URL (set for Kimi / local models) | — |
| `CWRITR_REASONING_EFFORT` | Thinking effort; ignored by models that don't support it | `high` |
| `CWRITR_TELEMETRY` | Anonymous telemetry | `false` (off) |
| `PERPLEXITY_API_KEY` | Optional, enables the web-search tool | — |

**Kimi models you can use:**

| Model string | Notes |
| --- | --- |
| `openai/kimi-k2.5` | Default — agentic, strong tool use |
| `openai/kimi-k2-thinking` | Heavier reasoning |
| `openai/kimi-k2-0905` | 256K context |

## Usage examples

```bash
# Scan a local codebase
cwritr --target ./app-directory

# Review a GitHub repository
cwritr --target https://github.com/org/repo

# Black-box web application assessment
cwritr --target https://your-app.com

# Authenticated (grey-box) testing
cwritr --target https://your-app.com --instruction "Use credentials: user:pass"

# Multi-target (source + deployed app)
cwritr -t https://github.com/org/app -t https://your-app.com

# Focused scope via instruction file
cwritr --target api.your-app.com --instruction-file ./instruction.md

# Headless / CI: PR diff-scope against a base branch
cwritr -n --target ./ --scan-mode quick --scope-mode diff --diff-base origin/main
```

## Vulnerability coverage

Cyberwritr identifies, validates, and exploits issues across the OWASP Top 10
and beyond:

- **Broken Access Control** — IDOR, privilege escalation, auth bypass
- **Injection** — SQL / NoSQL / OS command injection, SSTI
- **Server-Side** — SSRF, XXE, insecure deserialization, RCE
- **Client-Side** — XSS (stored/reflected/DOM), prototype pollution, CSRF
- **Business Logic** — race conditions, payment manipulation, workflow bypass
- **Auth & Session** — JWT attacks, session fixation
- **API Security** — broken auth, mass assignment, rate-limit bypass

## CI/CD (GitHub Actions)

```yaml
name: cyberwritr-security-test

on:
  pull_request:

jobs:
  security-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Install Cyberwritr
        run: pipx install .

      - name: Run Cyberwritr
        env:
          CWRITR_LLM: ${{ secrets.CWRITR_LLM }}
          LLM_API_BASE: ${{ secrets.LLM_API_BASE }}
          LLM_API_KEY: ${{ secrets.LLM_API_KEY }}
        run: cwritr -n -t ./ --scan-mode quick
```

## Attribution

Cyberwritr is based on **[Strix](https://github.com/usestrix/strix)** (Apache-2.0,
Copyright 2025 OmniSecure Inc.) and has been rebranded and modified — including a
Kimi-first LLM configuration and telemetry disabled by default. See the
[`NOTICE`](NOTICE) file for details.

It also builds on excellent open-source work:
[LiteLLM](https://github.com/BerriAI/litellm),
[Caido](https://github.com/caido/caido),
[Nuclei](https://github.com/projectdiscovery/nuclei),
[Playwright](https://github.com/microsoft/playwright), and
[Textual](https://github.com/Textualize/textual).

## License

[Apache License 2.0](LICENSE).

---

> ⚠️ **Only test applications you own or have explicit written permission to test.**
> You are solely responsible for using Cyberwritr ethically and legally.

