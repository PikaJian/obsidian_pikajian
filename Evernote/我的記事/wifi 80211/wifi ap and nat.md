# wifi ap and nat

# 在linux下用hostapd自己架无线AP

## 目录

 \[[隐藏](http://xdlinux.info/wiki/index.php?title=%E5%9C%A8linux%E4%B8%8B%E7%94%A8hostapd%E8%87%AA%E5%B7%B1%E6%9E%B6%E6%97%A0%E7%BA%BFAP#)\] 

* [1 Linux下用hostapd架无线AP](http://xdlinux.info/wiki/index.php?title=%E5%9C%A8linux%E4%B8%8B%E7%94%A8hostapd%E8%87%AA%E5%B7%B1%E6%9E%B6%E6%97%A0%E7%BA%BFAP#Linux.E4.B8.8B.E7.94.A8hostapd.E6.9E.B6.E6.97.A0.E7.BA.BFAP)
	* [1.1 查看网卡芯片信息](http://xdlinux.info/wiki/index.php?title=%E5%9C%A8linux%E4%B8%8B%E7%94%A8hostapd%E8%87%AA%E5%B7%B1%E6%9E%B6%E6%97%A0%E7%BA%BFAP#.E6.9F.A5.E7.9C.8B.E7.BD.91.E5.8D.A1.E8.8A.AF.E7.89.87.E4.BF.A1.E6.81.AF)
	* [1.2 确定无线网卡是否支持master mode(AP)](http://xdlinux.info/wiki/index.php?title=%E5%9C%A8linux%E4%B8%8B%E7%94%A8hostapd%E8%87%AA%E5%B7%B1%E6%9E%B6%E6%97%A0%E7%BA%BFAP#.E7.A1.AE.E5.AE.9A.E6.97.A0.E7.BA.BF.E7.BD.91.E5.8D.A1.E6.98.AF.E5.90.A6.E6.94.AF.E6.8C.81master_mode.28AP.29)
	* [1.3 安装并配置hostapd](http://xdlinux.info/wiki/index.php?title=%E5%9C%A8linux%E4%B8%8B%E7%94%A8hostapd%E8%87%AA%E5%B7%B1%E6%9E%B6%E6%97%A0%E7%BA%BFAP#.E5.AE.89.E8.A3.85.E5.B9.B6.E9.85.8D.E7.BD.AEhostapd)
	* [1.4 为无线网卡指定IP](http://xdlinux.info/wiki/index.php?title=%E5%9C%A8linux%E4%B8%8B%E7%94%A8hostapd%E8%87%AA%E5%B7%B1%E6%9E%B6%E6%97%A0%E7%BA%BFAP#.E4.B8.BA.E6.97.A0.E7.BA.BF.E7.BD.91.E5.8D.A1.E6.8C.87.E5.AE.9AIP)
	* [1.5 安装配置dhcp服务器](http://xdlinux.info/wiki/index.php?title=%E5%9C%A8linux%E4%B8%8B%E7%94%A8hostapd%E8%87%AA%E5%B7%B1%E6%9E%B6%E6%97%A0%E7%BA%BFAP#.E5.AE.89.E8.A3.85.E9.85.8D.E7.BD.AEdhcp.E6.9C.8D.E5.8A.A1.E5.99.A8)
	* [1.6 设置网络封包的流动与转送](http://xdlinux.info/wiki/index.php?title=%E5%9C%A8linux%E4%B8%8B%E7%94%A8hostapd%E8%87%AA%E5%B7%B1%E6%9E%B6%E6%97%A0%E7%BA%BFAP#.E8.AE.BE.E7.BD.AE.E7.BD.91.E7.BB.9C.E5.B0.81.E5.8C.85.E7.9A.84.E6.B5.81.E5.8A.A8.E4.B8.8E.E8.BD.AC.E9.80.81)
	* [1.7 配置脚本](http://xdlinux.info/wiki/index.php?title=%E5%9C%A8linux%E4%B8%8B%E7%94%A8hostapd%E8%87%AA%E5%B7%B1%E6%9E%B6%E6%97%A0%E7%BA%BFAP#.E9.85.8D.E7.BD.AE.E8.84.9A.E6.9C.AC)
	* [1.8 参考来源](http://xdlinux.info/wiki/index.php?title=%E5%9C%A8linux%E4%B8%8B%E7%94%A8hostapd%E8%87%AA%E5%B7%B1%E6%9E%B6%E6%97%A0%E7%BA%BFAP#.E5.8F.82.E8.80.83.E6.9D.A5.E6.BA.90)

## Linux下用hostapd架无线AP

本文以Arch Linux为例进行说明，其它发行版可能略有不同

### 查看网卡芯片信息

对于pci网卡：

lspci |grep Network

我电脑上的输出信息是：

06:00.0 Network controller: Atheros Communications Inc. AR9285 Wireless Network Adapter (PCI-Express) (rev 01)

对于usb网卡：

lsusb

安装ethtool：

sudo pacman -S ethtool

查看网卡的详细信息：

ethtool -i wlan0

我电脑上的输出信息是：

driver: ath9k
version: 3.2.6-2-ARCH
firmware-version: N/A
bus-info: 0000:06:00.0
supports-statistics: no
supports-test: no
supports-eeprom-access: no
supports-register-dump: no
supports-priv-flags: no

### 确定无线网卡是否支持master mode(AP)

安装iw无线网卡配置工具：

sudo pacman -S iw

查看无线网卡所支持的模式：

iw list

找到如下类似信息：

Supported interface modes:
	 \* IBSS
	 \* managed
	 \* AP
	 \* AP/VLAN
	 \* WDS
	 \* monitor
	 \* mesh point
	 \* P2P-client
	 \* P2P-GO

如果”Supported interface modes”中有AP的话，那么恭喜，你的网卡支持用hostapd来架设软AP。

如果没有的话可以看看这个列表：[网卡信息](http://linuxwireless.org/en/users/Drivers) 如果还不行的话，死心吧...网上去买个支持AP的无线网卡。

### 安装并配置hostapd

安装hostapd：

sudo pacman -S hostapd

配置hostapd.conf 这是一个WPA-PSK的配置，用hostapd -d /etc/hostapd/hostapd.conf来测试配置是否可用。

interface=wlan0
driver=nl80211
ssid=valid\_is\_here
channel=6
hw\_mode=g
ignore\_broadcast\_ssid=0
auth\_algs=1
wpa=2
wpa\_passphrase=your\_password
wpa\_key\_mgmt=WPA-PSK
wpa\_pairwise=TKIP
rsn\_pairwise=CCMP

对interface=wlan0的说明：wlan0指的是欲架设无线AP的那个无线网卡(如果是笔记本外的无线网卡则有可能是wlan1)
对Atheros系列的网卡的说明：某些网络上的文章说得装Madwifi，其实大可不必，对于Arch来说AUR上的madwifi目前不可用，需要到madwifi官网下载源码编译安装。

启动hostapd服务

sudo /etc/rc.d/hostapd start

### 为无线网卡指定IP

sudo ifconfig wlan0 10.10.10.1 netmask 255.255.255.0

### 安装配置dhcp服务器

需要注意的是dhcp客户与服务器的区别，这里安装dhcp服务器为其它通过AP接入的手机等提供网络参数。 安装dhcp：

sudo pacman -S dhcp

编辑配置文件dhcpd.conf：

sudo vim /etc/dhcpd.conf

用如下文本替代(编辑之前最好先备份dhcpd.conf)：

default-lease-time 600;
max-lease-time 7200;
subnet 10.10.10.0 netmask 255.255.255.0 {
    range 10.10.10.10 10.10.10.100;
    option routers 10.10.10.1;
    option domain-name-servers 8.8.8.8;
    option ip-forwarding off;
    option broadcast-address 10.10.10.255;
}

启动dhcpd服务：

sudo /etc/rc.d/dhcp4 start

**这里，如果你装了pdnspd 或者 dnsmasq 的话就不用再安装dhcp了。**

### 设置网络封包的流动与转送

安装iptables：

sudo pacman -S iptables

启用内核转发功能：

sudo vim /etc/sysctl.conf
net.ipv4.ip\_forward=1

要想即时生效的话，直接用下面这句吧(以root身份执行)：

su
echo 1 >/proc/sys/net/ipv4/ip\_forward
exit

设置转送规则，使得接入AP的手机等设备通过有线网络(比如计算机上已经接入网络的eth0网卡)间接接入外网：

sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

### 配置脚本

当然，你不想每次输入这么多行Shell代码，理所当然的，自动化脚本就很有必要了。将以下代码保存为文件ap4xx

#!/bin/bash

sudo /etc/rc.d/hostapd restart
sudo ifconfig wlan0 10.10.10.1 netmask 255.255.255.0
sudo /etc/rc.d/dhcp4 restart
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
