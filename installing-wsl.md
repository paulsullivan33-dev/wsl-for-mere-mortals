# Installing WSL

One command does the whole job on a current Windows.
This chapter runs it, explains what it changed under
the covers, covers the first-launch moment, and ends
with the variations — no distribution, the inbox
build, and older Windows versions.

## Before you start

You need:

- **Windows 11**, or **Windows 10 version 2004
  (build 19041) or higher**. Press `Win + R`, type
  `winver`, and check if in doubt. The single-command
  install needs that baseline; older Windows 10 has a
  manual route at the end of this chapter.
- An **administrator** PowerShell or Windows Terminal
  session for the install command. Right-click the
  Start button and choose *Terminal (Admin)* or
  *Windows PowerShell (Admin)*.
- A few gigabytes of free space and an internet
  connection.

## The one command

In the administrator shell:

```
wsl --install
```

That is the whole install. Behind the scenes it:

1. Enables the two Windows components WSL needs —
   the *Windows Subsystem for Linux* itself and the
   *Virtual Machine Platform*.
2. Downloads and installs the latest Linux kernel.
3. Sets **WSL 2** as the default for new distros.
4. Downloads and installs **Ubuntu**, the default
   distribution.

A restart may be required; if Windows asks, do it,
and let the install finish afterwards.

A quick tell: if `wsl --install` answers with a page
of *help text* instead of installing, WSL is already
present on the machine. Skip to chapter 5 and add the
distribution you actually want.

## The first launch

Open your new distribution from the Start menu (or
type `ubuntu`, or just `wsl`). The first launch
finishes setting the distro up — a minute or two —
and then asks the only two questions that matter:

```
Enter new UNIX username:
New password:
```

Pick any username you like; it becomes your Linux
account, with `sudo` rights. The password is a
*Linux* password, unrelated to your Windows sign-in,
and Linux will show **nothing at all** while you
type it — no dots, no asterisks. That silence is
normal Unix behaviour, not a frozen keyboard.

You are now at a Linux prompt on your own machine.
Chapter 5 shows how to see what you've got:

```
wsl -l -v
```

run from PowerShell lists each distro, whether it is
running, and which WSL version it uses.

## Variations worth knowing

**Install a different (or extra) distro in one go.**
Name it, using a name from `wsl --list --online`:

```
wsl --install -d Debian
```

**Choose where the distro lives.** By default a
distro installs under your user profile. To put it
on another drive from the start:

```
wsl --install -d Ubuntu --location D:\WSL\Ubuntu
```

**Install WSL with no distribution at all** — handy
on a machine you're preparing for someone else:

```
wsl --install --no-distribution
```

**Store build vs inbox build.** By default the
command installs the **Microsoft Store** version of
WSL, which Microsoft updates frequently and
independently of Windows. The alternative `wsl
--install --inbox` installs WSL as a built-in Windows
component instead, updated only through Windows
Update — slower to gain features, but the choice
some managed workplaces prefer. The Store build is
the right default for almost everyone, and several
features in this guide (systemd among them) assume
it. Check which you have with `wsl --version`: if
that command isn't recognised, you have an older
inbox build.

## If the install stalls

The documented nuisance: the install can sit at
**0.0%** indefinitely when the Store path is blocked
or sulking. The documented remedy is to fetch the
distro directly from the web instead:

```
wsl --install --web-download -d Ubuntu
```

The same `--web-download` switch rescues updates
later, in chapter 5.

## Older Windows 10

On Windows 10 builds too old for the one-command
install (but new enough for WSL 2 — version 1903,
build 18362+), the manual route still exists: enable
the two components yourself from *Turn Windows
features on or off* (or the PowerShell equivalents
Microsoft's install page lists), install the
kernel update package Microsoft offers there, set
`wsl --set-default-version 2`, then install a distro
from the Microsoft Store. It is ten minutes of
clicking instead of one command — annoying, not
hard, and Microsoft's install documentation walks it
step by step. Anything older than build 18362 can
run WSL 1 only.

Next: choosing, adding and managing distributions.
