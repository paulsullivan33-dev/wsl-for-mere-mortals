# Networking

WSL's networking confuses people because it almost
works by magic — until the day it doesn't, and the
magic has no visible strings. This chapter shows the
strings: the default NAT arrangement, the mirrored
mode that replaces it on newer Windows, and how
servers on either side reach each other.

## The default: NAT

Out of the box, a WSL 2 distro sits behind **NAT** —
network address translation — on a small private
virtual network that Windows creates for it. The
distro gets its own IP address on that private
network; Windows acts as its router and translator
to the real network.

Consequences, one by one:

- **Outbound just works.** From Linux, the internet,
  your LAN and your DNS all behave. WSL even tunnels
  DNS queries through Windows (`dnsTunneling`, on by
  default on current versions), which is why VPNs
  and corporate DNS mostly survive.
- **The distro's IP changes.** The private address
  is assigned afresh, so scripts that hard-code it
  break on restart. Ask, don't assume:
  `hostname -I` inside the distro prints its current
  address.
- **Windows is reachable at a discoverable address.**
  The Windows host, seen from inside, is the gateway
  of that private network: `ip route show default`
  prints it, and it is also the nameserver address in
  `/etc/resolv.conf` — Microsoft's own documentation
  uses those commands for exactly this purpose.
- **Other machines cannot see the distro.** To the
  rest of your LAN, the distro does not exist. Only
  Windows can talk to it directly.

## The magic part: localhost forwarding

One piece of kindness is switched on by default
(`localhostForwarding=true` in chapter 7's
`.wslconfig`): start a server inside WSL —

```
python3 -m http.server 8000
```

— and from Windows, `http://localhost:8000` in your
browser *just opens it*, even though the server is
technically on another (virtual) machine at another
address. For the common case — developing something
in Linux, viewing it in Windows — you never need the
distro's real IP at all. Let a magic be a magic,
but now you know its name and where its switch is.

## Mirrored mode: the modern answer

On **Windows 11, version 22H2 and later**, there is
a better arrangement. In `.wslconfig`:

```ini
[wsl2]
networkingMode=mirrored
```

(then the eight-second rule: full shutdown, restart).
Mirrored mode *mirrors* Windows' own network
interfaces into Linux instead of hiding the distro
behind NAT. What changes:

- **localhost works both ways, truly.** Windows and
  the distro reach each other's servers at
  `127.0.0.1`, both directions — no forwarding
  special cases. (IPv6 localhost, `::1`, is not
  supported.)
- **The LAN can see the distro.** Services inside
  WSL become reachable from other machines on your
  network, like services on any computer. The
  Hyper-V firewall applies, and WSL prompts about
  new listeners as Windows does.
- **VPNs and odd networks behave better**, because
  the distro now shares Windows' view of the network
  rather than maintaining a translated one — IPv6
  included.

Microsoft's own guidance is to *try mirrored mode*
if NAT networking gives you trouble. On an eligible
Windows it is the setting this guide would pick.
WSL 1, living on Windows' own networking, never had
the NAT problem to begin with.

## The remaining NAT-era fiddles

If you stay on NAT, two questions recur:

- **"Let the LAN reach my WSL server."** Under NAT
  this needs Windows to forward a port inward to the
  distro's *current* address — doable with Windows'
  own `netsh interface portproxy`, but it must be
  redone whenever the distro's address changes.
  This is the chore mirrored mode exists to delete.
- **"DNS broke after I edited resolv.conf."** WSL
  regenerates `/etc/resolv.conf` at every start by
  default. Hand-manage it only after setting
  `generateResolvConf=false` in `wsl.conf` (chapter
  7) — otherwise your edit evaporates, silently,
  overnight.

## And no network at all

`networkingMode=none` in `.wslconfig` gives the
distro a loopback interface and nothing else — a
legitimate choice for a distro used to run untrusted
experiments, at the cost of package installs and
updates needing the mode lifted first.
