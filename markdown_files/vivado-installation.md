# Vivado installation on Ubuntu 20.04.3 LTS

## Problem description
These days I was trying to help another guy to construct a time-to-digital converter using FPGA.
So I needed vivado to build the circuit netlist and run some simulation. I actually have already
installed vivado on my PC and we also have vivado available on the cluster. But I wanted to try
to install vivado on Linux by myself. So I wept out my Arch Linux and reinstall Ubuntu on my office
PC. Then I tried to install vivado. The previous steps run perfectly, but after downloading and
installation, it was performing a step called "generating installed device list" and then it stucked.
I first thought maybe it's some kind of permission issue, so I canceled the installation and reinstalled
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

## Additional Information for 2019.2
As written at https://support.xilinx.com/s/article/63794?language=en_US :

The following packages were reported by customers as needing to be installed on Ubuntu 64bit to get the Documentation Navigator running.

You can install the following packages using apt-get (not just for the Documentation Navigator)

 

sudo apt-get install libstdc++6:i386

sudo apt-get install libgtk2.0-0:i386

sudo apt-get install dpkg-dev:i386

 

The SDK requires gmake, but Ubuntu only contains make (it is the same binary, but a different filename).

gmake needs to be created as a link:

 

sudo ln -s /usr/bin/make /usr/bin/gmake

 

On a minimal Ubuntu install you will need to install pip to work with Python packages:

 

apt install python3-pip

 

Install these missing Ubuntu packages:

apt install libtinfo5 libncurses5

Without libtinfo5 Vivado will not start.
Without libncurses5 simulation fails.
After running the Vivado installer on Linux you will need to install cable drivers as root.

If you do not install cable drivers you will not be able to connect to boards.

This is the case for all Linux installs, not just Ubuntu.

Installation steps:

### change directory to your Vivado install, for example:

cd /opt/Xilinx/Vivado/2019.2

 

### cd into the drivers directory (the script MUST be run there)

cd data/xicom/cable_drivers/lin64/install_script/install_drivers

 

### run the cable installer with root privileges

sudo ./install_drivers
