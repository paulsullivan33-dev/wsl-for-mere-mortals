# Limits

An honest accounting, in one place, of where WSL
stops — so that you meet these on paper rather than
mid-project.

## It is not a server

The subsystem sleeps when you do. Close the last
thing keeping a distro awake and WSL stops it and
the virtual machine, handing memory back to Windows.
Services, timers and cron jobs exist only inside
that waking window. Chapter 13 has the workarounds;
they remain workarounds. Anything that must run at
3 a.m., or while you're away from the machine,
belongs on a machine that is always on.

## It is not a desktop

WSLg shows you Linux *applications* on the Windows
desktop, beautifully. It does not give you a Linux
desktop session, and desktop environments and
session tools are explicitly outside the supported
story. For a desktop, use a virtual machine.

## The file door has a toll

Chapter 6's rule, stated as a limit: traffic across
the Windows↔Linux file boundary is taxed per file.
Bulk work on the wrong side is slow in a way no
setting fixes. Both directions are fine for
documents; neither is fine for a 40,000-file build
tree living on the far side.

## Disks only grow

A distro's `.vhdx` expands as you write and does not
shrink when you delete. Long-lived distros therefore
drift toward their high-water mark on your Windows
drive. Remedies exist — the experimental `sparseVhd`
setting in `.wslconfig` (chapter 7) lets disks hand
space back, and a distro can always be exported and
re-imported to come back lean — but by default,
growth is one-way. Keep an eye on any distro you
feed large datasets.

## Hardware reach is narrow

Beyond GPU compute (real, chapter 11) and USB via
the usbipd add-on (a hand-off, chapter 11), WSL does
not do hardware: no raw PCIe devices, no direct
serial ports, no passing a whole disk controller
through. `wsl --mount` can attach a physical disk's
filesystems to WSL 2 — genuinely useful for reading
a Linux drive — and that is about the limit of it.

## Networking is a guest's networking

NAT by default means the distro is invisible to your
LAN and borrows Windows' identity to the internet.
Mirrored mode fixes most of that on current Windows
11, and what remains — publishing services to the
world, exotic routing, being a network *citizen*
with its own address on an older Windows — is VM
and real-machine territory.

## The kernel is Microsoft's

On WSL 2 you run the kernel Microsoft builds. You
can supply your own (chapter 7's `kernel=` key), but
then it is *yours* to maintain. Drivers in the usual
Linux sense don't exist here; hardware support is
whatever the shipped kernel and the virtual devices
provide.

## Small print

- **WSL 1 is in maintenance weather.** It still
  ships and still serves its one niche (chapter 3),
  but every new feature is WSL 2's.
- **The distro's clock can drift** when the host
  sleeps — see chapter 16 for the one-command fix.
- **Windows Home vs Pro** barely matters here: WSL
  needs the Virtual Machine Platform, not full
  Hyper-V, so Home editions are full citizens for
  everything in this guide.
