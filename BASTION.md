# Bastion repositories

Extra rules for `bastion` (the hub, no code) and `bastion_apple`, `bastion_linux`, `bastion_windows` and `bastion_android` (one per platform). Everything else is in [GOVERNANCE.md](GOVERNANCE.md).

- `main` in the four platform repositories also requires signed commits.
- Issues that concern one platform belong in that platform's repository (move them with Transfer issue). Issues for every platform belong in `bastion`.
- The documentation is a Zensical site: `docs/`, `zensical.toml` and `overrides/home.html`. New pages go into `nav` in `zensical.toml`. Build locally with `pip install --upgrade zensical && zensical build --strict`.
- Releases are driven by pull request titles and `.github/release-required-checks`. The platform repositories describe the process in `docs/release.md`.
- The sync format is owned by `bastion_apple`. The other platforms hold copies of its fixtures. When the format changes, follow `docs/sync-format.md`.
- Values that reach a shell go through `env:` and quoted variables.
- Change build paths and manifests together with the documentation.
- If an app cannot be built or tested in the session (Android SDK, Xcode, WinUI, NuGet blocked by the network policy), say in the pull request what was not tested. Never skip silently.
- Before working with issues or the domain, read `docs/agents/issue-tracker.md` and `docs/agents/domain.md` in the repository.
