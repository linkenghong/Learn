# 故障解决
1. 登录界面输入正确密码后仍返回登录界面，无法进入桌面。

   现象：在Ubuntu登陆界面输入密码之后，黑屏一闪后，又跳转到登录界面。

   原因：主目录下的.Xauthority文件拥有者变成了root，从而以用户登陆的时候无法都取.Xauthority文件。

   说明：Xauthority，是startx脚本记录文件。Xserver启动时，读文件~/.Xauthority,读入对应其display的记录。当一个需要显示的客户程序启动调用XOpenDisplay()也读这个文 件，并把找到的magic code 发送给Xserver。

   当Xserver验证这个magic code正确以后，就同意连接啦。观察startx脚本也可以看到，每次startx运行，都在调用xinit以前使用了xauth的add命令添加了一个新的记录到~/.Xauthority，用来这次运行X使用认证

   解决方法：我们需要将.Xauthority的拥有者改为登陆用户

   开机后在登陆界面按下shift + ctrl + F1进入tty命令行终端登陆后输入：
   ```
   cd ~
   ls .Xauthority -l
   ```
   发现拥有者是root，所以出错了:
   ```
   -rw------- 1 root root 140 2月 28 10:41 .Xauthority
   ```
   输入：
   ```
   sudo chown 用户名:用户名 .Xauthority
   ```
   修改后输入`ls .Xauthority -l`显示如下：
   ```
   -rw------- 1 用户名 用户名 140 2月 28 10:41 .Xauthority
   ```
   此时拥有者已经变为用户。按下shift + ctrl + F7切换回图形登陆界面登陆即可。

# 硬件信息
1. 查看PCI

   显示系统中所有PCI总线设备或连接到该总线上的所有设备的工具，包括显卡、USB等。
   ```
   lspci
   
   lspci | grep xxx
   ```
   grep相当于匹配，以上语句即显示lspci返回信息中匹配xxx的信息
2. 查看CPU
   ```
   lscpu
   ```
3. 查看内存
   ```
   free -h
   ```
4. 查看硬盘
   ```
   df -h
   ```
   df命令的含义是disk free


# 命令
1. 打开文字界面

   `ctrl+alt+[F1/F2.../F6]` 
   
   切换回图形界面

   `ctrl+alt+F7`
   
   图形界面下打开终端

   `ctrl+alt+t`

   终端下在同一窗口下打开新终端

   `ctrl+shift+t`

1. cd相关命令

   `cd -`   切换到上一个目录

   `cd ..`  切换到上一级目录

   `cd --`  切换到home目录

1. 查看隐藏文件夹

   使用ctrl+h键

1. Ubuntu 查找文件夹中内容包含关键字的文件，路径为当前文件夹
   ```
   grep -rl "keyword" ./
   ```
   在根文件夹（也可详细写明路径）下查找含有关键字route的文件，列出文件名和route所在行。
   ```
   find / -name '*' | xargs grep 'route'
   ```
   在根文件夹（也可详细写明路径）下查找后缀名为txt且含有关键字route的文件，列出文件名和route所在行。
   ```
   find / -name '*.txt' | xargs grep 'route'
   ```
1. 查看IP信息
   ```
   ifconfig -a
   ```

1. ubuntu多线程下载
   ```
   sudo apt-get install axel
   axel [参数] 文件下载地址
    可选参数：
     -n 指定线程数
     -o 指定另存为目录
     -s 指定每秒的最大比特数
     -q 静默模式
   axel -n 10 -o /tmp/ http://testdownload.net/test.tar.gz
   ```
1. ubuntu开启及查看后台

   > **nohup _command_ > /dev/null 2>&1 &**
   ```
   /dev/null ：代表空设备文件
   > ：代表重定向到哪里，例如：echo "123" > /home/123.txt
   1 ：表示stdout标准输出 (the handle for standard output or STDOUT)，系统默认值是1，所以">/dev/null"等同于"1>/dev/null"
   2 ：表示stderr标准错误 (the handle for standard error or STDERR)
   & ：表示等同于的意思，2>&1，表示2的输出重定向等同于1
   ```
   \> /dev/null 2>&1 语句含义:
   ```
   > /dev/null ： 首先表示标准输出重定向到空设备文件，也就是不输出任何信息到终端，说白了就是不显示任何信息。
   2>&1 ：接着，标准错误输出重定向（等同于）标准输出，因为之前标准输出已经重定向到了空设备文件，所以标准错误输出也重定向到空设备文件。在结尾加上"&"来将命令同时放入后台运行。
   ```   

   查看运行的后台进程：
   > jobs -l
   
   jobs命令只看当前终端生效的，关闭终端后，在另一个终端jobs已经无法看到后台跑得程序了，此时利用ps（进程查看命令）

   > ps -aux|grep xx
   ```
   a:显示所有程序 
   u:以用户为主的格式来显示 
   x:显示所有程序，不以终端机来区分
   ```