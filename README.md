# Insomnia SAST — GitHub Action

[![insom.ai](https://img.shields.io/badge/insom.ai-SAST-7c3aed)](https://insom.ai/sdlc)

Free, fast **static application security testing** for your CI — the same
engine behind `pip install sast`, `npm i -g sastai` and the Insomnia desktop
scanner, wrapped as a one-line GitHub Action.

- **Native AST + cross-file taint** (source → sink) across **16 languages** — no external tools to install.
- **Secret / credential detection** (~230 rules) with **live key validation**.
- **SCA** — dependency CVE matching (OSV) + dependency-confusion detection.
- **IaC misconfig** rules (Terraform / Kubernetes / Docker / CloudFormation / …).
- **Vulnerable JS libraries**, **CMS** plugin/theme/core CVEs (WordPress / Joomla / Drupal / Magento), **web-shell & malware** detection.
- **SARIF 2.1.0** output that lands in the **GitHub Security tab**, plus HTML/JSON reports as build artifacts.
- **CI gating** with `fail-on` and **incremental** (changed-files-only) scans.

## Quick start

```yaml
name: SAST
on:
  push:
    branches: [main]
  pull_request:

permissions:
  contents: read
  security-events: write   # required for the Security-tab upload

jobs:
  sast:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: vulnz/sast-action@v1
        with:
          path: .
          fail-on: high
```

That replaces the ~40-line install-and-run block — the Action downloads the
OS-matched engine, scans, uploads SARIF to code scanning, and attaches the
HTML/JSON report as an artifact.

## Inputs

| Input          | Default              | Description |
|----------------|----------------------|-------------|
| `path`         | `.`                  | Directory to scan (relative to repo root). |
| `fail-on`      | _(empty)_            | Fail the job (exit 2) at/above this severity: `critical`/`high`/`medium`/`low`/`info`. Empty = report only. |
| `format`       | `sarif,json,html`    | Comma-separated report formats. `sarif` is always included. |
| `output`       | `sast-report`        | Directory for report files. |
| `upload-sarif` | `true`               | Upload SARIF to GitHub code scanning (needs `security-events: write`). |
| `secrets-only` | `false`              | Only scan for secrets / hardcoded credentials (fast). |
| `no-sca`       | `false`              | Skip the dependency CVE / dep-confusion (SCA) pass. |
| `changed-only` | `false`              | Scan only files changed vs git HEAD (incremental). |
| `version`      | `latest`             | Engine channel/version to download. |
| `args`         | _(empty)_            | Extra raw flags passed verbatim to the `sast` CLI. |

## Outputs

| Output       | Description |
|--------------|-------------|
| `sarif-file` | Path to the generated SARIF report. |
| `exit-code`  | CLI exit code (`0` clean / `2` findings at-or-above `fail-on` / `1` error). |

## Examples

**Pull-request gate, changed files only, fail on high:**

```yaml
      - uses: actions/checkout@v4
        with: { fetch-depth: 0 }
      - uses: vulnz/sast-action@v1
        with:
          changed-only: true
          fail-on: high
```

**Secrets-only fast pre-commit gate:**

```yaml
      - uses: vulnz/sast-action@v1
        with:
          secrets-only: true
          fail-on: medium
```

**Report only (no build failure), keep the HTML artifact:**

```yaml
      - uses: vulnz/sast-action@v1
        with:
          format: sarif,html
```

## Runners

Works on `ubuntu-*`, `macos-*` and `windows-*` runners — the engine binary is
auto-matched to the runner OS.

## Links

- Pipeline wizard & docs: <https://insom.ai/sdlc>
- `pip install sast` · `npm i -g sastai` · `brew install vulnz/sast/sast`

---

© CQR Cybersecurity LLC — Insomnia / insom.ai. Proprietary; free to use in CI.
