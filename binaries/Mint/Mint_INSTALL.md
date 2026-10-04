# AMD FakeRAID (`rcraid`) Installation Guide for Linux Mint

This guide documents the steps required to install or use Linux Mint with the `rcraid` driver on AMD FakeRAID hardware.

## Environment
- **OS**: Linux Mint 22.1
- **Kernel**: `6.8.0-51-generic`
- **Driver Module**: `rcraid.ko`

## Installation Steps

1. **Load Module in Live Environment / Installer**:
   - Boot Linux Mint Live USB.
   - Open a terminal and load the pre-compiled module:
     ```bash
     sudo insmod /path/to/rcraid.ko
     ```

2. **Inject Module into Installed System (Chroot)**:
   - If setting up post-installation or via a chroot environment, copy the module to the proper kernel modules directory:
     ```bash
     sudo mkdir -p /lib/modules/6.8.0-51-generic/kernel/drivers/scsi/
     sudo cp rcraid.ko /lib/modules/6.8.0-51-generic/kernel/drivers/scsi/
     echo "rcraid" | sudo tee -a /etc/initramfs-tools/modules
     ```

3. **Update Initramfs & Dependencies**:
   - Update the module dependencies and initramfs image:
     ```bash
     sudo depmod -a 6.8.0-51-generic
     sudo update-initramfs -u -k 6.8.0-51-generic
     ```

4. **Kernel Pinning**:
   - To prevent kernel updates from breaking the RAID driver until a new module is available:
     ```bash
     sudo apt-mark hold linux-image-generic linux-image-6.8.0-51-generic
     ```
