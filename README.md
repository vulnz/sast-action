<p align="center">
  <a href="https://insom.ai/en/sdlc">
    <img src="https://insom.ai/static/img/logo.png" width="80" alt="Insomnia">
  </a>
</p>

<h1 align="center">SAST GitHub Action - SCA &amp; Secret Scanning (Insomnia)</h1>

<p align="center">
  Free <b>SAST</b> + <b>SCA</b> + secret scanning - catch code vulnerabilities &amp; hardcoded secrets on every push — in one line of YAML.<br>
  Findings land straight in your <b>GitHub Security tab</b>. No account, no upload, nothing to install.
</p>

<p align="center">
  <a href="https://github.com/marketplace/actions/sast-by-insomnia"><img src="https://img.shields.io/badge/GitHub%20Marketplace-vulnz%2Fvulnerability-7c3aed?logo=github" alt="GitHub Marketplace"></a>
  <img src="https://img.shields.io/badge/output-SARIF%202.1.0-2563eb" alt="SARIF 2.1.0">
  <img src="https://img.shields.io/badge/runners-Linux%20·%20macOS%20·%20Windows-64748b" alt="Runners">
  <img src="https://img.shields.io/badge/price-free-16a34a" alt="Free">
</p>

<p align="center">
  <a href="https://insom.ai/en/sdlc">Pipeline builder</a> ·
  <a href="https://insom.ai/en/plugin">IDE plugins</a> ·
  <a href="https://www.youtube.com/watch?v=KrmuPx_3ZSA">Watch the video</a>
</p>

<p align="center">
  <a href="https://www.youtube.com/watch?v=KrmuPx_3ZSA" title="Watch the Insomnia SAST overview">
    <img src="https://img.youtube.com/vi/KrmuPx_3ZSA/maxresdefault.jpg" width="560" alt="Watch the Insomnia SAST overview video">
  </a>
</p>

## Quick start

Add `.github/workflows/sast.yml`:

```yaml
name: SAST
on:
  push:
    branches: [main]
  pull_request:

permissions:
  contents: read
  security-events: write   # lets findings show in the Security tab

jobs:
  sast:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: vulnz/vulnerability@v1
        with:
          fail-on: high
```

The action installs the engine with `pip install sast`, scans the repo, fails the build on
high-severity findings, uploads SARIF to the Security tab, and saves the HTML/JSON report as an artifact.

> Prefer to build your pipeline visually? Generate the YAML with the
> **[Pipeline Builder → insom.ai/en/sdlc](https://insom.ai/en/sdlc)**.

## What it covers

One step instead of five tools — all on your runner, nothing leaves CI:

- **SAST** — cross-file taint (source → sink) across **16 languages**
- **Secrets** — ~230 rules, with **live validation** that proves whether a key still works
- **SCA** — dependency CVEs (OSV) + dependency-confusion
- **Containers & IaC** — OS-package CVEs, Terraform / Kubernetes / Docker / CloudFormation misconfig
- **More** — CMS advisories (WordPress/Joomla/Drupal/Magento), vulnerable JS libs, web-shell / malware
- **Output** — SARIF 2.1.0, JSON and HTML

## How it compares

| | Insomnia&nbsp;SAST | CodeQL | Semgrep | Snyk | Trivy |
|---|:---:|:---:|:---:|:---:|:---:|
| Free on private repos | ✅ | ⚠️ | ✅ | ⚠️ | ✅ |
| Runs fully on your runner | ✅ | ✅ | ✅ | ❌ | ✅ |
| Cross-file taint | ✅ | ✅ | ⚠️ | ✅ | ❌ |
| Secret detection | ✅ | ❌ | ⚠️ | ✅ | ✅ |
| Live secret validation | ✅ | ❌ | ❌ | ❌ | ❌ |
| Dependency / container CVEs | ✅ | ❌ | ❌ | ✅ | ✅ |
| IaC misconfig | ✅ | ❌ | ✅ | ✅ | ✅ |
| CMS / web-shell / malware | ✅ | ❌ | ❌ | ❌ | ❌ |

<sub>✅ built in · ⚠️ partial or conditional · ❌ not available. Names are trademarks of their owners.</sub>

## Inputs

| Input | Default | Description |
|---|---|---|
| `path` | `.` | Directory to scan. |
| `fail-on` | _(empty)_ | Fail the job at/above this severity: `critical`/`high`/`medium`/`low`/`info`. Empty = report only. |
| `format` | `sarif,json,html` | Report formats (`sarif` is always included). |
| `output` | `sast-report` | Report output directory. |
| `upload-sarif` | `true` | Upload SARIF to GitHub code scanning (needs `security-events: write`). |
| `secrets-only` | `false` | Only scan for secrets / credentials (fast). |
| `no-sca` | `false` | Skip the dependency CVE (SCA) pass. |
| `changed-only` | `false` | Scan only files changed vs git HEAD. |
| `version` | `latest` | Engine version to download. |
| `args` | _(empty)_ | Extra raw flags for the `sast` CLI. |
| `artifact-name` | `insomnia-sast-report` | Name of the uploaded report artifact (give each matrix job a unique value). |

**Outputs:** `sarif-file` (path to the SARIF report) · `exit-code` (`0` clean / `2` gated / `1` error).

## Examples

```yaml
# PR gate — changed files only, fail on high
- uses: actions/checkout@v4
  with: { fetch-depth: 0 }
- uses: vulnz/vulnerability@v1
  with: { changed-only: true, fail-on: high }
```

```yaml
# Fast secrets-only check
- uses: vulnz/vulnerability@v1
  with: { secrets-only: true, fail-on: medium }
```

## Same engine, everywhere

| Where | How |
|---|---|
| CI / CD | `uses: vulnz/vulnerability@v1` |
| Terminal | `pip install sast` · `brew install vulnz/sast/sast` · `apt-get install sast` |
| Editor | [VS Code · JetBrains · Visual Studio](https://insom.ai/en/plugin) |

Runs on `ubuntu-*`, `macos-*` and `windows-*` runners — the engine is installed via `pip install sast`.

---

<p align="center">
  <a href="https://insom.ai/en/sdlc">insom.ai/en/sdlc</a> ·
  <a href="https://insom.ai/en/plugin">insom.ai/en/plugin</a><br>
  <sub>Free · same engine as the Insomnia CLI &amp; IDE plugins · © CQR Cybersecurity LLC</sub>
</p>
