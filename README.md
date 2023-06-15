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
```

* meta-rvgpu  
Yocto recipes to introduce UnifiedHMI software.

## How to build
Please follow the [AGL documentation](https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/01_Build_Process_Overview/) for the build process, and set up the "Initializing Your Build Environment" section as described below to enable the AGL feature 'agl-rvgpu'.For example:
```
$ cd $AGL_TOP/master
$ source ./meta-agl/scripts/aglsetup.sh -f \
                    -m raspberrypi4 \
                    -b build-raspberrypi4-agl-demo \
                    agl-demo agl-devel uhmi-rvgpu
```

## How to run RVGPU remotely
We will explain the methods for remote transfer from AGL to Linux and from Linux to AGL, respectively.
Please refer to the "Build instructions" of RVGPU's open-source project for setting up the environment on Linux.  
URL: https://github.com/Panasonic-Automotive/remote-virtio-gpu

- ## From AGL (Sender) to Linux (Receiver)
### Launch the rvgpu-renderer on Linux (Receiver).

The following statement is written almost the same in the RVGPU's OSS.  
...

- ## From Linux (Sender) to AGL (Receiver)
### ...
### ...
