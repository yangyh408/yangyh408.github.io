---
title: Colaboratory的使用
date: 2019-04-08 20:44:10
tags: memo
---

# Google备份与同步

在使用Colaboratory之前，由于涉及GoogleDrive的文档同步，需要在电脑上安装Google备份与同步软件

## GoogleDrive安装
<https://www.google.com/intl/zh-CN_ALL/drive/download/backup-and-sync/>

<!--more-->
## 登陆问题处理
在安装完软件之后需要登陆Google账户，但是一直在提示“无法正常登陆，请检查网络连接”
在确认VPN可以正常使用后百度到了处理这类问题的正确姿势[解决Mac下Google备份和同步网络连接问题][1]
> 系统编号设置->网络->高级...->代理->网页代理(HTTP)->设置代理（127.0.0.1:1087）

![](/images/post_images/Colaboratory的使用-01.jpg)

登陆成功！

# 设置GPU加速
> 修改->笔记本设置->硬件加速器->GPU

1. 检查GPU是否启用

```
import tensorflow as tf
tf.test.gpu_device_name()
```

2. 显示GPU详细信息

```
from tensorflow.python.client import device_lib
device_lib.list_local_devices()
```

3. 显示CPU详细信息

```
!cat /proc/cpuinfo
```

# 使用Colab打开.py文件
{% note info %}

### 转载说明

该部分转载自[薅资本主义羊毛，用Google免费GPU][2]

{% endnote %}

1. 配置服务器

```
!apt-get install -y -qq software-properties-common python-software-properties module-init-tools
!add-apt-repository -y ppa:alessandro-strada/ppa 2>&1 > /dev/null
!apt-get update -qq 2>&1 > /dev/null
!apt-get -y install -qq google-drive-ocamlfuse fuse
from google.colab import auth
auth.authenticate_user()
from oauth2client.client import GoogleCredentials
creds = GoogleCredentials.get_application_default()
import getpass
!google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret} < /dev/null 2>&1 | grep URL
vcode = getpass.getpass()
!echo {vcode} | google-drive-ocamlfuse -headless -id={creds.client_id} -secret={creds.client_secret}
```
按步骤要求点击链接输入验证码即可

2. 授权完成后挂载GoogleDrive：

```
!mkdir -p drive
!google-drive-ocamlfuse drive
```

3. GoogleDrive文档打开位置

```
cd /drive/xxx
```

# 重置(reset)服务器

```
!kill -9 -1
```

[1]:https://www.fangpengjun.com/2017/09/08/%E8%A7%A3%E5%86%B3Mac%E4%B8%8BGoogle%E5%A4%87%E4%BB%BD%E5%92%8C%E5%90%8C%E6%AD%A5%E7%BD%91%E7%BB%9C%E8%BF%9E%E6%8E%A5%E9%97%AE%E9%A2%98/
[2]:https://zhuanlan.zhihu.com/p/33344222