# Arch-Install
My personal arch linux setup

sudo vim /etc/pacman.conf && sudo pacman -Syu base-devel make git && cd && rm -rf yay && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si && cd ../ && rm -rf yay && sudo pacman -S firefox mpv && yay -S standardnotes-bin && sudo pacman -S mesa lib32-mesa	vulkan-radeon	lib32-vulkan-radeon	xf86-video-amdgpu nvidia-dkms lib32-nvidia-utils && sudo vim /etc/pacman.d/hooks/nvidia.hook && sudo echo -e "[g14]\nSigLevel = DatabaseNever Optional TrustAll\nServer = https://arch.asus-linux.org">>/etc/pacman.conf && sudo pacman -Syu asusctl linux-xanmod-rog	linux-xanmod-rog-headers supergfxctl

