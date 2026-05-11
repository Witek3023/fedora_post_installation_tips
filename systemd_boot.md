# Systemd-boot Installation

## Requirements:

- Secure boot disabled

## Installation:

https://kowalski7cc.xyz/blog/systemd-boot-fedora-32/

or

```
inst.sdboot
```
in everything iso parameters

## Showing Windows entry from another drive while dual-booting

1. **Mount the Windows EFI partition:**

- Run `sudo fdisk -l` to list partitions.
- Look for a partition on drive where windows is installed with a size of 100M and type "EFI System".
- Create a directory and mount the Windows EFI partition that was identified earlier into it:

```shell
sudo mkdir /mnt/winefi
sudo mount /dev/nvme0n1p2 /mnt/winefi
```

2. **Copy the boot configuration data (BCD) to the systemd-boot EFI menu:**

```shell
sudo cp -R /mnt/winefi/EFI/Microsoft/ /boot/efi/EFI/Microsoft    
```

3. **Unmount the Windows partition and clean up:**

```shell
sudo umount /mnt/winefi
sudo rm -rf /mnt/winefi
```

## Setting up Secure Boot

### 1. Sbctl instalation from COPR repo

```shell
sudo dnf copr enable petersen/sbctl
sudo dnf install sbctl
```

### 2. Setup Mode in UEFI

Before creating the keys you need to enter ssetup mode in BIOS

- Reset PC to BIOS.
- Look for Secure boot settings
- Reset to Setup Mode.

After restart check status:

```shell
sbctl status
```

Look for `Setup Mode: Enabled`.

### 3. Creating and enrolling keys

```shell
sudo sbctl create-keys

sudo sbctl enroll-keys -m
```
`-m` This Flag enrolls also microsoft signed keys (Required for Windows dual boot).

### 4. Signing system files

You need to sign bootloader and kernels for them to be able to load.

```shell
sudo sbctl sign -s /boot/efi/EFI/systemd/systemd-bootx64.efi

sudo sbctl sign -s /boot/vmlinuz-$(uname -r)
```

### 5. Kernel Install

Thanks to the integration of `sbctl` with `kernel-install`, every newly installed kernel during a system update will be automatically signed.

To check the list of tracked files:

```shell
sudo sbctl list-files
```

### 6. Final Verification

Restart your computer. Once the system boots up, verify the security status:

```shell
sbctl status
```

**Expected output:**

- `Secure Boot: Enabled`
- `Setup Mode: Disabled`

---