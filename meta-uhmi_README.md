## Unified HMI support layer (meta-uhmi)
"Unified HMI" is a common platform technology for UX innovation in integrated cockpit by flexible information display on multiple displays of various applications. Applications can be rendered to any display via a unified virtual display.

## Remote Virtio GPU Device support layer(meta-rvgpu)
Remote Virtio GPU Device (RVGPU) is a client-server based rendering engine, which allows to render 3D on one device (client) and display it via network on another device (server). For build instructions and testing methods, please refer to the README.md file in the meta-rvgpu directory.
RVGPU is OSS. For more details, please visit the following URL:  
https://github.com/Panasonic-Automotive/remote-virtio-gpu

## Remote Virtio GPU Device Demo support layer(meta-rvgpu-demo)
This updated version of meta-rvgpu includes demo scripts, making it easy to test RVGPU command transfers. For build instructions and testing methods, please refer to the README.md file in the meta-rvgpu-demo directory.

### Yocto layers structure
The Yocto layers related to UnifiedHMI are as follow:

```
meta-uhmi-\
          |-meta-rvgpu
          |-meta-uhmi-demo
```
