# The Neighbours

WSL is one of several ways to have Linux near
Windows. Each neighbour is the right answer to a
different question; this chapter exists so you pick
deliberately rather than by habit.

## A full virtual machine

VirtualBox, VMware, or Hyper-V running a complete
Linux installation — its own kernel, its own virtual
hardware, its own desktop if you install one.

Choose it when: you want a **real Linux desktop**;
you need an always-on Linux server on this machine
with its own stable network identity; you need to
hand it a whole device; or you are rehearsing an
install exactly as it would happen on real hardware.
Costs: gigabytes of disk, a machine-within-a-machine
to update and back up, and a hard wall between the
two systems' files (crossed with shared folders,
which are exactly as clunky as they sound). The
*Virtual Machines for Mere Mortals* guide is the
full tour.

## Docker Desktop

A special case wearing a neighbour's clothes: Docker
Desktop on Windows *uses WSL 2* as its engine room,
running containers in a managed distro of its own.
Choose it when the thing you actually want is
**containers** — packaged services, reproducible
environments — rather than a Linux to live in. You
will still want a distro of your own beside it for
the command line. The *Docker for Mere Mortals*
guide covers containers themselves.

## Dual boot

Linux installed beside Windows, chosen at power-on.
Perfect Linux performance, perfect separation —
and you can only use one at a time, which for most
people settles it. Choose it when the Linux work
*is* the day's work and Windows is the occasional
visitor, the mirror image of WSL's audience.

## The small second machine

A Raspberry Pi or a retired laptop running Linux
full-time, reached over SSH. Choose it when the
services must be **always on** (it sips power and
never sleeps when your laptop does), when USB
hardware should live on native Linux (chapter 11's
fine print), or when the experiment must not share a
machine with your documents. It costs a shelf slot
and a little administration, and it is the natural
graduation for anything in WSL you find yourself
keeping alive with tricks from chapter 13.

## Git Bash, MSYS2 and Cygwin

Windows-native toolkits that provide Unix-*style*
commands in a Windows shell. Choose them for a
handful of familiar utilities (`grep`, `ssh`, `rsync`
in scripts) on a machine where WSL isn't available
or isn't permitted. They are not Linux — no kernel,
no distro packages, no services — and anything
beyond the utilities themselves will eventually
remind you of that.

## A Linux in the cloud

A small rented server, or a free cloud shell, reached
from the same terminal. Choose it when the audience
is the internet — a public website, a service others
depend on. WSL's networking is built for an audience
of one desk (chapter 9); publishing from it is
possible and generally a mistake.

The line through all of these: WSL wins when the
Linux and the Windows work are *the same sitting*,
both open, one machine, files and commands passing
between them without ceremony. The moment the work
is *only* Linux, or *always* on, or *public*, a
neighbour is the honest tool.
