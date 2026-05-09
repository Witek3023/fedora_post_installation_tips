# Systemd-boot Installation

## Requirements

- Secure boot disabled

Reference: https://kowalski7cc.xyz/blog/systemd-boot-fedora-32/

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
