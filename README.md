# LinOS 1.0 · tytyel

**A pocket-sized operating system that boots straight into a text shell — with its own bootloader, its own kernel, and a browser that reaches the real internet. No Linux. No GRUB. No bloat.**

LinOS is a hobby x86_64 UEFI system written from scratch in freestanding C. The whole OS is two small EFI binaries: a **6 KB bootloader** and a **67 KB kernel**. It boots from a read-only ISO in a virtual machine, gives you a shell, files, a calculator, an assistant and the **tytyel** browser — and tytyel really searches the web and opens real pages.

> **Not Linux.** LinOS does not contain the Linux kernel, GNU userland, GRUB, systemd or any distro base. The `Lin` in the name is just the name. The boot chain is: UEFI firmware → `BOOTX64.EFI` (LinOS bootloader) → `kernel.efi` (LinOS kernel).

---

## What's inside

| Part | What it is |
|---|---|
| `EFI/BOOT/BOOTX64.EFI` | LinOS bootloader, written from scratch (6 KB) |
| `kernel.efi` | LinOS kernel: memory, LinFS filesystem, shell, drivers, apps (67 KB) |
| Shell | ~60 built-in commands: files, disks, colors, history, install |
| tytyel | Text browser: DHCP, DNS, ARP, TCP and HTTP implemented in the kernel |
| Apps | `calc`, `01 gpt` assistant, `beep` |

Everything is a single bootable ISO. Nothing is installed on your computer unless you explicitly run `setup` inside LinOS.

## Download

| File | Size | What it is |
|---|---|---|
| `LinOS.iso` | 34 MB | Bootable UEFI ISO (system + apps) |
| `SHA256SUMS.txt` | tiny | Checksums for the files above |

## Quick start in VirtualBox (tested configuration)

1. Install **VirtualBox 7.x** (Extension Pack not required).
2. **New** → Name: `LinOS` → Type: **Other** → Version: **Unknown (64-bit)**.
3. Memory: **2048 MB**. Hard disk: **Do not add a virtual hard disk**.
4. **Settings → System → Motherboard**: enable **EFI (special OSes only)**. Secure Boot: **off**.
5. **Settings → Display**: Controller **VMSVGA**, Video Memory **32 MB**, 3D acceleration **off**.
6. **Settings → Network → Adapter 1**: Attached to **NAT**, Adapter Type **Intel PRO/1000 MT Desktop (82540EM)**.
7. **Settings → Storage**: Controller **IDE** → add optical drive → choose `LinOS.iso`.
8. Start the VM. You will see `LinOS />`. Type `help` or `tytyel`.

These are the exact settings the release was tested with. Other e1000 adapters may work; Wi-Fi is not supported.

## Internet in tytyel

tytyel uses VirtualBox NAT and speaks plain HTTP directly: DHCP, DNS, ARP, TCP and HTTP/1.0 are implemented inside the kernel. There is no host-side helper, no script and no proxy — the virtual machine talks to the network on its own.

Type a site address such as `example.com`. HTTPS is not implemented yet: TLS is on the roadmap together with the offline web archive.

A self-contained offline archive — one multi-gigabyte file with its own search index — is planned for the next release, so tytyel can search without any network at all.

### Using tytyel

Type a site address at the `tytyel>` prompt:

```
tytyel> example.com
tytyel> http://wiby.me
```

After a page loads:

| Key | Action |
|---|---|
| number | open that link |
| `s` | new search |
| `b` | back |
| `r` | reload |
| `q` | quit to the LinOS shell |

tytyel is a text browser. JavaScript-heavy sites will look incomplete or empty; simple pages and plain HTML work best.

## Shell cheat sheet

```
help                 list commands
ver / uname          system version
ls / cd / pwd        folders
mkdir / write / cat  files
calc                 integer calculator
beep 880 300         test the PC speaker
net                  network interfaces and link status
tytyel               browser
reboot / shutdown    power
```

## Is this a virus? No — and you can verify it

This is a hobby OS, not a product. Here is exactly what it does and does not do:

- **It runs entirely inside the virtual machine.** It never installs anything on your host, adds no startup entries, services or registry keys, and cannot read your host files.
- **No telemetry, no analytics, no auto-update, no remote code.** The kernel only makes network requests you trigger in tytyel.
- **No host-side scripts at all.** Nothing runs on your computer besides VirtualBox, and the OS never writes to your disk.
- **The EFI binaries are unsigned.** Windows SmartScreen and some antivirus tools warn about any unsigned bootloader. This is expected for a hobby OS and is not a malware signal.
- **Check the hashes.** Compare `SHA256SUMS.txt` with the files you downloaded.
- **The ISO is easy to inspect.** It contains two EFI binaries, three text files and a checksum file inside one FAT boot image. No installers, no scripts, no music, no hidden payloads.

If you want zero network access, simply unplug the VM's network adapter in VirtualBox settings — the system still boots and works offline.

## Apps wanted — LinOS 1.1

LinOS currently has no package manager and no dynamic loader: apps are compiled into the kernel. That is about to change, and I want your help.

**The plan:** when **three community apps** are accepted, I will publish **LinOS 1.1** with those apps included.

What to send (open an **Issue** in this repository):

- an app **idea** with a short description and screenshots/mockups, or
- **C source** that builds freestanding for x86_64 UEFI (no libc), or
- a **shell utility** idea that fits the current text interface.

Accepted apps get credit in the release notes. Small, useful and text-friendly wins: notes, calculator upgrades, unit converter, hex viewer, calendar, task list, text editor improvements, music metadata viewer, and so on.

**Roadmap for 1.1:** community apps, an app interface (so apps no longer need to be linked into the kernel), and audio output with real MP3 playback. Audio is not implemented in 1.0 — `beep` tests the PC speaker, and the ISO ships without music files.

## Limitations (honest list)

- Text only; no graphics desktop yet.
- No audio driver and no MP3 decoder yet.
- No Wi-Fi; e1000 wired NIC only.
- No multitasking or protected user mode; apps run in kernel context.
- HTTPS is not implemented yet (no TLS inside LinOS).
- `setup` / `install` can copy the two EFI binaries to a writable disk (experimental).
- Some pages render poorly; tytyel is not a full browser engine.

## Technical notes

- Language: freestanding C11, no libc, no runtime.
- Target: x86_64 UEFI (OVMF, VirtualBox EFI, QEMU).
- Toolchain: `x86_64-w64-mingw32-gcc`, `-ffreestanding -nostdlib -Wl,--subsystem,10`.
- Boot protocol: UEFI application (`BOOTX64.EFI`) loads `kernel.efi` from the ESP/ISO.
- Network: Intel e1000 MMIO driver, ARP, DHCP, DNS over UDP, minimal TCP, HTTP/1.0.
- Filesystem: LinFS over the firmware FAT volume; in-memory shell filesystem.
- Binaries are stripped; source code is not included in this binary preview.

## По-русски

**LinOS 1.0 · tytyel** — маленькая x86_64 UEFI-система, написанная с нуля: свой загрузчик (`BOOTX64.EFI`, 6 КБ) и своё ядро (`kernel.efi`, 67 КБ). Это **не Linux**: внутри нет ядра Linux, GRUB, systemd и GNU-окружения.

Как запустить:

1. VirtualBox → создать VM: тип **Other / Unknown (64-bit)**, 2048 МБ RAM, без жёсткого диска.
2. Включить **EFI**, Secure Boot выключить. Видео: **VMSVGA**, 32 МБ, 3D off.
3. Сеть: **NAT**, адаптер **Intel PRO/1000 MT Desktop (82540EM)**.
4. Подключить `LinOS.iso` к IDE-приводу и запустить VM.
5. В консоли `LinOS />` набрать `help` или `tytyel`.

Интернет: сеть работает напрямую через NAT VirtualBox (DHCP, DNS, ARP, TCP и HTTP внутри ядра). Никаких скриптов и шлюзов на хосте нет. В VM запустите `tytyel` и введите адрес сайта, например `example.com`. HTTPS пока не поддерживается — TLS и офлайн-архив в планах.

Это не вирус: система работает только внутри виртуальной машины, ничего не ставит на хост, не имеет телеметрии и автозапуска. Хэши проверяйте по `SHA256SUMS.txt`. EFI-файлы без подписи, поэтому Windows может предупреждать — для домашней ОС это нормально.

**Нужны приложения!** Присылайте идеи или C-код через Issues. После трёх принятых приложений выйдет обновление **LinOS 1.1** с ними, интерфейсом приложений и звуком (MP3-плеером).

---

Copyright © 2026 LinOS project. All rights reserved. Binaries are provided as-is, without warranty.
