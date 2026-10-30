# Hardware: GPU and USB

Two doors WSL does not open by itself: the graphics
card, for compute work, and USB devices. This
chapter covers both — the GPU story is a built-in
feature with one golden rule; the USB story is an
add-on with a paper trail.

## GPU compute: the graphics card as a calculator

WSL 2 can hand your Windows graphics card to Linux
for **compute** work — training and running machine-
learning models, scientific computing, anything that
speaks CUDA or DirectML. The game-rendering side of
the card is a different matter (chapter 10 covers
display graphics); this is about the card as an
engine.

Requirements: WSL 2, and Windows 11 or Windows 10
21H2 or later. Then the rule, stated first because
breaking it is the classic failure:

**Install the current driver for your card on
Windows. That driver is the only driver you
install.** NVIDIA's own CUDA-on-WSL guide puts it
in bold: *do not install any Linux display driver
inside WSL.* The Windows driver contains the WSL
support; a Linux driver installed into the distro
can only fight it.

With that done:

- **NVIDIA:** from inside a glibc-based distro
  (Ubuntu, Debian), confirm the card is visible:

  ```
  nvidia-smi
  ```

  Then install the CUDA toolkit *userspace* (the
  libraries, not a driver) following NVIDIA's
  CUDA-on-WSL guide, or use their container route —
  NVIDIA Container Toolkit plus Docker, running GPU
  containers from inside WSL. Multiple GPUs in the
  machine are all visible to WSL.
- **AMD and Intel:** the route is **DirectML**, via
  the DirectML builds of TensorFlow or PyTorch that
  Microsoft documents. Coverage is narrower than
  CUDA's — framework support, not general compute —
  and for AI tinkering it is often enough.

What "enough" means, honestly: this is real GPU
acceleration, used by plenty of people for real
model work, at near-native speed for compute-bound
jobs. It is not a promise that every CUDA program
ever written runs — the card appears to Linux
through a Microsoft paravirtual device, and very
low-level tools that expect to own a PCIe device
will be disappointed. For the far end of that road —
PCIe passthrough, a card wholly given to Linux —
you want a real VM or a real Linux machine.

## USB devices: the usbipd bridge

Out of the box, WSL cannot see your USB devices at
all. The bridge is **usbipd-win**, an open-source
Microsoft-adjacent project — separate software, not
part of WSL — which shares a USB device from Windows
into a WSL 2 distribution over the virtual network.

Requirements: Windows 11 build 22000 or later
(Windows 10 has a route involving a custom kernel;
the project's documentation covers it), an x64 or
ARM64 machine (32-bit x86 is not supported), and a
WSL 2 distro.

The routine, once usbipd-win is installed from its
project page:

1. Keep a WSL terminal open — the distro must stay
   awake to hold the device.
2. In an **administrator** PowerShell, list what
   Windows can see:

   ```
   usbipd list
   ```

   Note the **bus ID** of your device (something like
   `4-4`).
3. Share it — once per device, elevated:

   ```
   usbipd bind --busid 4-4
   ```
4. Attach it to WSL — no elevation needed:

   ```
   usbipd attach --wsl --busid 4-4
   ```
5. Confirm from inside the distro:

   ```
   lsusb
   ```

   Your device is now a Linux device: a serial
   adapter appears as a `/dev/ttyUSB*` or
   `/dev/ttyACM*`, ready for the usual Linux tools.
   (A group-permission fix — adding your user to the
   `dialout` group — is the customary last step for
   serial hardware.)
6. Hand it back with `usbipd detach --busid 4-4`, or
   just unplug it.

The honest fine print: while a device is attached to
WSL, **Windows cannot use it** — the sharing is a
hand-off, not a split. And this is community-grade
plumbing: it works well for the classic cases —
flashing an Arduino or ESP32 board, talking to a
smart-card reader, driving a serial console — and
devices with hard real-time or isochronous demands
(USB audio, webcams you care about) are not its
strength. If a board matters to you daily, a $35
single-board computer speaking native Linux USB is
the less interesting but more reliable answer; the
*Neighbours* chapter makes that comparison properly.
