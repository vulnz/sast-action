<p align="center">
  <a href="https://insom.ai/en/sdlc">
    <img src="https://insom.ai/static/img/logo.png" width="92" alt="Insomnia">
  </a>
</p>

<h1 align="center">Insomnia SAST — GitHub Action</h1>

<p align="center">
  <b>Catch hardcoded secrets &amp; code vulnerabilities on every push — in one line of YAML.</b><br>
  Native AST taint across <b>16 languages</b>, secrets with <i>live key validation</i>, SCA, IaC &amp; container CVEs —
  results land straight in your <b>GitHub Security tab</b>. No external service, no account, nothing to upload.
</p>

<p align="center">
  <a href="https://github.com/marketplace/actions/insomnia-sast">
    <img src="https://img.shields.io/badge/Get%20it%20on-GitHub%20Marketplace-7c3aed?style=for-the-badge&logo=github&logoColor=white" alt="Get it on the GitHub Marketplace">
  </a>
</p>

<p align="center">
  <a href="https://github.com/marketplace/actions/insomnia-sast"><img src="https://img.shields.io/badge/marketplace-vulnz%2Fsast--action%40v1-7c3aed?logo=github" alt="Marketplace"></a>
  <a href="https://pypi.org/project/sast/"><img src="https://img.shields.io/pypi/v/sast?label=pip%20install%20sast&color=16a34a&logo=python&logoColor=white" alt="PyPI"></a>
  <img src="https://img.shields.io/badge/languages-16-2563eb" alt="16 languages">
  <img src="https://img.shields.io/badge/output-SARIF%202.1.0-2563eb" alt="SARIF 2.1.0">
  <img src="https://img.shields.io/badge/runners-Linux%20%C2%B7%20macOS%20%C2%B7%20Windows-64748b" alt="Runners">
  <img src="https://img.shields.io/badge/price-free-16a34a" alt="Free">
</p>

<p align="center">
  <a href="https://insom.ai/en/sdlc"><b>Pipeline builder</b></a> ·
  <a href="https://insom.ai/en/sdlc"><b>SDLC pipeline</b></a> ·
  <a href="https://insom.ai/en/plugin"><b>IDE plugins</b></a> ·
  <a href="https://insom.ai/api/plugin/manifest">Manifest</a>
</p>

---

## ▶ See it in action

<p align="center">
  <a href="https://www.youtube.com/watch?v=KrmuPx_3ZSA" title="Watch the Insomnia SAST overview on YouTube">
    <img src="https://img.youtube.com/vi/KrmuPx_3ZSA/maxresdefault.jpg" width="640" alt="Watch the Insomnia SAST overview video on YouTube">
  </a>
  <br>
  <a href="https://www.youtube.com/watch?v=KrmuPx_3ZSA"><b>▶ Watch the overview on YouTube</b></a>
  &nbsp;·&nbsp;
  <a href="https://insom.ai/en/sdlc">Read the full SDLC walkthrough →</a>
</p>

---

## 🚀 Quick start

Drop this into `.github/workflows/sast.yml`:

```yaml
name: SAST
on:
  push:
    branches: [main]
  pull_request:

permissions:
  contents: read
  security-events: write   # lets findings show up in the Security tab

jobs:
  sast:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: vulnz/sast-action@v1
        with:
          fail-on: high
```

That's it. The action downloads the OS-matched engine, scans your repo, **fails the
build on high-severity findings**, uploads **SARIF to the Security tab**, and attaches the
HTML/JSON report as an artifact — replacing the ~40-line install-and-run block you'd
otherwise copy-paste.

> 🧩 **Prefer to build it visually?** Generate a full pipeline — targets, formats, scan policy,
> schedule and notifications — with the **[Pipeline Builder / Wizard → insom.ai/en/sdlc](https://insom.ai/en/sdlc)**,
> then copy the YAML straight into your repo.

---

## 🥇 How it compares

One action instead of stitching five tools together. Everything below runs in a single
step, fully on your runner — no SaaS account, no code leaving CI.

| Capability | **Insomnia SAST** | CodeQL Action | Semgrep OSS | Snyk | Trivy | Gitleaks |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| One-line `uses:` setup | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Free on **private** repos | ✅ | ⚠️ GHAS only | ✅ | ⚠️ limited | ✅ | ✅ |
| No account / no upload to a service | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ |
| Cross-file **taint** (source → sink) | ✅ 16 langs | ✅ fewer langs | ⚠️ intra-file | ✅ | ❌ | ❌ |
| Secret / credential detection | ✅ ~230 rules | ❌ | ⚠️ add-on | ✅ | ✅ | ✅ |
| **Live** secret validation (proves the key works) | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Dependency CVEs (SCA) | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ |
| Container / OS-package CVEs | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ |
| IaC misconfig (Terraform/K8s/Docker/CFN) | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ |
| CMS advisories (WordPress/Joomla/Drupal/Magento) | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Web-shell / malware detection | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| SARIF → GitHub Security tab | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

<sub>✅ built in · ⚠️ partial / conditional · ❌ not available. Comparison reflects each tool's free/OSS GitHub Action as of 2026; competitor names are trademarks of their owners.</sub>

---

## ⚙️ Inputs

| Input | Default | Description |
|---|---|---|
| `path` | `.` | Directory to scan (relative to the repo root). |
| `fail-on` | _(empty)_ | Fail the job (exit 2) at/above this severity: `critical`/`high`/`medium`/`low`/`info`. Empty = report only. |
| `format` | `sarif,json,html` | Comma-separated report formats. `sarif` is always included. |
| `output` | `sast-report` | Directory for report files. |
| `upload-sarif` | `true` | Upload SARIF to GitHub code scanning (needs `security-events: write`). |
| `secrets-only` | `false` | Only scan for secrets / hardcoded credentials (fast). |
| `no-sca` | `false` | Skip the dependency CVE / dependency-confusion (SCA) pass. |
| `changed-only` | `false` | Scan only files changed vs git HEAD (incremental). |
| `version` | `latest` | Engine channel/version to download. |
| `args` | _(empty)_ | Extra raw flags passed verbatim to the `sast` CLI. |

## 📤 Outputs

| Output | Description |
|---|---|
| `sarif-file` | Path to the generated SARIF report. |
| `exit-code` | CLI exit code: `0` clean · `2` findings at/above `fail-on` · `1` error. |

---

## 🧪 More recipes

**Pull-request gate — only the changed files, fail on high:**

```yaml
      - uses: actions/checkout@v4
        with: { fetch-depth: 0 }
      - uses: vulnz/sast-action@v1
        with:
          changed-only: true
          fail-on: high
```

**Fast secrets-only pre-merge check:**

```yaml
      - uses: vulnz/sast-action@v1
        with:
          secrets-only: true
          fail-on: medium
```

**Report only (never fail the build), keep the HTML artifact:**

```yaml
      - uses: vulnz/sast-action@v1
        with:
          format: sarif,html
```

**Use an output in a later step:**

```yaml
      - id: sast
        uses: vulnz/sast-action@v1
      - run: echo "SARIF written to ${{ steps.sast.outputs.sarif-file }} (exit ${{ steps.sast.outputs.exit-code }})"
```

---

## 🛡️ One engine, everything covered

Multi-language **SAST + taint** (16 languages), **secrets with live key validation**,
**dependency/SCA CVEs**, container &amp; OS-package CVEs, **IaC** misconfig, **CMS** advisories,
vulnerable JS libraries, and **web-shell / malware** detection — exported as **SARIF / JSON / HTML**.

The exact same engine powers `pip install sast`, the npm launcher, and the
[VS Code / JetBrains / Visual Studio plugins](https://insom.ai/en/plugin) — so there's
**zero rule drift** between a developer's editor, the terminal, and your CI.

| Where | How |
|---|---|
| **CI / CD** | this action · `uses: vulnz/sast-action@v1` |
| **Terminal / any CI** | `pip install sast` · `brew install vulnz/sast/sast` · `apt-get install sast` |
| **Editor** | [VS Code · JetBrains · Visual Studio](https://insom.ai/en/plugin) |

---

## 🔐 Permissions &amp; runners

- Set `security-events: write` on the job for the Security-tab upload (set `upload-sarif: false` to skip it).
- Runs on `ubuntu-*`, `macos-*` and `windows-*` runners — the engine binary is auto-matched to the runner OS and fetched from `https://insom.ai/latest/sast/<os>`.

---

<p align="center">
  <a href="https://insom.ai/en/sdlc"><b>insom.ai/en/sdlc</b></a> ·
  <a href="https://insom.ai/en/plugin"><b>insom.ai/en/plugin</b></a><br>
  <sub>Free · same engine as the Insomnia CI pipeline, CLI &amp; IDE plugins · © CQR Cybersecurity LLC</sub>
</p>
