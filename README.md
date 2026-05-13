# diega/nethermind_etc

Self-contained Nethermind builds that work with the
[Ethereum Classic plugin](https://github.com/ETCCooperative/nethermind-etc-plugin).

## What's in this fork

This repository holds **only the release pipeline**. It carries no
Ethereum Classic source code of its own — that lives in the plugin repo
above. The upstream Nethermind source itself is mirrored as the `master`
branch and tracks
[`NethermindEth/nethermind`](https://github.com/NethermindEth/nethermind).

A release in this fork is exactly:

  *(an upstream Nethermind tag) + (zero or more cherry-picks from upstream
  master that haven't yet shipped in that tag but are needed for ETC
  operation)*

Each release is **deterministically reproducible** from those two inputs.

## How to cut a release

1. Go to **Actions → Release Ethereum Classic → Run workflow** (button
   on the top right).
2. Fill in:
   - `upstream_tag` — the Nethermind tag to base the release on,
     e.g. `1.37.2`.
   - `cherry_picks` — space-separated SHAs from upstream master to
     apply on top, e.g. `3e8bbfce08`. Leave empty for a clean
     upstream build.
   - `version_suffix` — optional, e.g. `rc1` produces
     `v1.37.2-rc1-etc`. Leave empty for a stable release.
3. The workflow checks out `NethermindEth/nethermind` at the requested
   tag, applies the cherry-picks, builds five platforms
   (Linux x64/arm64, macOS x64/arm64, Windows x64) as self-contained
   single-file binaries, tags `v<version>-etc` at the resulting commit
   and publishes a GitHub release with all five archives attached.
4. Re-running the workflow for the same `upstream_tag` is safe — the
   tag is force-moved and the prior release is replaced. The archives
   are reproducible byte-for-byte given the same inputs (modulo
   build-time stamps).

## Branches

| Branch | Purpose |
|---|---|
| `release-tooling` *(default, this one)* | Release workflow + this README. Disjoint history. |
| `master` | Mirror of `NethermindEth/nethermind`'s `master`. Updated periodically. |

There are no per-release branches. Each released version is captured
solely by its tag (`v<x.y.z>-etc`) and the GitHub release attached to
that tag.
