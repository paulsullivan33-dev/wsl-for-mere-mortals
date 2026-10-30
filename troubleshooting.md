# Troubleshooting

The classic failures, in the order you are likely to
meet them. Error codes in WSL are unusually honest —
they name the broken piece — so this chapter is
mostly a phrasebook for reading them.

## First moves, whatever the symptom

```
wsl --status        :: what WSL thinks the world looks like
wsl -l -v           :: every distro, its state, its version
wsl --shutdown      :: stop everything; fixes a surprising amount
```

A full shutdown and a fresh start clears stuck
distros, stale networking and ignored config changes
(chapter 7's eight-second rule) in one move. If a
*config change* seems ignored, this is not a first
move but the actual fix — WSL reads its files at
start, only at start.

## "WSL is not installed" / help text everywhere

`wsl --install` printing help, or `wsl` itself
unrecognised, means the feature isn't on yet. Run
chapter 4's command in an **administrator** shell —
a non-elevated one cannot enable Windows components.
If you *just* installed, restart Windows: the
components and the kernel finish settling across a
reboot.

## Error 0x8007019e — the subsystem is not enabled

*The Windows Subsystem for Linux optional component
is not enabled.* Fix it either in *Control Panel →
Programs → Turn Windows features on or off* (tick
**Windows Subsystem for Linux**) or in an elevated
PowerShell:

```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

## Error 0x800701bc — the WSL 2 kernel needs updating

Your WSL 2 kernel component is missing or stale.
`wsl --update` is the modern fix; Microsoft's
documentation also offers the standalone kernel
update package (the "WSL2 Linux kernel update
package" on the install page) for machines where the
command itself is too old to know it.

## Error 0x80370102 — virtualization is not available

The machine could not start the virtual machine WSL
2 lives in. Three layers to check, in order:

1. **The BIOS/UEFI switch.** Intel VT-x or AMD-V
   (often labelled SVM) must be *enabled* in the
   firmware setup. Task Manager's Performance tab
   shows "Virtualization: Enabled" when it is.
2. **The CPU itself.** WSL 2 needs **SLAT** —
   second-level address translation. Any processor
   from roughly 2009 onward has it; Core 2 Duo-era
   hardware genuinely cannot run WSL 2 (WSL 1 remains
   available there).
3. **The Virtual Machine Platform** feature must be
   on (*Turn Windows features on or off*).

Trying WSL *inside* a Windows virtual machine adds a
fourth: the outer hypervisor must expose nested
virtualization — on Hyper-V, an elevated
`Set-VMProcessor -VMName <name>
-ExposeVirtualizationExtensions $true` against the
stopped VM.

## Error 0x80070003 — the path is wrong

Seen at install time when the default install
location points at a drive or folder that isn't
there (classically, after changing Windows' "new
apps" drive to a removed disk). Set the app-install
drive back to a real one in *Settings → System →
Storage*, or install the distro naming its home
explicitly with chapter 4's `--location`.

## The virtual-disk limitation error

*"The requested operation could not be completed due
to a virtual disk system limitation. Virtual hard
disk files must be uncompressed and unencrypted and
must not be sparse."* — your distro's `.vhdx` has
been compressed, encrypted or made sparse by some
well-meaning disk cleanup or backup tool. Undo it:
in Explorer, the file's *Properties → Advanced* —
untick *Compress contents* and *Encrypt contents* —
or move the distro (chapter 12) onto plain storage.

## Install hangs at 0.0%

The Store path is stuck. Cancel and use the web
route:

```
wsl --install --web-download -d Ubuntu
```

The same switch revives a stuck `wsl --update`.

## GUI apps refuse to appear

Distro is WSL 1 (convert it); WSL is stale
(`wsl --update`, then `wsl --shutdown`); or a GUI
app complains about the display because something
exported a hand-set `DISPLAY` — clear it and let
WSLg do its job (chapter 10).

## localhost stopped working

A Windows-side change, a VPN, or a wedged subsystem
has broken chapter 9's forwarding. Restart the
subsystem (`wsl --shutdown`) before debugging
further; check `localhostForwarding` wasn't set to
`false` in `.wslconfig`; and if you recently
switched networking modes, verify the edit took —
the eight-second rule again.

## The clock is wrong inside Linux

After the host sleeps, the distro's clock may wake
up believing no time has passed. From inside:

```
sudo hwclock -s
```

resyncs Linux's clock from the hardware clock. If it
drifts constantly, that's the shape of the platform
(chapter 15), not a fault in your distro.

## When none of this fits

Microsoft maintains the fuller troubleshooting page
this chapter is distilled from, and the WSL project's
GitHub issues are searchable by exact error code —
paste the whole `0x…` string; someone has met it.
Before asking anywhere, have `wsl --version` and
`wsl -l -v` output ready; every helper will ask.
