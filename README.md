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

## Install

The Mac app, the terminal command for macOS and Linux, and the Docker image for Linux CI are on the
[latest release](https://github.com/GeometerLabs/geometer/releases/latest). How to install each, what your
computer needs, how to check a download, and how to upgrade, roll back or uninstall are on one page:
**[geometer.dev/docs/install](https://geometer.dev/docs/install)**. CI is covered in
[Run kept checks in CI](https://geometer.dev/docs/run-in-ci).

## First steps

- **App:** choose your project's folder and press **Check this project**. Each screen says
  what Geometer is doing and what it found; Help on the right explains every step.
- **Terminal:** `geo open` opens the same workspace in your browser. `geo status` prints
  what each action of your agent may do now and the next step. `geo ci` runs your kept
  checks. `geo --help` lists every command, grouped.

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
