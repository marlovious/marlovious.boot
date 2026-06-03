# Marlovious Boot

Appliance-grade rEFInd configuration for Marlovious machines.

## Overview

Marlovious Boot uses rEFInd as a quiet UEFI boot layer with manual entries and PARTUUID-based volume identification. It can run as a minimal single-node boot screen or as a multi-OS boot manager.

## Architecture

The system consists of three core components:

**Boot Manager (rEFInd)** — Runs from an EFI system partition, configured with `use_nvram false` and `scanfor manual,external`. Boot entries are manually defined with PARTUUID-based volume identification when needed.

**OS Disks** — Each operating system can keep its own bootloader installed via `grub-install --removable` to the fallback EFI path `\EFI\BOOT\BOOTX64.EFI`. This keeps disks independently bootable.

**Theme** — The bundled `themes/marlovious.boot` directory is intended to be installed beside `refind.efi`.

## Layout

```text
refind.conf
boots.conf
themes/
  marlovious.boot/
    theme.conf
    background.png
    fonts/
    icons/
```

Install these files into the rEFInd EFI directory:

```text
EFI/BOOT/
  BOOTX64.EFI
  refind.conf
  boots.conf
  themes/
    marlovious.boot/
```

`boots.conf` contains the machine-specific menu entries. `blkids.example.txt` shows the `blkid` field to use when filling in a PARTUUID.

## Current Status

**Proven**: rEFInd boot menu architecture, manual entry configuration, isolated OS disks with independent bootloaders.

**In Testing**: PARTUUID-based volume identification reliability across different hardware configurations.

**Planned**: install script, add-disk registration script, appliance profile, recovery/provisioning partition.

## Known Limitations

- PARTUUID booting in rEFInd is inconsistently documented and still needs validation on each target class.
- Appliance nodes should keep a tested recovery path before hiding menus completely.

## Testing Requirements

1. Validate PARTUUID-based menuentry booting on real hardware.
2. Confirm `grub-install --removable` produces bootable disks that chainload correctly through rEFInd.
3. Verify `boots.conf` entries generated from `blkid` output boot reliably.
4. Test the appliance profile on PaneBot Node hardware.

## Future Directions

- Recovery/provisioning partition accessible from rEFInd.
- Marlovious Disc: pre-provisioned NVMe images for drop-in installation.
- Multi-distro provisioning templates.
- Support for additional operating systems and bootloader configurations.

## License

MIT (pending — framework only, actual implementation details TBD based on component licensing).
