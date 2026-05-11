# Additional Repositories + Codecs

## RPM Fusion Repository Installation Guide

By default, Fedora follows a strict open-source policy. To install proprietary software or drivers, you must add the **RPM Fusion** repository.

### Installation Steps

Execute the following command in your terminal to add both the free and non-free repositories:

```shell
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

## AppStream Metadata

This enables you to find and install RPM Fusion packages directly through GUI stores like **GNOME Software** or **KDE Discover**.

To install the necessary metadata:

```shell
sudo dnf group upgrade core
```

## Codecs Installation

Fedora ships with "free" versions of codecs that may lack certain patent-encumbered features. To install the full versions:

### Complete FFMPEG

```shell
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf install ffmpeg ffmpeg-libs libva libva-utils
```

### Multimedia Groups

```shell
sudo dnf group upgrade multimedia
sudo dnf group install sound-and-video
```

## Hardware Accelerated Codec Installation

Enable hardware-level decoding to save CPU usage and battery life.

* **Intel (Modern):**
```shell
sudo dnf install intel-media-driver
```


* **Intel (Older):**

```shell
sudo dnf install libva-intel-driver
```
* **AMD (Mesa):**
```shell
sudo dnf swap mesa-va-drivers mesa-va-drivers-freeworld
sudo dnf swap mesa-vdpau-drivers mesa-vdpau-drivers-freeworld
```
* **i686 Compatibility (for Steam/32-bit apps):**
    
```shell
sudo dnf swap mesa-va-drivers.i686 mesa-va-drivers-freeworld.i686
sudo dnf swap mesa-vdpau-drivers.i686 mesa-vdpau-drivers-freeworld.i686
```

## OpenH264 Installation
To enable H.264 support in browsers like Firefox:
```shell
sudo dnf install gstreamer1-plugin-openh264 mozilla-openh264
```

## Troubleshooting

If applications or games struggle to play sound or video, you likely have missing libraries.

**Install GStreamer plugins:**

```shell
sudo dnf install gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel
```

**Install Lame:**

```shell
sudo dnf install lame\* --exclude=lame-devel
```

## Flathub - Flatpak Repository

Flathub is the primary repository for Flatpak applications, offering sandboxed versions of many popular apps.

**Add Flathub Stable:**

```shell
sudo dnf install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

**Add Flathub Beta:**

```shell
flatpak remote-add --if-not-exists flathub-beta https://flathub.org/beta-repo/flathub-beta.flatpakrepo
```
