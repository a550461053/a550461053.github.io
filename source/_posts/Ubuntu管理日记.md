## Ubuntu管理日记

teamviewer: 647025111

 - 开机时间：
	 - uptime
	 - uptime -v 查看版本
- 开机密码：
	+ sudo su第一次安装用户可以使用命令
	+ 然后切换到root后：passwd，即可修改root密码
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

 - 移动Unity所处位置：
    gsettings set com.canonical.Unity.Launcher launcher-position Bottom

 - 桌面实时流量：
	- sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor 
	- sudo apt-get update 
	- sudo apt-get install indicator-sysmonitor 
	- indicator-sysmonitor & 后台运行;
	- 自启动在软件自己的设置里面
	- 显示的内容可自定义

 - 安装vim，然后安装fish，更好的shell界面：
	- sudo apt-get fish
	- fish
 - 安装R:
	- sudo apt-get install r-base
	- R
	- RStudio的安装;
 - 创建链接文件：软链接即可，可跨文件系统：ln -s 源文件 目标文件
 	- sudo ln -s /usr/local/anaconda3/bin/python /usr/bin/python
	- 跨文件系统，需要自动mount ： sudo gedit /etc/fstab
	- /dev/sda1 /media/win-C ntfs nls=utf8,umask=000   0   0
 - 创建桌面快捷方式：


 - 中文编码：locale查看支持的字符集
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

 - dpkg -L packagename:查看安装位置

- 修复U盘只读：
	+ 卸载U盘
	+ 插上U盘
	+ tail -f /var/log/syslog 查看动态日志，找到所在分区
	+ sudo dosfsck -v -a /dev/sdb4
	+ Windows下式：chkdsk命令

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
 			- 没有ipv6的用:104.238.161.51
 		- Port: 1234
 		- password: luolvgen

- 远程连接：
	+ Ubuntu连接windows：
		* 安装rdesktop：sudo apt install rdesktop
		* rdesktop -f -a 16 10.10.4.42 -u Administrator -p 1qazZAQ!
		* 连接不上，报错，可能是账户密码错误；或者就是没有开启宿机windows的远程管理，放开只能网络连接限制
		* 或者使用Ubuntu自带的remote软件，都是使远程连接协议RDP

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
	- 

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

- nvidia;
	- 更新驱动: https://stackoverflow.com/questions/43016255/libegl-so-1-is-not-a-symbolic-link

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
	+ 

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
    缺少依赖,用：sudo apt-get -f install 下面不要也可以，重启即可～
    还是不行：sudo apt-get purge fcitx; 清理文件，包括配置文件 
    重启 - dpkg -i sogou..deb ; sudo apt-get -f install ; 重启
    还是搜狗的用着舒服～～～
    - 搜狗很容易出问题: 
    	- 就是回车+分号同时按的时候
    	- 此时: 运行一下fictx即可

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
- labelme:
	- 安装python版本的:
	- sudo apt-get install python-qt4 pyqt4-dev-tools
	- sudo pip install labelme
		- 报错: install the python-tk package
		- 解决: sudo apt install python-tk
	- 运行: labelme



 - docker：
 	- sudo apt-get install docker.io
 	- docker version ; 只显示client 1.12.6
 	- sudo systemctl status docker ; 运行daemon
 	- sudo usermod -aG docker $(whoami) ; 防止docker每次都要sudo
 	- sudo apt install docker-compose
 	- docker-compose version ; docker-compose1.5.2
 	- 版本太低，so用pip安装docker-compose
 	- sudo pip install docker-compose
 	- docker-compose -h ; 查看用法
 	- 卸载：dpkg --list | grep docker
 		- 卸载程序和配置：sudo apt-get --purge remove docker
 		- 只卸载程序：sudo apt-get remove docker
 		- sudo apt-get remove docker.io
 		- sudo apt-get remove docker-compose

- alias:
	- 临时: alias xmind='XMind'
	- 永久: 保存在.bashrc或其他文件下

 - cp：cp 待复制文件 新文件
 	- cp -r 待复制文件夹 新文件夹

 - pip：pip安装：pip install package
 	- sudo apt install python-pip
 	- sudo pip uninstall docker-compose
 	- sudo pip install --upgrade pip 升级pip
 	- sudo apt-get -f install 修复安装
 	- sudo apt-get upgrade升级已经安装的包
 	- sudo apt  dist-upgrade 升级系统
 	- 二进制包安装的方式，卸载直接rm即可
- apt-get 安装的包是系统化的包，在系统内完全安装。源是ubuntu仓库。
 pip install安装的python包，可以只安装在当前工程内。且源是pyPI和ubuntu仓库。
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
PPA，表示 Personal Package Archives，也就是个人软件包集。

- golang：
	- 下载: sudo wget https://storage.googleapis.com/golang/go1.7.4.linux-amd64.tar.gz
	- 解压后, 复制到GOROOT目录下
	- export GOROOT=/usr/lib/go
	- export GORACH=amd64
	- export GOOS=linux
	- export PATH=${PATH}:$GOROOT/bin
	- export GOPATH=$HOME/project/go #工作目录，至少包含bin/pkg/src
	- 32位则rach=386；win则改goos=windows
当你运行go build测试你的chaincode编译时，Go将在$GOPATH/src目录中查找你在importchaincode 的块中列出的非标准依赖。

	- 必须新建：$GOPATH/src/go/github.com/hyperledger/
	- 必须新建：$GOROOT/src/go/github.com/hyperledger/
	- 安装了:sudo apt install mercurial
	- 安装go中文教程：sudo go get bitbucket.org/mikespook/go-tour-zh/gotour
	- sudo go env是root环境下的环境变量了
	- go env才是当前目录下的环境变量
	- 可以sudo su 到root下,然后执行source /etc/profile, 再次执行命令
	- sudo -E go env保持当前用户下的环境变量，以root身份执行命令
	- sudo -E go install hello
	- su 一个用户； 或者sudo su；都是临时登录，所以/etc/profile对其无效，是用户变量，只对登录的所有用户有效，而/etc/environment为系统变量，对系统有效；
- 查看系统：lscpu
- 关闭防火墙：sudo ufw status ; sudo ufw disable;

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
    
- 端口冲突：
	- fuser 3000/tcp
	- kill 28597
	
- windows复制文件到Linux
    1.Linux服务器：XFTP方式（建立连接） 或 远程工具XSHELL（登录直接yum安装） 或 Linux安装samba服务器
    2.Linux虚拟机：XFTP方式（虚拟与实体） 或 利用VMTOOL工具直接拖拽（安装的操作系统版本很新，都是Vmware的版本比较老，晚于操作系统出来）

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

- 打包apt安装包:
	- 下载安装包: sudo apt download openssh-server
	- 下载需要依赖: sudo apt build-dep --download-only -o dir::cache=/home/yuziqi/Download/myinstaller openssh-server
	- 下载额外的依赖: sudo apt download openssh-sftp-server openssh-client
	- 给所有文件权限: sudo chmod 777 *
	- 安装: sudo dpkg -i *.deb

- IP网络配置:
	sudo gedit /etc/network/interfaces
	# net for xingongsuo
	# auto ens33
	iface ens33 inet static
	address 10.10.28.102 
	netmask 255.255.254.0
	gateway 10.10.28.1

- dia工具:
	- sudo apt install dia
	- dia
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

- 安装xgboost：


- 非root用户安装软件：
	1. 获取源代码，一般是wget方式，ubuntu可以使用apt-get source来获取源代码。
	2. 解压源代码，一般使用tar -zxvf xxx.tar.gz即可
	3. 切换到解压后的目录，运行 ./configure。其选项可以通过 ./configure –help来获取，非root用户下最重要的应该是定义安装目录，即应该定义 ./configure –prefix=/path/to/bin， 对于一些依赖库，可能还需要使用 ./configure  –prefix=xxx –with-xx-dir=xxx这种形式。
	4. 接着是编译源代码和安装软件： make &&  make install。这两条命令可以分开来用，因为编译的时候可以指定参数 -j来并行编译，这样能够加快编译进度。。
	5. 更新path路径。使用export PATH=/path/to/bin:$PATH，这句话在shell窗口运行只在本次会话中有效，可以将其写到.bashrc或者.bash_profile里面使其对当前用户有效。
	6.如果安装的是动态链接库，则需要更新动态链接库路径： export LD_LIBRARY_PATH=/path/to/library:$LD_LIBRARY_PATH，同样是export命令，最好将其写在.bashrc这类文件下面以便登陆的时候自动调用。
	
- install pycharm
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



# shell:
1. ll -rt

# vim技巧：

 - 配置vim：https://github.com/ma6174/vim
 - 手动配置vim：
 	+ vim ~/.vimrc
 	+ set nu "显示行号，nonu不显示行号
 	+ set hlserarch "搜索时关键字高亮反白
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
 	+ 

- vim技巧：
	1. w跳词头，e跳词尾
    2. ctrl+s是停止输入;ctrl+q是继续输入;看不见输入的内容了，直接输入exit回车，退出该终端;
    3. ctrl+a回到文件开头

 - vim粘贴：
   1. 系统粘贴板：复制后 - 在vim的编辑模式直接shift+inset键;
   2. vim粘贴板：esc正常模式 - v进入可视模式 - 选中文本 - 按y复制 - 用p粘贴

 - vim撤销：
   1. :u 或 :u[2]
   2. ctrl+R: 重做操作

- git上配置的vim使用：
	1. F5编译，F8调试
	2. 自动插入文件头;
	3. F2取消代码中的空行;
	4. ctrl+P 自动补全
	5. 



# 在github上托管博客
1. create a repository and named ：hacker.github.io
	- 进入github，找到设置，最下面有删除
2. clone the repository: sudo git clone https://github.com/hacker.github.io/
	
3. 最终打开：
	- https://a550461053.github.io/hacker

# git使用:
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
	- 5. 报错处理
		- fatal: remote origin already exists
			- 先执行: git remote rm origin
		- error:failed to push som refs to.......
			- 先执行: git pull origin master
	- 6. 设置免密码登录

7. 创建分支：
	- 创建分支：git branch dev
	- 切换分支：git checkout dev
	- 创建+切换：git checkout -b dev
	- 查看当前分支：git branch
	- 合并分支：在master下，git merge dev
	- 删除分支：git branch -d dev
	- 
8. 多人协作：
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
9. 常用命令：
	- cd G:/
	- 查看历史记录：git log
	- 查看历史记录的版本号：git reflog
	- 对工作区的文件修改撤销：git checkout 2.py
	- 		


# Ubuntu提速
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
11. 


# kali-linux:
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

- wifi:
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
	- node -v
	- 更新：
	- node更新:		
		* npm cache clean -f
		* npm install -g n
		* n stable
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

2. 包管理工具: npm
	- ...
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

应用程序有两种模式C/S、B/S。
C/S是客户端/服务器端程序，也就是说这类程序一般独立运行。
B/S就是浏览器端/服务器端应用程序，一般借助IE等浏览器来运行。

服务器端是远程服务器，运行结果是由服务器产生的；
	所有的动态网页都是在服务器端执行的，例如ASP、PHP、JSP；
客户端就是网友的电脑。
	所有的静态网页都是在客户端执行的，例如JavaScript、Flash都是属于静态网页。 







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
