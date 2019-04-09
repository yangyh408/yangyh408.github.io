---
title: DigitalOcean的使用
date: 2019-04-09 12:42:06
tags: memo
---

{% note info %}
## 创建Droplets
{% endnote %}

![创建][1]

<!--more-->

{% note info %}
## 配置选择
{% endnote %}

image 选择Ubuntu系统
Size 选第一个5$/mon
![配置][2]

{% note info %}
## 节点选择
联通用户推荐选择**SanFrancisco-01**
{% endnote %}

1. 通过ping来检测丢包率和延迟，200ms一下就很优秀
2. 通过[speedtest][9]来测单线程连接速率，能否把带宽跑满

![节点][3]

{% note info %}
## 查看服务器信息
{% endnote %}

进入您的邮箱，此时您会收到一封包含服务器详细登陆信息的邮件

![该服务器已注销][4]

{% note info %}
## 登陆服务器
{% endnote %}

在终端中输入：

```
ssh Username @ IP Address
```
![登陆][5]

{% note info %}
## 安装最新内核
{% endnote %}

```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```

![][6]

{% note warning %}
## 安装脚本
加密类型选择:
**14）chacha20-ietf-poly1305**
{% endnote %}

```
wget --no-check-certificate -O shadowsocks-libev-debian.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev-debian.sh
chmod +x shadowsocks-libev-debian.sh
./shadowsocks-libev-debian.sh 2>&1 | tee shadowsocks-libev-debian.log
```

稍等片刻，完成！

详细信息可以查看[秋水逸冰][10]的博文!

{% note info %}
## 导入Shadowsocks
{% endnote %}

在服务器设置中输入服务器的地址、节点、加密方法、密码即可
![Shadowsocks][7]

⚠️***注意***:
将指定网站加入PAC规则中：```|| www.example.com ^```

{% note success %}
### 福利鸭
{% endnote %}
完成在读大学生认证可以在[GitHub学生礼包][8]中免费领取$50的DigitalOcean抵用券嗷！

[1]:/images/post_images/DigitalOcean的使用-01.png
[2]:/images/post_images/DigitalOcean的使用-02.png
[3]:/images/post_images/DigitalOcean的使用-03.png
[4]:/images/post_images/DigitalOcean的使用-04.png
[5]:/images/post_images/DigitalOcean的使用-05.png
[6]:/images/post_images/DigitalOcean的使用-06.png
[7]:/images/post_images/DigitalOcean的使用-07.png
[8]:https://education.github.com/
[9]:https://speedtest.net/
[10]:https://teddysun.com/358.html