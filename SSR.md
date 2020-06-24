SSR是SS的一个分支，据说可以减少被识别的可能性。折腾半天还是直接使用一键安装快些。。。。

### 1. Ubuntu升级内核并使用BBR加速

BBR是谷歌开源的拥塞控制算法，使用其可以提高网络速度。内核较高的Ubuntu已经自带BBR，所以建议把Ubuntu的内核升至5.0及以上。升级方法如下：

#### Ubuntu升级内核
```
# 查看内核版本号
uname -sr
uname -a
# 下载升级包
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2.4/linux-headers-5.2.4-050204_5.2.4-050204.201907280731_all.deb
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2.4/linux-headers-5.2.4-050204-generic_5.2.4-050204.201907280731_amd64.deb
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2.4/linux-image-unsigned-5.2.4-050204-generic_5.2.4-050204.201907280731_amd64.deb
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.2.4/linux-modules-5.2.4-050204-generic_5.2.4-050204.201907280731_amd64.deb
# 安装
sudo dpkg -i *.deb
# 安装后重启
reboot
```

在安装过程中可能回出现依赖问题，提示如下：
```
linux-headers-5.2.4-050204-generic : Depends: libssl1.1 (>= 1.1.0)
```
此时需要升级libssl1.1，命令如下：
```
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4_amd64.deb
sudo dpkg -i libssl1.1_1.1.0g-2ubuntu4_amd64.deb
```

#### 开启BBR
```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
# 开启BBR
sysctl net.ipv4.tcp_available_congestion_control
# 如果返回下面的结果代表开启成功
net.ipv4.tcp_available_congestion_control = reno cubic bbr
# 也可以通过lsmod | grep bbr来查看是否开始成功
```

### 2.SSR一键安装
```
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```
配置文件在/etc/shadowsocks-r/config.json
启动等：
```
启动SSR：
/etc/init.d/shadowsocks-r start
退出SSR：
/etc/init.d/shadowsocks-r stop
重启SSR：
/etc/init.d/shadowsocks-r restart
SSR状态：
/etc/init.d/shadowsocks-r status
卸载SSR：
./shadowsocks-all.sh uninstall
```

### 3. [windows客户端](https://github.com/shadowsocksrr/shadowsocksr-csharp)
#### 客户端下载
在[releases](https://github.com/shadowsocksrr/shadowsocksr-csharp/releases)下载最新版本，解压即可

#### 配置
本地配置与服务器端的需相同，可直接复制配置后的SSR链接


### 4.修改hosts登谷歌学术
不一定有效
在/etc/hosts中修改

```
## Scholar 学术搜索
2404:6800:4008:c06::be scholar.google.com
2404:6800:4008:c06::be scholar.google.com.hk
2404:6800:4008:c06::be scholar.google.com.tw
2404:6800:4005:805::200e scholar.google.cn #www.google.cn
```

