# Backup, Moving, and Cloning

A distro is software *plus* everything you have put
in it, and WSL's tooling treats the whole bundle as
one portable object. This chapter exports a distro
to a file, imports it back, moves one to another
drive, clones one, and restores one — the complete
life-cycle that makes chapter 5's deadly
`--unregister` safe to use.

## Export: the backup

From PowerShell, with the distro stopped or not
(`wsl --terminate` it first for the cleanest
snapshot):

```
wsl --export Ubuntu D:\Backups\ubuntu-2026-10.tar
```

That file — a plain **tar archive** of the entire
Linux filesystem — *is* your backup: the distro,
its users, its packages, its files. Store it like
any backup. Two refinements:

- `wsl --export Ubuntu D:\Backups\ubuntu.vhdx --vhd`
  exports the WSL 2 disk image itself instead
  (WSL 2 only). A `.vhdx` export suits moving a
  distro as-is; a tar export suits restoring across
  versions and inspecting or rescuing individual
  files with ordinary archive tools.
- An exported distro makes an excellent *base
  image*: set one distro up perfectly, export it,
  and every future machine starts from *yours*
  rather than from a bare shelf install.

## Import: the restore, and the clone

```
wsl --import UbuntuRestored D:\WSL\UbuntuRestored D:\Backups\ubuntu-2026-10.tar
```

Syntax: a new **name**, a folder to **install
into**, and the **file** to import. Options:
`--vhd` if the file is a `.vhdx`; `--version 1` or
`2` to choose the architecture. Done — the distro
exists, stopped, under its new name.

Notice what that makes trivial:

- **Moving to another drive** is export, then
  `wsl --unregister Ubuntu` (chapter 5's warning
  applies — you are deleting the original), then
  import from the backup *to the new location*: one
  command each, and the export you made first is
  your proof the move worked before the original
  goes away. Verify the imported copy starts and
  holds your files *before* unregistering anything.
- **Cloning** is import under a *different name* —
  same command, original left untouched. Two
  identical distros, one to break and one to keep,
  in under five minutes.
- **A fresh machine** is just an import of the tar
  file you carried over.

One behaviour surprises everyone once: an imported
distro starts as **root**, because the tar carries no
"which user was default" setting from the source.
The fix is chapter 7's `wsl.conf`:

```ini
[user]
default=alex
```

added inside the imported distro, followed by a full
shutdown and restart. (Store-installed distros set
their default user through their launcher app
instead — the `config --default-user` command — but
imported distros have no launcher, so `wsl.conf` is
the way.)

## Import in place

Have a bare `.vhdx` — say, a distro disk you kept
when its registration was lost? Register it as-is:

```
wsl --import-in-place Ubuntu D:\WSL\old-ext4.vhdx
```

The disk must be formatted ext4, and it becomes the
distro without copying. This is the recovery door:
the data file outlives the paperwork.

## What a backup routine looks like

Nothing exotic. A weekly `wsl --export` of the
distro you actually live in, to a dated file on
another drive or in your normal backup folder,
covers everything in this guide — because the
export *is* everything: there are no registry keys,
licenses or hidden companions to chase. What it
does not cover is anything you deliberately kept on
the Windows side (chapter 6's rule cuts both ways):
back up `/mnt/c` files with your Windows backup,
and let each world mind its own.
