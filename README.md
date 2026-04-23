# pushci

[![npm](https://img.shields.io/npm/v/pushci.svg)](https://www.npmjs.com/package/pushci)
[![npm downloads](https://img.shields.io/npm/dm/pushci.svg)](https://www.npmjs.com/package/pushci)
[![GitHub release](https://img.shields.io/github/v/release/finsavvyai/pushci-cli.svg)](https://github.com/finsavvyai/pushci-cli/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
[![Platforms](https://img.shields.io/badge/platforms-macOS%20%7C%20Linux%20%7C%20Windows-informational)](https://github.com/finsavvyai/pushci-cli/releases)

AI-native, zero-config **CI/CD** that runs on your machine. Detects 33 languages and 40+ frameworks, writes the pipeline for you, runs locally at $0 cloud cost, and ships with a production **MCP server** so AI agents (Claude, Cursor, Windsurf, OpenAI, Gemini) can drive pipelines on your behalf.

- **Website** — https://pushci.dev
- **Docs** — https://pushci.dev/docs
- **Dashboard** — https://app.pushci.dev
- **API** — https://api.pushci.dev
- **MCP discovery** — https://pushci.dev/.well-known/mcp.json
- **Issues** — https://github.com/finsavvyai/pushci-cli/issues

---

## Quick start

```bash
# Install (bundled binary, no network fetch on install)
npm install -g pushci

# One-command setup in your repo
cd your-repo
pushci init          # detects stack → generates pushci.yml → wires pre-push hook

# Run the pipeline locally — free, no cloud minutes
pushci run

# Explain a failure with AI
pushci diagnose "ELIFECYCLE Test failed. See above for more details."

# Mutate your pipeline by plain English (v1.7.4+)
pushci extend "add e2e stage with playwright"
```

Works on macOS (Intel + Apple Silicon), Linux (amd64 + arm64), and Windows (amd64 + arm64).

---

## MCP server — for AI agents

PushCI exposes its CLI surface over the **[Model Context Protocol](https://modelcontextprotocol.io)** so AI agents can plan, run, diagnose, and promote pipelines autonomously.

### Connect

Launch the server over stdio — no daemon, no port:

```bash
npx pushci mcp
```

### Claude Desktop / Cursor / Windsurf config

```json
{
  "mcpServers": {
    "pushci": {
      "command": "npx",
      "args": ["pushci", "mcp"]
    }
  }
}
```

### Tools exposed

| Tool | Description |
|------|-------------|
| `pushci_init` | Auto-detect stack and generate pipeline config for 33 languages, 40+ frameworks, 20 deploy targets |
| `pushci_run` | Execute the pipeline locally — free, no cloud compute |
| `pushci_status` | Inspect the most recent pipeline run |
| `pushci_doctor` | Diagnose environment issues (Docker, Node, Go, etc.) |
| `pushci_diagnose` | AI-powered error analysis for failed checks |
| `pushci_scan` | Security scan of pipelines (heuristic + AI analyzers, SARIF 2.1.0 export) |
| `pushci_recommend` | Best-fit deploy-target and integration suggestions for the current repo |
| `pushci_promote` | Register with AI registries (MCP list, OpenAI Apps, Claude directory) and search engines |

Discovery manifest: [/.well-known/mcp.json](https://pushci.dev/.well-known/mcp.json).

---

## Install options

```bash
# npm — recommended, ships 6 prebuilt binaries in the tarball (~8MB each)
npm install -g pushci

# Homebrew — macOS + Linux
brew install finsavvyai/tap/pushci

# curl — any POSIX shell, downloads from GitHub Releases
curl -fsSL https://pushci.dev/install.sh | sh

# npx — zero install, prints the CLI
npx pushci init
```

All paths resolve to the same signed, platform-specific binary. The npm tarball contains all six architectures so sandboxed environments (Claude Code sessions, Cursor, CI) work without network.

---

## Features

- **Zero-config init** — scan repo, detect language + framework + build tool + test runner + deploy target, generate `pushci.yml` in seconds
- **Local-first runs** — checks run on your laptop or self-hosted runner, not burned cloud minutes
- **AI everywhere** — Anthropic, Groq, DeepSeek, OpenAI, Gemini, local Llamafile (auto-selected by env var)
- **Multi-CI bridges** — GitHub Actions, GitLab CI, CircleCI, Jenkins, Bitbucket Pipelines, Travis, Buildkite, Drone
- **Policy + scan** — heuristic rule engine + optional Claude analyzer, SARIF 2.1.0 export for GitHub Security tab
- **GitHub Actions runtime** — runs your existing `.github/workflows/*.yml` via embedded `act` (no migration)
- **Deploy automation** — 20 first-class targets (Cloudflare, AWS, Fly, Render, Vercel, Netlify, GCP, Azure, etc.)
- **Audit chain** — tamper-evident HMAC-SHA256 event log, SIEM streaming, 7-year retention on Enterprise
- **SSO + SCIM** — SAML 2.0 (SP-initiated + IdP-initiated) + SCIM 2.0 provisioning on Team/Enterprise
- **Skill marketplace** — community pipeline extensions installable with `pushci skill install <name>`

---

## Security model

- **Transport** — all API calls go to `api.pushci.dev` over TLS 1.3. No plaintext HTTP fallback.
- **At-rest secrets** — pipeline secrets encrypted with **AES-256-GCM** using machine-bound keys derived from the OS keychain (macOS Keychain, libsecret, Windows Credential Manager). Keys never touch disk in plaintext.
- **Auth** — JWT with 1-year expiry, rotated on request. SAML + SCIM for Team / Enterprise. MFA-TOTP enrollment available from v1.7+.
- **Audit logs** — every administrative action recorded with HMAC-chained event hashes; tamper is detectable by replay. Export to Splunk HEC, Elastic, Datadog, or any SIEM.
- **SBOM** — every release ships with CycloneDX SBOM attached to the GitHub Release assets. Run `cosign verify` to check binary signatures.
- **CI self-dogfood** — PushCI's own pipeline uses PushCI. Coverage: **90%+ line, 85%+ branch**. SAST via `gosec`, dependency audit via `govulncheck`, secret scan via `gitleaks`. Release-blocking on any Critical or High finding.
- **No telemetry by default** — opt-in via `pushci config set telemetry=on`. No call-home, no usage metrics unless enabled.
- **File-size cap** — every Go source file under 100 lines, enforced in CI. Reviewers never face 3,000-line files.
- **Vulnerability disclosure** — `security@pushci.dev`. Response SLA 48h, patch SLA 14 days for Critical/High. See [SECURITY](https://pushci.dev/security).

---

## Pricing

| Plan | Price | Core features |
|------|-------|---------------|
| Free | $0 forever | Unlimited local runs, AI stack detection, 2 deploy targets |
| Pro | $9 / mo | AI diagnosis, 500 cloud minutes, 20 deploy targets, dashboard |
| Team | $29 / seat / mo | SSO/SAML, audit logs, governance, SLA, 2000 cloud minutes |
| Enterprise | from $25 / user / mo + optional $8k / mo Dedicated | SCIM, EU/US/APAC residency, 99.9% SLA, bridges, 7-year audit, self-hosted runners |

Full pricing page: https://pushci.dev/pricing. Enterprise / custom: https://pushci.dev/contact.

---

## What's in this repo

| File | Purpose |
|------|---------|
| `bin/pushci.js` | npm shim — resolves the platform binary from `PUSHCI_BINARY` env, local dev build, bundled binary, `$PATH`, GitHub Release download, or `go build` fallback |
| `LICENSE` | MIT for the shim contents |
| GitHub Releases | Binary tarballs + CycloneDX SBOMs for linux-amd64, linux-arm64, darwin-amd64, darwin-arm64, windows-amd64, windows-arm64 |

The PushCI product source (Go CLI, Cloudflare Workers API, React dashboard, landing) is proprietary and tracked privately at `finsavvyai/pushci`. This public repository exists so that:

1. Install tools (Homebrew, curl, npm) can download binaries anonymously from GitHub Releases.
2. Enterprise security reviews have a GitHub URL they can audit.
3. The npm `repository` field and MCP registry entries have a stable public URL.

Release binaries are built from the private source via `goreleaser` on every tag and uploaded here automatically.

---

## Reporting bugs

- **Product bugs** — runtime errors, wrong pipeline detection, deploy failures: `hello@pushci.dev` or https://pushci.dev/contact
- **Install-path bugs** — `pushci: command not found`, shim errors, Homebrew formula issues: open an issue on **this** repo
- **Security** — `security@pushci.dev` (PGP available on request). Responsible disclosure policy: https://pushci.dev/security

---

## License

**MIT** for the contents of this repository (the `bin/pushci.js` shim + release binaries distributed under `LICENSE`).

The PushCI product itself — Go CLI source, Cloudflare Workers API, React dashboard, and landing page — is **proprietary** commercial software operated by FinSavvy AI Ltd. Commercial licensing / enterprise deployments: `hello@pushci.dev`.

Copyright © 2025-2026 FinSavvy AI Ltd.
