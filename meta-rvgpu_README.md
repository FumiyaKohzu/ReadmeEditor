## Remote Virtio GPU Device support layer(meta-rvgpu)
Remote Virtio GPU Device (RVGPU) is a client-server based rendering engine, which allows to render 3D on one device (client) and display it via network on another device (server). For build instructions and testing methods, please refer to the README.md file in the meta-rvgpu directory.
RVGPU is OSS. For more details, please visit the following URL:  
https://github.com/Panasonic-Automotive/remote-virtio-gpu

## How to build
This guide explains how to run RVGPU on the agl-demo-platform and agl-image-weston as a sender or receiver.Therefore, this guide will cover the build methods for both agl-demo-platform and core-image-weston.  
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
$ bitbake agl-demo-platform
```
To build agl-image-weston, execute the command:
```
$ bitbake agl-image-weston
```
For detailed environment setup instructions for each platform, please refer to the following link in the AGL Documentation.  
[Building for x86(Emulation and Hardware)](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/07_Building_for_x86_%28Emulation_and_Hardware%29/)  
[Building for Raspberry Pi 4](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/08_Building_for_Raspberry_Pi_4/)  
[Building for Supported Renesas Boards](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/09_Building_for_Supported_Renesas_Boards/)

## How to run RVGPU remotely
This guide explains how to use RvGPU for remote rendering when **the sender is agl-image-weston** and **the receiver is agl-demo-platform**.

Receiver side (agl-demo-platform)
```
$ export XDG_RUNTIME_DIR=/run/user/1001
$ rvgpu-renderer -b 1280x720@0,0 -p 55667 &
```
Sender side (agl-image-weston)
```
$ rvgpu-proxy -s 1280x720@0,0 -n <IP address of Receiver>:55667 &
$ export LD_LIBRARY_PATH=/usr/lib/mesa-virtio
$ weston --backend drm-backend.so --tty=2 --seat=seat_virtual -i 0 &
```
Please replace <IP adress of Receiver> with the actual IP address of the Receiver.(For example: 192.168.0.130)
  
After executing these steps, the weston screen launched in agl-image-weston will be transferred and displayed on the agl-demo-platform via rvgpu-proxy and rvgpu-renderer. You can then launch graphical applications such as `$ glmark2-es2-wayland` to verify that everything is working properly.
