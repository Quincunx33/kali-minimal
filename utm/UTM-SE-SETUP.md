# UTM SE setup for kali-minimal

These manifests describe the tested settings for the released terminal-only Kali images. They are intentionally labeled as settings manifests rather than pretending to be universal importable `.utm` bundles. UTM stores host-specific configuration and file bookmarks inside a `.utm` bundle, so the reliable workflow is to create the VM in UTM SE and attach the release ISO manually.

## amd64 / x86_64

Download [kali-amd64-ultra-mini.iso](https://github.com/Quincunx33/kali-minimal/releases/download/v1.2.0-amd64/kali-amd64-ultra-mini.iso), then create a new QEMU virtual machine with the following values:

| UTM setting | Value |
|:--|:--|
| Backend | QEMU |
| Architecture | x86_64 |
| Machine | PC / i440FX-compatible PC |
| Firmware | BIOS; UEFI disabled |
| RAM | 768 MiB; 512 MiB minimum |
| CPU | 2 cores |
| Display | Text/serial console; no graphical desktop |
| Boot media | ISO attached as removable CD/DVD |
| CD/DVD interface | IDE |
| Network | Shared/NAT networking |
| Network card | Intel E1000-compatible adapter |
| Serial | Enabled; host pseudo-terminal when available |

Set the CD/DVD drive as the first boot device. The expected console ends at a prompt similar to `kali@kali-64-tiny:~$` after the live SquashFS is mounted.

## ARM64 / aarch64

Download [kali-arm64-ultra-mini.iso](https://github.com/Quincunx33/kali-minimal/releases/download/v1.1.0-arm64/kali-arm64-ultra-mini.iso), then create an ARM64 QEMU virtual machine:

| UTM setting | Value |
|:--|:--|
| Backend | QEMU |
| Architecture | aarch64 / ARM64 |
| Machine | `virt` |
| Firmware | UEFI enabled |
| RAM | 768 MiB; follow the iOS device’s memory limits |
| CPU | 2 cores |
| Display | Text/serial console; no graphical desktop |
| Boot media | ISO attached as removable CD/DVD |
| CD/DVD interface | SCSI or the default UTM ARM64 removable-drive interface |
| Network | Shared/NAT networking |
| Network card | VirtIO network adapter |
| Serial | PL011 / ARM serial console when exposed by UTM |

The ARM64 image must be used with an ARM64 VM profile. Do not attach it to an x86_64 profile. The expected console is `kali-64-tiny`, with automatic login and `uname -m` returning `aarch64`.

## i386 / 32-bit x86

Download [kali-32bit-ultra-mini.iso](https://github.com/Quincunx33/kali-minimal/releases/download/v1.0.0/kali-32bit-ultra-mini.iso) and use an x86 QEMU profile with legacy BIOS, an i440FX-compatible PC machine, IDE CD/DVD, 512–1024 MiB RAM, and a text/serial console. Use an i386 or 32-bit x86 architecture profile where UTM SE exposes one; otherwise use full x86 emulation rather than hardware virtualization.

## First boot checklist

Attach the ISO before starting the VM, set the optical drive first in the boot order, and keep the display mode text-only or serial. If UTM shows a graphical blank window, open the VM’s terminal/serial console instead. After login, run `uname -m`, `ip link`, and `ip route` to confirm the architecture and network device.

On iOS, avoid allocating all available memory to UTM. The official UTM gallery recommends using roughly 20% of device RAM for iOS guests because excessive allocation can cause iOS to terminate the app. On macOS or other hosts, 768 MiB is a practical starting point for these small images.

## Important limitation

The JSON files in this directory are readable settings manifests for the corresponding UTM SE VM. They are not complete `.utm` bundles and are not advertised as double-click import files. This avoids embedding an ISO path or host-specific bookmark that would be invalid on another device.

## References

[1]: https://docs.getutm.app/ UTM documentation  
[2]: https://docs.getutm.app/scripting/reference/ UTM scripting and QEMU configuration reference  
[3]: https://mac.getutm.app/gallery/ UTM official image gallery and memory guidance
