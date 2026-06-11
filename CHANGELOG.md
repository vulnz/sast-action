# Changelog

## 1.6.11
- Action renamed to **SAST** on the GitHub Marketplace.
- Engine now installed via `pip install sast` (replaces the direct binary download).
- Crescent **moon** brand icon (purple).

## v1.0.1
- Brand icon set to the Insomnia crescent **moon** (purple) on the Marketplace card.
- Decluttered README (compact badges, tighter comparison table).

## v1.0.0
- Initial release of the Insomnia SAST GitHub Action.
- Composite action: downloads the OS-matched SAST engine from insom.ai, scans,
  emits SARIF/JSON/HTML, uploads SARIF to GitHub code scanning and the report
  as a build artifact.
- Inputs: `path`, `fail-on`, `format`, `output`, `upload-sarif`, `secrets-only`,
  `no-sca`, `changed-only`, `version`, `args`.
- Outputs: `sarif-file`, `exit-code`.
- Linux / macOS / Windows runners.
