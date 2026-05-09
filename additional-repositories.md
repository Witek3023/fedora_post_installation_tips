# Additional Repositories + Codecs

## RPM Fusion Repository Installation Guide

By default, Fedora, following its operating system policy, utilizes open-source software. To install closed-source software/drivers, you need to add the RPM Fusion repository.

### Installation Steps

To install the RPM Fusion repository, execute the following code in the terminal:

```shell
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

### AppStream Metadata

RPM Fusion repositories also provide Appstream metadata to enable users to install packages using Gnome Software/KDE Discover. Please note that these are a subset of all packages since the metadata are only generated for GUI packages.

For the current Fedora releases, the suggested method is to install appstream-data using DNF. Execute the following command to install the required packages:

```shell
sudo dnf group upgrade core
```

## Codecs Installation

To ensure the complete FFMPEG is downloaded, execute the following command:

```shell
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf install ffmpeg ffmpeg-libs libva libva-utils
```

Then install multimedia codecs:

```shell
sudo dnf group upgrade multimedia
sudo dnf group install sound-and-video
```

## Hardware Accelerated Codec Installation

### Intel (recent) hardware

Using the rpmfusion-nonfree section:

```shell
sudo dnf install intel-media-driver
```

### Older Intel hardware

Using the rpmfusion-free section:

```shell
sudo dnf install libva-intel-driver
```

### AMD hardware (mesa)

Using the rpmfusion-free section:

```shell
sudo dnf swap mesa-va-drivers mesa-va-drivers-freeworld
sudo dnf swap mesa-vdpau-drivers mesa-vdpau-drivers-freeworld
```

If using i686 compat libraries (for steam or alikes):

```shell
sudo dnf swap mesa-va-drivers.i686 mesa-va-drivers-freeworld.i686
sudo dnf swap mesa-vdpau-drivers.i686 mesa-vdpau-drivers-freeworld.i686
```

## OpenH264 Installation

To enable OpenH264, execute the following commands:

```shell
sudo dnf install gstreamer1-plugin-openh264 mozilla-openh264
```

## Troubleshooting

If you encounter issues with displaying images, photos, or sound in applications or games, it's likely due to missing libraries (codecs). To fix this issue, you can use the following commands:

### Install GStreamer plugins

```shell
sudo dnf install gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel
```

### Install Lame

```shell
sudo dnf install lame\* --exclude=lame-devel
```
