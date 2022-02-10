# Arch-Install
My personal arch linux setup

sudo systemctl enable --now bluetooth && sudo vim /etc/pacman.conf && sudo pacman -Syu base-devel make git $(pacman -Ssq gnome-icon-theme) && cd && rm -rf yay && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si && cd ../ && rm -rf yay && sudo pacman -S firefox mpv bluez-utils && yay -S standardnotes-bin && sudo pacman -S mesa lib32-mesa	vulkan-radeon	lib32-vulkan-radeon	xf86-video-amdgpu nvidia-dkms lib32-nvidia-utils && sudo mkdir /etc/pacman.d/hooks && sudo vim /etc/pacman.d/hooks/nvidia.hook

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

yay -S linux-xanmod-rog linux-xanmod-rog-headers && grub-mkconfig -o /boot/grub/grub.cfg && reboot

yay -S freetube-bin jdownloader2 protonvpn visual-studio-code-bin && sudo pacman -Syu libreoffice torbrowser-launcher gnome-software-packagekit-plugin bitwarden gnucash kdeconnect libappindicator-gtk3 gnucash kdeconnect variety

nextcloud(software store)

sudo pacman -Syu ufw qbittorrent simplescreenrecorder grub-customizer alsa-utils && sudo systemctl enable --now ufw && yay -S zoom

for microphone not detected	https://wiki.archlinux.org/title/PulseAudio/Troubleshooting#Microphone_not_detected_by_PulseAudio

# To run games on Linux
sudo pacman -S wine-staging && sudo pacman -S --needed alsa-lib alsa-plugins cabextract fluidsynth giflib gnutls gst-plugins-bad gst-plugins-base gst-plugins-base-libs gst-plugins-good gst-plugins-ugly jq lib32-alsa-lib lib32-alsa-plugins lib32-giflib lib32-gnutls lib32-gst-plugins-base-libs lib32-libjpeg-turbo lib32-libldap lib32-libpng lib32-libpulse lib32-libxcomposite lib32-libxinerama lib32-libxslt lib32-mpg123 lib32-openal lib32-opencl-icd-loader lib32-sdl2 lib32-v4l-utils libgphoto2 libjpeg-turbo libldap libpng libpulse libxcomposite libxinerama libxslt mono mpg123 openal opencl-icd-loader sdl2 v4l-utils winetricks wine-mono samba && yay -S dxvk-bin vkd3d-proton-bin rum-bin

# Blender and UnityHub
sudo pacman -S blender && yay -S unityhub

# Steam
sudo pacman -S lutris steam ttf-liberation wqy-zenhei
