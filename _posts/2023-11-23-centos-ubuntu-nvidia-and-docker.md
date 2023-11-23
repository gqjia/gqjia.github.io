---
title:  Centos/ubuntu安装显卡驱动
date: 2023-11-23 11-31-45
---



## 安装NV驱动



网址：https://www.nvidia.com/DOWNLOAD/Find.aspx?lang=en-us

1. wget 驱动

2. 然后 chmod +x <下载好的.run文件>

3. ./<下载好的.run文件>

4. 如果出现了”Unable to find the kernel source tree for the currently running kernel.“报错则运行

    ```
    sudo yum install kernel-devel
    ```

5. 执行该命令打开persistant mode（每次服务器重启都要运行该命令）

    ```
    nvidia-persistenced --persistence-mode
    ```



### 可能遇到的问题

#### close X Server

```shel
sudo service lightdm stop
sudo service gdm stop
sudo service kdm stop
```



#### How to disable Nouveau kernel driver

```shell
sudo vim /etc/modprobe.d/blacklist-nouveau.conf
```

输入内容：

```shell
blacklist nouveau
options nouveau modeset=0
```



```shell
sudo update-initramfs -u
sudo reboot
```



#### gcc

```shell
sudo apt install build-essential
sudo apt-get install manpages-dev
gcc --version
```





## 安装docker

### Centos安装docker

1. 依次执行命令(https://docs.docker.com/engine/install/centos/)

    ```shell
    sudo yum install -y yum-utils
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```

2. docker移动安装目录到挂载的大硬盘下（必须要做）

    ```shell
    systemctl stop docker # 停掉docker服务
    mv /var/lib/docker /data/docker # 转移docker安装目录
    ln -s /data/docker /var/lib/docker # 软链
    ```





### ubuntu 安装docker

https://docs.docker.com/engine/install/ubuntu/

1.   Set up the repository

```shell
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
```



```shell
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```



```shell
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

2.   Install Docker Engine

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```



```shell
sudo docker run hello-world
```



#### 异常处理

```bash
root@midu-System-Product-Name:/home/midu/Downloads# apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
正在读取软件包列表... 完成
正在分析软件包的依赖关系树... 完成
正在读取状态信息... 完成
没有可用的软件包 docker-ce，但是它被其它的软件包引用了。
这可能意味着这个缺失的软件包可能已被废弃，
或者只能在其他发布源中找到

E: 软件包 docker-ce 没有可安装候选
E: 无法定位软件包 docker-ce-cli
E: 无法定位软件包 containerd.io
E: 无法按照 glob ‘containerd.io’ 找到任何软件包
E: 无法按照正则表达式 containerd.io 找到任何软件包
E: 无法定位软件包 docker-buildx-plugin
E: 无法定位软件包 docker-compose-plugin
```



需要更新apt-get：

```bash
sudo apt-get update
```







## 安装nvidia-docker2

### Centos安装nvidia-docker2

1. 依次执行下面的命令

    ```shell
    distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
    && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.repo | sudo tee /etc/yum.repos.d/nvidia-docker.repo
    
    sudo yum clean expire-cache
    
    sudo yum install -y nvidia-docker2
    ```

    

2. 更新/etc/docker/daemon.json⽂件

    ```json
    {
        "registry-mirrors": [
            "https://registry.docker-cn.com",
            "https://docker.mirrors.ustc.edu.cn",
            "http://hub-mirror.c.163.com"
        ],
        "insecure-registries": [
            "harbor.miduchina.com",
            "harbor-nh.miduchina.com"
        ],
        "bip":"192.51.0.1/16",
        "log-driver":"json-file",
        "log-opts": {"max-size":"500m", "max-file":"2"},
        "runtimes": {
            "nvidia": {
                "path": "nvidia-container-runtime",
                "runtimeArgs": []
            }
        }
    }
    ```

    

3. 启动docker服务

    ```shell
    systemctl start docker
    ```








### ubuntu安装nvidia-docker2



1.   安装

```shell
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```



```shell
sudo apt-get update
```



```shell
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd
```



```shell
sudo apt-get remove nvidia -384 ; sudo apt-get install nvidia-384
```



1. 更新/etc/docker/daemon.json⽂件

    ```json
    {
        "registry-mirrors": [
            "https://registry.docker-cn.com",
            "https://docker.mirrors.ustc.edu.cn",
            "http://hub-mirror.c.163.com"
        ],
        "insecure-registries": [
            "harbor.miduchina.com",
            "harbor-nh.miduchina.com"
        ],
        "bip":"192.51.0.1/16",
        "log-driver":"json-file",
        "log-opts": {"max-size":"500m", "max-file":"2"},
        "runtimes": {
            "nvidia": {
                "path": "nvidia-container-runtime",
                "runtimeArgs": []
            }
        }
    }
    ```

    

2. 启动docker服务

    ```shell
    systemctl start docker
    ```





## 测试



```shell
docker run --rm --runtime=nvidia --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
```


