# Glossary

**9P** — the file-sharing protocol, descended from
the Plan 9 operating system, by which Windows
browses a distro's files under `\\wsl$`.

**Distribution (distro)** — a complete Linux system
(Ubuntu, Debian, Fedora…) installed under WSL, with
its own filesystem, packages and users. You can have
several; each is independent and disposable.

**DrvFs** — the driver that mounts your Windows
drives inside Linux under `/mnt`, and the name for
the Windows filesystem as Linux sees it.

**ext4** — the standard Linux filesystem; a WSL 2
distro's entire filesystem is an ext4 volume stored
inside its `.vhdx` file.

**Inbox WSL** — WSL delivered as a built-in Windows
component, updated with Windows Update, as opposed
to the more frequently updated Microsoft Store
build. Installed with `wsl --install --inbox`.

**Interop** — the two-way ability to run programs
across the boundary: Windows programs from a Linux
shell (with their `.exe`), Linux commands from
PowerShell (via `wsl <command>`). Governed by
`wsl.conf`'s `[interop]` section.

**Kernel** — the core of an operating system. On
WSL 2 it is a real Linux kernel built and shipped by
Microsoft, shared by all distros on the machine and
updated by `wsl --update`.

**localhost forwarding** — WSL's default kindness: a
server listening inside WSL is reachable from
Windows at `localhost:<port>` too.

**Mirrored networking** — the Windows 11 (22H2+)
networking mode that mirrors Windows' interfaces
into Linux: localhost works both ways, and the LAN
can reach the distro directly. Set in `.wslconfig`.

**NAT** — network address translation. WSL 2's
default networking: the distro hides on a private
virtual network and Windows translates its traffic.

**SLAT** — second-level address translation (Intel
EPT, AMD NPT), the CPU feature WSL 2 requires and
very old processors lack.

**systemd** — Linux's standard service manager.
Optional in WSL, enabled per distro with
`systemd=true` in `wsl.conf`'s `[boot]` section; the
gate to background services and snap packages.

**usbipd-win** — the separate open-source tool that
shares a Windows USB device into a WSL 2 distro
(`usbipd bind`, `usbipd attach --wsl`).

**VHDX** — a virtual hard disk file. Each WSL 2
distro's filesystem lives in one; it grows as you
write and does not shrink on its own.

**WSL 1** — the original architecture: Linux system
calls translated to Windows ones, no kernel. Fast
across the file boundary, limited everywhere else.

**WSL 2** — the current architecture: a real Linux
kernel in a small managed virtual machine. Default
for new installs; required for systemd, GUI apps
and GPU compute.

**WSLg** — the built-in companion that displays
Linux GUI applications as windows on the Windows
desktop.

**`.wslconfig`** — the Windows-side settings file
(`%UserProfile%\.wslconfig`) governing all of WSL 2:
memory, processors, networking mode, and more.

**`wsl.conf`** — the per-distro settings file at
`/etc/wsl.conf` inside each distribution: default
user, automount behaviour, interop, systemd.
