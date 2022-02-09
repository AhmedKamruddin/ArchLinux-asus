# Arch-Install
My personal arch linux setup

sudo vim /etc/pacman.conf && sudo pacman -Syu base-devel make git && cd && rm -rf yay && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si && cd ../ && rm -rf yay && sudo pacman -S firefox mpv bluedevil && yay -S standardnotes-desktop  && cd standardnotes-desktop && makepkg -si && cd ../ && rm -rf standardnotes-desktop
