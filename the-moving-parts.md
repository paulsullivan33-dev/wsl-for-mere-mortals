# The Moving Parts

This chapter names the pieces, so that when a later
chapter says "restart the subsystem" or "the distro's
disk file", you know what is being talked about. WSL
has more pieces than its one-command install suggests.

## `wsl.exe` — the one tool

Nearly everything is driven by a single Windows
program, **`wsl.exe`**, typed as `wsl` in PowerShell
or a Command Prompt. It installs distributions,
starts and stops them, converts them between
versions, exports and imports them, and updates WSL
itself. Run from *inside* a distribution you still
call it `wsl.exe` — the `.exe` is how Linux knows you
mean the Windows program, a detail chapter 6 returns
to.

If you learn one habit, make it this: `wsl --help`
lists every command your installed version supports,
and `wsl --version` reports the version itself.
Features in this guide occasionally depend on having
a recent version, and those two commands are how you
check.

## The two shapes of WSL

Underneath, a distribution runs in one of two ways,
and the difference matters enough to get its own
chapter next:

- **WSL 1** translates Linux system calls into
  Windows ones, on the fly. There is no Linux kernel
  involved at all.
- **WSL 2** runs a real Linux kernel — one Microsoft
  builds and maintains — inside a small, managed
  **virtual machine** that starts in about a second
  and that you never have to administer like a normal
  VM. No window, no boot screen, no login prompt of
  its own: it exists only to host your distributions.

New installs default to WSL 2, and most of this guide
assumes it.

## The distro's disk is a file

On WSL 2, each distribution's entire Linux filesystem
lives inside one Windows file: a **virtual hard disk**
with the extension `.vhdx`, formatted inside as
**ext4** — the standard Linux filesystem. Your
distribution is, physically, a disk image in a folder
under your Windows user profile.

That one fact explains a lot of WSL's behaviour:

- A distro can be *exported* to a single file and
  carried to another machine (chapter 12).
- The disk file *grows* as you add files, up to a
  1 TB default ceiling — but deleting files inside
  Linux does not automatically shrink it back. It is
  a one-way balloon unless you take steps, covered in
  the *Limits* chapter.
- Windows tools must never edit the files *inside*
  that disk image directly. The supported way in is
  through WSL itself, which presents the Linux files
  to Windows safely (chapter 6).

## The file bridge

Two small translators let each side browse the other:

- From Linux, your Windows drives appear under
  `/mnt` — drive C: as `/mnt/c`, and so on. A
  filesystem driver called **DrvFs** does that
  mounting.
- From Windows, each distro's files appear under the
  network-style path `\\wsl$` (newer versions also
  accept `\\wsl.localhost`). Under the covers this is
  served by a file-sharing protocol descended from
  the Plan 9 operating system's **9P** — Windows asks,
  the Linux side answers.

Both directions work; they do not work at the same
*speed*, and chapter 6 is blunt about which side to
keep your working files on.

## WSLg — the graphics piece

Windowed Linux programs are shown by **WSLg**, a
small built-in companion distribution Microsoft ships
for exactly that job. It runs the display servers
(a Wayland compositor and an X server) that Linux
GUI apps expect, and passes their windows through to
the Windows desktop, where they sit beside your
Windows programs like natives. You never log into
WSLg or manage it; it is simply there when a GUI app
needs it — WSL 2 only. Chapter 10 puts it to work.

## Configuration lives in two files

Two plain-text files steer everything, and mixing
them up is the most common beginner confusion:

- **`/etc/wsl.conf`**, *inside* each distribution:
  settings for that one distro — whether systemd
  runs, which user is the default, how Windows drives
  are mounted.
- **`%UserProfile%\.wslconfig`**, on the Windows
  side: settings for the whole WSL 2 machine — how
  much memory and how many processors the VM may use,
  which networking mode, and more.

Chapter 7 walks both files key by key.

## The lifecycle in one paragraph

A distribution *runs* when you open it and keeps
running while anything inside it is alive. Close
everything and, after a short idle wait, WSL stops
the virtual machine by itself to hand the memory
back. `wsl --shutdown` stops everything *now* — you
will use it after changing configuration, and in
troubleshooting. Nothing here needs babysitting; the
lifecycle only matters because it explains why a
config change "didn't take" until a restart, and why
a background service stops when you close the last
terminal.
