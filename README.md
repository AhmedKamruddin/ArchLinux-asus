<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->
[Arch Installation](Installation.md)  
[Software](https://github.com/AhmedKamruddin/My-Arch-Linux/blob/main/README.md#software)

# Post-installation

```
sudo pacman -Syu terminus-font && setfont ter-132n
```

- Display server
```
sudo pacman -S xorg xorg-xinit
```

- Desktop environment
```
sudo pacman -S archlinux-keyring gnome gnome-tweaks && sudo systemctl enable gdm
```

- Window manager
```
sudo pacman -S i3
```

---
```
reboot
```

- Enable multilib  

      sudo vim /etc/pacman.conf  

- Install firefox and yay  

      sudo pacman -Syu firefox make git && cd && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si && cd ../ && rm -rf yay
      
<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

# Graphics drivers

- ## Amd

      sudo pacman -Syu mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon xf86-video-amdgpu 

- ## Nvidia

      sudo pacman -Syu nvidia-dkms lib32-nvidia-utils && sudo mkdir /etc/pacman.d/hooks && sudo vim /etc/pacman.d/hooks/nvidia.hook  
      
      
     Add to nvidia.hook  
    

      [Trigger]  
      Operation=Install  
      Operation=Upgrade  
      Operation=Remove  
      Type=Package  
      Target=nvidia-dkms  
      Target=linux-xanmod-rog  
      #Change the linux part above and in the Exec line if a different kernel is used  

      [Action]  
      Description=Update Nvidia module in initcpio  
      Depends=mkinitcpio  
      When=PostTransaction  
      NeedsTargets  
      Exec=/bin/sh -c 'while read -r trg; do case $trg in linux-xanmod-rog) exit 0; esac; done; /usr/bin/mkinitcpio -P'              

---

    reboot
    
<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

# asus-linux  

- Add g14 repository to pacman.conf

```
sudo vim /etc/pacman.conf
```
```
[g14]  
SigLevel = DatabaseNever Optional TrustAll  
Server = https://arch.asus-linux.org  
```
#
 - Install asus packages
```
sudo pacman -Syu asusctl linux-g14 linux-g14-headers supergfxctl && sudo systemctl enable --now power-profiles-daemon.service && sudo systemctl enable --now && grub-mkconfig -o /boot/grub/grub.cfg supergfxd 
```

<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

# linux-xanmod-rog

- Edit makeflags variable to use parallel compilation
```
sudo vim /etc/makepkg.conf
```
```
MAKEFLAGS="-j$(nproc)"
```
#
- Install linux-xanmod-rog

      yay -S linux-xanmod-rog linux-xanmod-rog-headers && grub-mkconfig -o /boot/grub/grub.cfg
      
<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

# Software

[List of packages](Packages.txt)  
```
sudo pacman -Syu libreoffice torbrowser-launcher gnome-software-packagekit-plugin bitwarden gnucash kdeconnect libappindicator-gtk3 gnucash kdeconnect variety ufw qbittorrent simplescreenrecorder grub-customizer alsa-utils $(pacman -Ssq gnome-icon-theme) mpv bluez-utils alsa-utils blender ntfs-3g gparted
```
```
yay -S freetube-bin jdownloader2 protonvpn visual-studio-code-bin zoom standardnotes-bin unityub chrome-gnome-shell touchegg
```
#
Create syslinks
```
sudo systemctl enable --now ufw && ufw enable && sudo systemctl eenable --now touchegg
```     

#### Software store
- Nextcloud

<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

# Linux gaming

- johncena141's repacks requirements
```
sudo pacman -S wine-staging && sudo pacman -S --needed alsa-lib alsa-plugins cabextract fluidsynth giflib gnutls gst-plugins-bad gst-plugins-base gst-plugins-base-libs gst-plugins-good gst-plugins-ugly jq lib32-alsa-lib lib32-alsa-plugins lib32-giflib lib32-gnutls lib32-gst-plugins-base-libs lib32-libjpeg-turbo lib32-libldap lib32-libpng lib32-libpulse lib32-libxcomposite lib32-libxinerama lib32-libxslt lib32-mpg123 lib32-openal lib32-opencl-icd-loader lib32-sdl2 lib32-v4l-utils libgphoto2 libjpeg-turbo libldap libpng libpulse libxcomposite libxinerama libxslt mono mpg123 openal opencl-icd-loader sdl2 v4l-utils winetricks wine-mono samba && yay -S dxvk-bin vkd3d-proton-bin rum-bin
```
- Steam
```
sudo pacman -S lutris steam ttf-liberation wqy-zenhei
```
<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

# Extensions
https://extensions.gnome.org/extension/615/appindicator-support/  
https://extensions.gnome.org/extension/779/clipboard-indicator/  
https://extensions.gnome.org/extension/906/sound-output-device-chooser/  
https://extensions.gnome.org/extension/1112/screenshot-tool/  
https://extensions.gnome.org/extension/4099/no-overview/  
https://extensions.gnome.org/extension/4158/gnome-40-ui-improvements/  
https://extensions.gnome.org/extension/4320/asusctl-gex/  
https://extensions.gnome.org/extension/1401/bluetooth-quick-connect/  
https://extensions.gnome.org/extension/355/status-area-horizontal-spacing/  
https://extensions.gnome.org/extension/307/dash-to-dock/  
https://extensions.gnome.org/extension/4033/x11-gestures/  
