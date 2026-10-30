# Writing Criteria

This guide was written to a fixed standard. The
standard first, then how this particular guide
measures up against it — including, in the ledger at
the end, exactly what could and could not be
verified, and why.

## The standard

1. **Outcome-first structure.** The opening chapter
   tells the reader what WSL is for and what it will
   do for them; every chapter states the outcome it
   delivers — an install that exists, files that move
   correctly, a service that starts itself — before
   it discusses configuration or syntax.

2. **Failure first.** The first troubleshooting
   material appears early and honestly: the install's
   known stalls are named in the install chapter
   itself, the dangerous command (`--unregister`,
   which deletes everything) carries its warning at
   first use rather than in an appendix, and the
   error-code chapter is a translation of the
   failures readers will actually meet, in the order
   they will meet them.

3. **Pragmatic language.** Prose and the terms it
   must teach: *distribution*, *interop*, *the WSL 2
   virtual machine*, *NAT* and *mirrored mode*. The
   smallest useful vocabulary is chosen deliberately,
   defined on first use in plain words, and collected
   in the glossary. Concepts come before the commands
   that manipulate them.

4. **The settings tell a story about the tool.**
   WSL's whole configuration surface is two files —
   one per distro, one per machine — and the guide
   teaches them as a pair, key by key, because where
   a setting *lives* is the thing beginners get
   wrong. Every key described is a key Microsoft's
   configuration reference documents; every default
   quoted (half your memory, a quarter for swap, a
   1 TB disk ceiling) is the documented default.

5. **Scales with the skill level.** The reader's
   first five minutes (one command, one distro, one
   prompt) work with nothing else known. The guide
   then grows with them: a second distro beside the
   first, settings files shared between machines, an
   exported base image that makes every new machine
   start from *their* setup. Nothing in the early
   chapters has to be unlearned later.

6. **Treats real Linux as a feature, not a legacy.**
   WSL's central promise is that the distro is a
   genuine Linux — its own packages, its own systemd,
   its own files — so the guide never teaches a
   WSL-flavoured imitation of anything. Where WSL's
   behaviour differs from a Linux machine's (the
   kernel is Microsoft's, some files are generated,
   the machine sleeps), the difference is named
   exactly where it will bite.

7. **Open, standard, inspectable.** Everything WSL
   does to your system is inspectable with the tools
   in the guide: `wsl --status`, `wsl -l -v`, the two
   text config files, the distro's own logs. The
   command reference the guide leans on is
   Microsoft's public documentation, and the WSL
   project's own issue tracker is named as the place
   where unexplained behaviour gets explained.

8. **Point of view, stated early.** WSL is not a
   sandbox, and chapter 1 says so before any command
   is run: the two-way door between Windows and Linux
   is the feature *and* the exposure. The reader
   deserves that sentence before they need it.

9. **Verifiable, with the receipts published.** The
   artefacts behind this guide's claims — the
   documentation consulted, the cross-checks made,
   and this ledger — are in this repository's
   `wsl-lab` directory, so any reader can re-check
   any claim against its source. A claim whose source
   cannot be shown does not belong in the guide.

## How this guide measures up

- Systemd in WSL is presented as the distro's real
  systemd — because it is — and the guide says
  plainly what it does not fix: the distro still
  sleeps when nothing holds it open.
- The two-file configuration story is taught as one
  chapter with one table, because the single common
  beginners' error is putting a correct key in the
  wrong file. The eight-second rule gets its own
  section for the same reason.
- The performance advice is a rule, not a tuning
  guide: files live on the side whose tools use
  them. It is Microsoft's own guidance, it is the
  fix for the most common WSL complaint, and the
  guide declines to dress it up with settings that
  do not change it.
- Networking is taught in both its modes, with the
  default (NAT) explained before the better one
  (mirrored) — because the default is what the
  reader has, and its localhost behaviour looks like
  magic until somebody names it.
- Nothing in the guide is modelled on any person's
  or organisation's real machines. Example users,
  projects and drives are generic, because the
  reader's first distro should be built from their
  own life, not from the author's.

## The verification ledger

This guide's ceiling is stated once here and not
hidden anywhere: **the lab is a Linux machine. WSL
is a Windows feature. The product itself could not
be run, so the product is documented-only in this
guide.** That is the same category earlier guides in
this series used for features they could not reach —
except here it is the whole subject, which is why
the ledger matters more than usual.

**Verified against Microsoft's primary
documentation** (Microsoft Learn, read 2026-10-30;
full list with URLs in `wsl-lab/sources.md`):

- The install command, its prerequisites (Windows 10
  version 2004 / build 19041 or Windows 11), what it
  enables, and the `--no-distribution`, `--location`,
  `--inbox` and `--web-download` variations.
- The full `wsl.exe` command surface used in the
  guide — list, install, set-version, set-default,
  status, version, update, shutdown, terminate,
  export, import, import-in-place, unregister, mount.
- `wsl.conf` and `.wslconfig`: every section and key
  described, including defaults (memory 50%, swap
  25%, `defaultVhdSize` 1 TB, `networkingMode=nat`),
  the 8-second rule, and the malformed-file behaviour.
- systemd enablement (`[boot] systemd=true`), its
  confirmation command, and its Store-build
  requirement.
- Networking: NAT as default architecture, localhost
  forwarding, mirrored mode's requirements (Windows
  11 22H2+) and documented benefits, DNS tunnelling.
- WSLg: platform requirements (Windows 10 build
  19044+ / Windows 11), WSL 2 only, install and update
  paths, and the apps-not-desktop scope.
- GPU compute: the Windows-driver-only rule (stated
  in NVIDIA's own CUDA-on-WSL guide as well as
  Microsoft's), `nvidia-smi` as the visibility check,
  and the DirectML route for AMD and Intel.
- USB: usbipd-win's flow (`list`, `bind`, `attach
  --wsl`, `detach`), its administrator requirement
  for binding, and its platform limits.
- Export/import formats (tar default, `--vhd`),
  import-in-place, and unregister's data-loss
  warning.
- The troubleshooting chapter's error codes and
  remedies (0x8007019e, 0x800701bc, 0x80370102,
  0x80070003, the virtual-disk limitation error),
  each from Microsoft's troubleshooting documentation.
- The WSL 1 / WSL 2 comparison: translation versus a
  real kernel in a managed VM, compatibility, the
  cross-boundary speed difference, and WSL 2's
  requirements including SLAT.

**Cross-checked from Linux knowledge** (the lab *is*
a Linux machine — transcript in
`wsl-lab/cross-checks.txt`): the systemd behaviours
and commands chapter 8 attributes to a WSL distro
are the stock behaviours of systemd 255 on this
machine, which is the same software a distro runs;
the roles WSL assigns to `/etc/resolv.conf`,
`/etc/hosts` and `/etc/fstab` are their ordinary
Linux roles; the ext4 filesystem inside a distro's
`.vhdx` is the standard Linux filesystem. Where a
claim is really a Linux claim, it was checked as one.

**Documented-only, and why:** everything requiring a
Windows host — every `wsl.exe` invocation's real
output, install behaviour and timings, the WSL
Settings app, GUI application behaviour, GPU
visibility and throughput, USB attach success,
mirrored-mode behaviour on a real network, and error
messages beyond the wording Microsoft's
troubleshooting page quotes. The guide presents
those commands exactly as the documentation gives
them and claims no hands-on test that did not
happen. If you run the guide on a real machine and
find a place where the product and this text differ,
the text yields — the documentation, not the guide,
was the source, and the source is cited in the lab
notes so the difference can be chased down.
