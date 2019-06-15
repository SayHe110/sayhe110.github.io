---
title: Homestead 下切换 PHP 版本
date: 2019-02-11 21:54:22
tags:
---
因为原来使用 `Laravel` 框架的原因，所以一直使用着 `Homestead`，所以就会遇到需要切换 PHP 版本的问题
1. 可以在 `Homestead.yaml` 中指定 PHP 版本，如：
![blob.jpg](https://i.loli.net/2019/02/11/5c617e06a7d2a.jpg)

2. 可以在虚拟机环境中切换指定的 PHP 版本

> update-alternatives --display php 查看所有 php 版本和当前版本 <br>
update-alternatives --config php 执行后，会列出当前 php 所有版本和编号，输入编号，切换到执行的版本

![blob.jpg](https://i.loli.net/2019/02/11/5c617e5664c0d.jpg)

Linux 很注重权限，所以在执行命令时加上 `sudo` 即可