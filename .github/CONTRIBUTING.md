# Contributing to spotatui-librespot

This fork is part of [spotatui](https://github.com/LargeModGames/spotatui): a merged PR here counts as a spotatui contribution. spotatui's [CONTRIBUTING.md](https://github.com/LargeModGames/spotatui/blob/main/CONTRIBUTING.md) applies here too, its "Using AI Tools" rules included.

## Before you start

- Report what you see in spotatui on the [spotatui tracker](https://github.com/LargeModGames/spotatui/issues/new/choose). Open an issue here only when you know the bug is in the library code.
- Open your PR against the `spotatui` branch. It is the default branch and the one we publish from.
- The minimum Rust version is the `rust-version` in the root `Cargo.toml`.

## Checks

CI checks the formatting on Linux, and runs clippy and the unit tests on Linux, Windows and macOS, each with the playback backend spotatui uses there (`alsa-backend`, `rodio-backend`, `portaudio-backend`). On Linux:

```bash
P="-p spotatui-librespot-core -p spotatui-librespot-connect -p spotatui-librespot-oauth -p spotatui-librespot-metadata -p spotatui-librespot-protocol -p spotatui-librespot-audio -p spotatui-librespot-playback"
cargo fmt --all -- --check
cargo clippy --locked $P --features spotatui-librespot-playback/alsa-backend -- -D warnings
cargo test --locked $P --features spotatui-librespot-playback/alsa-backend --lib
```

Only the unit tests run: the integration tests, doc tests and examples still import the upstream crate names and do not compile after the rename.

## Changelog

Add one line under `## [Unreleased]` in `CHANGELOG.md`:

- start it with the crate in brackets, for example `[connect]`;
- end it with `Fork-only.` for a change that exists only here, or name the upstream PR it ports;
- add `(breaking)` when code that uses the crate must change.

## Test your change inside spotatui

1. Clone spotatui next to this repository, so the two folders sit side by side.
2. Check that the `version` of the fork crates here is the same as the `=` pins in spotatui's `Cargo.toml`. If it is not, the patch below is ignored with a warning.
3. Add this to the end of spotatui's `Cargo.toml`:

   ```toml
   [patch.crates-io]
   spotatui-librespot-audio = { path = "../spotatui-librespot/audio" }
   spotatui-librespot-connect = { path = "../spotatui-librespot/connect" }
   spotatui-librespot-core = { path = "../spotatui-librespot/core" }
   spotatui-librespot-metadata = { path = "../spotatui-librespot/metadata" }
   spotatui-librespot-oauth = { path = "../spotatui-librespot/oauth" }
   spotatui-librespot-playback = { path = "../spotatui-librespot/playback" }
   spotatui-librespot-protocol = { path = "../spotatui-librespot/protocol" }
   ```

4. Run the full build, `cargo run` in spotatui. The slim build has no librespot at all. To test the desktop app, add the same block to spotatui's `desktop/Cargo.toml` with `../../spotatui-librespot/...` paths; it has its own lock.
5. Never commit the patch or the `Cargo.lock` it changes. spotatui's CI builds with `--locked` against crates.io.

## Upstream

A fix that is not specific to spotatui is worth offering to [librespot](https://github.com/librespot-org/librespot) too. Say in your PR if you did.

## Releases

Maintainers publish the seven crates with one new version, then update the pins in spotatui in a separate PR. [SPOTATUI_FORK.md](../SPOTATUI_FORK.md) has the steps.

## Credit

After the merge, comment on [spotatui#872](https://github.com/LargeModGames/spotatui/issues/872) with a link to your PR here:

```text
@all-contributors please add @your-name for code
```
