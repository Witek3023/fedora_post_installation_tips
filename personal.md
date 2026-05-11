# Custom Fedora Setup Guide

## Introduction

This guide provides steps to customize your Fedora system for enhanced functionality and performance.

## Zsh Installation and Configuration

1. **Install Zsh:**
```shell
sudo dnf install zsh
```

2. **Change default shell to Zsh:**

```shell
   chsh -s $(which zsh)
```
3. My shell settings are available in: [Dotfiles](https://github.com/Witek3023/Dotfiles).

## SELinux Configuration

1. **Check SELinux status:**
```shell
sestatus
```

2. **Open SELinux configuration file:**

```shell
sudo nano /etc/sysconfig/selinux

```

3. To **Disable SELinux** by changing the value of `SELINUX` to `disabled`.

## Custom Kernel

Features and instructions to install are available in:<br>
[CachyOS Copr Github Repository](https://github.com/CachyOS/copr-linux-cachyos)

## Additional Software Installation and Configuration

1. **Install essential tools:**

```shell
sudo dnf install net-tools python3-pip htop fastfetch git unzip btop zathura feh vim figlet lolcat tar xz p7zip zip gzip cpio unace inxi stow sl cmus mpv foot zsh fzf wl-clipboard xdg-utils curl fontconfig flatpak bzip2 unrar xz-lzma-compat
```

2. **Set hardware clock to local time:**
```shell
sudo timedatectl set-local-rtc '0'
```

3. **Disable terminal bell:**

```shell
   echo "blacklist pcspkr" | sudo tee /etc/modprobe.d/blacklist-pcspkr.conf > /dev/null
```

4. **Installing VS Codium**

Add the repository:
```shell
Fedora/RHEL/CentOS/Rocky Linux:
sudo tee -a /etc/yum.repos.d/vscodium.repo << 'EOF'
[gitlab.com_paulcarroty_vscodium_repo]
name=gitlab.com_paulcarroty_vscodium_repo
baseurl=https://paulcarroty.gitlab.io/vscodium-deb-rpm-repo/rpms/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg
metadata_expire=1h
EOF
```

Install the software: (if you want vscodium-insiders, then replace codium by codium-insiders)

```shell
sudo dnf install codium
```

5. **Brave Origin**

Add Repository
```shell
sudo dnf install dnf-plugins-core
sudo dnf config-manager addrepo --from-repofile=https://brave-browser-rpm-nightly.s3.brave.com/brave-browser-nightly.repo
```
Install
```shell
sudo dnf install brave-origin-nightly
```

## Firmware Updates
```shell
sudo fwupdmgr get-devices 
sudo fwupdmgr refresh --force 
sudo fwupdmgr get-updates 
sudo fwupdmgr update
```

## Power Management Configuration

### Install tuned-ppd (Alternative)

Replace power-profiles-daemon with tuned-ppd:

```shell
sudo dnf swap power-profiles-daemon tuned-ppd
```

### Install TLP (Alternative)

1. **Install TLP:**

```shell
   sudo dnf install tlp tlp-rdw
```

2. **Remove Power Profiles Daemon:**

```shell
   sudo dnf remove power-profiles-daemon
```

3. **Enable TLP:**
```shell
sudo systemctl enable tlp.service
```

4. **Mask rfkill services:**

```shell
   sudo systemctl mask systemd-rfkill.service systemd-rfkill.socket
```

5. **Add ThinkPad Extras:**

```shell
   sudo dnf install https://repo.linrunner.de/fedora/tlp/repos/releases/tlp-release.fc$(rpm -E %fedora).noarch.rpm
```

6. **Install kernel-devel and tp_smapi:**
```shell
sudo dnf install kernel-devel akmod-tp_smapi
sudo dnf --enablerepo=tlp-updates-testing install kernel-devel akmod-tp_smapi
```

## Building Thermald on Fedora

1. **Install Dependencies:**
```bash
dnf install automake libevdev-devel upower-devel gtk-doc libxml2-devel dbus-glib-devel glib-devel gcc-c++ gcc autoconf-archive
```

2. **Build and Install:**
```bash
git clone https://github.com/intel/thermal_daemon
cd thermal_daemon
./autogen.sh prefix=/
make
sudo make install
```

3. **Manage Service:**
```bash
sudo systemctl enable thermald.service
sudo systemctl start thermald.service
```

## How to Make KDE Faster

### Baloo Configuration

1. **Disable Baloo:**
```bash
balooctl6 disable
```

2. **Optimize Baloo:** Edit `~/.config/baloofilerc` to exclude directories.

### Disabling Akonadi

1. **Stop server:**

```bash
   akonadictl stop
```

2. **Edit config:** In `~/.config/akonadi/akonadiserverrc`, set `StartServer=false`.

### Desktop Effects

* Navigate to **System Settings** > **Apps & Windows** > **Window Manager** > **Desktop Effects**.
* Disable **Blur**, **Fade**, and **Sliding Popups**.

### Background Services

* Navigate to **System Settings** > **Background Services**.
* Disable unused services like **Vaults**, **SMB Watcher**, or **Write Daemon**.

### Plasma Search

* Navigate to **System Settings** > **Workspace** > **Search** > **Plasma Search**.
* Uncheck categories you do not use.

### Animation Speed

* Navigate to **System Settings** > **Workspace** > **General Behavior**.
* Set **Animation speed** to **Instant**.

### User Feedback

* Navigate to **System Settings** > **Security & Privacy** > **User Feedback**.
* Set level to **None**.