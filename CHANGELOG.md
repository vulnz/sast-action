# Changelog

## v1.0.0
- Initial release of the Insomnia SAST GitHub Action.
- Composite action: downloads the OS-matched SAST engine from insom.ai, scans,
  emits SARIF/JSON/HTML, uploads SARIF to GitHub code scanning and the report
  as a build artifact.
- Inputs: `path`, `fail-on`, `format`, `output`, `upload-sarif`, `secrets-only`,
  `no-sca`, `changed-only`, `version`, `args`.
- Outputs: `sarif-file`, `exit-code`.
- Linux / macOS / Windows runners.
