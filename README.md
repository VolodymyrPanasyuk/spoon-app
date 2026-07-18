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

**Spoon is in active beta.** The current release is a **prerelease** (`v0.1.0-beta.3` as of this writing).
The core feature set is functional and gets daily use, but:

- **Nothing is code-signed or notarized yet.** See the [unsigned installers](#a-note-on-unsigned-installers)
  note below before you install — this is not a bug, it's expected for now.
- **No stable release has shipped.** Expect rough edges, and expect things to change between betas.
- **Git LFS, submodules, and worktrees are not supported yet.** If your repository uses Git LFS in
  particular, avoid staging or committing through Spoon for now — it can corrupt LFS pointer files, since
  Spoon doesn't yet understand them.

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

- **Git must already be installed** and on your `PATH`. Spoon doesn't bundle its own — it shells out to
  your real `git` for every operation that changes a repository (commit, stage, merge, push, …), so it
  behaves exactly like your terminal, including your existing credential helpers and SSH setup.

## Download & install

Grab the installer for your OS from the
[latest release](https://github.com/VolodymyrPanasyuk/spoon-app/releases/latest).

### Windows

| File | What it is |
| --- | --- |
| `Setup.exe` | Recommended. Installs Spoon and enables in-app auto-update. |
| `Spoon-<version>.msi` | For silent/managed deployment (Group Policy, SCCM, Intune). Does **not** auto-update — redeploy a newer `.msi` to upgrade. |

Run the installer, click through the SmartScreen warning as described [above](#a-note-on-unsigned-installers).

### macOS

| File | What it is |
| --- | --- |
| `Setup.pkg` | Installs the Spoon `.app` and enables in-app auto-update. |

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
[`SPOON-GPG-KEY.asc`](SPOON-GPG-KEY.asc).

```bash
# 1. Import the public key once (skip this if you've already imported it before)
gpg --import SPOON-GPG-KEY.asc

# 2. Confirm the checksum manifest itself is genuine and untampered
gpg --verify SHA256SUMS.asc SHA256SUMS

# 3. Confirm your downloaded installer(s) match the verified manifest
sha256sum --ignore-missing -c SHA256SUMS      # Linux
shasum -a 256 --ignore-missing -c SHA256SUMS  # macOS
```

On Windows (PowerShell), compute the hash and compare it by hand against the matching line in
`SHA256SUMS`:

```powershell
Get-FileHash .\Setup.exe -Algorithm SHA256
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
| Linux `.deb` / `.rpm` / `.pkg.tar.zst` | Your OS package manager (`apt`/`dnf`/`pacman`) |
| Linux `.flatpak` | `flatpak update` |

## What's inside

- A commit graph with a full commit-detail panel, per-hunk diff staging with unified/split views, and
  live refresh as your working tree changes.
- Branches, tags, and remotes as a collapsible tree, with merge, rebase, cherry-pick, stash, and reflog
  recovery.
- Per-line blame and per-file history.
- Connect a GitHub or GitLab account (personal access token, stored in your OS keychain) to browse and
  clone your repositories.
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
