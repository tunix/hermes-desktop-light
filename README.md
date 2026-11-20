# hermes-desktop-light

Build repository for the **Hermes Light** Linux desktop AppImage — the
remote-only desktop client of [Hermes Agent](https://github.com/NousResearch/hermes-agent)
(Electron shell + chat UI, no bundled agent runtime; it connects to a Hermes
backend running on another machine).

Upstream publishes no Linux desktop packages yet
([NousResearch/hermes-agent#98171](https://github.com/NousResearch/hermes-agent/issues/98171)),
so this repository builds upstream's own bundling pipeline at each stable
release tag and publishes the result as a `hermes-desktop-v<version>` release.

Each release contains:

- `HermesLight-<version>-linux-x86_64.AppImage` — the desktop AppImage
- `SHA256SUMS` — its sha256

Built from a clean checkout of the tag with upstream's
`python -m scripts.bundles.desktop --tag=vX.Y.Z --variant=light`; the recipe
lives in [`.github/workflows/hermes-desktop-releases.yml`](.github/workflows/hermes-desktop-releases.yml),
which catches up on new upstream stable tags twice daily (or via
`workflow_dispatch` with an explicit `tag`).

This is the artifact source pinned by the FlatPark package
[`com.nousresearch.hermes-light`](https://github.com/flatpark/flatpark/tree/main/registry/com.nousresearch.hermes-light).
If upstream starts publishing Linux desktop packages, consumers move there and
this repository is retired.
