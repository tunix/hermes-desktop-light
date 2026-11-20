# hermes-desktop-light

Build repository for the **Hermes Light** Linux desktop AppImage — the
remote-only desktop client of [Hermes Agent](https://github.com/NousResearch/hermes-agent)
(Electron shell + chat UI, no bundled agent runtime; it connects to a Hermes
backend running on another machine).

Upstream publishes no Linux desktop packages yet
([NousResearch/hermes-agent#98171](https://github.com/NousResearch/hermes-agent/issues/98171)),
so this repository builds upstream's own bundling pipeline and publishes the
result as a `hermes-desktop-v<version>` release.

## How it works

1. **Bump** — [`bump-upstream`](.github/workflows/bump-upstream.yml) runs twice
   daily (or on demand) and compares [`version.txt`](version.txt) against
   upstream's newest stable tag. When a new tag exists it opens one PR bumping
   `version.txt`; if the PR isn't merged yet and a newer tag lands, the same
   PR is force-updated in place — no new PR per tag.
2. **Build & release** — merging the bump PR (or editing `version.txt` on
   `main`) triggers [`hermes-desktop-releases`](.github/workflows/hermes-desktop-releases.yml),
   which builds the Hermes Light AppImage from a clean checkout of that tag
   with upstream's `python -m scripts.bundles.desktop --tag=vX.Y.Z --variant=light`
   and publishes it as `hermes-desktop-v<version>`. Manual rebuilds: run the
   workflow with a `tag` input.

Each release contains:

- `HermesLight-<version>-linux-x86_64.AppImage` — the desktop AppImage
- `SHA256SUMS` — its sha256

This is the artifact source pinned by the FlatPark package
[`com.nousresearch.hermes-light`](https://github.com/flatpark/flatpark/tree/main/registry/com.nousresearch.hermes-light).
If upstream starts publishing Linux desktop packages, consumers move there and
this repository is retired.
