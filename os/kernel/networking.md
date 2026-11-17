# Networking

## Stack and devices
- **PCI** enumeration and device inspection.
- **Intel e1000** network adapter driver (polled, ring-based TX/RX).
- **IPv4** helpers with frame/checksum utilities, **ARP**, **ICMP** (echo/ping), **UDP**, and a **DHCP** client for address configuration.

## Shell/diagnostic commands (kernel environment)
- `PCI` — list/inspect PCI devices (including network adapter details)
- `NET` — show interface/addressing; `NET <ip>` set static address; `NET DHCP`/`NET AUTO` obtain via DHCP
- `PING <ip> [n]` — ICMP echo requests with round-trip reporting

## Default and QEMU notes
- QEMU user networking (slirp) is commonly used (`-netdev user -device e1000`).
- Typical user-net addressing (e.g. guest `10.0.2.15/24`, gateway/DHCP `10.0.2.2`) applies in that environment.
- **Limitation:** external ICMP to the internet may not traverse QEMU user-net as expected; targets within the virtual network (such as the gateway) respond normally.

> Sources: `osb.kernel/README.md`, `osb.kernel/Network/*`.
