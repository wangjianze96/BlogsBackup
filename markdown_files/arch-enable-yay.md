# Install yay helper in Arch Linux

1. clone yay repostory from [AUR](https://aur.archlinux.org/)
```
$ git clone https://aur.archlinux.org/yay.git
```

2. change to yay folder and install yay
```
$ cd yay/
$ makepkg -si
```

3. install package from AUR
```
$ yay -Syu
$ yay -S google-chrome
```