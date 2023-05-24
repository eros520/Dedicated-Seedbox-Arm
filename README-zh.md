# Seedbox Installation Script
### !!! 这些脚本仅可在新安装的Debian 10/11上运行 
### !!! 缓存的单位由GiB更改为MiB。识用于有微调要求或者机器内存较紧张的用户。 1GiB = 1024MiB
本脚本不保证能提高盒子性能，并且可能会导致您的服务器直接卡死。改写此脚本的菜鸡对编程一窍不通（几乎对杰佬的脚本没有改动），并且可能在脚本里埋下了很多坑，请谨慎使用。

魔改BBR会增加数据包的重传率，做成带宽浪费。在10Gbps网络上，开销大约是您真实上传量的30％，而在1Gbps上大约是10％。

本人只改写了Qbittorrent的编译文件，并且只有4.3.8版本以上的改写了。

我没有时间管理此脚本，有什麽问题请自己解决啦~
## 用法
### Install.sh
`bash <(wget -qO- https://raw.githubusercontent.com/eros520/Dedicated-Seedbox-Arm/main/Install.sh) <用户名称> <用户密码> <缓存大小(单位:MiB)>`

### Tuning.sh 假如你已经安装了盒子环境 (有机会导致bug，请小心使用)

`bash <(wget -qO- https://raw.githubusercontent.com/eros520/Dedicated-Seedbox-Arm/main/Tune.sh)`
## 功能
### Install.sh
###### 1. 安装盒子环境
	BitTorrent 客户端
		1.优化版qBittorrent
		2.优化版Deluge
	Autoremove-torrents
###### 2. 优化
	处理器优化
		1.Tuned
	网络优化
		1.网卡优化
		2.ifconfig
		3.ip route
	内核参数
		1./proc/sys/kernel/
		2./proc/sys/fs/
		3./proc/sys/vm
		4./proc/sys/net/core
		5./proc/sys/net/ipv4/
	硬盘优化
		1.I/O Scheduler
		2.File Open Limit
	魔改 BBR
### Tuning.sh
###### 优化选择:
	1. Deluge Libtorrent 优化 (只能用在Libtorrent 1.1.14 并且需要先安装 ltconfig 插件)
	2. 系统优化
		处理器优化
		网络优化
		内核数据
		硬盘优化
	3. 魔改 BBR 安装
	4. 设置开机自动优化的脚本
### 进阶优化备注
- 缓存大小应该设置在机器内存大小的 1/4 左右. 假如你使用的是qBittorrent 4.3.x, 你需要考虑到内存溢出的问题并且设置缓存大小在机器内存大小的 1/8. 

- 异步 I/O 綫程数的基础设定是 4， 这设定对HDD比较友好. 假如你使用的是SSD甚至是NVMe的话, 你可以调整此参数到 8 甚至到 16. 
	- 在qBittorrent 4.3.x 的话，你可以在高级选项栏目中更改此项设定. 
	- 在Deluge 的话，你可以通过[ltconfig](https://github.com/ratanakvlun/deluge-ltconfig/releases/tag/v0.3.1)更改此项设定
		- aio_threads=8

- 在一些 I/O 较差的机器，send_buffer_low_watermark, send_buffer_watermark & send_buffer_watermark_factor 这三项设定应该调低
	- 在qBittorrent 4.3.x 的话，你可以在高级选项栏目中更改此项设定. 
	- 在Deluge 的话，你可以通过[ltconfig](https://github.com/ratanakvlun/deluge-ltconfig/releases/tag/v0.3.1)更改此项设定
		- send_buffer_low_watermark=1048576
		- send_buffer_watermark=5242880
		- send_buffer_watermark_factor=150

- 在一些 CPU 较差的机器，tick_internal 应该调高来节省CPU指令周期
	- qBittorrent 暂时还没为修改这设定
	- 在Deluge 的话，你可以通过[ltconfig](https://github.com/ratanakvlun/deluge-ltconfig/releases/tag/v0.3.1)更改此项设定
		- tick_interval=250

- 在/etc/sysctl.conf 设置的 TCP 缓存大小对于一些低端机器来説可能会太大。 请根据情况更改.
	- 在 /etc/sysctl.conf 文档中也能找到别的优化备注

- 文件系统的话, 本人强烈推荐使用 XFS 
### 呜谢
qBittorrent 安装 - https://github.com/userdocs/qbittorrent-nox-static

qBittorrent 密码设置 - https://github.com/KozakaiAya/libqbpasswd & https://amefs.net/archives/2027.html

Deluge 密码设置 - https://github.com/amefs/quickbox-lite

autoremove-torrents - https://github.com/jerrymakesjelly/autoremove-torrents

BBR 安装 - https://github.com/KozakaiAya/TCP_BBR
