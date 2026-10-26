# LinOS 1.0 — tytyel

**Boot your own operating system in about two minutes. No Linux. No GRUB. A 6 KB bootloader, a 67 KB kernel, a text shell — and a browser that reaches the real internet.**

LinOS 1.0 is the first public build of a small x86_64 UEFI system written from scratch in freestanding C. It is not a Linux distribution: there is no Linux kernel, no GNU userland, no GRUB and no systemd inside. The `Lin` is just the name.

## Highlights

- **Own bootloader** (`BOOTX64.EFI`, 6 KB) and **own kernel** (`kernel.efi`, 67 KB), both stripped and built without libc.
- **tytyel browser** — DHCP, DNS, ARP, TCP and HTTP/1.0 implemented in the kernel; HTTPS and Wiby search through a tiny host-side gateway.
- **Text shell** with around 60 commands: files, folders, colors, history, disks, calculator, assistant, PC-speaker beep.
- **Runs from a read-only ISO** inside VirtualBox or QEMU. Nothing is installed on your host.
- **34 MB ISO** — small enough to keep on a USB stick.
- **Windows host gateway** included as a readable PowerShell script — local only, `127.0.0.1:8088`, no admin rights.

## Tested VirtualBox settings

| Setting | Value |
|---|---|
| Type / Version | Other / Unknown (64-bit) |
| RAM | 2048 MB |
| Hard disk | none |
| EFI | enabled |
| Secure Boot | off |
| Display | VMSVGA, 32 MB, 3D off |
| Network | NAT, Intel PRO/1000 MT Desktop (82540EM) |
| Storage | IDE optical drive with `LinOS.iso` |
| Audio | disabled |

Boot the VM, wait for `LinOS />`, then type `help` or `tytyel`.

## Internet

1. Unzip `Tytyel-Gateway-1.0.zip` on the Windows host and run `Start Gateway.bat`.
2. Keep its window open. It listens only on `127.0.0.1:8088`.
3. In the VM run `tytyel` — it should print `Shlyuz DOSTUPEN`.
4. Type a query like `linux` or a URL, then press a result number.

Without the gateway tytyel can still try direct HTTP, but HTTPS and search will not work.

## Is it a virus?

No. It is a hobby operating system contained in a virtual machine:

- no installer, no services, no registry changes, no host file access;
- no telemetry, no analytics, no auto-update, no remote code;
- the gateway is a local-only helper you can read before running;
- the EFI binaries are unsigned, so SmartScreen may warn — normal for hobby OSes;
- verify everything with `SHA256SUMS.txt`:

```
4f99098412206994dab44489d5e6dabbb56ab1f7e08f18e90473e4453a1820ad  LinOS.iso
d0d6a5b02a489ec5bc8e0a503a0e94f582f3c68a26d1d67a12c0edd52d5d893a  Tytyel-Gateway-1.0.zip
33fafd741b26130c26bfbc0dfc817205340d9840fd9db58e27d225ae804d8c22  kernel.efi
490a3c3030a670b49ea14240d1b53335d5c43325dc670f6a4eeac4673f8fc3ce  EFI/BOOT/BOOTX64.EFI
```

## Apps wanted — LinOS 1.1

LinOS has no package manager yet: apps are compiled into the kernel. **After three accepted community apps, LinOS 1.1 will be released with them included.** Open an Issue with an app idea, a mockup, or freestanding C source for x86_64 UEFI. Text-friendly utilities, notes, converters, hex viewers, calendars and task lists are ideal.

The 1.1 roadmap also includes an app interface and audio output with real MP3 playback. Audio is not available in 1.0: `beep 880 300` tests the PC speaker, and the ISO ships without music files.

## Known limitations

Text-only interface, no audio, no Wi-Fi, no multitasking, no TLS inside LinOS, JavaScript-heavy sites render poorly. `setup` can install the two EFI binaries to a writable disk, but treat it as experimental.

Full instructions, shell cheat sheet and the Russian quick start are in the README.

---

Copyright © 2026 LinOS project. Binary preview, provided as-is.
