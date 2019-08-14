# 显卡驱动
1. 驱动安装文件下载

   从[Nvidia](http://www.nvidia.cn/Download/index.aspx?lang=cn) 官网下载显卡对应的驱动安装文件
   如果不清楚显卡型号，可使用下列命令查询
   ```
   lspci -vnn | grep VGA
   ```
   返回的类似下面：
   ```
   01:00.0 VGA compatible controller [0300]: NVIDIA Corporation Device [10de:1b06] (rev a1) (prog-if 00 [VGA controller])
   ```
   以上显示厂家是NVIDIA，Device后[]内就是显卡的Device ID，可以拿这个去查询。

2. 准备工作(可选)

   在待安装驱动的主机上打开一个终端(Ctrl+Alt+T)，或者直接切换到终端界面(Ctrl+Alt+F1),进行如下操作：

   卸载可能存在的旧版本 nvidia 驱动（可选，对没有安装过 nvidia 驱动的主机，这步可以省略，但推荐执行，无害）
   ```
   sudo apt-get remove --purge nvidia*
   ```
   安装驱动可能需要的依赖(可选)
   ```
   sudo apt-get update

   sudo apt-get install dkms build-essential linux-headers-generic
   ```
   把 nouveau 驱动加入黑名单（以下一直到第3步很多地方都要求需要操作，但我装的时候没操作也没问题）
   ```
   sudo nano /etc/modprobe.d/blacklist-nouveau.conf
   ```
   在文件 blacklist-nouveau.conf 中加入如下内容：
   ```
   blacklist nouveau
   blacklist lbm-nouveau
   options nouveau modeset=0
   alias nouveau off
   alias lbm-nouveau off
   ```
   禁用 nouveau 内核模块
   ```
   echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
   sudo update-initramfs -u
   ```

3. 运行驱动安装文件

   切换到终端界面(Ctrl+Alt+F1)，关闭图形界面
   ```
   sudo service lightdm stop
   或者
   sudo lightdm stop
   ```
   在驱动所在路径下安装驱动
   ```
   bash NVIDIA∗∗∗.run
   ```
   重启图形界:
   ```
   sudo service lightdm start
   或者
   sudo lightdm start
   ```
   重启

# 网卡驱动
1. 查看网卡型号及是否安装驱动
   
   终端下（Ctrl+Alt+T）输入命令查看网卡型号：
   ```
   lspci | grep -i eth
   lspci -vv | grep -A 10 -i eth
   ``` 
   上面第二条命令(-A为显示字符匹配后x行的信息，x为-A后跟的数字）输入后大概显示如下信息：
   ```
   00:1f.6 Ethernet controller: Intel Corporation Device 15d6
	Subsystem: ASUSTeK Computer Inc. Device 8672
	Control: I/O- Mem+ BusMaster+ SpecCycle- MemWINV- VGASnoop- ParErr- Stepping- SERR- FastB2B- DisINTx+
	Status: Cap+ 66MHz- UDF- FastB2B- ParErr- DEVSEL=fast >TAbort- <TAbort- <MAbort- >SERR- <PERR- INTx-
	Latency: 0
	Interrupt: pin A routed to IRQ 132
	Region 0: Memory at df300000 (32-bit, non-prefetchable) [size=128K]
	Capabilities: <access denied>
	Kernel driver in use: e1000e
   ```
   如果没有Kernel driver这行证明没有驱动，以上信息第一行也显示了网卡Device id为15d6

   也可输入以下命令查看网卡是否有驱动：
   ```
   ifconfig
   ```
   输入后返回如下：
   ```
   eth0      Link encap:以太网  硬件地址 70:4d:7b:8a:39:09  
          inet 地址:192.168.51.213  广播:192.168.51.255  掩码:255.255.255.0
          inet6 地址: fe80::724d:7bff:fe8a:3909/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  跃点数:1
          接收数据包:32227 错误:0 丢弃:0 过载:0 帧数:0
          发送数据包:31313 错误:0 丢弃:0 过载:0 载波:0
          碰撞:0 发送队列长度:1000 
          接收字节:14983356 (14.9 MB)  发送字节:5264987 (5.2 MB)
          中断:16 Memory:df300000-df320000 

   lo        Link encap:本地环回  
          inet 地址:127.0.0.1  掩码:255.0.0.0
          inet6 地址: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  跃点数:1
          接收数据包:4218 错误:0 丢弃:0 过载:0 帧数:0
          发送数据包:4218 错误:0 丢弃:0 过载:0 载波:0
          碰撞:0 发送队列长度:1 
          接收字节:463862 (463.8 KB)  发送字节:463862 (463.8 KB)
   ```
   如果只有lo没有eth0，则说明没有安装驱动。

2. 从[官网](http://tenet.dl.sourceforge.net/project/e1000)下载驱动
   选择 e1000e stable文件夹后选择驱动下载即可，尽量选高版本比较多人下载，高版本一般都向下兼容的。（我选的3.3.5）

3. 安装驱动
   在下载驱动所在路径下，执行以下操作：
   ```
   tar -xvf e1000e-3.3.5.tar.gz  --解压
   cd e1000e-3.3.5/src/          --找到程序所在目录
   sudo make install             --安装
   sudo modprobe e1000e          --载入
   sudo dhclient eth0            --dhclient 是直接控制 eth 来进行网络操作自动获取 IP 
   ```
   以上操作完成后如果还是不可以就重启即可。

