# My Dotfiles!
As an Arch Linux user myself, I passed most of my time in university customizing and
finding the perfect desktop environment. In this process, I tried and tested multiple
ones till I arrived to my current configuration. My focus has been on constructing a
functional and easy to use desktop experience, with aesthetics more of an after
thought.

![My Environment](.local/share/assets/my-desktop.png)

In this repository, you'll find all the configuration of my desktop that you can use
for creating your own configuration files or, at least, get some inspiration on
creating your own. My desktop is mainly built by:

- [Qtile](https://github.com/qtile/qtile): A highly customizable tilling window manager
written in python.
- [Alacritty](https://github.com/alacritty/alacritty): A fast terminal emulator written
in rust.
- [Nvim](https://github.com/neovim/neovim): A fork of text editor vim improving
maintenaibility and extensability of the original project.
- [Dracula](https://github.com/dracula): A dark color scheme focused on usability.

And many more to compose a full desktop experience.

## Setup
To setup my dotfiles, first clone the bare repository in the desired place.

```bash
git clone --bare git@github.com:juanscr/arch_qtile_dotfiles.git
```

Then, use the following command to get all the files from the repository and, for
usability, stop showing files not uploaded to git.

```bash
alias dfiles="/usr/bin/git --git-dir=path/to/repository --work-tree=$HOME"
dfiles config status.showUntrackedFiles no
dfiles restore --staged .
```

And there you go. Happy tinkering!

## Common Configuration Steps
In this section, you will find some common steps that I use in my computer to have
a fully working desktop experience. Although many of the information here can be found
in the Arch Wiki, I know first hand it is difficult to take in all the information
that the wiki presents at once; as such, the information presented here is a quick
summary of what I usually need to run on a freh installation of Arch.

### Installation Guide Annotations
When running the pacstrap command, is common to forget some basic packages that are
completely needed to have a fully working installation. For a minimal installation, I
would add the following packages to be installed so when you log in to the TTY some
basic functionality can be executed.

- `networkmanager`: Automatic configuration for network interfaces.
- `sudo`: Privilige escalation.
- `neovim`: Text editor.
- `grub`: Boot loader, for uefi support install `efibootmgr`.
- `man-db`, `man-pages`, `texinfo`: Package information.
- `intel-ucode`: Microcode for intel CPUs.
- `openssh`: SSH client.

Additionaly, the packages that I selected to construct my fully customized desktop
environment are:

- `git`: Version control.
- `base-devel`: AUR manager.
- `network-manager-applet`: Tray icon for network manager.
- `sddm`: Display manager.
- `qtile`: Tilling window manager, for using my configuration also install `python-psutil` and `python-dbus-next`.
- `dunst`: Notification daemon.
- `flameshot`: Screenshot tool.
- `chromium`: My preferred browser.
- `pulseaudio`: Audio control, for bluetooth and cli management install respectively `pulseaudio-bluetooth` and `pamixer`.
- `pavucontrol`: GUI for pulseaudio settings.
- `arandr`: UI for xrandr settings.
- `autorandr`: Automatic xrandr configuration by saving monitor configuration.
- `blueman`, `bluez` and `bluez-utils`: Bluetooth support and tray management.
- `polkit-gnome` and `polkit`: Privilige escalation.
- `playerctl`: Media controls.
- `spotify-launcher`: Spotify media launcher.
- `vlc`: Media player.
- `alacritty`: Rust terminal.
- `picom`: Compositor for X11.
- `chrony`: NTP time syncing.
- `xf86-input-libinput`: Touchpad customization.
- `bash-completion`: Autocompletion in bash.
- `hdparm`: Set hard drive parameters.
- `zathura`: PDF viewer.
- `nsxiv`: Simple image viewer.
- `exa`: ls but better.
- `feh`: Setup image background.
- `pcmanfm`: GUI File explorer.
- `conky` and `htop`: System monitoring.

On the other hand, I install fonts, gtk and qt themes so I can highly customize my
desktop environment look and feel. For that, I run:

- `ttf-jetbrains-mono` and `ttf-jetbrains-mono-nerd`: Main fonts I use.
- `gnome-themes-extra`: GTK theme for Adwaita Dark.
- `breeze`: QT Theme for Breeze Dark.
- `xcursor-vanilla-dmz`: Cursor theme.
- `papirus-icon-theme`: Pretty icon theme
- For other fonts, I install `ttf-dejavu`, `gnu-free-fonts`, `adobe-source-code-pro-fonts`, `cantarell-fonts`, `ttf-liberation`, `ttf-bitstream-vera`, `ttf-droid`, `noto-fonts`, `ttf-croscore`, `ttf-ibm-plex`.

Lastly, some additional packages I install outside the mainline arch repositories are:

- [yay](https://github.com/Jguer/yay): AUR package manager.
- [betterlockscreen](https://github.com/betterlockscreen/betterlockscreen): Beautiful lock screen.
- [rate-mirrors](https://github.com/westandskif/rate-mirrors): Rate arch mirros for download speed.
- [networkmanager-dispatcher-chrony](https://aur.archlinux.org/packages/networkmanager-dispatcher-chrony): Dispatch time sync to chrony when online.
- [qt5ct-kde](https://aur.archlinux.org/packages/qt5ct-kde): QT5ct for improved KDE compatibility.
- [dmenu](https://github.com/juanscr/dmenu): My own custom fork of dmenu.

### Time Internet Synching
When I first did my Arch installation I notice that, as time passed on, the clock that
was showing on my desktop was slowly getting out of sync of the real time. That occured
because I didn't configure any network time syncing mechanisms to constantly sync the
time. For network time syncing, NTP is not suggested as laptops do not have a permanent
network connection and is really slow for time syncing. Instead use `chrony` that is
specifically designed to combat this issues. The Colombian NTP servers can be seen
[this page](https://www.ntppool.org/zone/co).

To activate the servers when I'm connected to a network, I use the following
[aur package](https://aur.archlinux.org/packages/networkmanager-dispatcher-chrony/).
Remember to install the `chrony` package and enable `chronyd.service`;
additionally, disable and stop `systemd-timesyncd` for enabling chrony properly.
My chrony configuration file is:

```
# Colombia NTP servers
server 3.co.pool.ntp.org iburst offline
server 0.south-america.pool.ntp.org iburst offline
server 1.south-america.pool.ntp.org iburst offline

rtcsync

# Drift file
driftfile /var/lib/chrony/drift

# If you specify an NTP server with the nts option to enable authentication
# with the Network Time Security (NTS) mechanism, or enable server NTS with
# the ntsservercert and ntsserverkey directives below, the following line will
# allow the client/server to save the NTS keys and cookies in order to reduce
# the number of key establishments (NTS-KE sessions).

ntsdumpdir /var/lib/chrony

# If the system timezone database is kept up to date and includes the
# right/UTC timezone, chronyd can use it to determine the current
# TAI-UTC offset and when will the next leap second occur.

leapsectz right/UTC

# INITIAL CLOCK CORRECTION
makestep 1.0 3
```

### Nvidia Optimus
On my personal laptop, I have an integrated Intel Graphics card with a discrete Nvidia
GeForce GTX 1050. This setup is common in laptops to improve battery life, by mostly
using the intel graphics card and delivering the more intensive tasks to the nvidia
card. Although the easiest route is to make the Nvidia card the default one and turn
off the Intel one (which I used for many years), the battery life clocked below an
hour and the computer had really high temperatures most of the time.

For that, I switched to use PRIME GPU Offloading in order to just use the Nvidia GPU
when needed (i.e., running steam and other intensive tasks). The related Arch wiki
pages that cover how to configure it are:

- [Prime Offload](https://wiki.archlinux.org/title/PRIME#PRIME_render_offload).
- [NVIDIA driver wiki](https://wiki.archlinux.org/title/NVIDIA).

First, I installed the respective graphics driver for Nvidia and Intel with vulkan
support (with their respective 32 bits libraries from the
[multilib](https://wiki.archlinux.org/title/official_repositories#multilib)
repository):

```
pacman -S \
    nvidia nvidia-utils lib32-nvidia-utils \
    mesa intel-media-driver lib32-mesa \
    vulkan-intel lib32-vulkan-intel \
    nvidia-prime
```

Then, I removed `kms` from the `HOOKS` key in `/etc/mkinitcpio.conf` to avoid booting
with the `nouveau` module (open source nvidia driver). Then regenerate the initramfs
by:

```
mkinitcpio -P
```

After that, I created the `pacman` hook in order to always regenerate the initramfs
when updating the `nvidia` package. By that I created the
`/etc/pacman.d/hooks/nvidia.hook` with:

```
[Trigger]
Operation=Install
Operation=Upgrade
Operation=Remove
Type=Package
Target=nvidia
Target=linux
# Change the linux part above if a different kernel is used

[Action]
Description=Update NVIDIA module in initcpio
Depends=mkinitcpio
When=PostTransaction
NeedsTargets
Exec=/bin/sh -c 'while read -r trg; do case $trg in linux*) exit 0; esac; done; /usr/bin/mkinitcpio -P'
```

Then, I executed a reboot to load the `nvidia` propietary driver. For veryfing that it
worked as I expected, I did the following commands and got the correct output:

![Prime Test](.local/share/assets/prime-run-test.png)
