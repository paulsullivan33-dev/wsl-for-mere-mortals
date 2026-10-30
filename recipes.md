# Recipes

Small, complete jobs that combine the earlier
chapters. Each assumes a working WSL 2 distro.

## Serve a folder from Linux, view it in Windows

Inside the distro, in any folder:

```
cd ~/site
python3 -m http.server 8000
```

Leave it running, open your Windows browser at
`http://localhost:8000`. Chapter 9's localhost
forwarding is doing the work. `Ctrl+C` stops it.
This is the everyday development loop for half the
people who install WSL.

## Run your Windows editor on Linux files

From inside the distro, standing in the project:

```
explorer.exe .
code .
```

The first opens this folder in File Explorer; the
second — if Visual Studio Code is installed on
Windows — opens it there, files still living in
Linux, editing at native speed (chapter 6's rule,
respected by working through the door rather than
across it).

## The disposable experiment box

Need to try something risky — an unfamiliar install
script, a tutorial you half-trust?

```
wsl --export Ubuntu D:\Backups\clean-base.tar
wsl --import Sandbox D:\WSL\Sandbox D:\Backups\clean-base.tar
```

Do the experiment in `Sandbox`. Whatever happens,
your real distro never saw it, and:

```
wsl --unregister Sandbox
```

makes it as if it never was. Distros are cheap;
treat them that way.

## A door-stop for services

You want a database or a wiki in WSL to be up
whenever you sit down, without keeping a terminal
open. Enable systemd (chapter 8), enable the service
(`systemctl enable --now <name>`), and pin a tiny
Windows-side keeper: a shortcut in your Startup
folder targeting:

```
wsl -d Ubuntu -e sleep infinity
```

Crude, effective, and honest about what it is — the
distro stays awake because something is running in
it. When you outgrow this, chapter 14 is waiting:
always-on services belong on an always-on machine.

## Convert the project that felt slow

Your project lives under `/mnt/c` (because it always
has) and builds like it's underwater. Move it home,
where it belongs:

```
mkdir -p ~/projects
cp -r /mnt/c/Users/YourName/Projects/reading-list ~/projects/
cd ~/projects/reading-list
```

Work there. When something must cross back — a
finished export, a release zip — copy that one thing
out deliberately. Build speed recovers immediately;
this is the performance truth of chapter 6 with its
sleeves rolled up.

## Two distros, one job each

A pattern worth stealing: a *stable* distro (Ubuntu
LTS, your tools, your dotfiles, touched carefully)
and a *current* distro (Debian, Fedora, whatever
you're evaluating) that you reinstall without
ceremony whenever it gets untidy. `wsl -d` picks the
one for the job; the default stays the stable one.
Everything in chapter 12's export toolkit makes the
second one's disposability free.
