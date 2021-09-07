# Change resolution in VMWare for Arch Linux

- install open-vm-tools
```
sudo pacman -S open-vm-tools
```
- install necessary packages
```
sudo pacman -S xf86-input-vmmouse xf86-video-vmware mesa gtk2 gtkmm
```
- create service
```
sudo systemctl enable vmtoolsd.service
sudo systemctl start vmtoolsd.service
```
## Reference
[VMTools on Arch Linux](https://www.reddit.com/r/archlinux/comments/b0ona0/vmtools_on_arch_linux_full_screen_or_resizing/)
