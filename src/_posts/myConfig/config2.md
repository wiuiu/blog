---
category: 我的电脑
tags:
  - configures
date: 2023-4-16
title: 换国内源
--- 

> 关于zsh的环境变量的设置，~/.zshrc（也就是zsh的配置文件），每次启动shell会运行这个配置文件，所以可以在.zshrc中用export添加环境变量。

# pip
换清华源
```zsh
python -m pip install --upgrade pip
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
查看源
```zsh
pip config list
```

# npm
更换淘宝镜像源（最新的地址）
```zsh
npm config set registry http://registry.npmmirror.com
```
查看当前使用源
```zsh
npm config get registry
```




