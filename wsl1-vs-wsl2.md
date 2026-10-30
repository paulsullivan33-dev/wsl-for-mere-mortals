# WSL 1 vs WSL 2

You will meet both version numbers in settings,
forums and error messages, so this chapter compares
the two architectures honestly — including the one
job where the older one still wins.

## Two different machines wearing the same coat

**WSL 1** has no Linux kernel. When a Linux program
asks the kernel to open a file or start a process,
WSL 1 *translates* that request into the equivalent
Windows operation and carries it out there. It is an
interpreter standing between two operating systems,
translating at full speed.

**WSL 2** does not translate. Microsoft ships a real
Linux kernel, runs it in a small virtual machine, and
the distro's programs talk to that kernel exactly as
they would on a Linux computer. The VM is managed for
you — it boots in about a second, grows and returns
memory as needed, and has no desktop or console of
its own.

## Where WSL 2 is simply better

- **Compatibility.** A translator can only translate
  what it knows. WSL 1's system-call coverage is
  *partial* — most everyday tools work, and then some
  program does something unusual and fails in a way
  that's nobody's fault. WSL 2's kernel is the real
  thing, so compatibility is effectively total.
- **Speed inside Linux.** Work done entirely within
  the Linux filesystem — unpacking archives,
  compiling, `git` operations, package installs — is
  dramatically faster on WSL 2. Microsoft's own early
  measurements put it at up to twenty times faster
  unpacking a large archive, and two to five times
  faster on everyday developer chores.
- **The modern features are WSL 2 only.** systemd
  (chapter 8), windowed GUI apps (chapter 10), GPU
  compute (chapter 11), mounting physical disks, and
  most of what's new in current releases simply do
  not exist on WSL 1.

## Where WSL 1 still wins

Exactly one place, and it is a real one: **files that
live on the Windows side.** Because WSL 1 works
directly on Windows files, a Linux program reading
and writing thousands of files under `/mnt/c` can be
quicker there than on WSL 2, where every such access
crosses between the VM and Windows through the file
bridge.

If your workflow is "Linux tools, Windows files" —
say, running Linux text utilities over a folder of
documents on C: — WSL 1 can be the right choice for
that one distro. You can run distributions of both
kinds side by side, so this is a per-distro decision,
not a commitment.

## Requirements differ

WSL 1 asks almost nothing of the hardware. WSL 2
needs virtualization: a processor with the usual
virtualization support plus **SLAT** (second-level
address translation — Intel's EPT or AMD's
equivalent, present in essentially every CPU since
about 2009, but genuinely absent from Core 2 Duo-era
machines), virtualization switched on in the BIOS,
and Windows 11 or Windows 10 version 1903, build
18362 or later. Chapter 16's troubleshooting section
covers what happens when those pieces are missing —
the error codes name the culprit.

## Which should you use?

WSL 2, for everything, unless your work is the
specific WSL 1 case above: heavy file churn across
the Windows boundary. WSL 2 is the default for new
installs, it is where every new feature lands, and
converting a distro either way is one command —
`wsl --set-version <Distro> 2` — so the decision is
cheap to revisit. Back up a distro before converting
it if it holds anything you care about; conversion
rewrites its storage, and Microsoft warns it can be
slow on large distros. Chapter 12's export command
is the two-minute insurance policy.
