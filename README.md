# nix-deadnix-bin

Prebuilt [deadnix](https://github.com/astro/deadnix) binaries for unused nix code detection.

## Install via mise

```toml
[tools]
"github:OlechowskiMichal/nix-deadnix-bin" = "latest"
```

## Platforms

| Target | Runner |
|--------|--------|
| `aarch64-apple-darwin` | macOS 14 (Apple Silicon) |
| `x86_64-apple-darwin` | macOS 15 Intel |
| `x86_64-unknown-linux-gnu` | Ubuntu latest |
| `aarch64-unknown-linux-gnu` | Ubuntu latest (cross) |

## Updating

Run the **Build and Release** workflow with the upstream version (e.g. `1.2.1`).

The **Check Upstream Release** workflow runs weekly and creates an issue when a new version is available.

## Development

Prerequisites: [mise](https://mise.jdx.dev/)

```bash
# Install tools and git hooks
task setup

# Run all linters (actionlint + yamllint + tool.json validation)
task lint
```
