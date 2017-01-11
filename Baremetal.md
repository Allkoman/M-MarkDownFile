#OpenStack 

##overview
* [Baremetal Environment](#Baremetal Environment)
* [Minimum System Requirements](#Minimum System Requirements)
* [Preparing the Baremetal Environment](#Preparing the Baremetal Environment)
* [Setting Up The Undercloud Machine](#Setting Up The Undercloud Machine)
* [Undercloud Networking Setup](# Undercloud Networking Setup)
* [Validations](#Validations)
* [Configuration Files](#Configuration Files)

## Baremetal Environment
## Minimum System Requirements

最小规模安装每台机器至少需要4G内存，40G硬盘，安装如下机器：
* 1 Undercloud   (host?)
* 1 Overcloud Controller
* 1 Overcloud Compute

裸机的最低配置
* Virtualization hardware extensions enabled (nested KVM is not supported)
* 1 quad core CPU
* 32 GB free memory
* 240 GB disk space

支持的系统
* RHEL 7.1 x86_64 
* CentOS 7 x86_64

## Preparing the Baremetal Environment

### Networking
>所有的overcloud节点由undercloud节点部署，所以需要自定义机器的网络以允许使用undercloud以PXE boot的方式启动overcloud 节点

*  所有的overcloud节点设置必须支持IPMI。
*  为所有的overcloud节点搭建一个管理配置网络(management provisioning network)，在这个网络中所有节点的网卡(Network Interface Card)必须在一个广播域(broadcast domain)。在测试环境中，需要在交换机(switch)上搭建一个新的虚拟局域网(VLAN)。注意在每一台机器上都应该使用同一块网卡。这是因为安装过程会使用到网卡的名字。
*  管理配置网络的网卡不能是overcloud机器与undercloud机器相连所使用的网卡。在undercloud的安装过程中，会为OpenStack Neutron搭建一个开放交换虚拟标准网桥(openvswitch bridge),同时overcloud节点的管理配置网卡会连接到这个网桥中。所以，如果用同一块网卡作为管理配置及与undercloud连接，安装到这里连接反而会断开。
*  overcloud机器可以使用PXE boot卸载私有虚拟局域网上的网卡。在测试环境中这就需要保证如下设置：在BIOS中disable除了我们想要用于PXE boot网卡之外的所有网卡的network booting，并确保PXE boot网卡放置于所有CD/DVD,本地硬盘之上，首选启动项。
*  对于每一台overcloud机器你需要有如下信息：用于PXE boot的NIC的MAC地址，机器的IPMI信息，例如IPMI网卡的IP地址，IPMI的用户名及密码。

## Setting Up The Undercloud Machine

1. 选一台裸机安装undercloud
2. 安装64位的RHEL 7.1 或者 CentOS 7
3. 如果需要，创建一个none-root 账户，并设置免密。

## Undercloud Networking Setup
1. 虚拟机添加bridge网卡 Inter I350 & Inter 82599 #2
2. 安装虚拟机Centos7 x86_64 minimal 最小安装版，默认管理员账户为stack，双网卡对应顺序为en0ps3,en0ps8
3. `sudo vi /etc/sysconfig/network-scripts/ifcfg-en0ps3`
4. 修改部分包括： 
```
BOOTPOROT=staic
ONBOOT=yes
IPADDR=192.168.100.92
NETMASK=255.255.255.0
GATEWAY=192.168.100.101
DNS=202.118.224.100
```
5. `sudo vi /etc/resolv.conf` : 
添加 `nameserver 114.114.114.114 \n nameserver 202.118.224.100`
6. `sudo ifdown en0ps3` & `sudo ifup en0ps3`
7. 对en0ps8重复3，4，6操作，注意IP及GATEWAY等参数对应修改。

## Validations

```
 sudo yum install ansible
 git clone https://git.openstack.org/openstack/tripleo-validations
 cd tripleo-validations
 export UNDERCLOUD_HOST=ip or hostname
 printf "[undercloud]\n$UNDERCLOUD_HOST" > hosts
 grep -l '^\s\+-\s\+prep' -r validations
```
上一步会打印出一系列文件名，下一步依次每个单独运行即可（下面的xxx.yaml）
```
ansible-playbook -i hosts validations/xxx.yaml
```

## Configuration Files

cpu : `cat /proc/cpuinfo | grep "processor" | wc -l`







## Undercloud 

### Repo
> 更新为阿里源，国外源无法下载

Repo of Aliyuan
```
yum clean all
rm -rf /etc/yum.repos.d/*.repo
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```
添加ceph
vi /etc/ceph.repo
```
[ceph]
name=ceph
baseurl=http://mirrors.aliyun.com/ceph/rpm-jewel/el7/x86_64/
gpgcheck=0
[ceph-noarch]
name=cephnoarch
baseurl=http://mirrors.aliyun.com/ceph/rpm-jewel/el7/noarch/
gpgcheck=0
```

