# Files Both Ways

The two-way file door is WSL's best feature and its
sharpest edge. This chapter gives you the one rule
that matters, the paths in both directions, and the
truth about speed.

## The golden rule

**Keep your files on the side whose tools you are
using.**

Working in a Linux shell, on a Linux project? The
files belong in the Linux filesystem — your home
directory, `~`. Working in PowerShell or a Windows
app? The files belong in a normal Windows folder.
Microsoft's documentation states this plainly, and
nearly every "WSL is slow" story traces back to
breaking it: a Linux build grinding through thousands
of files on the Windows drive, or a Windows program
fishing around inside Linux storage.

Your Linux home is fast, native ext4 storage. The
crossing is the slow part. Arrange your work so the
crossing happens rarely and deliberately — copy a
finished thing across; don't *live* across.

## Linux looking at Windows

Windows drives appear in Linux under `/mnt`:

```
/mnt/c/Users/YourName/Documents
/mnt/d ...
```

Drives mount automatically, read-write. A Windows
text file edited here keeps its Windows line endings
unless your editor says otherwise — Linux tools
mostly cope, and occasionally a script refuses to
run until you convert them (the `dos2unix` utility
exists for exactly that). Paths contain spaces and
backslashes in Windows-land; quote them, or use
`wslpath` — a small utility inside the distro that
translates a path from one world's spelling to the
other's:

```
wslpath -a 'C:\Users\YourName\notes.txt'
# /mnt/c/Users/YourName/notes.txt
```

Case matters on this side and not the other:
`Notes.txt` and `notes.txt` are two different files
in Linux and the same file in Windows. Under
`/mnt/c` you are standing on Windows ground, so
Windows rules win there; in your Linux home, Linux
rules.

## Windows looking at Linux

In File Explorer's address bar, type:

```
\\wsl$
```

(newer builds also accept `\\wsl.localhost`). Every
running distro appears as a folder, and inside it is
the whole Linux filesystem, readable and writable.
From *inside* a distro, the mirror-image trick:

```
explorer.exe .
```

opens Explorer at your current Linux directory. That
trailing dot — and the `.exe` — are both required;
chapter 13's recipes lean on both directions.

The same door lets Windows programs edit Linux files:
your editor, your file manager, your backup tool can
all work through `\\wsl$`. What they must **not** do
is reach for the raw disk image — the distro's
`.vhdx` file, buried in your profile — and edit
*that* with Windows tools. Microsoft is blunt about
this: touching the disk image's contents directly can
corrupt the filesystem. Always go through `\\wsl$`,
where WSL itself brokers the access.

## Commands across the wall

The door swings for programs too, not just files:

- From PowerShell, run a Linux command in your
  default distro: `wsl ls -la`, `wsl grep -r TODO`.
  Output lands in your Windows terminal like a local
  program's.
- From inside Linux, run a Windows program by naming
  it *with* its extension: `notepad.exe notes.txt`,
  `code .`, `explorer.exe .`. The `.exe` is
  mandatory, and your Windows `PATH` is carried into
  Linux by default, which is why these are found at
  all. Both behaviours can be switched off in config
  (chapter 7's `[interop]`) if you'd rather have a
  wall than a door.

## The performance truth, without varnish

Inside one side: full native speed. Across the door:
fine for documents, scripts, a few hundred files —
and increasingly painful as the file *count* climbs,
because each small file pays a crossing toll. A
project with a `node_modules` folder or a large git
history on the wrong side can feel ten times slower
than it should. This is the single most common WSL
complaint, and the fix is never a setting: it is
moving the project to the side where its tools live.

One more asymmetry to remember from chapter 3: if
your work genuinely *must* live on the Windows side
but be processed by Linux tools all day, that is the
one case where a WSL 1 distro can outrun WSL 2.
Everywhere else, native side wins.
