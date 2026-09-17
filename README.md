# MyOS

A custom Live/Install ISO distribution based on **Arch Linux** and the **archiso** profile, featuring the **KDE Plasma** desktop environment and the **Calamares** graphical installer.

## Description

MyOS is an Arch Linux build based on the standard `releng` profile from [archiso](https://gitlab.archlinux.org/archlinux/archiso), extended with:

- the **KDE Plasma** desktop environment (`plasma-desktop`, `sddm`, `konsole`, `dolphin`);
- the **Calamares** graphical installer with the `kpmcore` partitioning module;
- a custom set of wallpapers, icons, and a boot loader splash image;
- boot support for both BIOS (Syslinux) and UEFI (systemd-boot / GRUB), plus **rEFInd**;
- a set of networking and system utilities (NetworkManager, OpenVPN, WireGuard-compatible tools, TPM2, disk encryption, etc.).

## Features

| Category | Components |
|---|---|
| Desktop | KDE Plasma, SDDM, Konsole, Dolphin |
| Installation | Calamares + kpmcore |
| Bootloaders | Syslinux (BIOS), systemd-boot / GRUB (UEFI), rEFInd |
| Networking | NetworkManager, iwd, ModemManager, OpenVPN, OpenConnect, WireGuard compatibility |
| Filesystems | ext4, Btrfs, XFS, F2FS, NILFS2, Bcachefs, NTFS, exFAT, FAT |
| Diagnostics | fastfetch, smartmontools, testdisk, ddrescue, memtest86+ |
| Virtualization | qemu-guest-agent, open-vm-tools, hyperv, VirtualBox Guest Utils |

The full package list is in [`packages.x86_64`](./packages.x86_64).

## Repository structure

```
myos_iso/
├── airootfs/          # Root filesystem included in the live image
│   ├── etc/            # System configuration (systemd, Calamares, locales, etc.)
│   ├── root/            # Root user files, startup scripts
│   └── usr/             # Icons, wallpapers, local scripts
├── efiboot/            # systemd-boot configuration (UEFI)
├── grub/                # GRUB configuration
├── syslinux/            # Syslinux configuration (BIOS) and splash.png
├── bootstrap_packages   # Package list for the bootstrap image
├── packages.x86_64      # Packages installed into airootfs
├── pacman.conf          # pacman configuration used for the build
└── profiledef.sh        # Main archiso profile file (ISO metadata, boot modes)
```

## Build requirements

- A host system based on Arch Linux (or a derivative with access to `pacman`);
- the `archiso` package installed:

```bash
sudo pacman -S archiso
```

## Building the ISO

1. Clone the repository:

   ```bash
   git clone https://github.com/<your-account>/<repo-name>.git
   cd <repo-name>
   ```

2. Run the profile build (from the directory containing `myos_iso/`):

   ```bash
   sudo mkarchiso -v -o out/ myos_iso/
   ```

3. The resulting image will be placed in the `out/` directory.

## Writing the image to a USB drive

Replace `/dev/sdX` with the correct device:

```bash
sudo dd bs=4M if=out/*.iso of=/dev/sdX status=progress oflag=sync
```

Or use [Ventoy](https://www.ventoy.net/) / [Rufus](https://rufus.ie/) / [balenaEtcher](https://etcher.balena.io/).

## Installation

After booting from the media, the **Calamares** graphical installer is available — launch it from the KDE Plasma desktop and follow the setup wizard.

## Notes

- Some system strings (boot menu title, `profiledef.sh` metadata) are still inherited from the base Arch Linux profile and can be renamed to the MyOS brand if desired.
- The default locale is `C.UTF-8`; edit `airootfs/etc/locale.conf` and `airootfs/etc/locale.gen` if you need a different one.

## License

This project is built on the [archiso](https://gitlab.archlinux.org/archlinux/archiso) framework, distributed under the GPL license. Add the license that applies to your own changes (icons, wallpapers, scripts) here.

## Acknowledgements

- [Arch Linux](https://archlinux.org/) and the [archiso](https://gitlab.archlinux.org/archlinux/archiso) team
- [Calamares](https://calamares.io/) — a distribution-agnostic installer
- [KDE Plasma](https://kde.org/plasma-desktop/)
