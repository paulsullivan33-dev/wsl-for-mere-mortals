# What WSL Is

By the end of this chapter you will know what the
Windows Subsystem for Linux actually puts on your
machine, what it is *for*, and the one thing to keep
in mind before you treat it as a toy.

**WSL** — the **Windows Subsystem for Linux** — is a
Microsoft feature that runs a real Linux system
inside Windows. Not a look-alike, not a set of ported
command-line tools, and not a screenshot of Linux in
a window: a genuine Linux **kernel**, running genuine
Linux *distributions* — full copies of Ubuntu,
Debian, Fedora and friends, with their real package
managers, shells and services.

You open a terminal, type `ubuntu` or just `wsl`,
and you are at a Linux prompt. The files you make are
Linux files. The programs you install come from the
distro's own repositories. Close the terminal and the
Linux side goes quiet; Windows carries on none the
wiser.

## Why it exists

People kept needing *both* systems. A developer whose
laptop runs Windows might deploy to a Linux server.
A sysadmin might need one Linux tool that has no
decent Windows equivalent. A student might be
following a course that assumes `bash`, `grep` and
`make`. The old answers were all heavier than the
problem: a full virtual machine you have to boot and
maintain, a second computer, or dual-booting and
rebooting every time you switch sides.

WSL's answer: keep one machine, one desktop, and let
the two systems share it — each able to reach the
other's files, and each able to run the other's
programs. A Linux web server can serve a page your
Windows browser opens. A Windows editor can edit a
file that lives in Linux. That two-way door is the
whole point, and most of this guide is about using it
well and knowing where its seams are.

## What you get

- A real Linux environment a couple of minutes after
  one install command, with no separate partition and
  no reboot-to-Linux ritual.
- As many distributions as you like, side by side,
  each disposable: wreck one, delete it, start clean.
- Linux command-line tools, services, and even
  windowed Linux programs, next to your Windows apps.
- A Linux that can be snapshotted to a single file
  and restored, moved, or copied to another machine.

## The one thing to keep in mind

WSL is not a sandbox. By default, Linux programs can
see your Windows files, and Windows can see the
Linux files — that openness is the feature. It means
a careless command in the Linux shell can delete a
Windows document, and a compromised Linux program is
not walled off from the rest of your machine. Treat
WSL with the same respect you would give a second
operating system installed on the same computer,
because that is what it is. This guide will point
out, as they come up, the settings that widen or
narrow that door.

## An honest note about this guide

WSL is a Windows feature, and this guide's lab has no
Windows machine in it. Everything here was written
and checked against Microsoft's own documentation —
the claims are Microsoft's, the explanations and the
warnings are ours, and nothing pretends to a hands-on
test that did not happen. The verification ledger in
the *Writing Criteria* chapter says exactly what was
checked and how.
