---
title: 拿到新丹炉时该怎么做？
date: 2021-04-28 17:46:01
---

是时候给新丹炉装好cuda和nvcc了！
先贴一个官方的方案：
[NVIDIA CUDA Installation Guide for Linux](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)

查看系统版本：
```txt
lsb_release -a
```


查看硬件版本：
```shell
lspci | grep -i nvidia
```

确定系统可以安装：
```shell
uname -n && cat /etc/*release
```

查看gcc、g++版本：
```shell
gcc --version
g++ --version
```

拿到手的丹炉型号是：
```txt
ad:00.0 3D controller: NVIDIA Corporation Device 1db6 (rev a1)
```
在这个网站查询一下
[PCI devices](http://pci-ids.ucw.cz/mods/PC/10de?action=help?help=pci)  
1db6对应的显卡是：
```txt
GV100GL [Tesla V100 PCIe 32GB]
```

然后就可以下载驱动了
[NVIDIA 驱动程序下载](https://www.nvidia.cn/Download/index.aspx?lang=cn)

下个10.1版本的cuda
```txt
https://us.download.nvidia.com/tesla/418.197.02/NVIDIA-Linux-x86_64-418.197.02.run
```

先禁用nouveau驱动：
```shell
lsmod | grep nouveau
blacklist nouveau
options nouveau modeset=0
sudo dracut --force
```


开始安装驱动
```shell
chmod +x ./NVIDIA-Linux-x86_64-418.197.02.run
./NVIDIA-Linux-x86_64-418.197.02.run
```

错误 ERROR: You appear to be running an X server; please exit X before installing.的处理按照[How to install NVIDIA.run?](https://askubuntu.com/questions/149206/how-to-install-nvidia-run) 的解决方案：
关闭X Server：
```shell
systemctl stop gdm.service
```
之后打开的指令：
```shell
systemctl start gdm.service
```


下载cuda toolkit10.1的runfile。
[CUDA Toolkit 10.1 original Archive](https://developer.nvidia.com/cuda-10.1-download-archive-base?target_os=Linux&target_arch=x86_64&target_distro=CentOS&target_version=7&target_type=runfilelocal)

安装：
```shell
chmod +x ./cuda_10.1_105_418.39_linux.run
sudo sh ./cuda_10.1_105_418.39_linux.run
```


最后，卸载的方案：
```shell
sudo /usr/local/cuda/bin/cuda-uninstaller
sudo /usr/bin/nvidia-uninstall
```

在安装一下cuDNN：
贴个官方教程：[NVIDIA CUDNN DOCUMENT](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html#install-linux)
下载地址：[cuDNN Archive](https://developer.nvidia.com/rdp/cudnn-archive)
根据[cuDNN Support Matrix](https://docs.nvidia.com/deeplearning/cudnn/support-matrix/index.html),找到对应的版本是cuDNN 7.5.1 - 7.6.2。

解压得到一个cuda/文件：
```shell
sudo cp cuda/include/cudnn*.h /usr/local/cuda/include
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
```


~/.bashrc中增加
```shell
# cuda
export PATH="/usr/local/cuda/bin:$PATH"
# nvcc
export LD_LIBRARY_PATH="/usr/local/cuda/lib64:$LD_LIBRARY_PATH"
```
```shell
source ~/.bashrc
```


查看cuda版本
```shell
cat /usr/local/cuda/version.txt
```
```txt
CUDA Version 10.1.105
```

确认cudnn是否安装好了
```txt
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```

最后下载一下pytorch测试一下是否可以使用GPU。
```shell
pip install torch-1.6.0+cu101-cp38-cp38-linux_x86_64.whl torchvision-0.7.0+cu101-cp38-cp38-linux_x86_64.whl
```
进入python测试一下
```shell
python
```
```python
import torch
torch.cuda.is_available()
```
返回结果为True就可以了！
```python
True
```


大功告成！开始炼丹！
