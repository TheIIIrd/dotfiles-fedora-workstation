# FEDORA-CUSTOM-SETUP

## Working with software

#### Deleting unnecessary repositories
```sh
dnf repolist
sudo dnf config-manager --set-disabled <repo>
sudo dnf clean all && sudo dnf check-update
```

#### Package manager speedup
```sh
sudo nano /etc/dnf/dnf.conf

# add these lines to the config
max_parallel_downloads=6
fastestmirror=True
defaultyes=True
```

#### Uninstalling software
```sh
sudo dnf remove gnome-contacts gnome-weather gnome-maps gnome-calendar cheese totem && sudo dnf autoremove
```

#### Installing useful software
```sh
sudo dnf install git wget curl inxi neovim neofetch java-latest-openjdk python python3-pip

# choose the latest version of java
sudo update-alternatives --config java
```

#### Adding community repositories
```sh
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

sudo dnf groupupdate core
```

#### NVIDIA graphics card driver installation
```sh
# open gnome-system-monitor and monitor CPU load
sudo dnf install akmod-nvidia
sudo dnf install xorg-x11-drv-nvidia-cuda

# reboot and check inxi
inxi -G
```

#### Adding a flathub repository
```sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

#### Basic software installation
```sh
sudo dnf install gitg gparted gwe inkscape steam transmission krita gnome-tweaks torbrowser-launcher grub-customizer dconf-editor vlc
```

#### Virt-manager installation
```sh
sudo dnf install -y qemu-kvm libvirt virt-install bridge-utils virt-manager libvirt-devel virt-top libguestfs-tools guestfs-tools
sudo usermod -G libvirt -a <username>
sudo systemctl enable libvirtd
```

#### Zerotier installation
```sh
sudo dnf install zerotier-one
sudo systemctl enable zerotier-one.service

# or you could do this
curl -s 'https://raw.githubusercontent.com/zerotier/ZeroTierOne/master/doc/contact%40zerotier.com.gpg' | gpg --import && \  
if z=$(curl -s 'https://install.zerotier.com/' | gpg); then echo "$z" | sudo bash; fi
```

#### ClamAV antivirus installation
```sh
sudo dnf install clamtk
sudo freshclam
```

#### Flatpak application installation
```sh
flatpak install flathub com.github.tchx84.Flatseal org.kde.kdenlive org.onlyoffice.desktopeditors com.orama_interactive.Pixelorama com.github.Matoking.protontricks io.github.fabrialberio.pinapp com.github.GradienceTeam.Gradience com.vscodium.codium fr.romainvigier.MetadataCleaner com.belmoussaoui.Authenticator com.github.ADBeveridge.Raider org.darktable.Darktable org.ppsspp.PPSSPP io.github.spacingbat3.webcord io.github.realmazharhussain.GdmSettings com.mattjakeman.ExtensionManager com.vysp3r.ProtonPlus com.heroicgameslauncher.hgl
```

### Installing mod launcher for games (especially important for Risk Of Rain 2)

> Download the latest release of the program
> https://github.com/ebkr/r2modmanPlus/releases

```sh
sudo rpm -ivh <programname>
```

#### Installing the native engine for the game STALKER
```sh
# download the necessary dependencies
sudo dnf install git cmake make gcc gcc-c++ glew-devel openal-devel cryptopp-devel libogg-devel libtheora-devel libvorbis-devel SDL2-devel lzo-devel libjpeg-turbo-devel

git clone https://github.com/OpenXRay/xray-16.git --recurse-submodules
cd xray-16 && mkdir bin && cd bin
cmake .. -DCMAKE_INSTALL_LIBDIR=lib64 -DCMAKE_INSTALL_PREFIX=/usr

# the number of cores is counted from zero
# if you have 4 cores, you should write 3
make -j<numberofcores>

QA_RPATHS=$(( 0x0001|0x0010 )) make package
sudo rpm -ivh <programname>
```

#### Installing the open source version of the Minecraft Launcher

> Download the latest release of the program (we need a .jar version of the game)
> https://tlaun.ch/jar

```sh
nvim ~/.local/share/applications/Minecraft.desktop

# add it to the file
[Desktop Entry]
Name=Minecraft
Comment=Infinite idea generator
Exec=gamemoderun java -jar /path/to/LegacyLauncher_legacy.jar
Icon=minecraft
Terminal=false
Type=Application
Categories=Game;
```

#### Installing a program for fan control on MSI notebooks

> Download the latest version of the software
> https://github.com/dmitry-s93/MControlCenter/releases/

```sh
sudo ./install.sh
```
