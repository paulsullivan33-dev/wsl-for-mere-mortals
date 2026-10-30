# Lab notes — sources and cross-checks

This guide's lab had **no Windows machine**. WSL is a Windows
feature, so nothing in this guide was run against the product
itself. Everything taught here was checked against Microsoft's
own documentation on Microsoft Learn, fetched and read on
2026-10-30, and cross-checked from ordinary Linux knowledge
where the claim is really a Linux claim (systemd's behaviour,
what `/etc/resolv.conf` does, what an ext4 filesystem is).

## Primary sources (Microsoft Learn, read 2026-10-30)

- Install WSL — https://learn.microsoft.com/en-us/windows/wsl/install
  Prerequisites (Windows 10 version 2004 / build 19041 or
  Windows 11), `wsl --install` in an administrator PowerShell,
  what the command does (enables the WSL and Virtual Machine
  Platform components, downloads the kernel, sets WSL 2 as the
  default, installs Ubuntu), the 0.0% hang and the
  `--web-download` remedy.
- Basic commands for WSL — https://learn.microsoft.com/en-us/windows/wsl/basic-commands
  `--list --online`, `--list --verbose`, `--set-version`,
  `--set-default-version`, `--set-default`, `--status`,
  `--version`, `--update` (and `--web-download`), `--shutdown`,
  `--terminate`, `--export` (tar, `--vhd`), `--import`
  (`--vhd`, `--version`), `--import-in-place`, `--unregister`
  and its data-loss warning, `--mount` / `--unmount`,
  install options `--no-launch`, `--location`,
  `--no-distribution`, `--inbox`, `--enable-wsl1`.
- Advanced settings configuration in WSL —
  https://learn.microsoft.com/en-us/windows/wsl/wsl-config
  Read in full. The 8-second rule; `wsl.conf` sections
  (`[automount]`, `[network]`, `[interop]`, `[user]`,
  `[boot]`, `[gpu]`, `[time]`) and keys; the `[boot] systemd`
  switch and its confirmation command; `.wslconfig` `[wsl2]`
  keys and defaults — `memory` 50% of total, `processors` all,
  `swap` 25% of RAM, `localhostForwarding` true,
  `networkingMode` NAT (values `none`, `nat`, `mirrored`,
  deprecated `bridged`, `consomme`), `dnsTunneling` true,
  `firewall` true, `autoProxy` true, `defaultVhdSize` 1 TB;
  `[general]` and `[experimental]` including `sparseVhd`;
  the note that a missing or malformed `.wslconfig` is ignored
  and WSL still launches; the WSL Settings app recommendation.
- Accessing network applications with WSL —
  https://learn.microsoft.com/en-us/windows/wsl/networking
  NAT is the default architecture; mirrored mode on Windows 11
  22H2+ mirrors Windows interfaces into Linux; in mirrored
  mode host and distro reach each other at `localhost`
  (127.0.0.1); mirrored benefits include IPv6, VPN
  compatibility and reaching WSL services directly from the
  LAN; `dnsTunneling` on by default on 22H2+.
- Working across file systems —
  https://learn.microsoft.com/en-us/windows/wsl/filesystems
  Keep project files on the side whose tools you are using;
  Linux files under `/home/<user>/Project`, not under
  `/mnt/c/...`; browse Linux files from Windows at `\\wsl$`;
  run Linux tools from PowerShell with `wsl <command>`;
  case sensitivity differs between the two systems.
- Run Linux GUI apps with WSL —
  https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps
  Windows 10 build 19044+ or Windows 11; fresh installs get
  WSLg with `wsl --install`; existing installs use
  `wsl --update`; GUI apps are WSL 2 only; support is for
  individual apps, not a full desktop.
- Enable NVIDIA CUDA on WSL 2 / GPU accelerated ML training —
  https://learn.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute
  and the CUDA-on-WSL page: install the current Windows driver
  for the GPU on the Windows side; the only driver needed;
  glibc-based distro; check the card from inside with
  `nvidia-smi`; DirectML routes exist for AMD and Intel cards.
  NVIDIA's own CUDA-on-WSL guide adds, in bold, "Do not
  install any Linux display driver in WSL."
- Connect USB devices —
  https://learn.microsoft.com/en-us/windows/wsl/connect-usb
  usbipd-win is an add-on project, not part of WSL; Windows 11
  build 22000+ (Windows 10 needs a custom kernel route);
  x64 or ARM64 only; distro must be WSL 2; `usbipd list`,
  `usbipd bind --busid` (administrator, once), `usbipd attach
  --wsl --busid` (no elevation needed, keep a WSL terminal
  open), `lsusb` to confirm, `usbipd detach`; while attached
  the device cannot be used by Windows.
- Troubleshooting Windows Subsystem for Linux —
  https://learn.microsoft.com/en-us/windows/wsl/troubleshooting
  0x8007019e (the WSL optional component is not enabled, and
  the fix), 0x80070003 and 0x80370102 (virtualization in the
  BIOS, SLAT requirement), 0x80370102 again (enable Virtual
  Machine Platform; on a Windows VM, nested virtualization via
  `Set-VMProcessor -ExposeVirtualizationExtensions $true`),
  the virtual-disk-system limitation error (VHD files must be
  uncompressed, unencrypted and not sparse). Error 0x800701bc
  (WSL 2 kernel component needs its update) is from the same
  documentation set's long-standing kernel-update page.
- Comparing WSL 1 and WSL 2 (Microsoft Learn, via Canonical's
  summary of the same table at
  https://documentation.ubuntu.com/wsl/stable/explanation/compare-wsl-versions/):
  WSL 1 translates system calls and has partial compatibility;
  WSL 2 runs a full Linux kernel in a managed, lightweight VM
  with full system call compatibility; WSL 1 is faster across
  the OS file boundary, WSL 2 faster inside Linux; systemd,
  GUI apps and snap support are WSL 2 only.

## Cross-checks done from this Linux machine

Recorded in `cross-checks.txt`: this sandbox is an ordinary
systemd Linux, and the systemd behaviours the guide describes
(`systemctl list-unit-files --type=service`, services starting
at boot, `systemctl status`) are the stock behaviours of the
tool the reader will meet inside WSL — WSL's systemd is the
distro's own, switched on, not a Microsoft imitation. The
roles of `/etc/resolv.conf`, `/etc/hosts` and `/etc/fstab`
inside WSL are the roles they have on any Linux, which is
why WSL generating them surprises people, and why the guide
can explain the surprise from first principles.

## What was NOT verified

Anything that needs a Windows host: every `wsl.exe` command's
exact output text, install timings, the WSL Settings app's
screens, GUI app behaviour, GPU throughput, USB attach
success, mirrored-mode behaviour on a real LAN, and every
error message's exact wording beyond what the troubleshooting
page quotes. The guide presents commands as Microsoft's
documentation gives them and never claims the lab ran them.
