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
sudo dnf install net-tools python3-pip htop fastfetch git unzip btop zathura feh vim figlet lolcat tar xz p7zip zip gzip cpio unace inxi stow sl cmus mpv foot zsh fzf wl-clipboard xdg-utils curl fontconfig flatpak bzip2 unrar xz-lzma-compat fuzzel sway
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

Here is your optimized, English version of the instructions formatted in clean Markdown. I have also ensured the file path for `thinkfan` is correct (Fedora typically uses `.yaml` for newer versions, but I've kept it consistent with your request).

---

## Silent Optimization

Configuration of **Thinkfan** for a silent acoustic profile and disabling **Intel Turbo Boost** to prevent sudden temperature spikes.

### Thinkfan

Install the utility and configure the sensors and fan thresholds.

**Install Thinkfan:**

```bash
sudo dnf install thinkfan
```

**Edit the Configuration File:**
*(Newer versions often use `/etc/thinkfan.yaml`)*

```bash
sudo nano /etc/thinkfan.conf
```

**Paste the following configuration:**

```yaml
# 1. Sensor Configuration (Targeting the Intel Package Temperature)
sensors:
  - hwmon: /sys/class/hwmon
    name: coretemp
    indices: [1]

# 2. Fan Configuration Hardwere Specific (ThinkPad ACPI Driver)
fans:
  - tpacpi: /proc/acpi/ibm/fan

# 3. Custom Quiet Curve
# [Level, Start_Temp, Stop_Temp]
levels:
  - [0, 0, 55]          # TOTAL SILENCE: Fan stays off up to 55°C.
  - [1, 50, 62]         # Level 1: Starts at 55°C, stops at 50°C.
  - [2, 55, 68]         # Level 2: Soft hum. Filters out minor spikes.
  - [3, 63, 75]         # Level 3: Noticeable fan noise for sustained loads.
  - [6, 70, 85]         # Level 6: Aggressive cooling to drop temps quickly.
  - [7, 80, 93]         # Level 7: Maximum RPM.
  - ["level auto", 88, 32767] # Emergency Mode: Hands control back to BIOS.
```

**Apply and Verify:**
Restart the service to apply the new curve:

```bash
sudo systemctl restart thinkfan
```

Check the service status:

```bash
sudo systemctl status thinkfan
```

### 2. Disable Intel Turbo Boost

To prevent the CPU from hitting high temperatures disable Turbo Boost using `tmpfiles.d`.

**Create the Configuration File:**

```bash
sudo nano /etc/tmpfiles.d/disable-turbo.conf
```

**Add the following line:**

```text
w /sys/devices/system/cpu/intel_pstate/no_turbo - - - - 1
```

### 3. Disable KSM (Kernel Same-page Merging)

To reclaim ~5% CPU usage and allow the processor to enter deeper sleep states (C-states), disable the KSM background scanner.

**Create the Sysctl Override:**

```bash
sudo nano /etc/sysctl.d/99-disable-ksm.conf
```

**Add these lines:**

```text
vm.ksm_pages_to_scan = 0
kernel.mm.ksm.run = 0
```

**Apply changes immediately:**

```bash
sudo sysctl --system
echo 2 | sudo tee /sys/kernel/mm/ksm/run
```



## How to Make KDE Faster

### Disable update checking for Discover

```Shell
sudo killall packagekitd
sudo systemctl stop packagekit
sudo systemctl mask packagekit
```

### Baloo Configuration

1. **Disable Baloo:**
```shell
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