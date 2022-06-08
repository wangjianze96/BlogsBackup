# Compile ngspice on Ubuntu 20.04

## Prerequiest
### Utilies
- libreadline-dev
- libx11-dev (optional, required if X display is needed)
- libeditline-dev
- libxaw7-dev (optional, required if X display is needed)
- libtool
- libncurses5-dev
- autoconf
- automake
- adms (optional, required if AMS model is needed)
- bison
- flex
- gcc
- g++
- make
### Tarball/zip packages
- [ngspice-37.tar.gz](https://sourceforge.net/projects/ngspice/files/ng-spice-rework/37/)
- [ngspice-adms-va.7z](https://sourceforge.net/projects/ngspice/files/ng-spice-rework/37/) (optional, required if AMS model is needed)

## Installation
Decription can be found in the official manual in Chapter 32. I provide more detailed instructions for the installation.
1. Go to the folder of the compressed file and expand the tarball
```
tar xzvf ngspice-37.tar.gz
```

2. (Optianal) Expand the `ngspice-adms-va.7z` into the `ngspice-37/src/spicelib/devices/adms`

3. (Optional) Use autogen.sh to generate configurations for adms models
```
./autogen.sh --adms
```

4. Configure installation options
```
./configure --with-x --enable-xspice --enable-cider --enable-openmp --enable-adms --disable-debug --with-readline=yes --prefix=/path/you/want/to/install
```

5. Make
```
make clean
make
```

7. Install
```
sudo make install
```
