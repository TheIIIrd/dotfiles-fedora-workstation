# FEDORA-CUSTOM-SETUP
One more way to customize fedora

## Working with software

### Deleting unnecessary repositories

Perhaps not all repositories "out of the box" should be left on the system.

```sh
dnf repolist
sudo dnf config-manager --set-disabled <repo>
sudo dnf clean all && sudo dnf check-update
```

### Package manager speedup

The package manager is initially quite slow. These settings will speed it up.

```sh
sudo nano /etc/dnf/dnf.conf

# add these lines to the config
max_parallel_downloads=8
fastestmirror=True
defaultyes=True
```

### Uninstalling software

Some of the out-of-the-box software I never needed. So, I just delete it.

```sh
sudo dnf remove gnome-contacts gnome-weather gnome-maps gnome-calendar cheese totem && sudo dnf autoremove
```

### Installing useful software

Basic packages that we will need as we work with the system.

```sh
sudo dnf install git wget curl inxi zsh neovim fastfetch java-latest-openjdk python python3-pip gcc-c++ clang

# choose the latest version of java
sudo update-alternatives --config java
```

### Adding community repositories

The community repository allows you to greatly expand the list of available software for the system.

```sh
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

sudo dnf config-manager --enable fedora-cisco-openh264

sudo dnf groupupdate core
```

### NVIDIA graphics card driver installation

When installing drivers, it is very important to keep an eye on CPU load.

```sh
# open gnome-system-monitor and monitor CPU load
sudo dnf install akmod-nvidia
sudo dnf install xorg-x11-drv-nvidia-cuda
```

Reboot and check inxi

```sh
inxi -G
```

### Adding a flathub repository

Flathub is used further to install some programs.

```sh
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

### Basic software installation

This is the base.

```sh
sudo dnf install steam gamemode torbrowser-launcher transmission-gtk vlc inkscape krita gparted gnome-tweaks dconf-editor
```

### Virt-manager installation

One of the best virtual machine managers.

```sh
sudo dnf install -y qemu-kvm libvirt virt-install bridge-utils virt-manager libvirt-devel libguestfs-tools guestfs-tools
sudo usermod -G libvirt -a <username>
sudo systemctl enable libvirtd
```

### Zerotier installation

A free way to deploy a small network to communicate with computers outside the local network.

```sh
sudo dnf install zerotier-one
sudo systemctl enable zerotier-one.service
```

### Flatpak application installation

A huge suite of software for work and play.

```sh
flatpak install flathub com.github.tchx84.Flatseal org.kde.kdenlive org.onlyoffice.desktopeditors com.orama_interactive.Pixelorama com.github.Matoking.protontricks com.vscodium.codium io.github.realmazharhussain.GdmSettings com.mattjakeman.ExtensionManager com.vysp3r.ProtonPlus com.heroicgameslauncher.hgl dev.vencord.Vesktop
```

### Installing mod launcher for games (especially important for Risk Of Rain 2)

Native launcher for modding games.

> Download the latest release of the program
> https://github.com/ebkr/r2modmanPlus/releases

```sh
sudo rpm -ivh <package_name>

# or use AppImage
./<package_name>.AppImage
```

> Create desktop shortcut

```sh
nvim .local/share/applications/r2modman.desktop

# add it to the file
[Desktop Entry]
Name=R2modman
Comment=Infinite mod generator
Exec=/path/to/r2modman.AppImage --no-sandbox
Icon=fuse-emulator
Terminal=false
Type=Application
Categories=Game;
MimeType=x-scheme-handler/ror2mm;
```

### Installing the native engine for the game STALKER

Engine for the STALKER series of games. Makes the game native.

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
sudo rpm -ivh <package_name>
```

### Installing the open source version of the Minecraft Launcher

Launcher with open source code and convenient settings.

> Download the latest release of the program (we need a .jar version of the game) (! maybe to old !)
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

### Installing a program for fan control on MSI notebooks

A system for managing special parameters on MSI laptops.

> Download the latest version of the software
> https://github.com/dmitry-s93/MControlCenter/releases/

```sh
sudo ./install.sh
```

## System customization

### Icon setup

> To make icons available to all users of the system, instead of ```./install.sh standard```, run ```sudo ./install.sh standard```.

```sh
# traditional icons
git clone https://github.com/vinceliuice/Tela-icon-theme.git
cd Tela-icon-theme
./install.sh standard

# rounded icons
git clone https://github.com/vinceliuice/Tela-circle-icon-theme.git
cd Tela-circle-icon-theme
./install.sh standard
```

### Installing the unofficial GTK3 port of libadwaita

```sh
sudo dnf install adw-gtk3-theme

# adding support in flatpak applications
flatpak install org.gtk.Gtk3theme.adw-gtk3 org.gtk.Gtk3theme.adw-gtk3-dark
```

### List of useful GNOME extensions

```
# changing keyboard layout without a popup window
quick-lang-switch@ankostis.gmail.com

# application icon running in the background
appindicatorsupport@rgcjonas.gmail.com

# weather display for your region
openweather-extension@jenslody.de

# Gnome Shell interface blur
blur-my-shell@aunetx

# clipboard manager for Gnome Shell
clipboard-indicator@tudmotu.com

# sleep mode on/off
caffeine@patapon.info

# system performance monitoring
Vitals@CoreCoding.com

# advanced Gnome Shell settings
just-perfection-desktop@just-perfection
```

### ZSH installation and configuration

Install zsh and run it.

> Zsh will ask you to configure it after the first run.
> Look at all the items and click 0 in each if you are satisfied.

```sh
sudo dnf install zsh

# how to run
zsh
```

> This font supports icons.
> It is necessary for correct output of information in the terminal.

```sh
wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf

sudo mv MesloLGS\ NF\ Regular.ttf /usr/share/fonts/
```

### Oh-my-zsh framework

Oh-my-zsh will extend the capabilities of regular zsh.

```sh
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Plugins for zsh

These extensions will provide hints while using zsh.

```sh
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

> After installing the zsh extensions, you should put them in ```.zshrc```.
> Open the config and find the line ```plugins=(git)```.
> It should be changed to ```plugins=(git zsh-autosuggestions zsh-syntax-highlighting)```.

```sh
nvim .zshrc

plugins=(git zsh-autosuggestions zsh-syntax-highlighting)

# restart the config
source .zshrc
```

### P10K theme

P10k will make zsh more beautiful.

```sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

> After installing the 10k, you should put them in ```.zshrc```.
> You must replace the value of ZSH_THEME with ```ZSH_THEME="powerlevel10k/powerlevel10k"```.

```sh
nvim .zshrc

ZSH_THEME="powerlevel10k/powerlevel10k"

# restart the config
source .zshrc
```
