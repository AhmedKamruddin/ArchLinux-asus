# Arch-Install
My personal arch linux setup

sudo vim /etc/pacman.conf && sudo pacman -Syu base-devel make git && cd && rm -rf yay && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si && cd ../ && rm -rf yay && sudo pacman -S firefox mpv && yay -S standardnotes-bin && sudo pacman -S mesa lib32-mesa	vulkan-radeon	lib32-vulkan-radeon	xf86-video-amdgpu nvidia-dkms lib32-nvidia-utils && sudo mkdir /etc/pacman.d/hooks && sudo vim /etc/pacman.d/hooks/nvidia.hook

[Trigger]

Operation=Install

Operation=Upgrade

Operation=Remove

Type=Package

Target=nvidia

Target=linux

\# Change the linux part above and in the Exec line if a different kernel is used

[Action]

Description=Update Nvidia module in initcpio

Depends=mkinitcpio

When=PostTransaction

NeedsTargets

Exec=/bin/sh -c 'while read -r trg; do case $trg in linux) exit 0; esac; done; /usr/bin/mkinitcpio -P'



sudo vim /etc/pacman.conf
[g14]

SigLevel = DatabaseNever Optional TrustAll

Server = https://arch.asus-linux.org

sudo pacman -Syu asusctl linux-g14	linux-g14-headers supergfxctl && sudo systemctl enable --now power-profiles-daemon.service && sudo systemctl enable --now supergfxd && sudo vim /etc/makepkg.conf

MAKEFLAGS="-j$(nproc)"

yay -S linux-xanmod-rog linux-xanmod-rog-headers && grub-mkconfig -o /boot/grub/grub.cfg

yay -S freetube-bin jdownloader2 protonvpn visual-studio-code-bin && sudo pacman -Syu libreoffice torbrowser-launcher

