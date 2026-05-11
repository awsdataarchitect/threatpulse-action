# ThreatPulse Security Scan

Scan your dependencies for **weaponized** vulnerabilities in CI/CD. Powered by [threatpulse.waltsoft.net](https://threatpulse.waltsoft.net).

Unlike Trivy/Snyk/Inspector, ThreatPulse tells you if a CVE has a working exploit (Metasploit module, GitHub PoC, or active exploitation in the wild).

## Usage

```yaml
- uses: awsdataarchitect/threatpulse-action@v1
  with:
    fail-on-urgency: 80
    key: ${{ secrets.THREATPULSE_KEY }}  # Optional: unlimited lookups
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `fail-on-urgency` | Fail if any CVE urgency score >= this value (0-100) | `80` |
| `lockfile` | Path to lockfile (auto-detected if not set) | — |
| `format` | Output format: `table`, `json`, `sarif` | `table` |
| `key` | API key for unlimited lookups (or set `THREATPULSE_KEY` secret) | — |

## Outputs

| Output | Description |
|--------|-------------|
| `vulnerabilities` | Number of vulnerabilities found |
| `max-urgency` | Highest urgency score found |
| `sarif` | SARIF output (if format=sarif) |

## Example with SARIF upload

```yaml
name: ThreatPulse Security Scan
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
          key: ${{ secrets.THREATPULSE_KEY }}
      - uses: github/codeql-action/upload-sarif@v3
        if: always()
        with:
          sarif_file: threatpulse-results.sarif
```

## Authentication

- **Free tier**: 10 lookups/day (no key needed)
- **API key**: Buy at [threatpulse.waltsoft.net](https://threatpulse.waltsoft.net), add as `THREATPULSE_KEY` secret

## Links

- [Landing page](https://threatpulse.waltsoft.net)
- [CLI (pip install threatpulse)](https://pypi.org/project/threatpulse/)
- [CLI source](https://github.com/awsdataarchitect/threatpulse-cli)
