# Geometer

**Earn your agents the right to act.** Geometer runs your AI agent against things going
wrong on purpose (a lost reply, a request cut short, a service that crashes), finds where
it breaks your business rules, replays the problem exactly, checks your fix, and keeps the
check so CI runs it from then on. The evidence it gathers is what lets a person grant the
agent more authority, one action at a time.

This repository holds Geometer's **downloads, release notes and issue tracker**. The
product's source is not public. The [verifier and the evidence and contract schemas](https://github.com/GeometerLabs/geometer-verify)
are open source under Apache-2.0, so anyone can check Geometer's records without trusting us.

> **This is an alpha.** It works on our sample projects and has not yet been used by people
> outside the team. Expect rough edges, and please [tell us what goes wrong](#feedback).

## What it does on your computer

- It reads your project and runs your agent in **Docker containers on your computer**.
  Payment, email and other outside services are simulated, so nothing real is charged or sent.
- It changes your project's files only when you ask it to **keep a check**: it then writes
  that check under `.geometer/regressions/` so your CI can run it.
- Its records live in `~/.geometer-host` (set `GEO_HOME` to move them).
- It sends nothing about you or your project anywhere. **There is no telemetry and no account.**
  The network is used to pull container images, and to call a model only if you choose one.
- "Under the hood" in the app shows its processes, containers, CPU (in cores) and the
  network activity it can see, for anyone who wants to check.

## Requirements

| | macOS | Linux |
| --- | --- | --- |
| Processor | Apple silicon (M1 or later). Intel Macs are not supported in the alpha. | x86_64 or arm64 |
| System | Tested on macOS 26. Built for macOS 12 and later, which we have not tested. | Any recent distribution; the binaries are static |
| Containers | Docker Desktop or Colima. The app offers to set up Colima with Homebrew if neither is there. | Docker Engine usable by your user, with the compose and buildx plugins (`docker compose version` and `docker buildx version` should both answer) |
| Disk | About 2 GB free for container images and records | Same |

## Install

### macOS: the app

1. Download `Geometer-<version>-macos-arm64.dmg` from the [latest release](https://github.com/GeometerLabs/geometer/releases/latest).
2. Open the disk image and drag **Geometer** to **Applications**.
3. Open Geometer. The alpha is **not signed by Apple yet**, so macOS refuses to open it the
   first time. To allow it:
   - open **System Settings → Privacy & Security**, scroll to the message about Geometer,
     click **Open Anyway** and confirm; or
   - in Terminal: `xattr -dr com.apple.quarantine /Applications/Geometer.app`.

   You only do this once per version. A signed app will follow once Geometer's company is
   registered with Apple.

### macOS or Linux: the terminal

```sh
curl -fsSL https://geometer.dev/install.sh | sh
```

The script picks the build for your computer, checks its SHA-256 against the release's
`SHA256SUMS`, and installs `geo` for your user under `~/.local` (no administrator rights).
To read it first: `curl -fsSL https://geometer.dev/install.sh -o install.sh`.

Or by hand: download `geo-<version>-<os>-<arch>.tar.gz` and `SHA256SUMS` from the
release, check it (`shasum -a 256 -c SHA256SUMS --ignore-missing` on macOS,
`sha256sum -c SHA256SUMS --ignore-missing` on Linux), unpack it and run `./install.sh`.

### Linux CI: the Docker image

```sh
docker run --rm --network host \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v "$PWD:$PWD" -w "$PWD" \
  ghcr.io/geometerlabs/geo:<version> ci
```

The image runs `geo` and uses the host's Docker for its sandboxes; it starts no Docker of
its own. Mount the project at the same path (`-v "$PWD:$PWD"`). Host networking is
recommended (it lets Geometer reach a model on the host, such as a local Ollama) but not
required. Exit codes are the same as the installed `geo ci`: 0 no problem, 1 a kept check
broke, 2 missing evidence, 3 infrastructure failure. The image runs as root, so a file it
writes into your project (for example a kept check) belongs to root on the host; each run
starts with empty records.

## First steps

- **App:** choose your project's folder and press **Check this project**. Each screen says
  what Geometer is doing and what it found; Help on the right explains every step.
- **Terminal:** `geo open` opens the same workspace in your browser. `geo status` prints
  what each action of your agent may do now and the next step. `geo ci` runs your kept
  checks. `geo --help` lists every command, grouped.

## Upgrade, roll back, uninstall

- Upgrade: run the installer again. The previous version stays installed and your records
  are backed up before the new version opens them.
- Roll back: `./install.sh --rollback` from the unpacked release.
- Uninstall: `~/.local/lib/geometer/current/uninstall.sh` removes the program and the containers,
  networks and images Geometer created. Your records stay unless you add `--purge-records`.
  For the app, also drag Geometer from Applications to the Bin.

## Feedback

Please [open an issue](https://github.com/GeometerLabs/geometer/issues/new/choose): a bug,
an install problem, a project where Geometer missed something, or a question. Say which
version, platform and project you used. Don't paste secrets, customer data or private code
you can't share; describe it instead.

Security problems: please report them privately through
[GitHub's private vulnerability reporting](https://github.com/GeometerLabs/geometer/security/advisories/new).

## Terms

Geometer's binaries are free to use under the [alpha terms](TERMS.md). Each download
includes the licences and notices of the open-source components it contains.
