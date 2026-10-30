# WSL for Mere Mortals

A real Linux, living inside Windows — installed with
one command, sharing files and programs with the
Windows side, and gone without a trace when you
delete it. This guide explains how the Windows
Subsystem for Linux actually works, how to run it
well, and where it honestly stops.

WSL suits anyone whose day straddles both systems:
developers who build on Windows and deploy to Linux,
sysadmins who need real Linux tools beside their
Windows ones, and the simply curious who want a
Linux they cannot break their computer with.

**An honest note before we start:** WSL is a Windows
feature, and this guide's lab has no Windows machine
in it. The guide was written from Microsoft's own
documentation — every command, key and error code
checked against Microsoft Learn, and every Linux-side
claim cross-checked from ordinary Linux knowledge —
rather than from hands-on runs. The *Writing
Criteria* chapter carries the full verification
ledger. Nothing here pretends otherwise.

## What this guide covers

1. [What WSL Is](what-wsl-is.md) — the idea, and the
   one thing to keep in mind: it is not a sandbox.
2. [The Moving Parts](the-moving-parts.md) — the
   `wsl.exe` tool, the distro disk file, the file
   bridge both ways, and the two config files.
3. [WSL 1 vs WSL 2](wsl1-vs-wsl2.md) — translation vs
   a real kernel, and the one job the older one
   still wins.
4. [Installing WSL](installing-wsl.md) — the one
   command, the first-launch questions, the Store
   vs inbox builds, and older Windows.
5. [Choosing and Managing
   Distributions](choosing-and-managing-distros.md) —
   adding, defaulting, converting, updating,
   removing — and the warning unregistering earns.
6. [Files Both Ways](files-both-ways.md) — `\\wsl$`
   and `/mnt/c`, the golden rule, and the truth
   about cross-boundary speed.
7. [Configuring WSL](configuring-wsl.md) —
   `wsl.conf` and `.wslconfig` key by key, and the
   eight-second rule.
8. [systemd and Services](systemd-and-services.md) —
   switching on the real service manager, and what
   it does and doesn't change.
9. [Networking](networking.md) — the default NAT,
   the localhost trick, and mirrored mode.
10. [GUI Apps with WSLg](gui-apps.md) — Linux
    windows on the Windows desktop, and what isn't
    supported.
11. [Hardware: GPU and
    USB](hardware-gpu-and-usb.md) — GPU compute with
    one golden rule, and usbipd for real devices.
12. [Backup, Moving, and
    Cloning](backup-and-moving.md) — export and
    import: the whole life-cycle in two commands.
13. [Recipes](recipes.md) — the everyday jobs,
    combined.
14. [The Neighbours](the-neighbours.md) — full VMs,
    dual boot, Docker Desktop, a second machine:
    when each beats WSL.
15. [Limits](limits.md) — not a server, not a
    desktop, disks that only grow, and the rest.
16. [Troubleshooting](troubleshooting.md) — the
    classic error codes, translated.
17. [Glossary](glossary.md) — every term, defined
    plainly.

## What it doesn't cover

Driving a full Linux desktop (a virtual machine's
job); publishing services to the internet from WSL
(a real server's job); building a custom WSL kernel
beyond pointing at the setting; and any distribution's
own internals in depth — your distro is a standard
Linux, and its own documentation takes over where
this guide stops.

---

This guide is part of the *For Mere Mortals* series — plain-language, honestly-verified guides to tools that matter.
Start at [the series index](https://paulsullivan33-dev.github.io/mere-mortals/).

Related guides:

- [Linux for Mere Mortals](https://paulsullivan33-dev.github.io/linux-for-mere-mortals/) — the operating system itself, for when the distro stops being a guest and becomes the machine.
- [Bash for Mere Mortals](https://paulsullivan33-dev.github.io/bash-for-mere-mortals/) — the shell you land in when WSL opens, and how to make it yours.
- [PowerShell for Mere Mortals](https://paulsullivan33-dev.github.io/powershell-for-mere-mortals/) — the Windows side of the terminal, where `wsl.exe` is driven from.
- [systemd for Mere Mortals](https://paulsullivan33-dev.github.io/systemd-for-mere-mortals/) — the service manager chapter 8 switches on, in full depth.
- [Docker for Mere Mortals](https://paulsullivan33-dev.github.io/docker-for-mere-mortals/) — containers, which on Windows ride on WSL 2 themselves.
- [Virtual Machines for Mere Mortals](https://paulsullivan33-dev.github.io/virtual-machines-for-mere-mortals/) — the heavier neighbour, for when WSL's limits are the ones you hit.

## License

Text: [CC BY 4.0](LICENSE). Code samples: [MIT](LICENSE-CODE).
