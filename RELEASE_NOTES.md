# LinOS 1.0 — tytyel

**Boot your own operating system in about two minutes. No Linux. No GRUB. A 6 KB bootloader, a 69 KB kernel, a text shell, a graphical desktop — and a browser that reaches the real internet.**

LinOS 1.0 is the first public build of a small x86_64 UEFI system written from scratch in freestanding C. It is not a Linux distribution: there is no Linux kernel, no GNU userland, no GRUB and no systemd inside. The `Lin` is just the name.

## Highlights

- **Own bootloader** (`BOOTX64.EFI`, 6 KB) and **own kernel** (`kernel.efi`, 69 KB), both stripped and built without libc.
- **Graphical desktop 0.1** — blue gradient wallpaper, top bar, icon dock, start menu and a real mouse cursor drawn straight into the UEFI framebuffer.
- **tytyel browser** — DHCP, DNS, ARP, TCP and HTTP/1.0 implemented in the kernel; opens real sites over plain HTTP through VirtualBox NAT.
- **Text shell** with around 60 commands: files, folders, colors, history, disks, calculator, assistant, PC-speaker beep.
- **Runs from a read-only ISO** inside VirtualBox or QEMU. Nothing is installed on your host.
- **34 MB ISO** — small enough to keep on a USB stick.
- **No host-side scripts** — the VM uses the network directly, nothing runs on your PC.

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

1. Start the VM with NAT networking and an Intel PRO/1000 MT Desktop adapter.
2. In the VM run `tytyel`.
3. Type a site address such as `example.com`, then press a link number.

HTTPS is not implemented in 1.0, so only plain-HTTP sites open.

## Desktop 0.1

Type `desktop` (or `gui`) in the shell to enter the graphical shell:

- blue gradient wallpaper with the `LinOS — not Linux` watermark;
- top bar with the build label and an icon dock for Browser, Calculator and Files;
- `Menu` button with the application list;
- mouse cursor drawn as an outlined circle with a blue core, driven by UEFI Simple Pointer.

Move the mouse, click an icon, press `Enter` to activate it, `Esc` to return to the text shell. The desktop is a static launcher for now: no windows and no app switching yet, those come with the 1.1 interface work.

## Is it a virus?

No. It is a hobby operating system contained in a virtual machine:

- no installer, no services, no registry changes, no host file access;
- no telemetry, no analytics, no auto-update, no remote code;
- no host-side helper scripts at all;
- the EFI binaries are unsigned, so SmartScreen may warn — normal for hobby OSes;
- verify everything with `SHA256SUMS.txt`:

```
5b67e71acef834c6b7c54a9f325a55df1fca9c3ff82de47947fc76b1daecf174  LinOS.iso
a8b15c8358b4dffc03a93cbffca72bfed0482e88594308c93e3a80d9f3c5bb83  kernel.efi
490a3c3030a670b49ea14240d1b53335d5c43325dc670f6a4eeac4673f8fc3ce  EFI/BOOT/BOOTX64.EFI
```

## Apps wanted — LinOS 1.1

LinOS has no package manager yet: apps are compiled into the kernel. **After three accepted community apps, LinOS 1.1 will be released with them included.** Open an Issue with an app idea, a mockup, or freestanding C source for x86_64 UEFI. Text-friendly utilities, notes, converters, hex viewers, calendars and task lists are ideal.

The 1.1 roadmap also includes an app interface and audio output with real MP3 playback. Audio is not available in 1.0: `beep 880 300` tests the PC speaker, and the ISO ships without music files.

## Known limitations

The desktop shell is version 0.1: static launcher, keyboard-driven cursor, no windows, no multitasking. There is no audio, no Wi-Fi, no TLS inside LinOS, and JavaScript-heavy sites render poorly. `setup` can install the two EFI binaries to a writable disk, but treat it as experimental.

Full instructions, shell cheat sheet and the Russian quick start are in the README.

---

Copyright © 2026 LinOS project. Binary preview, provided as-is.
