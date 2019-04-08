---
title: venv的安装和使用
date: 2019-04-08 20:30:57
tags: memo
---
# 安装venv

```
python3 install virtualenv
```

# venv的使用

+ 在当前目录创建虚拟环境

```
python3 -m venv .
```

<!--more-->

+ 在当前目录创建独立的python环境

```
virtualenv --no-site-packages venv
```

+ 激活虚拟环境

```
source venv/bin/activate
```

+ 停用虚拟环境

```
deactivate
```

+ 删除虚拟环境

```
rm -rf venv
```

# 参考网站

<https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001432712108300322c61f256c74803b43bfd65c6f8d0d0000>

<https://www.cnblogs.com/LittleMore/p/6693154.html>

<https://www.cnblogs.com/technologylife/p/6635631.html>