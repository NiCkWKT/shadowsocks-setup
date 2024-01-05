# Shadowsocks server set up

- Backup for https://github.com/flyzy2005/ss-fly and https://github.com/mku228/vpn-ss-ssr

## My Choice of Server

Red Hat Enterprise Linux 9 (HVM), SSD Volume Type on AWS

## Setup

```bash
sudo yum update
sudo yum install git gcc make cmake unzip -y
git clone -b master https://github.com/flyzy2005/ss-fly
sudo ss-fly/ss-fly.sh -i <password> <port>
sudo -i # change to root
/usr/local/bin/ssserver -c /etc/shadowsocks.json -d start # or restart
exit
sudo ss-fly/ss-fly.sh -bbr
```

---

# Reference from https://github.com/mku228/vpn-ss-ssr

## 一键搭建 shadowsocks

1.下载一键搭建 ss 脚本文件（直接复制这段代码运行即可）

git clone -b master https://github.com/flyzy2005/ss-fly

![image](https://user-images.githubusercontent.com/98797623/152299551-a189f370-3548-42ad-b015-c3765f610397.png)

2.运行搭建 ss 脚本代码

ss-fly/ss-fly.sh -i 你的密码 1024
其中【你的密码】换成你要设置的 shadowsocks 的密码即可（这个【你的密码】就是你 ss 的密码了，是需要填在客户端的密码那一栏的），密码随便设置，最好只包含字母+数字，一些特殊字符可能会导致冲突。而第二个参数 1024 是端口号，也可以不加，不加默认是 1024~（举个例子，脚本命令可以是 ss-fly/ss-fly.sh -i qwerasd，也可以是 ss-fly/ss-fly.sh -i qwerasd 8585，后者指定了服务器端口为 8585，前者则是默认的端口号 1024，两个命令设置的 ss 密码都是 qwerasd）：

![image](https://user-images.githubusercontent.com/98797623/152299779-c24d8995-4040-4ad8-8f5e-820ba35cdfe6.png)

出现如下界面就说明搭建好了：

![image](https://user-images.githubusercontent.com/98797623/152299917-3a0e7d08-0f65-438d-9f8b-c7b387416b90.png)

注：如果需要改密码或者改端口，只需要重新再执行一次搭建 ss 脚本代码就可以了，或者是修改/etc/shadowsocks.json 这个配置文件，之后重启 ss 服务。

3.相关 ss 操作

```
修改配置文件：vim /etc/shadowsocks.json
停止ss服务：ssserver -c /etc/shadowsocks.json -d stop
启动ss服务：ssserver -c /etc/shadowsocks.json -d start
重启ss服务：ssserver -c /etc/shadowsocks.json -d restart
```

4.卸载 ss 服务

```
ss-fly/ss-fly.sh -uninstal
```

## 一键搭建 shadowsocksR

再次提醒，如果安装了 SS，就不需要再安装 SSR 了，如果要改装 SSR，请按照上一部分内容的教程先卸载 SS！！！

1.下载一键搭建 ssr 脚本（只需要执行一次，卸载 ssr 后也不需要重新执行）

```
git clone -b master https://github.com/flyzy2005/ss-fly
```

2.运行搭建 ssr 脚本代码

```
ss-fly/ss-fly.sh -ssr
```

![image](https://user-images.githubusercontent.com/98797623/152300053-f4fb965b-3124-40b9-b7ed-a3783bfc8db0.png)

3.输入对应的参数

执行完上述的脚本代码后，会进入到输入参数的界面，包括服务器端口，密码，加密方式，协议，混淆。可以直接输入回车选择默认值，也可以输入相应的值选择对应的选项：

![image](https://user-images.githubusercontent.com/98797623/152300121-37a115de-4342-4be7-8fbe-aa4aabf84222.png)

全部选择结束后，会看到如下界面，就说明搭建 ssr 成功了：

```
Congratulations, ShadowsocksR server install completed!
Your Server IP        :你的服务器ip
Your Server Port      :你的端口
Your Password         :你的密码
Your Protocol         :你的协议
Your obfs             :你的混淆
Your Encryption Method:your_encryption_method

Welcome to visit:https://shadowsocks.be/9.html
Enjoy it!
```

4.相关操作 ssr 命令

启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status

配置文件路径：/etc/shadowsocks.json
日志文件路径：/var/log/shadowsocks.log
代码安装目录：/usr/local/shadowsocks

5.卸载 ssr 服务

```
./shadowsocksR.sh uninstall
```

# 一键开启 BBR 加速

[BBR](https://github.com/google/bbr)是 Google 开源的一套内核加速算法，可以让你搭建的 shadowsocks/shadowsocksR 速度上一个台阶，本一键搭建 ss/ssr 脚本支持一键升级最新版本的内核并开启 BBR 加速。

BBR 支持 4.9 以上的，如果低于这个版本则会自动下载最新内容版本的内核后开启 BBR 加速并重启，如果高于 4.9 以上则自动开启 BBR 加速，执行如下脚本命令即可自动开启 BBR 加速：

```
ss-fly/ss-fly.sh -bbr
```

![image](https://user-images.githubusercontent.com/98797623/152300367-3e441d52-be7a-4060-919b-7281f61285e3.png)

装完后需要重启系统，输入 y 即可立即重启，或者之后输入 reboot 命令重启。

判断 BBR 加速有没有开启成功。输入以下命令：

```
sysctl net.ipv4.tcp_available_congestion_control
```

如果返回值为：

```
net.ipv4.tcp_available_congestion_control = bbr cubic reno
```

后面有 bbr，则说明已经开启成功了。

# 开始使用自己的 VPN

## 各系统的下载地址：

1. Windows 客户端下载地址：https://github.com/shadowsocks/shadowsocks-windows/releases?after=2.5.1
2. Mac 客户端下载地址：https://github.com/shadowsocks/ShadowsocksX-NG/releases
3. Linux 客户端下载地址：https://github.com/shadowsocks/shadowsocks-qt5/wiki/Installation
4. Android/安卓客户端下载地址：https://github.com/shadowsocks/shadowsocks-android/releases。
5. iOS/苹果客户端直接在 App Store 里搜索 SsrconnectPro

## Windows 客户端配置

双击运行 shadowsocks.exe，之后会在任务栏有一个小飞机图标，右击小飞机图标，选择服务器->编辑服务器：

![image](https://user-images.githubusercontent.com/98797623/152300920-f583c226-89aa-4bc6-ba96-2bf4a16ce1c0.png)

在 shadowsocks 的 windows 客户端中，服务器 IP 指你购买的 VPS 的 IP，服务器端口指你服务器的配置文件中的端口，密码指你服务器的配置文件中的密码，加密指你服务器的配置文件中的加密方式，代理端口默认为 1080 不需要改动。其他都可以默认。设置好后，点击添加按钮即可。

## MAC OS 客户端配置

双击运行 shadowsocksX-NG.app，之后会在任务栏有一个小飞机图标，右击小飞机图标，选择服务器->服务器设置：

![image](https://user-images.githubusercontent.com/98797623/152301022-6db654af-d519-4be4-bd79-5221a387219f.png)

在 shadowsocks 的 Mac OS 客户端中，地址指你购买的 VPS 的 IP，冒号后面跟上配置文件中的端口，密码指你服务器的配置文件中的密码，加密指你服务器的配置文件中的加密方式。其他都可以默认。设置好后，点击确认即可。

## 安卓客户端配置

下载 apk 安装好后，打开影梭客户端，点击主界面左上角的编辑按钮（铅笔形状）：

![image](https://user-images.githubusercontent.com/98797623/152301087-f7822053-4397-4232-bbac-a7a026856f28.png)

在 shadowsocks 安卓客户端的配置中填入相应配置信息，其中，功能设置中，路由改成如上图所示，其他都可以默认。

## 苹果客户端配置

shadowsocks 苹果客户端经常会被 App Store 下架，可以在 App Store 搜索关键字 shadowsock 或者 wingy，找到一个软件截图中包括填写 ip，加密方式，密码的软件一般就是对的了（目前可以用的是 FirstWingy）。当然，你也可以下载 PP 助手，之后在 PP 助手上下载 Wingy（Wingy 支持 ssr）或者 shadowrocket（shadowrocket 支持 ssr）。

![image](https://user-images.githubusercontent.com/98797623/152301125-c3da2953-9bbf-4a87-9270-82e1f4a2bf4a.png)
