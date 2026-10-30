# Choosing and Managing Distributions

This chapter covers the daily administration of
distributions: seeing what's on offer, adding and
removing them, setting the default, converting
between WSL 1 and WSL 2, and keeping the kernel —
and WSL itself — up to date.

## What you can install

Ask WSL what is on offer:

```
wsl --list --online
```

(short form `wsl -l -o`). The list is Microsoft's
curated shelf: Ubuntu (in a couple of versions),
Debian, Kali, Fedora, Arch, openSUSE, SLES and a few
more. Install any of them with:

```
wsl --install -d Debian
```

Distributions also live in the Microsoft Store as
ordinary apps — installing from there does the same
thing. Which should you pick? For a first distro,
**Ubuntu** or **Debian**: the biggest communities,
the most tutorials, the fewest surprises. Pick
something exotic when you have a reason, not before.
A distro you choose for a specific job — Kali for
security tooling, say — can live alongside your
everyday one; they share nothing but the kernel.

## Taking stock

```
wsl -l -v
```

lists every installed distribution, whether it is
currently *Running* or *Stopped*, and its WSL
version (1 or 2). Add `--running` to see only those
awake, `--all` to include any mid-install. The
asterisk marks the **default** — the distro plain
`wsl` opens. Change it with:

```
wsl --set-default Debian
```

Start a specific distro, whatever the default, with
`wsl -d Debian`, and as a particular user with
`wsl -d Debian -u root`.

## Converting between versions

```
wsl --set-version Ubuntu 1
wsl --set-version Ubuntu 2
```

Conversion rewrites the distro's storage — a WSL 1
distro's files become a WSL 2 disk image or back
again — so it takes a while on a big distro, and
chapter 3's warning applies: export a backup first
(chapter 12) if it holds anything real. The default
for *future* installs is a separate setting:

```
wsl --set-default-version 2
```

## Keeping it current

Two different things update here, and the command
names don't make the distinction obvious:

- `wsl --update` updates **WSL itself** — including
  the Linux kernel it ships. On the Store build this
  is how new kernels and features arrive. If it
  stalls, `wsl --update --web-download` fetches the
  update from GitHub instead of the Store.
- The distro's *own* software updates the Linux way,
  from inside: `sudo apt update && sudo apt upgrade`
  on Ubuntu and Debian, and likewise for the others.
  WSL never touches those for you.

Check where you stand with `wsl --status` (default
distro, default version, kernel version) and
`wsl --version` (the fuller component list, Store
builds only).

The kernel deserves one sentence of its own, because
it confuses people: on WSL 2 the kernel belongs to
*WSL*, not to your distro. apt will never offer you a
new one, `uname -r` reports Microsoft's build, and
that is by design — one maintained kernel serves
every distro on the machine.

## Stopping, and removing

- `wsl --terminate Ubuntu` stops one distro, now.
- `wsl --shutdown` stops all of them and the VM.
- Stopped is the normal state; distros also stop on
  their own after an idle wait once nothing is
  running in them.

Removing is the dangerous one, so read the words
before you type them:

```
wsl --unregister Ubuntu
```

Unregistering **permanently deletes** the distro and
everything in it — files, settings, installed
software, all of it, with no recycle bin and no
undo. It is the right tool for a distro you built to
throw away, and a catastrophe for one you meant to
keep. Export first (chapter 12) if there is any
doubt at all. Reinstalling from the Store afterwards
gives you a *clean* distro, not your old one back.
