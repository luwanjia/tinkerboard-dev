# Tinker board develop manual

## 1 Description
This manual is for the development of tinker board. OS is Debian9 Gnome.
## 2 Create workspace dircectory
```
$mkdir Workspace
```
Assume all operations are executed under this directory.
## 3 Install common dependence
```
$sudo apt install bc
$sudo apt install lib32ncurses5 lib32z1 lib32stdc++6
$sudo apt install libssl-dev
```
## 4 Build kernel
### 4.1 Install complier
```
$sudo apt install gcc-arm-linux-gnueabihf
```

### 4.2 Get source code
```
$cd Workspace
$git clone https://github.com/TinkerBoard/debian_kernel.git
```
### 4.3 Build
```
$export ARCH=arm
$export CROSS_COMPILE=arm-linux-gnueabihf-
$cd debian_kernel
$make miniarm-rk3288_defconfig
$make rk3288-miniarm.img -j4
$make modules
$mkdir modules_install
$make INSTALL_MOD_PATH=./modules_install modules_install
```
## 5 Build u-boot
### 5.1 Install dependence
```
$sudo apt install device-tree-compiler
```
### 5.2 Get source code
```
$cd Workspace
$git clone https://github.com/TinkerBoard/debian_u-boot.git
```
### 5.3 Build
```
$cd debian_u-boot
$git checkout linux4.4-rk3288
$make tinker-rk3288_defconfig
$make -j4
```
### 5.4 Make image and flash
```
$./tools/mkimage -n rk3288 -T rksd -d spl/u-boot-spl-dtb.bin out
$cat u-boot-dtb.bin >> out
```
We can then flash the resulting image to the uSD card.
```
$ dd if=out of=/dev/sdc seek=64
```



