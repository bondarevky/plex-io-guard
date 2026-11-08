# Plex IO Guard — public distribution

This repository is the public distribution surface for the native Plex IO Guard
Unraid plugin. It contains release assets and stable/testing channel pointers.
The private source repository is `bondarevky/plex-io-guard-private`, with default
branch `dev`. Source history and credentials are kept separately.

## Publication status

This is the bootstrap README. A channel is usable only after a verified public
release and its matching channel pointer have been published. The addresses below
are planned endpoints; this README alone does not establish availability.

| Channel | Native Plugin Manager URL |
| --- | --- |
| stable | `https://raw.githubusercontent.com/bondarevky/plex-io-guard/main/stable/plex-io-guard.plg` |
| testing | `https://raw.githubusercontent.com/bondarevky/plex-io-guard/main/testing/plex-io-guard.plg` |

Stable selects published ordinary releases. Testing selects the newest published
ordinary release or prerelease by source version. Draft and check builds are
excluded. Both channels use plugin identity and basename `plex-io-guard` /
`plex-io-guard.plg` and download the same immutable TXZ for a given version.

## Installation and updates

After publication is verified, use the chosen channel URL in Unraid Plugins →
Install Plugin. First installation defaults to dry-run. Configure Plex and
qBittorrent through Settings → Plex IO Guard. Keep credentials on your Unraid
host; never put them in this repository.

Unraid Plugin Manager and CA Auto Update own update checks and scheduling.
Guard has no updater scheduler. Ordinary updates/reinstalls preserve settings,
accounts, enrollment, holds and recovery journals. A same-version channel switch
requires native forced reinstall of the chosen PLG. An older version requires
the supported explicit clean transaction; `forced` alone cannot bypass downgrade
checks. Do not delete journals to force an install.

### Move an existing offline installation to a channel

Wait for a **new release** built for this public repository and a verified channel
pointer. The previous private `v2026.11.8-async-acquisition.1` bundle pins the old
source provenance and public URLs. Its immutable assets must stay unchanged; it
cannot be replayed here as the new distribution.

An installation still on the first GitHub release `v2026.11.6-ui-guard.1`
(protocol1/journal1) can move directly to the next new release
(protocol2/journal3). Installing the later private
`v2026.11.8-async-acquisition.1` first is unnecessary. Compatible old state is
preserved; unknown compatibility generations are rejected.

1. Check your installed source version in Versions / self-diagnostics. Compare it
   with this repository's `testing/channel.json` or `stable/channel.json`. The
   target must be equal or newer by SemVer. If stable is absent or older, keep
   your offline installation or use an eligible testing release.
2. On Unraid, as root, run **one** command after the selected feed is published:

   ```sh
   plugin install https://raw.githubusercontent.com/bondarevky/plex-io-guard/main/testing/plex-io-guard.plg forced
   ```

   Or, for stable:

   ```sh
   plugin install https://raw.githubusercontent.com/bondarevky/plex-io-guard/main/stable/plex-io-guard.plg forced
   ```

   Keep the basename `plex-io-guard.plg`; do not install a channel-suffixed asset
   under a different basename. `forced` also permits a same-version channel
   switch, while the updater still rejects an ordinary downgrade.
3. Wait for installation success, then check the live controller version and
   state with `/usr/local/libexec/plex-io-guard/plex-io-guard-host status`. Check
   `runtime_ready` with
   `/usr/local/libexec/plex-io-guard/plex-io-guard-host config status`. Check source and
   recovery health in the dashboard. The saved native PLG at
   `/boot/config/plugins/plex-io-guard.plg` should have the selected channel's
   `pluginURL`; Plugin Manager / CA Auto Update owns subsequent update checks.

Do not uninstall, reset state or repeat enrollment. This ordinary update preserves
config, credentials, apply/dry-run, accounts, enrollment, holds, budgets and
recovery journals. Only Guard services stop during replacement. If installation
fails, retain the target package/PLG and diagnostics and retry the same target;
do not remove journals, locks or receipts to bypass the failure.

The first update from `v2026.11.6-ui-guard.1`, whose legacy controller has no
`native-budget-stop-v1`, cannot
grant fast budget restore: it lacks the required checkpoint proof. Budgets return
through normal fresh observations and ramping. A capable controller still needs
all restore checks; service readiness and restored limits are separate facts.
Interrupted installation or reboot never creates a fast-restore grant.

## Assets and trust

Each release contains its TXZ, three PLGs, release.json, distribution.json and
SHA256SUMS. PLGs pin immutable archive URLs and hashes. Feed pointers are advanced
only after all assets can be downloaded anonymously and verified. A later older
publish does not roll back a newer channel. A checksum validates bytes; it does
not prove live acceptance on a particular Unraid host.

No GitHub credentials are needed or stored in the installed plugin. This
repository must contain no private source history, user configurations or secrets.
