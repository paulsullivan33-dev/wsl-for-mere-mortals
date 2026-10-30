# systemd and Services

Modern Linux expects **systemd** — the service
manager that starts daemons at boot, restarts them
when they crash, and answers `systemctl`. WSL spent
its first years without it, and a generation of
workarounds grew up in the gap. This chapter turns
the real thing on, shows how to check it, and
explains what changes — and what stops working —
once it's there.

## Turning it on

systemd support needs **WSL 2** and a reasonably
current WSL (the Store build — check with
`wsl --version`; if that command isn't recognised,
update first). Then, inside the distro, edit
`/etc/wsl.conf`:

```ini
[boot]
systemd=true
```

Apply the eight-second rule from chapter 7 — fully
shut WSL down (`wsl --shutdown` from PowerShell) and
start the distro again. Confirm from inside:

```
systemctl list-unit-files --type=service
```

A list of services means systemd is now your init
system — PID 1 inside the distro — and everything
below applies.

## What you get

Everything a normal Linux user expects:

- **Services that start themselves.** Install a
  server the distro way — `sudo apt install` a
  database, a web server — and `systemctl enable
  --now <service>` runs it at every distro start and
  keeps it running, no manual ritual.
- **The standard controls.** `systemctl status`,
  `start`, `stop`, `restart`; logs through
  `journalctl`; your own services as `.service`
  unit files in `/etc/systemd/system`, indistinguishable
  from a bare-metal Linux box's.
- **Things that assume systemd.** Notably **snap**
  packages, which cannot work without it — enabling
  systemd is what makes `snap install` possible in a
  distro at all.

This is genuinely the distro's own systemd, the same
software (this guide's lab machine runs systemd 255,
the same family your distro ships) — not a Microsoft
stand-in. What you know from any Linux server
transfers unchanged. The companion *systemd for Mere
Mortals* guide covers the service manager itself in
depth.

## The catches, honestly

- **The distro still sleeps.** systemd manages
  services *while the distro runs*. Close your last
  terminal with nothing holding it open and WSL
  still stops the VM after its idle wait — taking
  your services down with it. Something must keep
  the distro alive (an open session, a Windows-side
  scheduled `wsl` call) for services to be truly
  always-on. WSL is a desktop companion, not a server
  room; chapter 14 is candid about the line.
- **Timers and cron behave accordingly.** A systemd
  timer or a cron job fires only while the distro is
  awake. WSL is not where your 3 a.m. jobs live
  unless the machine and distro are both up then.
- **Some units will complain.** Units that expect
  real hardware, a real boot, or kernel facilities
  the shared WSL kernel lacks may fail or grumble in
  the journal. Read `systemctl --failed` after
  enabling, and don't be alarmed by a couple of
  expected casualties in a desktop distro.

## The pre-systemd way, for completeness

On WSL 1 — where systemd can never run — and on
older WSL 2 setups, the standing trick is the
`command=` key in `[boot]`:

```ini
[boot]
command=service cron start
```

It runs that command as root at every start. Crude,
one-shot, no supervision or restart-on-crash — but
sufficient for starting a lone service, and still
useful alongside systemd for one-off boot commands.
