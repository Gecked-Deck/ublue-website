# Bluefin

!!! Note "**This image is considered Beta**"

[![Bluefin Build](https://github.com/ublue-os/bluefin/actions/workflows/build.yml/badge.svg)](https://github.com/ublue-os/bluefin/actions/workflows/build.yml)

[![Ubuntu Toolbox Build](https://github.com/ublue-os/bluefin/actions/workflows/build-ubuntu-toolbox.yml/badge.svg)](https://github.com/ublue-os/bluefin/actions/workflows/build-ubuntu-toolbox.yml)

A familiar(ish) Ubuntu desktop for Fedora Silverblue. It strives to cover these three use cases:

- For end users it provides a system as reliable as a Chromebook with near-zero maintenance, with the power of Ubuntu and Fedora fused together.
- For developers we endeavour to provide the best cloud-native developer experience by enabling easy consumption of the [industry's leading tools](https://landscape.cncf.io/card-mode?sort=stars). These are included in dedicated `bluefin-dx` and `bluefin-dx-nvidia` images.
- For gamers we strive to deliver a world-class Flathub gaming experience.

![image](https://user-images.githubusercontent.com/1264109/224488462-ac4ed2ad-402d-4116-bd08-15f61acce5cf.png)

> "Let's see what's out there." - Jean-Luc Picard

## Usage

1. Download and install [the ISO from here](https://github.com/ublue-os/main/releases/latest/):
   - Select "Install ublue-os/bluefin" from the menu.
     - Choose "Install bluefin:38" if you have an AMD or Intel GPU.
     - Choose "Install bluefin-nvidia:38" if you have an Nvidia GPU.
   - [Follow the rest of the installation instructions](https://ublue.it/installation/).

### Rebase

For existing Silverblue/Kinoite users **AMD/Intel GPU users only**:

1. After you reboot you should [pin the working deployment](https://docs.fedoraproject.org/en-US/fedora-silverblue/faq/#_about_using_silverblue) so you can safely rollback.
2. Open a terminal and rebase the OS to this image:

Bluefin:

```bash
sudo rpm-ostree rebase ostree-image-signed:docker://ghcr.io/ublue-os/bluefin:38
```

Bluefin Developer Experience:

```bash
sudo rpm-ostree rebase ostree-image-signed:docker://ghcr.io/ublue-os/bluefin-dx:38
```

**Nvidia GPU users only** 

1. open a terminal and rebase the OS to this image:

    Bluefin:

    ```bash
    sudo rpm-ostree rebase ostree-image-signed:docker://ghcr.io/ublue-os/bluefin-nvidia:38
    ```

    Bluefin Developer Experience:

    ```bash
    sudo rpm-ostree rebase ostree-image-signed:docker://ghcr.io/ublue-os/bluefin-dx-nvidia:38
    ```  
  
2. Reboot the system and you're done!

### Revert

To revert back run:

  ```bash
  sudo rpm-ostree rebase fedora:fedora/38/x86_64/silverblue
  ```

  Check the [Silverblue documentation](https://docs.fedoraproject.org/en-US/fedora-silverblue/) for instructions on how to use rpm-ostree.

  We build date tags as well, so if you want to rebase to a particular day's release you can use the version number and date to boot off of that specific image:
  
  ```bash
  sudo rpm-ostree rebase ostree-image-signed:docker://ghcr.io/ublue-os/bluefin:37-20230310
  ```

The `latest` tag will automatically point to the latest build.

## Features

**This image heavily utilizes _cloud-native concepts_.**

System updates are image-based and automatic. Applications are logically separated from the system by using Flatpaks, and the CLI experience is contained within OCI containers.

## For Users

- Ubuntu-like GNOME layout.
  - Includes the following GNOME Extensions:
    - Dash to Dock - for a more Unity-like dock,
    - Appindicator - for tray-like icons in the top right corner,
    - GSConnect - Integrate your mobile device with your desktop,
    - Blur my Shell - for that bling.
- GNOME Software with [Flathub](https://flathub.org):
  - Use a familiar software center UI to install graphical software.
- Built on top of the the [Universal Blue main image](https://github.com/ublue-os/main).
  - Extra udev rules for game controllers and [other devices](https://github.com/ublue-os/config) included out of the box.
  - All multimedia codecs included.
  - System designed for automatic staging of updates.
    - If you've never used an image-based Linux before just use your computer normally.
    - Don't overthink it, just shut your computer off when you're not using it.

### Roadmap and Future Features

- Fedora 38 will be the initial release and will be considered Beta.
- Fedora 39 is the target for an initial GA release.

These are currently unimplemented ideas that we plan on adding:

- Provide a `:gts` tag aliased to the Fedora -1 release for an approximation of Ubuntu's release cadence.
- Provide a `:lts` tag derived from CentOS Stream for a more enterprise-like cadence.
- [Firecracker](https://github.com/firecracker-microvm/firecracker) - help wanted with this!

### Applications

- Mozilla Firefox, Mozilla Thunderbird, Extension Manager, Libreoffice, DejaDup, FontDownloader, Flatseal, and the Celluloid Media Player.
- Core GNOME Applications installed from Flathub:
  - GNOME Calculator, Calendar, Characters, Connections, Contacts, Evince, Firmware, Logs, Maps, NautilusPreviewer, TextEditor, Weather, baobab, clocks, eog, and font-viewer.
- All applications installed per user instead of system wide, similar to openSUSE MicroOS. Thanks for the inspiration Team Green!

### Recommended Extensions

The authors recommend the following extensions if you'd like to round out your experience. Use the included "Extensions Manager" application to search for these extensions; everything you need to get them to run is already included:

<img src="https://user-images.githubusercontent.com/1264109/224862317-569d018f-a7be-4895-82ff-e2c67652a0ab.png" width="400">

!!! Note 
    Installing extensions via extensions.gnome.org won't work, the extensions must be installed via this application.

- [Tailscale Status](https://extensions.gnome.org/extension/5112/tailscale-status/) for VPN.
- [Pano](https://extensions.gnome.org/extension/5278/pano/) for clipboard management.
- [Desktop Cube](https://extensions.gnome.org/extension/4648/desktop-cube/) if you really want to go retro.

## Frequently Asked Questions

> What about codecs?

Everything you need is included. You will need to [configure Firefox for hardware acceleration](/guide/codecs/)

> How do I get my GNOME back to normal Fedora defaults?

We set the default dconf keys in `/etc/dconf/db/local`, removing those keys and updating the database will take you back to the fedora default:

```bash
sudo rm -f /etc/dconf/db/local.d/01-ublue
sudo dconf update
```

If you prefer a vanilla GNOME installation check out [silverblue-main](https://github.com/ublue-os/main) or [silverblue-nvidia](https://github.com/ublue-os/nvidia) for a more upstream experience.

> Should I trust you?

This is all hosted, built, and pushed on GitHub. As far as if I'm a trustable fellow, here's my [bio](https://www.ypsidanger.com/about/). If you've made it this far, then hopefully you've come to the conclusion on how easy it would be to build all of this on your own trusted machinery. :smile:

## [![Repography logo](https://images.repography.com/logo.svg)](https://repography.com) / Recent activity [![Time period](https://images.repography.com/35181738/ublue-os/bluefin/recent-activity/FQtB4TpTHzW4xXgqpImZRpCa_73e9torMuxJiEGHGyI/dQfbRYx1KQiBimZnq3kUtRc3TOPc1aWB9etI3c1KNLs_badge.svg)](https://repography.com)

[![Timeline graph](https://images.repography.com/35181738/ublue-os/bluefin/recent-activity/FQtB4TpTHzW4xXgqpImZRpCa_73e9torMuxJiEGHGyI/dQfbRYx1KQiBimZnq3kUtRc3TOPc1aWB9etI3c1KNLs_timeline.svg)](https://github.com/ublue-os/bluefin/commits)
[![Issue status graph](https://images.repography.com/35181738/ublue-os/bluefin/recent-activity/FQtB4TpTHzW4xXgqpImZRpCa_73e9torMuxJiEGHGyI/dQfbRYx1KQiBimZnq3kUtRc3TOPc1aWB9etI3c1KNLs_issues.svg)](https://github.com/ublue-os/bluefin/issues)
[![Pull request status graph](https://images.repography.com/35181738/ublue-os/bluefin/recent-activity/FQtB4TpTHzW4xXgqpImZRpCa_73e9torMuxJiEGHGyI/dQfbRYx1KQiBimZnq3kUtRc3TOPc1aWB9etI3c1KNLs_prs.svg)](https://github.com/ublue-os/bluefin/pulls)
[![Activity map](https://images.repography.com/35181738/ublue-os/bluefin/recent-activity/FQtB4TpTHzW4xXgqpImZRpCa_73e9torMuxJiEGHGyI/dQfbRYx1KQiBimZnq3kUtRc3TOPc1aWB9etI3c1KNLs_map.svg)](https://github.com/ublue-os/bluefin/commits)

## [![Repography logo](https://images.repography.com/logo.svg)](https://repography.com) / Top contributors

[![Top contributors](https://images.repography.com/35181738/ublue-os/bluefin/top-contributors/FQtB4TpTHzW4xXgqpImZRpCa_73e9torMuxJiEGHGyI/dQfbRYx1KQiBimZnq3kUtRc3TOPc1aWB9etI3c1KNLs_table.svg)](https://github.com/ublue-os/bluefin/graphs/contributors)
