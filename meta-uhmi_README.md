## Unified HMI support layer (meta-uhmi)
"Unified HMI" is a common platform technology for UX innovation in integrated cockpit by flexible information display on multiple displays of various applications. Applications can be rendered to any display via a unified virtual display.

## Remote Virtio GPU Device support layer(meta-rvgpu)
Remote Virtio GPU Device (RVGPU) is a client-server based rendering engine, which allows to render 3D on one device (client) and display it via network on another device (server)
RVGPU is OSS. For more details, please visit the following URL:  
https://github.com/Panasonic-Automotive/remote-virtio-gpu

### Software Structure
The Yocto layers related to UnifiedHMI are as follow:

```
meta-uhmi-\
          |-meta-rvgpu
          |-meta-uhmi-demo
```

* meta-rvgpu  
Yocto recipes to introduce UnifiedHMI software.


## How to build
This guide explains how to run RVGPU on the agl-demo-platform and core-image-weston as a sender or receiver.Therefore, this guide will cover the build methods for both agl-demo-platform and core-image-weston.  
Please follow the [AGL documentation](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/01_Build_Process_Overview/) for the build process, and set up the "[Initializing Your Build Environment](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/04_Initializing_Your_Build_Environment/)" section as described below to enable the AGL feature 'agl-rvgpu'.For example:
```
$ cd $AGL_TOP/master
$ source ./meta-agl/scripts/aglsetup.sh -f \
                    -m raspberrypi4 \
                    -b build-raspberrypi4-agl-demo \
                    agl-demo agl-devel agl-rvgpu
```
After adding the feature, to build agl-demo-platform, execute the command:
```
$bitbake agl-demo-platform-uhmi
```
To build core-image-weston, execute the command:
```
$bitbake agl-image-weston-uhmi
```
For detailed environment setup instructions for each platform, please refer to the following link in the AGL Documentation.  
[Building for x86(Emulation and Hardware)](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/07_Building_for_x86_%28Emulation_and_Hardware%29/)  
[Building for Raspberry Pi 4](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/08_Building_for_Raspberry_Pi_4/)  
[Building for Supported Renesas Boards](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/09_Building_for_Supported_Renesas_Boards/)

## How to run RVGPU remotely
In this guide, we will discuss the process of transferring commands between core-image-weston and agl-demo-platform, with each platform acting as both Sender and Receiver.
   
- ### Sender: core-image-weston Receiver: agl-demo-platform

Receiver side (agl-demo-platform)
```
$export XDG_RUNTIME_DIR=/run/user/1001
$rvgpu-renderer -b 1280x720@0,0 -p 55667 &
```
Sender side (core-image-weston)
```
$rvgpu-proxy -s 1280x720@0,0 -n 192.168.0.130:55667 &
$export LD_LIBRARY_PATH=/usr/lib/mesa-virtio
$weston --backend drm-backend.so --tty=2 --seat=seat_virtual -i 0 &
```
After executing these steps, the weston screen launched in core-image-weston will be transferred and displayed on the agl-demo-platform via rvgpu-proxy and rvgpu-renderer. You can then launch graphical applications such as `$glmark2-es2-wayland` to verify that everything is working properly.

- ### Sender: agl-demo-platform *vs* Receiver: core-image-weston

Receiver side (core-image-weston)
```
(No additional commands are necessary)
```
Sender side (agl-demo-platform)
```
$run_demo_remote
```