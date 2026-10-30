# GUI Apps with WSLg

Linux programs with windows — editors, image tools,
the occasional utility that never got a Windows
port — can run inside WSL and appear on your Windows
desktop as ordinary windows. This chapter sets that
up and is honest about what it is *not*.

## What WSLg does

The piece is **WSLg**, introduced in chapter 2: a
small built-in system distribution that runs the
display servers Linux GUI programs expect — a
Wayland compositor and an X11 server — and composites
their windows onto your Windows desktop. A Linux GUI
app under WSLg:

- Opens as a normal window, beside your Windows
  programs, movable and resizable like any other.
- Appears in the **Start menu** under its distro's
  folder, launchable by clicking, no terminal needed.
- Shares the clipboard with Windows — copy in one
  world, paste in the other.
- Can open files through the same `\\wsl$` and
  `/mnt/c` doors as everything else.

There is no Linux desktop, no second taskbar, no
window to "log into Linux" through. Individual
applications are citizens of your Windows desktop;
the distribution they live in stays invisible.

## Requirements and setup

- Windows 11, or Windows 10 **build 19044 or later**.
- **WSL 2 only.** GUI apps do not run on WSL 1.
- A *fresh* install gets it automatically: the
  `wsl --install` of chapter 4 includes GUI support.
- An *existing* install needs one update and a full
  restart of the subsystem:

  ```
  wsl --update
  wsl --shutdown
  ```

## Trying it

From inside your distro, install something
deliberately visual — Microsoft's own examples are
as good as any:

```
sudo apt update
sudo apt install gimp -y
sudo apt install gnome-text-editor -y
```

Then just run it:

```
gimp
```

A moment later a very large GNU image editor is
sitting on your Windows desktop, running entirely in
Linux. The first launch is the slow one; WSLg itself
had to wake up. Afterwards the app is in your Start
menu, and clicking it there starts the distro behind
the scenes.

If a program reports it cannot open a display, the
usual causes are a WSL 1 distro (convert it, chapter
5), a WSL too old to carry WSLg (`wsl --update`), or
a hand-set `DISPLAY` variable fighting the automatic
one WSLg provides — unset it and try again.

## The honest limits

- **Apps, not a desktop.** A full Linux desktop
  environment is not the supported story. Desktop-
  focused tools that assume they own the screen, the
  session and the tray may run badly or not at all.
  For a real desktop, you want a virtual machine —
  the *Virtual Machines for Mere Mortals* guide
  covers those.
- **Performance is good, not magical.** Ordinary
  applications feel native. Heavyweight graphical
  workloads — 3D modelling, video editing — are
  served by translated graphics and will not match a
  real Linux install or the Windows native app.
- **It can be switched off.** `guiApplications=false`
  in `.wslconfig` disables WSLg for the machine, and
  each distro still works headless. Server-minded
  readers may prefer it that way.
