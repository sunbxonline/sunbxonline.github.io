---
layout: page
title: 在Ubuntu16.04上编译运行weston
image: https://wayland.freedesktop.org/wayland.png
hero_image: https://www.bing.com/th?id=OHR.OtterCreekVT_ZH-CN0564511657_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp
toc: true
tags: weston wayland
published: true
---

<!--more-->

### 设置环境变量
```
export WLD=$HOME/install
export LD_LIBRARY_PATH=$WLD/lib
export PKG_CONFIG_PATH=$WLD/lib/pkgconfig/:$WLD/share/pkgconfig/
export PATH=$WLD/bin:$PATH
```

> WLD指定了编译后的安装目录 \
LD_LIBRARY_PATH指定了编译时lib查找的路径 \
PKG_CONFIG_PATH指定了运行时so的查找路径

### prepare
```
# ./autogen.sh: 7: ./autogen.sh: autoreconf: not found
sudo apt install autoconf

# Makefile.am:176: error: Libtool library used but 'LIBTOOL' is undefined
sudo apt install libtool

# No package 'libffi' found
sudo apt install libffi-dev

# configure: error: Can't find expat.h. Please install expat.
sudo apt install libexpat1-dev

# configure: error: Package requirements (libxml-2.0) were not met:
sudo apt install libxml2-dev

sudo apt install doxygen xmlto graphviz
```

### install wayland
```
git clone git://anongit.freedesktop.org/wayland/wayland
cd wayland
./autogen.sh --prefix=$WLD # --disable-documentation
make check
make && make install
cd ..
```

### compile and install wayland-protocols
```
git clone git://anongit.freedesktop.org/wayland/wayland-protocols
cd wayland-protocols
./autogen.sh --prefix=$WLD
make check
make && make install
cd ..
```

### compile and install libinput
+ prepare

> sudo apt install libmtdev-dev libudev-dev libevdev-dev libwacom-dev

+ install pip3 first

+ install meson

> pip3 install --user meson

```
git clone git://anongit.freedesktop.org/wayland/libinput
cd libinput
meson build/ --prefix=$WLD
ninja -C build/
cd build
ninja install
```

### compile and install weston
+ prepare

> sudo apt install libgles2-mesa-dev libxcb-composite0-dev libxcursor-dev \
  libcairo2-dev libgbm-dev libpam0g-dev

```
git clone git://anongit.freedesktop.org/wayland/weston
cd weston
meson build/ --prefix=$WLD -Dbackend-drm-screencast-vaapi=false -Dbackend-rdp=false -Dcolor-management-colord=false -Dsystemd=false -Dremoting=false -Dpipewire=false
ninja -C build/
cd build
ninja install
```

### 运行

在$WLD/bin目录下运行weston

### 编译参考

+ 在ubuntu 16.04上的编译参考

https://wayland.freedesktop.org/building.html
https://wayland.freedesktop.org/ubuntu16.04.html
https://gitlab.freedesktop.org/wayland/weston
https://github.com/wayland-project/wayland

+ upgrade gtk from 3.18 to 3.20

```
sudo add-apt-repository ppa:gnome3-team/gnome3-staging
sudo add-apt-repository ppa:gnome3-team/gnome3
sudo apt update
sudo apt dist-upgrade
```

https://askubuntu.com/questions/933010/how-to-upgrade-gtk-3-18-to-3-20-on-ubuntu-16-04

+ meson

https://www.cnblogs.com/xleng/p/10795952.html
