# depoly-Fabric-IPFS-on-IoTs
This project aims to establish a system that includes 4m Raspberry Pi devices and n hosts (where m ≥ n, and n > 2). The Raspberry Pis are configured with TPM (Trusted Platform Module) and are capable of measuring their system state to generate verification evidence. Using this verification evidence and their private keys, the Raspberry Pis generate attestation reports. Every m Raspberry Pis form a group, with a host serving as the server within the group. The n hosts form a blockchain network based on the Hyperledger Fabric architecture. This network collects the information (including verification evidence, attestation reports, etc.) uploaded by the Raspberry Pi devices to the server and then uploads it to an IPFS (InterPlanetary File System) node. Through the design of communication protocols and consensus mechanisms, the system will achieve distributed large-scale storage, distributed root of trust generation, and verification.
# 一. 安装Debian系统
在windows下安装debian系统。

参考文章:<https://blog.csdn.net/grin99/article/details/104562852>

使用网线连接主机和笔记本，达到主机联网的目的
参考文章:<https://blog.csdn.net/iamjingong/article/details/119379129>

# 二、搭建hyperledger网络
## 1. 安装docker-ce
由于docker安装会遭遇各种各样的问题，选择安装更方便的docker-ce。

参考文章:<https://developer.aliyun.com/mirror/docker-ce?#>

debian12用户需要更改写入软件源的信息

## 2.安装docker-compose
apt-get install docker-compose
## 3.安装golang
1）wget获取go语言安装包
```bash
wget https://go.dev/dl/go1.19.8.linux-amd64.tar.gz
```
 2）解压并安装

```bash
sudo tar -C /usr/local -xzf go1.19.8.linux-amd64.tar.gz
```

3）设置环境变量

这里怪怪的，虚拟机上只在/etc/profile里面设置了环境变量：

```bash
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
```

可以成功使用go，并且重启终端也不影响使用。

但是主机上需要在/etc/profile和~/.bashrc中同时配置才可以，而且不能source ~/.profile，否则终端不显示中文。

没有配置GO111MODULE和GOPROXY，等进行到后面需要go mod时再配置。

## 4.搭建hyperledger fabric网络
参考文章:<https://mp.weixin.qq.com/s?__biz=MjM5OTI2NDMwMg==&mid=2247484856&idx=1&sn=77c8131f471a5e36b853eb6403222489>

./bootstrap.sh下载镜像超时的解决方法：

参考文章:<https://tencentcloud.csdn.net/66977892962e585a25634c35.html>
/etc/docker/daemon.json内容：

```bash
{
    "registry-mirrors": [
        "https://docker.m.daocloud.io",
        "https://dockerproxy.com",
        "https://docker.mirrors.ustc.edu.cn",
        "https://docker.nju.edu.cn"
    ],
    "experimental": true
}
```

如果有没有拉取成功的镜像使用docker pull自行拉取：
```bash
docker pull hyperledger/fabric-xxx:[version]
```

拉取速度很慢，be patient

# 三、安装IPFS
