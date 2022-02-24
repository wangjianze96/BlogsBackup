# Vivado installation on Ubuntu 20.04.3 LTS

## Problem description
These days I was trying to help another guy to construct a time-to-digital converter using FPGA.
So I needed vivado to build the circuit netlist and run some simulation. I actually have already
installed vivado on my PC and we also have vivado available on the cluster. But I wanted to try
to install vivado on Linux by myself. So I wept out my Arch Linux and reinstall Ubuntu on my office
PC. Then I tried to install vivado. The previous steps run perfectly, but after downloading and
installation, it was perfroming a step called "generating device list" and then it stucked. I first
thought maybe it's some kind of permission issue, so I canceled the installation and reinstalled
vivado. I knew this step can be time consuming so I just left it and went home. Over a night I came
back to office and the same issue was still there.

## Possible solution

Base on [this pose](https://support.xilinx.com/s/question/0D52E00006hpRxQSAU/vivado-20202-installation-stuck-at-generating-installed-device-list-on-ubuntu-2004lts?language=en_US)
and [this pose](https://support.xilinx.com/s/question/0D52E00006iHjbcSAC/vivado-20211-installation-hangs-at-generating-installed-device-list?language=en_US),
some addition libraries are needed for this installation.
```
sudo apt install libncurses5
sudo apt install libncurses5-dev
sudo apt install libncursesw5-dev
sudo apt install libtinfo5
sudo apt install libtinfo-dev
```

There is another package mentioned is `ncurses-compat-libs`, but I cannot install it since in ubuntu 20.04.3 it gives me unable to locate error.

## Result

Still ongoing installation, will update if it works.
