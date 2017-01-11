#Issue

192.168.100.92:8443
issue ：can't connect to Fuel Web
support : 怀疑是端口未开放
issue : can't ping baidu.com
support : Edit /etc/sysconfig/network-scripts/ifcfg-eth0

issue : 安装firewall，开通8443端口依旧无法访问fuel web
support : 待解决，2017.1.4 截止11.25网络无法使用

issue ：蹭网，ip 219.217.238.205
support : 尝试通过“隧道”解决8443端口未开放的问题


fuel Openstack Nat+HostOnly网络设置


virtualbox 5.1.10/WindowServer2012 ： 无法卸载，通过安装包修复后卸载
ftp security solution:   http://www.oxfordsbsguy.com/2013/03/14/your-current-security-settings-do-not-allow-this-file-to-be-downloaded/


fuel Openstack : xxx:8000 not 8443

网络环境：
1、10.20.0.1  master节点通过pxe网络部署openstack的专用网络
2、172.16.0.1 openstack公共网络，给实例提供浮动IP并提供外部网络
3、192.168.0.1

openstack的内部，管理，存储网络，该网络的混杂模式要全部允许。（host-only禁dhcp）

极路由Shadowsocks 不稳定 连接异常：  待解决




YouTube.com： fuel openstack installation



2017.1.10
安装openstack时会默认安装centos，随后进行nginx等服务的安装，在安装centos后会重启，重启切记不要不操作，会默认安装为10.20.0.2网络，并且，由于安装centos时ip10.20.0.2被占用，无法同时安装多台处于同一网络的机器进行测试。
