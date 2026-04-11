# pushci-cli

Public release distribution channel for [**PushCI**](https://pushci.dev) — AI-native, zero-config CI/CD that runs on your machine.

> [!NOTE]
> **This repository holds the CLI shim and release binaries only.** The PushCI product itself — Go CLI, Cloudflare Workers API, dashboard, landing page — is proprietary commercial software operated by FinSavvy AI Ltd.
>
> For commercial licensing, enterprise deployments, or source access: **hello@pushci.dev**

## Install

```bash
# npm (recommended — bundled binaries, no network fetch)
npm install -g pushci

# Homebrew (macOS + Linux)
brew install finsavvyai/tap/pushci

# curl installer (any POSIX shell)
curl -fsSL https://pushci.dev/install.sh | sh

# npx (no install)
npx pushci init
```

## What's in this repo

| File | Purpose |
|---|---|
| `bin/pushci.js` | The npm shim — resolves the platform-specific Go binary from `PUSHCI_BINARY`, a local dev build, a bundled binary in the npm tarball, `$PATH`, a GitHub Release download, or a `go build` fallback |
| `LICENSE` | MIT for the shim contents. See below. |
| Release assets | Binary tarballs + zip files for the 6 supported platforms (linux/amd64, linux/arm64, darwin/amd64, darwin/arm64, windows/amd64, windows/arm64), published to GitHub Releases here |

## Why is source not here?

The PushCI Go CLI, Cloudflare Workers API, React dashboard, and landing page are proprietary commercial software. This public repository exists so that:

1. **Install tools work without authentication.** Homebrew, `curl | sh`, and the npm shim's release-fallback path can all download binaries from this public repo's releases.
2. **Enterprise security reviews have a GitHub URL to point at.** Your security team can audit the shim (`bin/pushci.js` is 9KB of plain JavaScript) and verify the release binary signatures.
3. **npm has a source-of-truth URL.** `package.json`'s `repository` field points here.

The actual product source is tracked internally at `finsavvyai/pushci` (private). Release binaries are built from that source via `goreleaser` and uploaded to this repo's Releases on every tag.

## Reporting bugs

- **Product bugs** (CLI behavior, AI integration, deploy logic): open an issue at [pushci.dev/issues](https://pushci.dev/issues) or email hello@pushci.dev
- **Install-path bugs** (`pushci: command not found`, `brew install` broken, shim errors): open an issue on **this** repo

## License

MIT for the contents of this repository (the shim + release binaries). The PushCI product itself is not MIT-licensed — see [LICENSE](./LICENSE) for the full text.

Copyright © 2025-2026 FinSavvy AI Ltd.
