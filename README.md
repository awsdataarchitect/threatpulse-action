[![ThreatPulse](https://threatpulse.waltsoft.net/badge/CVE-2024-45257)](https://threatpulse.waltsoft.net) [![PyPI](https://img.shields.io/pypi/v/threatpulse)](https://pypi.org/project/threatpulse/)

# ThreatPulse Action

Scan your dependencies for **weaponized** vulnerabilities in CI. Powered by [threatpulse.waltsoft.net](https://threatpulse.waltsoft.net).

## Usage

```yaml
- uses: awsdataarchitect/threatpulse-action@v1
  with:
    fail-on-urgency: 80
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `fail-on-urgency` | Fail if any CVE urgency >= this value | `80` |
| `lockfile` | Path to lockfile (auto-detected) | - |
| `format` | Output: `table`, `json`, `sarif` | `table` |
| `key` | ThreatPulse payment key | - |

## What it checks

Unlike Trivy/Snyk, ThreatPulse tells you if a CVE is **actively weaponized**:
- 🔴 Metasploit module exists
- 🔴 ExploitDB entry (working exploit)
- 🟡 GitHub PoC available
- Composite urgency score (0-100)

## Full workflow with SARIF

```yaml
name: Security Scan
on: [push, pull_request]
jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: awsdataarchitect/threatpulse-action@v1
        with:
          fail-on-urgency: 80
          format: sarif
      - uses: github/codeql-action/upload-sarif@v3
        if: always()
        with:
          sarif_file: threatpulse-results.sarif
```

## Links

- CLI: `pip install threatpulse`
- API: https://threatpulse.waltsoft.net
- PyPI: https://pypi.org/project/threatpulse/
