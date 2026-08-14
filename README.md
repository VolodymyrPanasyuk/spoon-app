# 🥄 Spoon

**A fast, native Git GUI client for Windows, macOS, and Linux.**

> _"There is no Fork."_

Spoon is a rival to [Fork](https://git-fork.com/) and [GitKraken](https://www.gitkraken.com/) for people who
want the same kind of Git client on **Linux**, not just Windows and macOS. It's built with Avalonia UI on
.NET 10 (Skia rendering — no Electron, no bundled Chromium), so it's one native app rendered identically on
every OS.

This repository is Spoon's **landing page** — what it is, how to get it, how to verify what you downloaded,
and where to report problems. It does not contain any source code; Spoon's source is private.

- **Download:** [Releases](https://github.com/VolodymyrPanasyuk/spoon-app/releases)
- **Report a problem:** [open an issue in this repository](https://github.com/VolodymyrPanasyuk/spoon-app/issues)

---

## Why Linux

Most Git GUIs treat Linux as an afterthought — bolted onto an Electron shell, if it's supported at all.
Spoon is built the other way around: **Linux is a first-class citizen**, not a port. One Avalonia/Skia
codebase, one feature set, on all three desktop OSes.

## Status: beta software

[![Latest release](https://img.shields.io/github/v/release/VolodymyrPanasyuk/spoon-app?include_prereleases&label=latest%20release)](https://github.com/VolodymyrPanasyuk/spoon-app/releases)

**Spoon is in active beta.** Every release published so far is a prerelease — the badge above tracks the
current one. The core feature set is functional and gets daily use, but:

- **Nothing is code-signed or notarized yet.** See the [unsigned installers](#a-note-on-unsigned-installers)
  note below before you install — this is not a bug, it's expected for now.
- **No stable release has shipped.** Expect rough edges, and expect things to change between betas.
- **Git LFS setup is not Spoon's job.** Spoon never runs `git lfs install`/`track`/`fetch`/`push` itself, so
  set LFS up with the `git` CLI first. Once a repository is LFS-tracked, staging and committing whole files
  through Spoon is safe — git's own LFS filters run exactly as they would from your terminal. Spoon blocks
  the one operation that could corrupt a pointer file: staging, unstaging, or discarding only *part* of an
  LFS-tracked file through the per-hunk or per-line diff view. It tells you to act on the whole file instead.

If something breaks, [file an issue](https://github.com/VolodymyrPanasyuk/spoon-app/issues) — beta feedback
is exactly what this stage is for.

## A note on unsigned installers

Every installer below is currently **unsigned**:

- **Windows** will show a SmartScreen warning ("Windows protected your PC"). Click **More info**, then
  **Run anyway**.
- **macOS Gatekeeper will block the installer by default.** Right-click (or Control-click) the downloaded
  `.pkg` and choose **Open**, then confirm **Open** in the dialog that appears — a plain double-click will
  be refused. (If it's still blocked, check **System Settings → Privacy & Security** for an "Open Anyway"
  button.)
- **Linux** has no OS-level Gatekeeper/SmartScreen equivalent, but the same "unsigned" fact applies —
  verify your download (below) if that matters to you.

This is a known, deliberate gap at this stage, not an oversight — see
[Verifying your download](#verifying-your-download) for a way to confirm a release is genuinely the one
published here, independent of OS-level code signing.

## Requirements

Whether you need `git` installed first depends on which package you pick:

- **Windows `Setup.exe`** and the **Linux Flatpak** bundle their own `git` — nothing to install first.
- **Linux `.deb` / `.rpm` / `.pkg.tar.zst`** declare `git` as a package dependency, so your package manager
  pulls it in for you if it's missing.
- **Windows `.msi`**, **macOS `.pkg`**, and the **Linux AppImage** do *not* bundle `git`. For these, git
  must already be installed and on your `PATH`.

Whichever `git` Spoon ends up using, it shells out to it for every operation that changes a repository
(commit, stage, merge, push, …), so it behaves exactly like your terminal — including your existing
credential helpers and SSH setup.

## Download & install

Grab the installer for your OS from the
[Releases page](https://github.com/VolodymyrPanasyuk/spoon-app/releases) — every release published so far is
a prerelease, so pick the newest one at the top of the list.

Two things about the file names in the tables below: beta installers carry `-beta` in their names (that part
goes away when a stable release ships), and every release also publishes `*.nupkg` and `releases*.json`
files that are **not** meant for direct download — they are the feed and payload Spoon's own auto-updater
reads.

### Windows

| File | What it is |
| --- | --- |
| `Spoon-win-beta-Setup.exe` | Recommended. Installs Spoon and enables in-app auto-update. |
| `Spoon-<version>.msi` | For silent/managed deployment (Group Policy, SCCM, Intune). Does **not** auto-update — redeploy a newer `.msi` to upgrade. |

Run the installer, click through the SmartScreen warning as described [above](#a-note-on-unsigned-installers).

### macOS

| File | What it is |
| --- | --- |
| `Spoon-osx-beta-Setup.pkg` | Installs the Spoon `.app` and enables in-app auto-update. |

**Apple Silicon (M1/M2/M3…) only.** The published `.pkg` is built for `arm64`; it will not run on an
Intel Mac, and Rosetta doesn't help here (it translates x86_64 → arm64, not the other way around). Intel
Mac support isn't available yet — no date to share on that.

Right-click → **Open** to get past Gatekeeper on first launch, as described
[above](#a-note-on-unsigned-installers).

### Linux

| File | What it is | Install with |
| --- | --- | --- |
| `Spoon-linux-beta.AppImage` | Self-installing — adds Spoon to your applications menu on first run. Enables in-app auto-update. | `chmod +x`, then run it |
| `.deb` | Debian / Ubuntu package | `sudo apt install ./<file>.deb` |
| `.rpm` | Fedora / RHEL package | `sudo dnf install ./<file>.rpm` |
| `.pkg.tar.zst` | Arch package | `sudo pacman -U ./<file>.pkg.tar.zst` |
| `.flatpak` | Flatpak bundle | `flatpak install --user ./<file>.flatpak` |

Notes:
- The AppImage is **self-installing**, not a throwaway portable file: run it once and it integrates into
  your desktop (menu entry, icon, a `~/.local/bin/spoon` symlink) even after you delete the downloaded
  file. Run `./Spoon-linux-beta.AppImage --uninstall` to remove everything it installed.
- Spoon is **not yet on Flathub or the AUR** — the `.flatpak` and `.pkg.tar.zst` above are direct downloads
  for now, installed by hand rather than through those stores.

## Verifying your download

Every release publishes a `SHA256SUMS` manifest and a detached GPG signature, `SHA256SUMS.asc`, alongside
the installers. The public key is published in this repository as
[`SPOON-GPG-KEY.asc`](SPOON-GPG-KEY.asc); its fingerprint is
`451D 1B4F 3F3C 010B 6843 3C3F 5372 2859 4565 3FE0`.

```bash
# 1. Import the public key once (skip this if you've already imported it before)
gpg --import SPOON-GPG-KEY.asc

# 2. Confirm the checksum manifest itself is genuine and untampered
gpg --verify SHA256SUMS.asc SHA256SUMS

# 3. Confirm your downloaded installer(s) match the verified manifest
sha256sum --ignore-missing -c SHA256SUMS      # Linux
shasum -a 256 --ignore-missing -c SHA256SUMS  # macOS
```

`gpg --verify` warns that the key "is not certified with a trusted signature". That warning is expected and
is **not** a failure — it only means you haven't personally signed this key. What matters is the
`Good signature from "Spoon Releases"` line, and that the fingerprint `gpg` prints matches the one above.

On Windows (PowerShell), compute the hash and compare it by hand against the matching line in
`SHA256SUMS`:

```powershell
Get-FileHash .\Spoon-win*Setup.exe -Algorithm SHA256
```

This confirms supply-chain **integrity** — that the file matches what was actually published here — which
is independent of, and doesn't remove, the OS-level SmartScreen/Gatekeeper warnings described above.

## Keeping Spoon up to date

| Install method | How it updates |
| --- | --- |
| Windows `Setup.exe` | In-app: **Settings → Check for updates** |
| macOS `Setup.pkg` | In-app: **Settings → Check for updates** |
| Linux AppImage | In-app: **Settings → Check for updates** |
| Windows `.msi` | Redeploy a newer `.msi` |
| Linux `.deb` / `.rpm` / `.pkg.tar.zst` | In-app: **Settings → Check for updates** downloads the new package, checks its GPG signature, and hands you the exact `apt`/`dnf`/`pacman` command to finish the install yourself |
| Linux `.flatpak` | `flatpak update` |

For the `.deb`/`.rpm`/`.pkg.tar.zst` hand-over, Spoon needs `gpg` on your `PATH` to check the signature; if
it isn't there, Spoon still offers the download but says plainly that the signature went unchecked. The
hand-over is only offered to installs that actually came from one of those packages — otherwise Spoon falls
back to telling you to update through your package manager.

## What's inside

- A commit graph with a full commit-detail panel, per-hunk diff staging with unified/split views, and
  live refresh as your working tree changes.
- Branches, tags, and remotes as a collapsible tree, with merge, interactive rebase, cherry-pick, stash,
  and reflog recovery.
- Per-line blame and per-file history.
- Worktrees and submodules managed from the same sidebar — list, add, and remove worktrees; view,
  initialize, and update submodules, recursively if you want.
- Connect a GitHub or GitLab account — personal access token or in-browser sign-in, stored in your OS
  keychain, self-hosted instances included — to browse and clone repositories, and to list and create
  pull/merge requests.
- Dark, light, or follow-OS theming; resizable, persisted layout.

This list is intentionally short.

## Licensing

Spoon is **free to download and use**, including for commercial use — but it's **proprietary and closed
source**: no redistribution, modification, or reverse engineering without permission. See
[LICENSE](LICENSE) in this repository for the exact terms. Licensing questions can go through
[this repository's issue tracker](https://github.com/VolodymyrPanasyuk/spoon-app/issues).

## Feedback & issues

This repository's [issue tracker](https://github.com/VolodymyrPanasyuk/spoon-app/issues) is the place to
report bugs, request features, or ask questions about Spoon. Since the source isn't public, this is also
where to flag anything install-related — a bad checksum, a broken package, a crash on launch.
