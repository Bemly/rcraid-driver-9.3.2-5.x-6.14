# AMD FakeRAID (`rcraid`) Installation Guide for Ubuntu

This guide documents the steps required to install or use Ubuntu with the `rcraid` driver on AMD FakeRAID hardware.

## Environment
- **OS**: Ubuntu `25.04`
- **Kernel**: `6.14.0-37-generic`
- **Driver Module**: `rcraid.ko`

## Installation Steps

1. **Load Module in Live Environment / Installer**:
   - Boot Ubuntu Live USB.
   - Open a terminal and load the pre-compiled module:
     ```bash
     sudo insmod /path/to/rcraid.ko
     ```

2. **Inject Module into Installed System**:
   - Copy the module to the proper system path:
     ```bash
     sudo mkdir -p /lib/modules/6.14.0-37-generic/kernel/drivers/scsi/
     sudo cp rcraid.ko /lib/modules/6.14.0-37-generic/kernel/drivers/scsi/
     echo "rcraid" | sudo tee -a /etc/initramfs-tools/modules
     ```

3. **Update Initramfs & Dependencies**:
   - Rebuild initramfs to include the driver at boot:
     ```bash
     sudo depmod -a 6.14.0-37-generic
     sudo update-initramfs -u -k 6.14.0-37-generic
     ```

4. **Kernel Pinning**:
   - To prevent automatic kernel upgrades from breaking the driver:
     ```bash
     sudo apt-mark hold linux-image-generic linux-image-6.14.0-37-generic
     ```
