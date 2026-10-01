# AMD FakeRAID (`rcraid`) Installation Guide for Debian 13 (Trixie)

This guide documents the steps required to install Debian 13 using the `rcraid` driver on AMD FakeRAID hardware.

## Environment
- **OS**: Debian 13 (Trixie)
- **Kernel**: `6.12.57+deb13-amd64`
- **Driver Module**: `rcraid.ko`

## Installation Steps

1. **Load Module in Installer Shell**:
   - Boot Debian 13 Installer.
   - Switch to TTY2 (`Ctrl` + `Alt` + `F2`).
   - Load the pre-compiled module:
     ```bash
     insmod /path/to/rcraid.ko
     ```

2. **Workaround Ventoy / ISO Mount Conflict**:
   - If `/boot/efi` mount fails due to ISO conflict on `/dev/sda2`, set `/dev/sda2` to **Do not use** in the installer partition menu.
   - Select your actual EFI partition (e.g., `/dev/sdc1`) without formatting to preserve existing bootloaders.

3. **Inject Module into Installed System (Before Reboot)**:
   - Before completing the installation (at the "Finish Installation" screen), switch to TTY2 (`Ctrl` + `Alt` + `F2`).
   - Copy module and update `initramfs`:
     ```bash
     mkdir -p /target/lib/modules/6.12.57+deb13-amd64/kernel/drivers/scsi/
     cp /path/to/rcraid.ko /target/lib/modules/6.12.57+deb13-amd64/kernel/drivers/scsi/
     echo "rcraid" >> /target/etc/initramfs-tools/modules

     mount --bind /dev /target/dev
     mount --bind /proc /target/proc
     mount --bind /sys /target/sys
     chroot /target /bin/bash

     depmod -a 6.12.57+deb13-amd64
     update-initramfs -u -k 6.12.57+deb13-amd64
     exit
     ```

4. **Kernel Pinning**:
   - Once booted into Debian, hold kernel updates to prevent breakage until new drivers are built:
     ```bash
     sudo apt-mark hold linux-image-amd64 linux-image-6.12.57+deb13-amd64
     ```
