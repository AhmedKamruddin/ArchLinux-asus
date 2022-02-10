- Enable multilib  

      sudo vim /etc/pacman.conf  

- Install yay  

      sudo pacman -Syu make git && cd && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si && cd ../ && rm -rf yay
<!--------------------------------------------------------------------------------------------------------------->
# Install graphics drivers

- ## Amd

      sudo pacman -Syu mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon xf86-video-amdgpu 

- ## Nvidia

      sudo pacman -Syu nvidia-dkms lib32-nvidia-utils && sudo mkdir /etc/pacman.d/hooks && sudo vim /etc/pacman.d/hooks/nvidia.hook  
      
      
     _Add to nvidia.hook_  
    

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
<!--------------------------------------------------------------------------------------------------------------->
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
<!--------------------------------------------------------------------------------------------------------------->
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
<!--------------------------------------------------------------------------------------------------------------->
# Install packages

    sudo pacman -Syu libreoffice torbrowser-launcher gnome-software-packagekit-plugin bitwarden gnucash kdeconnect libappindicator-gtk3 gnucash kdeconnect variety ufw qbittorrent simplescreenrecorder grub-customizer alsa-utils $(pacman -Ssq gnome-icon-theme) mpv bluez-utils 
---

    yay -S freetube-bin jdownloader2 protonvpn visual-studio-code-bin zoom standardnotes-bin

---
Create syslinks

    sudo systemctl enable --now ufw && sudo systemctl enable --now bluetooth
     

#### Software store
- Nextcloud


# To run games on Linux
    sudo pacman -S wine-staging && sudo pacman -S --needed alsa-lib alsa-plugins cabextract fluidsynth giflib gnutls gst-plugins-bad gst-plugins-base gst-plugins-base-libs gst-plugins-good gst-plugins-ugly jq lib32-alsa-lib lib32-alsa-plugins lib32-giflib lib32-gnutls lib32-gst-plugins-base-libs lib32-libjpeg-turbo lib32-libldap lib32-libpng lib32-libpulse lib32-libxcomposite lib32-libxinerama lib32-libxslt lib32-mpg123 lib32-openal lib32-opencl-icd-loader lib32-sdl2 lib32-v4l-utils libgphoto2 libjpeg-turbo libldap libpng libpulse libxcomposite libxinerama libxslt mono mpg123 openal opencl-icd-loader sdl2 v4l-utils winetricks wine-mono samba && yay -S dxvk-bin vkd3d-proton-bin rum-bin

# Blender and UnityHub
    sudo pacman -S blender && yay -S unityhub

# Steam
    sudo pacman -S lutris steam ttf-liberation wqy-zenhei
