<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->
My arch linux setup for asus laptop features and gaming.

Jump to :  
- [Software](#software)  
- [i3 Window Manager](#i3wm)

<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

# Installation

- Verify the boot mode
```
ls /sys/firmware/efi/efivars
```
- Change the font
```
setfont ter-132n
```

- Connect to the internet
```
ip a
iwctl
device list
station wlan0 scan
station wlan0 get-networks
station wlan0 connect <wifi name>
ping archlinux.org
```

- Update the system clock
```
timedatectl set-ntp true
timedatectl status
timedatectl set-local-rtc 1 --adjust-system-clock
```

- Partition the disks
```
fdisk -l
fdisk /dev/nvme0n1
 t to change the filesystem
 p to print the partitions
 w to write changes to disk
```
  Root (linux filesystem): 50G  
  Boot (EFI partition): 1G  
  Home (linux filesystem): 450G  
  Swap (swap): 20G  

- Format the partitions
```
mkfs.ext4 /dev/root_partition
mkfs.fat -F 32 /dev/efi_system_partition
mkfs.ext4 /dev/home_partition
mkswap /dev/swap_partition
```

- Mount the file systems
```
mount /dev/root_partition
mkdir /mnt/boot
mount /dev/efi_system_partition /mnt/boot
mkdir /mnt/home
mount /dev/home_partition /mnt/home
swapon /dev/swap_partition

df
```

- Install essential packages
```
pacstrap /mnt base base-devel linux linux-firmware
```

- Fstab
```
pacman -S vim

genfstab -U /mnt>>/mnt/etc/fstab
vi /mnt/etc/fstab
```

- Chroot
```
arch-chroot /mnt
```

- Time zone
```
ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
hwclocl --systohc
```

- Localization
```
vim /etc/locale.gen
#uncomment en_US-UTF-8 UTF-8

locale-gen

vim /etc/locale.conf
#add LANG=en_US.UTF-8
```

- Network configuration
```
vim /etc/hostname
#add <hostname>

vim /etc/hosts
#add  127.0.0.1 localhost
      ::1       localhost
      127.0.1.1 <hostname>.localdomain  <hostname>
```

- Root password
```
passwd
```

- Add a user
```
useradd -g users -G wheel -m <username>
passwd <username>
EDITOR=vim visudo
#uncomment wheel ALL=(ALL) ALL
```
- Boot loader
```
pacman -S grub efibootmgr os-prober
grub-install --target=x86_64-efi --efi-directory=/boot/ --bootloader-id=GRUB

mkdir /mnt2
mount /dev/windows_efi_partition /mnt2

vim /etc/default/grub
#uncomment GRUB_DISABLE_OS_PROBER=false

grub-mkconfig -o /boot/grub/grub.cfg
```

- Microcode
```
pacman -S amd-ucode
grub-mkconfig -o /boot/grub/grub.cfg
```

- Install network packages
```
pacman -S sudo networkmanager wireless_tools netctl dialog dhclient && systemctl enable --now NetworkManager && systemctl enable --now bluetooth

#If wifi can't be connected to after reboot: sudo nmcli device wifi connect <wifi> password <password>

sudo vim /etc/NetworkManager/conf.d/dhcp-client.conf
#add [main]
     dhcp=dhclient
```

---
```
reboot
```

<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

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

- Enable multilib and parallel downloads  

      sudo vim /etc/pacman.conf  

- Install firefox and yay  

      sudo pacman -Syu firefox make git && cd && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si && cd ../ && rm -rf yay

- Script to change refresh rate  

      xrandr | grep -q "143.98\*"
      x=`echo $?`
      if [ $x -eq 0 ]
      then
       xrandr -r 60
      else
       xrandr -r 144
      fi


<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

# Graphics drivers

- ## Amd

      sudo pacman -Syu mesa lib32-mesa xf86-video-amdgpu 

- ## Nvidia

      sudo pacman -Syu nvidia-dkms lib32-nvidia-utils && sudo mkdir /etc/pacman.d/hooks && sudo vim /etc/pacman.d/hooks/nvidia.hook  
      
      
    - Add to nvidia.hook  
    

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
    - Reduce power usage
      
          sudo vim /etc/modprobe.d/nvidia.conf
          #add options nvidia "NVreg_DynamicPowerManagement=0x02"
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
#
- Custom fan curves
```
sudo mv /etc/asusd/profile.conf /etc/asusd/profile.conf.old
asusctl fan-curve -m Performance -e true
asusctl fan-curve -m Performance -D 30c:0,40c:90,50c:150,60c:255,70c:255,80c:255,90c:255,100c:255 -e true -f cpu
asusctl fan-curve -m Performance -D 30c:0,40c:90,50c:150,60c:255,70c:255,80c:255,90c:255,100c:255 -e true -f gpu
sudo rm /etc/asusd/profile.conf.old
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
sudo pacman -Syu libreoffice torbrowser-launcher gnome-software-packagekit-plugin bitwarden gnucash kdeconnect libappindicator-gtk3 gnucash kdeconnect ufw qbittorrent simplescreenrecorder grub-customizer alsa-utils $(pacman -Ssq gnome-icon-theme) vlc bluez-utils alsa-utils ntfs-3g gparted dmenu wireshark-qt xterm putty octave dia alacritty gdb virt-manager qemu libvirt edk2-ovmf dnsmasq iptables-nft pavucontrol pinta tk pip gprolog
```
```
yay -S freetube-bin jdownloader2 protonvpn visual-studio-code-bin zoom standardnotes-bin chrome-gnome-shell touchegg i3-gnome gnome-shell-extension-pop-shell-git gns3-gui gns3-server dynamips ubridge vpcs
```
```
sudo pip install tk nltk pandas
```
#
Create syslinks
```
sudo systemctl enable --now ufw && ufw enable && sudo systemctl enable --now touchegg && systemctl enable --now libvirtd.service
```     

#### Software store
- Nextcloud

<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

# Linux gaming

- johncena141's repacks requirements
```
sudo pacman -S --needed giflib gnutls gst-plugins-bad gst-plugins-base gst-plugins-base-libs gst-plugins-good gst-plugins-ugly jq lib32-giflib lib32-gnutls lib32-gst-plugins-base-libs lib32-libjpeg-turbo lib32-libldap lib32-libpng lib32-libxcomposite lib32-libxinerama lib32-libxslt lib32-mpg123 lib32-opencl-icd-loader lib32-sdl2 lib32-v4l-utils libgphoto2 libjpeg-turbo libldap libpng libxcomposite libxinerama libxslt mono mpg123 opencl-icd-loader sdl2 v4l-utils wine-staging libxcrypt-compat alsa-lib alsa-plugins lib32-alsa-lib lib32-alsa-plugins lib32-libpulse libpulse fluidsynth openal lib32-openal wine-mono samba && yay -S dxvk-bin vkd3d-proton-bin rum-bin zpaq 
```
- Steam
```
sudo pacman -S lutris steam ttf-ms-win10-auto wqy-zenhei
```
<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

# i3wm

- Enable Natural Scrolling for touchpad
```
sudo vim /usr/share/X11/xorg.conf.d/40-libinput.conf 
```
```
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
	Option "NaturalScrolling" "True"
EndSection
```

- config file
```
 vim ~/.config/i3/config
```
```
# kill focused window
bindsym $mod+q kill
```

<!--------------------------------------------------------------------------------------------------------------------------------------------------------------->

# Extensions
https://extensions.gnome.org/extension/615/appindicator-support/  
https://extensions.gnome.org/extension/779/clipboard-indicator/  
https://extensions.gnome.org/extension/906/sound-output-device-chooser/  
https://extensions.gnome.org/extension/4099/no-overview/  
https://extensions.gnome.org/extension/4158/gnome-40-ui-improvements/  
https://extensions.gnome.org/extension/4320/asusctl-gex/  
https://extensions.gnome.org/extension/1401/bluetooth-quick-connect/  
https://extensions.gnome.org/extension/355/status-area-horizontal-spacing/  
https://extensions.gnome.org/extension/307/dash-to-dock/  
https://extensions.gnome.org/extension/4033/x11-gestures/  
https://extensions.gnome.org/extension/277/impatience/
https://extensions.gnome.org/extension/545/hide-top-bar/  
https://extensions.gnome.org/extension/517/caffeine/  
https://extensions.gnome.org/extension/904/disconnect-wifi/  
