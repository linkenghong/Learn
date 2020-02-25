## 概述
建议使用shadowsocks-libev + simple-obfs，前者是ss的轻量级应用，后者是流量混淆的。

因为在实验室经常出现手机能上vpn，但用实验室网就上不了。具体表现是一开始ping得通，但一开ss就ping不通，过一会又行，估计GFW已经能检测到异常的带ss特征的流量，然后对目的IP进行短暂封闭。

### 1. [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)

#### 安装

* Debian 8 或更高，Ubuntu 16.10 或更高可直接安装：

    ```
	sudo apt update
	sudo apt install shadowsocks-libev
	```

* 对于 Ubuntu 14.04 and 16.04 使用者，建议通过PPA安装:
	```
	sudo apt-get install software-properties-common -y
	sudo add-apt-repository ppa:max-c-lv/shadowsocks-libev -y
	sudo apt-get update
	sudo apt install shadowsocks-libev
	```
* 也可通过源码安装：
	```
	sudo apt-get install --no-install-recommends build-essential autoconf libtool libssl-dev gawk debhelper dh-systemd init-system-helpers pkg-config asciidoc xmlto apg libpcre3-dev zlib1g-dev libev-dev libudns-dev libsodium-dev
	git clone https://github.com/shadowsocks/shadowsocks-libev.git
	cd shadowsocks-libev
	git submodule update --init
	./autogen.sh && ./configure && make
	sudo make install
	```




### 2. [simple-obfs](https://github.com/shadowsocks/simple-obfs)

#### 安装
```
# Debian / Ubuntu
sudo apt-get install --no-install-recommends build-essential autoconf libtool libssl-dev libpcre3-dev libev-dev asciidoc xmlto automake

git clone https://github.com/shadowsocks/simple-obfs.git
cd simple-obfs
git submodule update --init --recursive
./autogen.sh
./configure && make
sudo make install
```

#### 使用
```
ss-server -c /etc/shadowsocks.json --plugin obfs-server --plugin-opts "obfs=http"
```

命令参数可通过`ss-server -h`查看，注意此处插件参数需与客户端一致。

后台使用的话使用以下命令
```
nohup ss-server -c /etc/shadowsocks.json  --plugin obfs-server --plugin-opts "obfs=http" > /dev/null 2>&1 &
```

### 3. [windows客户端](https://github.com/shadowsocks/shadowsocks-windows)
#### 客户端下载
在[releases](https://github.com/shadowsocks/shadowsocks-windows/releases)下下载最新版本，解压即可
#### 插件下载
在[simple-obfs](https://github.com/shadowsocks/simple-obfs/releases)中下载最新插件，和ss客户端放在同一路径下
#### 配置
本地配置与服务器端的需相同，插件程序填`obfs-local`，插件选项填`obfs=http;obfs-host=www.bing.com`


### 4. [谷歌学术](https://scholar.google.com/)
如果是ss是可以通过在`/etc/hosts` 中加入谷歌学术的IPv6地址来访问的，但由于shadowsocks-libev默认走IPv4，即使修改了hosts也没用。

所以需要通过[Unbound](https://github.com/sjtug/kxsw/wiki/Google-Scholar)，[参考步骤](http://fpcsongazure.top/how-to-fuck-gfw-to-get-a-paper/)如下：
#### 安装
   ```
   apt-get install -y unbound
   ```
#### 修改文件
在`/etc/unbound/`建立一个文本文件`google_scholar`(vim google_scholar)，写入：
```
local-data: "scholar.google.com AAAA 2404:6800:4008:c06::be"
```

再建立一个文件`google_scholar_bib`，写入：
```
local-data: "scholar.google.cn AAAA 2607:f8b0:4005:80a::200e"
local-data: "scholar.google.com.hk AAAA 2607:f8b0:4005:80a::200e"
local-data: "scholar.google.com.sg AAAA 2607:f8b0:4005:80a::200e"
local-data: "scholar.google.com.tw AAAA 2607:f8b0:4005:80a::200e"
local-data: "scholar.google.com.uk AAAA 2607:f8b0:4005:80a::200e"
local-data: "scholar.google.com AAAA 2607:f8b0:4005:80a::200e"
local-data: "scholar.l.google.com AAAA 2607:f8b0:4005:80a::200e"
```

之后修改unbound.conf文件如下：

```
# Unbound configuration file for Debian.
#
# See the unbound.conf(5) man page.
#
# See /usr/share/doc/unbound/examples/unbound.conf for a commented
# reference config file.
#
# The following line includes additional configuration files from the
# /etc/unbound/unbound.conf.d directory.
#
server:
    auto-trust-anchor-file:"/var/lib/unbound/root.key"
#include: "/etc/unbound/unbound.conf.d/*.conf"
include: "/etc/unbound/google_scholar"
include: "/etc/unbound/google_scholar_bib"
```

修改完成之后`service unbound restart` 并重启ss服务了
