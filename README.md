# Debian Post Install Guide

Note: This guide assumes the user has been able to realize a clean installation of the Debian GNU/Linux operating system in any of its current maintained versions: Debian 11, Debian 12, Debian 13. This guide will help you setup the non-free/proprietary repositories, firewall, drivers, multimedia codecs, browsers, flatpak and flathub repositories, software backports (under review), and make gaming optimizations using Steam and the Lutris and Heroic game launchers (upcoming).

Note 2: A secong guide pertaining how to execute Microsoft Windows applications is under development.

Note 3: ISO burning guides are under development.

You can find the Live and Net installation ISO of D11, D12, D13 and other Debian versions here:
* https://cdimage.debian.org/cdimage/archive/ > `General cdimage archive`
* https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/archive/ > `Included propietary firmware in versions previous to D12`

You can use `Rufus`, `balenaEtcher` or `dd` to burn the ISO file into a USB according to your operating system:
* `Rufus` > `Windows`
* `balenaEtcher` > `Windows` `macOS` `Linux`
* `dd` > `Linux`

## Debian Post Installation Guide - Main:

* Things to do after installing Debian 11, aka `Bullseye`
* Things to do after installing Debian 12, aka `Bookworm`
* Things to do after installing Debian 13, aka `Trixie`

## Non-free repositories install

* The default installation of Debian stable comes with free repositories
* To add non-free repositories to use proprietary software, open your console/terminal and direct to:
```
sudo nano /etc/apt/sources.list
```
* Add `contrib non-free` to each line (bookworm is taken as **example**, adjust accordingly):
```
deb http://deb.debian.org/debian/ bookworm main non-free-firmware contrib non-free
deb-src http://deb.debian.org/debian/ bookworm main non-free-firmware contrib non-free

deb http://security.debian.org/debian-security bookworm-security main non-free-firmware contrib non-free
deb-src http://security.debian.org/debian-security bookworm-security main non-free-firmware contrib non-free

deb http://deb.debian.org/debian/ bookworm-updates main contrib non-free non-free-firmware contrib non-free
deb-src http://deb.debian.org/debian/ bookworm-updates main non-free-firmware contrib non-free
```
* Update your repositories:
```
sudo apt update && sudo apt upgrade
```

## Firewall install

* Setting up a firewall on Debian can be done using `ufw` (Uncomplicated Firewall):
```
sudo apt install ufw
```
* Enable `ufw`:
```
sudo ufw enable
```
* Check `ufw` status:
```
sudo ufw status
```

## Intel drivers install

* To install non-free, proprietary Intel drivers for Intel UHD Graphics cards you may install the following packages:
```
sudo apt install firmware-linux intel-media-va-driver-non-free vainfo
```

## Nvidia drivers install

* If you have an Nvidia GPU, you can install the drivers with:
```
sudo apt install linux-headers-$(uname -r)
sudo apt install build-essential
sudo apt install dkms
sudo apt install nvidia-detect
nvidia-detect
sudo apt install nvidia-driver nvidia-kernel-dkms
```
* Wait at least 5min for the driver to build, then reboot:
* `sudo reboot`
* Check driver after reboot with:
* `nvidia-smi`

## Multimedia codecs install

* Install multimedia codecs to stream videos and other media content:
```
sudo apt install libavcodec-extra vlc
```

## Google Chrome browser install (APT)

* This method allows to update Google Chrome with `apt update`, if you prefer to use a flatpak (containerized) browser, omit this step and proceed below
* Download and install the Google GPG signing key:
```
wget -qO- https://dl.google.com/linux/linux_signing_key.pub \
  | gpg --dearmor \
  | sudo tee /usr/share/keyrings/google-chrome.gpg >/dev/null
```
* Add the Google Chrome `apt` repository:
```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/google-chrome.gpg] \
https://dl.google.com/linux/chrome/deb/ stable main" \
| sudo tee /etc/apt/sources.list.d/google-chrome.list
```
* Update repositories and install Google Chrome:
```
sudo apt update && sudo apt install google-chrome-stable
```
* You can update Google Chrome simply using:
```
sudo apt update && sudo apt upgrade
```
* Taken from https://www.google.com/linuxrepositories/

## Flatpak and flathub repositories install

* To install flatpak applications you must install flatpak first:
```
sudo apt install flatpak
```
* Then add the flathub repositories:
```
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
* For KDE Plasma:
```
sudo apt install plasma-discover-backend-flatpak -y
```
* For Gnome:
```
sudo apt install gnome-software-plugin-flatpak -y
```
* `sudo reboot`

## Removing Firefox ESR and installing flatpak browsers

* The default version of Firefox in Debian is Firefox ESR (Extended Support Release), which may be behind most updates, you can remove and install your favorite browser with:
```
sudo apt remove --purge firefox-esr
```
* To install Mozilla Firefox's flatpak:
```
flatpak install flathub org.mozilla.firefox
```
* To install Google Chrome's flatpak:
```
flatpak install flathub com.google.Chrome
```

# Miscellanea

## Random utilities install

* `htop`: General system overview
* `neofetch`: General system specifications (Debian 12 or older)
* `fastfetch`: General system specifications (Debian 13 or newer)
* `lm-sensors`: General temperature overview
* `cmatrix`: Matrix style terminal screensaver
* `cava`: Cross-Platform Audio Visualizer for alsa, pulse, pipewire, etc.
* `gimp`: GNU Image Manipulation Program, free and open source image editing
* `krita`: Professional free and open source program for painters and graphic designers
* Adjust accordingly:
```
sudo apt install htop fastfetch lm-sensors cmatrix cava gimp krita
```

## VSCodium install

* To install VSCodium in Debian you can use the official open source binaries from https://vscodium.com/
* Add the GPG signing key of the repository:
```
wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg \
    | gpg --dearmor \
    | sudo dd of=/usr/share/keyrings/vscodium-archive-keyring.gpg
```
* Add the repository:
* For Debian 13 or newer:
```
wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg \
    | gpg --dearmor \
    | sudo dd of=/usr/share/keyrings/vscodium-archive-keyring.gpg
```
* For Debian 12 or older:
```
echo 'deb [arch=amd64,arm64 signed-by=/usr/share/keyrings/vscodium-archive-keyring.gpg] https://download.vscodium.com/debs vscodium main' \
    | sudo tee /etc/apt/sources.list.d/vscodium.list
```
* Update and install `VSCodium`
```
sudo apt update && sudo apt install codium
```

## OpenModelica install

* To install OpenModelica in Debian you can use the official open source binaries from https://openmodelica.org/download/download-linux/
* Add the GPG signing key of the repository:
```
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
curl -fsSL https://build.openmodelica.org/apt/openmodelica.asc | \
  sudo gpg --dearmor -o /usr/share/keyrings/openmodelica-keyring.gpg
```
* Add the repository:
* For Debian 13:
```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/openmodelica-keyring.gpg] \
  https://build.openmodelica.org/apt \
  trixie \
  stable" | sudo tee /etc/apt/sources.list.d/openmodelica.list
```
* For Debian 12:
```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/openmodelica-keyring.gpg] \
  https://build.openmodelica.org/apt \
  bookworm \
  stable" | sudo tee /etc/apt/sources.list.d/openmodelica.list
```
* For auto detect operating system:
```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/openmodelica-keyring.gpg] \
  https://build.openmodelica.org/apt \
  $(cat /etc/os-release | grep "\(UBUNTU\\|DEBIAN\\|VERSION\)_CODENAME" | sort | cut -d= -f 2 | head -1) \
  stable" | sudo tee /etc/apt/sources.list.d/openmodelica.list
```
* Update and install `OpenModelica`
```
sudo apt update && sudo apt install openmodelica
```
* For more configurations refer to the official website

# Upcoming

## Backports install
## Gaming installations
## Steam install
## Lutris install
## Heroic install
