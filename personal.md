# Custom Fedora Setup Guide

## Introduction

This guide provides steps to customize your Fedora system for enhanced functionality and performance.

## Zsh Installation and Configuration

1. Install Zsh:

```shell
sudo dnf install zsh
```

2. Change default shell to Zsh:

```shell
chsh -s $(which zsh)
```

3. Install Git:

```shell
sudo dnf install git
```

4. Install Oh My Zsh:

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

5. Configure Zsh by replacing the `.zshrc` file with your custom configuration.

## SELinux Configuration

1. Open SELinux configuration file:

```shell
sudo nano /etc/sysconfig/selinux
```

2. Disable SELinux by changing the value of `SELINUX` to `disabled`.

3. Check SELinux status:

```shell
sestatus
```

## Custom Kernel

You can find the custom kernel in the [Bieszczaders Kernel-CachyOS repository](https://copr.fedorainfracloud.org/coprs/bieszczaders/kernel-cachyos/)

### Features

The custom kernel offers the following features:

- **AMD PSTATE Preferred Core**: AMD PSTATE is enabled as default with preferred core.
- **BTRFS and XFS Improvements**: Latest improvements and fixes for BTRFS and XFS filesystems.
- **ZSTD 1.5.5 Patch-Set**: Latest and improved ZSTD 1.5.5 patch-set.
- **UserKSM Daemon**: Integration of UserKSM daemon from pf for memory deduplication.
- **Improved BFQ Scheduler**: Enhanced version of the BFQ scheduler.
- **Linux-Next Patches**: Back-ported patches from linux-next.
- **BBRv3 TCP Congestion Control**: Integration of BBRv3 tcp_congestion_control.
- **Scheduler Patches**: Scheduler patches from linux-next/tip.
- **Improved Sysctl Settings**: General improved sysctl settings and upstream scheduler fixes.
- **OpenRGB and ACS Override Support**: Support for OpenRGB and ACS Override.
- **HDR Patches for AMD GPU's**: HDR patches for AMD GPU's and gamescope.
- **Steam Deck Support**: Default support for Steam Deck.
- **Lenovo Legion Patchset**: Lenovo Legion Patchset (removed from the LTS kernel from version 6.6.30).
- **GitHub copr-linux-cachyos**: Additional patches and enhancements available on GitHub in the copr-linux-cachyos repository.

These features aim to enhance performance, compatibility, and functionality of the kernel for various use cases.

### Installation Steps

1. Enable the Bieszczaders Kernel-CachyOS repository:

```shell
sudo dnf copr enable bieszczaders/kernel-cachyos
```

2. Install Kernel-CachyOS and its matched development package:

```shell
sudo dnf install kernel-cachyos kernel-cachyos-devel-matched
```

## Kernel Addons

### Enable COPR Repository

See the section above for enabling COPR repositories.
