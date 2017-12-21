---
title: windows技巧
--- 

环境变量：不区分大小写——使用cmd方式只在当前cmd有效，暂时性的
1、查看当前所有可用的环境变量：输入 set 即可查看。
2、查看某个环境变量：输入 “set 变量名”即可，比如想查看path变量的值，即输入 set path，不用！！！
3、修改环境变量 ：输入 “set 变量名=变量内容”
4、设置为空：如果想将某一变量设置为空，输入“set 变量名=”
5、给变量追加内容（不同于3，那个是覆盖）：输入“set 变量名=%变量名%;变量内容”
getenv是获取系统的环境变更，对于windows在系统属性-->高级-->环境变量中设置的变量将显示在此(对于linux,通过export设置的变量将显示在此)
getProperties是获取系统的相关属性,包括文件编码,操作系统名称,区域,用户名等,此属性一般由jvm自动获取,不能设置.

6. 资源管理器卡死：文件夹选项-查看-勾选“在单独的进程中打开文件夹窗口”

7. windows7自动关机：cmd - 输入：shutdown -f -s -t 3600*n个小时；-f表示不警告、-s关闭、-t多少秒后关闭；
        取消自动关机：shutdown -a
  
8. markdown:
      <pre> 
      Pubkey script: OP_HASH160 <Hash160(redeemScript)> OP_EQUAL
Signature script: <sig> [sig] [sig...] <redeemScript>
      <code>
      ```
        printf("测试代码显示");
      ```



      测试行内`code`代码块显示；

![显示图片][image1]
[image1]:D:\yuziqi\学习记录\区块链\区块链笔记\a.jpg 
      

9. ip地址重复是路由器分配错误导致的，因为主机ip是自动获取的，所以重启路由器即可；没有有效ip，其实也可能是路由器分配错误导致，也可能是自己改成了静态ip地址，有可能是改了dns配置，也可能是路由器DHCP功能卡死，当然都可以改为自动ip和dns，重置自己主机的网络配置，然后重启主机，重新分配ip，一般不会分配错误，所以才可以；管理员权限命令行：netsh winsock reset

10. win10的powershell配置
    - 把开始按钮右键菜单（Win+X菜单）中的“Windows PowerShell”改回“命令提示符”
    - shift+右键打开cmd：
        + 打开注册表编辑器：regedit

11. word出现很抱歉。。。
    - win+R，输入regedit
    - 找到以下键值：HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Word\Options，在右侧新建DWORD值，取名为NoReReg，并输入数值为1 并确认，关闭注册表编辑器，重新打开Word 即可
    - word16破解：直接用kms一键激活。

12. InftyReader公式识别工具：
    1.将“InftyReg.dll”文件（crack文件夹中）复制到安装文件夹（InftyReader\bin 目录下）
    2.打开软件，选择register，在“Serial No.” 中输入：111111-222222
    3.点击“Online authentication”按钮
    4.程序将关闭并提示错误消息（也可能不提示）
    5.在“License key”中输入：11111111112222222222
    6.破解成功

13. win10重装edge
    - 1. 卸载edge：文件夹地址栏输入：%USERPROFILE%\AppData\Local\Packages 
    - 找到Microsoft.MicrosoftEdge_8wekyb3d8bbwe文件夹，删除之
    - 2. 重装Edge浏览器：以管理员身份打开PowerShell：Get-AppXPackage -AllUsers -Name Microsoft.MicrosoftEdge | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register “$($_.InstallLocation)\AppXManifest.xml” -Verbose}
    - 按下回车，等待黄色背景滚动完毕后，重启计算机即可。
    

win10：
    win+E快捷键修改为我的电脑：查看 - 选项 - 资源管理器改为此电脑；
    修改alt+tab显示风格：改了不好看~regedit在HKEY_CURRENT_USER下搜索Explorer-新建一个32位值，键值为AltTabSettings，数据改为1，默认是3*7，搜索Desktop，修改CoolSwitchColumns即可。
	
    查看windows核心数：cmd   —— wmic —— cpu get * —— name代表CPU，numberofcores表示CPU核数，numberoflogicalprocessors表示是否超线程，为4

    关闭自动更新/补丁更新：打开更新和安全 - win更新 - 高级选项 - 如何提供更新 - 取消来自更多位置的更新；

win10 chrome：修改安装路径下的chrome.exe的名字。
    （1）在桌面上建立一个“新建文本文档.txt”。
    具体做法是：在桌面上空白处点击鼠标右键，在“新建”菜单中用鼠标左键点击“文本文档”。
    （2）把“新建文本文档.txt”重命名为“Go.html”(可以随便取名字,只要后缀是html即可)。
    具体做法是：鼠标移到“新建文本文档.txt”上，点击右键，找到“重命名”，点击左键，把“新建文本文档.txt”命名为“Go.html”。
    双击“Go.html”文件，就可以打开默认的浏览器了（是个空白页面）。在地址栏输入你喜欢的主页，以后只需要点击地址栏下拉列表中的网址就可以啦。
    Go.html是个空白文件，用它来启动浏览器的速度似乎更快哦！


sublime：
    1. package install不好用的话，add channel：https://packagecontrol.io/channel_v3.json
    2. 配置java编译环境
        - sublime的java.sublime-package，用压缩软件打开，找到JavaC.sublime-build并打开，添加：
            ```
            {
            "shell_cmd": "runJava.bat \"$file\"",
            "file_regex": "^(...*?):([0-9]*):?([0-9]*)",
            "selector": "source.java",
            //添加下面一段可支持编译中文，亲测Java可用
            "encoding": "GBK"
            }
            ```
        - JDK/bin下，新建一个文件,命名为runJava.bat:
            ```
            @ECHO OFF
            cd %~dp1
            IF EXIST %~n1.class (
            DEL %~n1.class
            )
            javac -encoding UTF-8 %~nx1
            IF EXIST %~n1.class (
            java %~n1
            )
            ```

            
cmd命令大全（第一部分）
　　winver---------检查Windows版本 
　　wmimgmt.msc----打开windows管理体系结构(WMI) 
　　wupdmgr--------windows更新程序 
　　wscript--------windows脚本宿主设置 
　　write----------写字板 
　　winmsd---------系统信息 
　　wiaacmgr-------扫描仪和照相机向导 
　　winchat--------XP自带局域网聊天

cmd命令大全（第二部分）
　　mem.exe--------显示内存使用情况 
　　Msconfig.exe---系统配置实用程序 
　　mplayer2-------简易widnows media player 
　　mspaint--------画图板 
　　mstsc----------远程桌面连接 
　　mplayer2-------媒体播放机 
　　magnify--------放大镜实用程序 
　　mmc------------打开控制台 
　　mobsync--------同步命令
cmd命令大全（第三部分）
　　dxdiag---------检查DirectX信息 
　　drwtsn32------ 系统医生 
　　devmgmt.msc--- 设备管理器 
　　dfrg.msc-------磁盘碎片整理程序 
　　diskmgmt.msc---磁盘管理实用程序 
　　dcomcnfg-------打开系统组件服务 
　　ddeshare-------打开DDE共享设置 
　　dvdplay--------DVD播放器
cmd命令大全（第四部分）
　　net stop messenger-----停止信使服务 
　　net start messenger----开始信使服务 
　　notepad--------打开记事本 
　　nslookup-------网络管理的工具向导 
　　ntbackup-------系统备份和还原 
　　narrator-------屏幕“讲述人” 
　　ntmsmgr.msc----移动存储管理器 
　　ntmsoprq.msc---移动存储管理员操作请求 
　　netstat -an----(TC)命令检查接口
cmd命令大全（第五部分）
　　syncapp--------创建一个公文包 
　　sysedit--------系统配置编辑器 
　　sigverif-------文件签名验证程序 
　　sndrec32-------录音机 
　　shrpubw--------创建共享文件夹 
　　secpol.m转载自电脑十万个为什么http://www.qq880.com，请保留此标记sc-----本地安全策略 
　　syskey---------系统加密，一旦加密就不能解开，保护windows xp系统的双重密码 
　　services.msc---本地服务设置 
　　Sndvol32-------音量控制程序 
　　sfc.exe--------系统文件检查器 
　　sfc /scannow---windows文件保护
cmd命令大全（第六部分）
　　tsshutdn-------60秒倒计时关机命令 
　　tourstart------xp简介（安装完成后出现的漫游xp程序） 
　　taskmgr--------任务管理器 
　　eventvwr-------事件查看器 
　　eudcedit-------造字程序 
　　explorer-------打开资源管理器 
　　packager-------对象包装程序 
　　perfmon.msc----计算机性能监测程序 
　　progman--------程序管理器 
　　regedit.exe----注册表 
　　rsop.msc-------组策略结果集 
　　regedt32-------注册表编辑器 
　　rononce -p ----15秒关机 
　　regsvr32 /u *.dll----停止dll文件运行 
　　regsvr32 /u zipfldr.dll------取消ZIP支持
cmd命令大全（第七部分）
　　cmd.exe--------CMD命令提示符 
　　chkdsk.exe-----Chkdsk磁盘检查 
　　certmgr.msc----证书管理实用程序 
　　calc-----------启动计算器 
　　charmap--------启动字符映射表 
　　cliconfg-------SQL SERVER 客户端网络实用程序 
　　Clipbrd--------剪贴板查看器 
　　conf-----------启动netmeeting 
　　compmgmt.msc---计算机管理 
　　cleanmgr-------垃圾整理 
　　ciadv.msc------索引服务程序 
　　osk------------打开屏幕键盘 
　　odbcad32-------ODBC数据源管理器 
　　oobe/msoobe /a----检查XP是否激活 
　　lusrmgr.msc----本机用户和组 
　　logoff---------注销命令 
　　iexpress-------木马捆绑工具，系统自带 
　　Nslookup-------IP地址侦测器 
　　fsmgmt.msc-----共享文件夹管理器 
　　utilman--------辅助工具管理器 
　　gpedit.msc-----组策略


