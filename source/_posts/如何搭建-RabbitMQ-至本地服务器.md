---
title: 如何搭建 RabbitMQ 至本地服务器
date: 2018-12-06 22:17:12
tags:
---
公司使用 RabbitMQ 来作消息队列，所以还是有必要学习以下 RabbitMQ 的（之前只有听说过没有接触过，所以还是趁早学比较好，😄）
本文参考：安装：https://blog.csdn.net/nextyu/article/details/79250174 <br>
错误：https://mysteps4learning.wordpress.com/2014/06/10/how-to-resolve-error-unauthorized-while-logging-to-rabbitmq-web-management/
### 环境：
Ubuntu 16.04
### 一、安装 Erlang
因为 RabbitMQ 是 Erlang 语言编写的，所以在安装 RabbitMQ 前，还是需要安装 Erlang 的。
首先在系统中添加 Erlang 库
```
wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb
sudo dpkg -i erlang-solutions_1.0_all.deb
```
接着
```
sudo apt-get update
sudo apt-get install erlang erlang-nox
```

### 二、安装 RabbitMQ
需要先在系统中加入 rabbitmq apt 仓库，再加入 rabbitmq signing key。
```shell
echo 'deb http://www.rabbitmq.com/debian/ testing main' | sudo tee /etc/apt/sources.list.d/rabbitmq.list
wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | sudo apt-key add -

sudo apt-get update
sudo apt-get install rabbitmq-server
```

这样就安装完成，在安装的过程中我并没有遇到什么问题、困难（没啥坑）

### 三、启用 RabbitMQ Web 管理插件
```shell
sudo rabbitmq-plugins enable rabbitmq_management
```
重启服务器
```shell
sudo systemctl restart rabbitmq-server
```
访问 `http://localhost:15672` ，默认账户、密码 `guest/guest`（图就不贴了。。编辑器没法获取粘贴板的图片。。）

这里我遇到一个坑。。。
会出现：
```shell
=ERROR REPORT==== 10-Jun-2014::10:27:17 ===
webmachine error: path="/api/whoami"
"Unauthorized"
```
解决的办法：
1. Add a new/fresh user, say user ‘test’ and password ‘test’
```shell
rabbitmqctl add_user test test
```
2.  Give administrative access to the new access
```shell
rabbitmqctl set_user_tags test administrator
```
3.  Set permission to newly created user
```shell
rabbitmqctl set_permissions -p / test ".*" ".*" ".*"
```
下载好 rabbitmq 之后，在 `/etc/rabbitmq` 目录下面默认没有配置文件，需要单独下载，[下载地址](https://github.com/rabbitmq/rabbitmq-server/blob/master/docs/rabbitmq.conf.example)
下载完成后重命名为 `rabbitmq.config`，接着找到 `lookback_users` 的地方去掉注释，修改好后的 `rabbitmq.config` 放置在 `/etc/rabbitmq` 目录下面

然后重启服务器
```shell
sudo systemctl restart rabbitmq-server
```
然后就可以在其他地址下登录
