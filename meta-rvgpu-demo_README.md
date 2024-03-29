## Remote Virtio GPU Device Demo support layer(meta-rvgpu-demo)
This updated version of meta-rvgpu includes demo scripts, making it easy to test RVGPU command transfers. 

## How to build
This guide explains how to run RVGPU on the agl-demo-platform-uhmi and agl-image-weston-uhmi as a sender or receiver.Therefore, this guide will cover the build methods for both agl-demo-platform-uhmi and agl-image-weston-uhmi.  
Please follow the [AGL documentation](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/01_Build_Process_Overview/) for the build process, and set up the "[Initializing Your Build Environment](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/04_Initializing_Your_Build_Environment/)" section as described below to enable the AGL feature 'agl-rvgpu'.For example:
```
$ cd $AGL_TOP/master
$ source ./meta-agl/scripts/aglsetup.sh -f \
                    -m raspberrypi4 \
                    -b build-raspberrypi4-agl-demo \
                    agl-demo agl-devel agl-rvgpu
```
After adding the feature, to build agl-demo-platform-uhmi, execute the command:
```
$ bitbake agl-demo-platform-uhmi
```
To build agl-image-weston-uhmi, execute the command:
```
$ bitbake agl-image-weston-uhmi
```
For detailed environment setup instructions for each platform, please refer to the following link in the AGL Documentation.  
[Building for x86(Emulation and Hardware)](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/07_Building_for_x86_%28Emulation_and_Hardware%29/)  
[Building for Raspberry Pi 4](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/08_Building_for_Raspberry_Pi_4/)  
[Building for Supported Renesas Boards](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/09_Building_for_Supported_Renesas_Boards/)

## How to run RVGPU remotely
This section discusses the various scenarios for using 'agl-image-weston-uhmi' and 'agl-demo-platform-uhmi' in both sender and receiver roles.
   
### Sender: agl-image-weston-uhmi Receiver: agl-demo-platform-uhmi

- **Receiver side (agl-demo-platform-uhmi)**  
Please use commands like 'ifconfig' to find out Receiver's IP address, which will be used to specify the sender's IP address.


- **Sender side (agl-image-weston-uhmi)**
```
$ run_demo_remote_weston <IP address of Receiver>
```

After executing these steps, the weston screen launched in agl-image-weston-uhmi will be transferred and displayed on the agl-demo-platform-uhmi via rvgpu-proxy and rvgpu-renderer. You can then launch graphical applications such as `$ glmark2-es2-wayland` to verify that everything is working properly.

### Sender: agl-demo-platform-uhmi *vs* Receiver: agl-image-weston-uhmi

- **Receiver side (agl-image-weston-uhmi)**  
Please use commands like 'ifconfig' to find out Receiver's IP address, which will be used to specify the sender's IP address.


- **Sender side (agl-demo-platform-uhmi)**
```
$ run_demo_remote_agl <IP address of Receiver>
```
After executing these steps, the homescreen app launched in agl-demo-platform will be transferred and displayed on the agl-image-weston-uhmi.
