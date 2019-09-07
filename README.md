# nvidia-patch

This patch removes restriction on maximum number of simultaneous NVENC video encoding sessions imposed by Nvidia to consumer-grade GPUs.

Requirements:
- x86\_64 system architecture
- ubuntu (< 18.04 for 375.39 nvidia driver or kernel < 4.15). Also known to work on Debian and CentOS, but not tested widely.
- nvenc-compatible gpu (https://developer.nvidia.com/video-encode-decode-gpu-support-matrix#Encoder)

- Nvidia driver. Patch availible for: 
  - [375.39](https://download.nvidia.com/XFree86/Linux-x86_64/375.39/NVIDIA-Linux-x86_64-375.39.run)
  - [390.77](https://download.nvidia.com/XFree86/Linux-x86_64/390.77/NVIDIA-Linux-x86_64-390.77.run)
  - [390.87](https://download.nvidia.com/XFree86/Linux-x86_64/390.87/NVIDIA-Linux-x86_64-390.87.run)
  - [396.24](https://download.nvidia.com/XFree86/Linux-x86_64/396.24/NVIDIA-Linux-x86_64-396.24.run)
  - [396.26](https://uk.download.nvidia.com/tesla/396.26/NVIDIA-Linux-x86_64-396.26.run)
  - [396.37](https://uk.download.nvidia.com/tesla/396.37/NVIDIA-Linux-x86_64-396.37.run)
  - [396.54](https://download.nvidia.com/XFree86/Linux-x86_64/396.54/NVIDIA-Linux-x86_64-396.54.run)
  - [410.48](https://download.nvidia.com/XFree86/Linux-x86_64/410.48/NVIDIA-Linux-x86_64-410.48.run)
  - [410.57](https://download.nvidia.com/XFree86/Linux-x86_64/410.57/NVIDIA-Linux-x86_64-410.57.run)
  - [410.73](https://download.nvidia.com/XFree86/Linux-x86_64/410.73/NVIDIA-Linux-x86_64-410.73.run)
  - [410.78](https://download.nvidia.com/XFree86/Linux-x86_64/410.78/NVIDIA-Linux-x86_64-410.78.run)
  - [410.79](https://uk.download.nvidia.com/tesla/410.79/NVIDIA-Linux-x86_64-410.79.run)
  - [410.93](https://uk.download.nvidia.com/tesla/410.93/NVIDIA-Linux-x86_64-410.93.run)
  - [410.104](https://international.download.nvidia.com/XFree86/Linux-x86_64/410.104/NVIDIA-Linux-x86_64-410.104.run)
  - [415.18](https://download.nvidia.com/XFree86/Linux-x86_64/415.18/NVIDIA-Linux-x86_64-415.18.run)
  - [415.25](https://download.nvidia.com/XFree86/Linux-x86_64/415.25/NVIDIA-Linux-x86_64-415.25.run)
  - [418.30](https://download.nvidia.com/XFree86/Linux-x86_64/418.30/NVIDIA-Linux-x86_64-418.30.run)
  - [418.43](https://international.download.nvidia.com/XFree86/Linux-x86_64/418.43/NVIDIA-Linux-x86_64-418.43.run)
  - [418.56](https://download.nvidia.com/XFree86/Linux-x86_64/418.56/NVIDIA-Linux-x86_64-418.56.run)
  - [418.74](https://international.download.nvidia.com/XFree86/Linux-x86_64/418.74/NVIDIA-Linux-x86_64-418.74.run)
  - [430.09](https://international.download.nvidia.com/XFree86/Linux-x86_64/430.09/NVIDIA-Linux-x86_64-430.09.run)
  - [430.14](https://international.download.nvidia.com/XFree86/Linux-x86_64/430.14/NVIDIA-Linux-x86_64-430.14.run)
  - [430.26](https://international.download.nvidia.com/XFree86/Linux-x86_64/430.26/NVIDIA-Linux-x86_64-430.26.run)
  - [430.34](https://international.download.nvidia.com/XFree86/Linux-x86_64/430.34/NVIDIA-Linux-x86_64-430.34.run)
  - [430.40](https://international.download.nvidia.com/XFree86/Linux-x86_64/430.40/NVIDIA-Linux-x86_64-430.40.run)
  - [435.17](https://international.download.nvidia.com/XFree86/Linux-x86_64/435.17/NVIDIA-Linux-x86_64-435.17.run)
  - [435.21](https://international.download.nvidia.com/XFree86/Linux-x86_64/435.21/NVIDIA-Linux-x86_64-435.21.run)


Tested on Ubuntu 18.04 LTS (GNU/Linux 4.15.0-23-generic x86\_64)

## Synopsis

```
# bash ./patch.sh -h

SYNOPSIS
       patch.sh [OPTION]...

DESCRIPTION
       The patch for Nvidia drivers to increase encoder sessions

       -s    Silent mode (No output)
       -r    Rollback to original (Restore lib from backup)
       -h    Print this help message

```

## Step-by-Step guide

Examples are provided for driver version 410.78. All commands are runned as root.

### Download driver

[https://download.nvidia.com/XFree86/Linux-x86\_64/410.78/NVIDIA-Linux-x86\_64-410.78.run](https://download.nvidia.com/XFree86/Linux-x86_64/410.78/NVIDIA-Linux-x86_64-410.78.run)

### Install driver

```bash
mkdir /opt/nvidia && cd /opt/nvidia
wget https://download.nvidia.com/XFree86/Linux-x86_64/410.78/NVIDIA-Linux-x86_64-410.78.run
chmod +x ./NVIDIA-Linux-x86_64-410.78.run
./NVIDIA-Linux-x86_64-410.78.run
```

### Check driver

```bash
nvidia-smi
```

Output should show no errors and details about your driver and GPU.

### Patch driver

This patch performs backup of original file prior to making changes.

```bash
bash ./patch.sh
```

You're all set!

## Rollback

If something got broken you may restore patched driver from backup:

```bash
bash ./patch.sh -r
```

## See also

https://habr.com/post/262563/

If you experience `CreateBitstreamBuffer failed: out of memory (10)`, then you have to lower buffers number used for every encoding session. If you are using `ffmpeg`, see option `-surfaces` ("Number of concurrent surfaces") and try value near `-surfaces 8`.
