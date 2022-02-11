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
pacman -S sudo networkmanager wireless_tools netctl dialog && systemctl enable --now NetworkManager && systemctl enable --now bluetooth

#If wifi can't be connected to after reboot: sudo nmcli device wifi connect <wifi> password <password>

sudo vim /etc/NetworkManager/conf.d/dhcp.conf
#add [main]
     dhcp=dhclient
```

---
```
reboot
```
