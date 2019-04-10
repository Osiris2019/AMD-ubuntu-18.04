# AMD-linux-18.04 setup
2019-4-10  
amd threadripper 1950x  
asus x399 extreme  
4 titan xp

# 1. BIOS setup
### 1.1 Disable Virtual Technology(SVM or VT-M)
location) bios -> cpu configuration

### 1.2 Enable Above 4G decoding  
location) boot or pcie setup  
if not found, update bios.  

#### to update bios:
need usb flash drive, get file from motherboard site  
bios -> tool -> asus ez flash utility

# 2. nvidia setup
### 2.1 Cuda
  https://developer.nvidia.com/cuda-toolkit-archive

### 2.2 CuDNN
  https://developer.nvidia.com/cudnn

### 2.3 nccl
  https://docs.nvidia.com/deeplearning/sdk/nccl-install-guide/index.html  
  please install) libnccl-dev together for nccl-test

### 2.4 update nvidia driver
##### Do 4 if nccl test and p2pBandwidthTest works fine
  https://hiseon.me/2018/02/17/install_nvidia_driver/ </br>

```
$ sudo apt-get purge nvidia-*
$ sudo apt-get install nvidia
or
$ sudo apt-get install nvidia-<version>
```

# 3. AMD IOMMU
https://github.com/pytorch/pytorch/issues/1637#issuecomment-338268158

```
$ sudo gedit /etc/default/grub  
```

```
#GRUB_CMDLINE_LINUX=""                    <----- Original commented  
GRUB_CMDLINE_LINUX="iommu=soft"           <------ Change  
```

```
$ sudo update-grub
$ sudo reboot
```

# 4. Test multi GPU
### 4.1 nccl Test
https://github.com/NVIDIA/nccl  
(follow Readmd)  
#### if error looks like)  
https://github.com/NVIDIA/nccl/issues/182  
bios or amd IOMMU problem

### 4.2 p2pBandwidthLatencyTest
``` 
$ cd /usr/local/cuda/samples/1_Utilities/p2pBandwidthLatencyTest
$ sudo make
$ ./p2pBandwidthLatencyTest
```

