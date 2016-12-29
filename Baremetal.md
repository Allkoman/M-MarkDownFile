#OpenStack 

[TOC]

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
3. 如果需要，创建一个none-

## Validations


