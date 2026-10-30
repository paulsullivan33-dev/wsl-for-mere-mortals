# Configuring WSL

Two text files hold all of WSL's settings, and this
chapter walks both of them: `/etc/wsl.conf` inside
each distro, and `.wslconfig` on the Windows side.
It ends with the eight-second rule that explains why
your edits "didn't work" — the classic beginner trap.

## Which file does what

| File | Where it lives | Governs |
| --- | --- | --- |
| `wsl.conf` | `/etc/wsl.conf` *inside* a distro | That one distro |
| `.wslconfig` | `%UserProfile%\.wslconfig` in Windows | All of WSL 2, globally |

A distro setting goes in the first; a machine-wide
limit goes in the second. Putting a key in the wrong
file is harmless — it is simply ignored — which is
precisely what makes it confusing.

## The eight-second rule

Configuration is read when WSL **starts**, not while
it runs. After editing either file you must stop the
subsystem completely and wait for it to really stop:
Microsoft's rule is to close every distribution's
shells and then wait about **eight seconds** for the
virtual machine to shut down on its own. `wsl --list
--running` tells you whether anything is still up.
The impatient, reliable method is `wsl --shutdown`
from PowerShell, then start your distro again.

Nearly every "the config file does nothing" report
is this rule being skipped.

## `/etc/wsl.conf` — per-distro settings

The file is divided into named sections. A typical
one:

```ini
[boot]
systemd=true

[user]
default=alex

[interop]
appendWindowsPath=false
```

The sections that matter most:

- **`[boot]`** — `systemd=true` switches on systemd
  (chapter 8). `command=` names a command to run, as
  root, every time the distro starts — the traditional
  way to launch things at boot on systems without
  systemd.
- **`[user]`** — `default=` sets which Linux user you
  land as. Defaults to the account created at first
  launch.
- **`[automount]`** — how Windows drives appear under
  `/mnt`. `enabled=true` by default; `root=` moves
  them (say, to `/` — no, keep `/mnt/`); the
  `options=` key can add `metadata`, which lets Linux
  permissions (`chmod`) be stored on Windows files —
  off by default, because DrvFs otherwise reports
  Windows files as fully writable and permission-less.
- **`[network]`** — `hostname=` names the distro on
  the network (default: your Windows machine's name);
  `generateHosts` and `generateResolvConf` control
  whether WSL writes `/etc/hosts` and
  `/etc/resolv.conf` for you. Set either to `false`
  when you want to manage those files by hand — and
  know that until you do, edits to them are silently
  overwritten on the next start.
- **`[interop]`** — the door from chapter 6, in
  settings form: `enabled=false` stops Windows
  programs running from Linux shells entirely;
  `appendWindowsPath=false` keeps the Windows `PATH`
  out of your Linux `PATH`.

After editing, the eight-second rule applies, then
`systemctl list-unit-files --type=service` — run
inside the distro — confirms systemd really is on,
if that was your change.

## `.wslconfig` — the whole-machine settings

This file exists only if you create it, in your
Windows user profile (the folder `%UserProfile%`
stands for, usually `C:\Users\YourName`). It applies
to **WSL 2 only**, and its main section is `[wsl2]`:

```ini
[wsl2]
memory=8GB
processors=4
networkingMode=mirrored
```

The keys you are most likely to meet:

- **`memory`** — the most the WSL 2 VM may use.
  Default: **50% of your Windows memory**. On a
  16 GB machine WSL can quietly take 8 GB; this key
  is how you fence it in (or, rarely, let it feast).
- **`processors`** — how many logical processors the
  VM may use. Default: all of them.
- **`swap`** and **`swapFile`** — a swap file for the
  VM, defaulting to 25% of your RAM on your temp
  drive.
- **`localhostForwarding`** — whether `localhost` on
  Windows reaches servers inside WSL (chapter 9).
  Default `true`; rarely touched.
- **`networkingMode`** — `nat` by default, `mirrored`
  on newer Windows (chapter 9), plus `none` for a
  distro with no network at all.
- **`guiApplications`** — `false` turns WSLg off.
- **`defaultVhdSize`** — the growth ceiling for *new*
  distro disks; 1 TB unless you say otherwise.
- **`kernel`** — a path to your own Linux kernel
  build, for people who need one. Note the format:
  paths here need **escaped backslashes** —
  `C:\\Users\\alex\\mykernel` — because the file
  parser treats a single backslash as an escape
  character.

A gentler editor exists: the **WSL Settings** app in
the Start menu adjusts the main `.wslconfig` values
with sliders and switches, and Microsoft recommends
it over hand-editing. The file still wins for
scripting, sharing settings between machines, and
the long tail of keys the app does not show — an
`[experimental]` section holds opt-in switches such
as `sparseVhd` (letting distro disks hand space back
automatically) and `autoMemoryReclaim`.

One mercy, in closing: if `.wslconfig` is missing or
malformed, WSL doesn't refuse to start. It ignores
the file and launches with defaults — so a typo
costs you a setting, not a working system. It also
means a typo can hide silently; if a change has no
effect even after a proper shutdown, suspect your
syntax before you suspect WSL.
