# SuperVisor

参考：[http://supervisord.org/installing.html#creating-a-configuration-file](http://supervisord.org/installing.html#creating-a-configuration-file)

## 一、安装(使用easy_install　在线安装)
- １、如果没有`pip` ，先安装：

```shell
# 第一步
$ wget -c https://bootstrap.pypa.io/get-pip.py 
# 第二步
$ python get-pip.py
```

- 2、验证是否安装`pip`、`easy_install`(执行命令验证)，如果没有安装`easy_install` 则安装：

```shell
# 安装之后验证
$ pip install setuptools
```

- 3、使用`easy_install` 安装`superversion`，下面是命令：
```shell
$ easy_install supervisor
# 安装之后运行　echo_supervisord_conf,这将打印一个“示例”Supervisor配置文件到您的终端的stdout。到此supervervisord 安装成功。
```
## 二、使用
- 1、创建配置
```shell
# 使用root把配置文件模板导入文件
$ echo_supervisord_conf > /etc/supervisord.conf
```

- 2、修改配置文件：

```shell
$ vim /etc/supervisord.conf
# 其他的都不用更改，直接加入一个配置文件的加载就行
[include]
files = /home/tools/superversion/conf/*.conf　# 自己存储配置文件的目录
```

- 3、启动：

```shell
1、$ supervisord -c /etc/supervisord.conf #启动服务端
2、$ supervisorctl 　# 使用客户端验证连接
```

- 4、写一个测试配置：

```ini
# 在/home/tools/superversion/conf 下新建foo.conf文件，写入如下内容。
[program:foo]
command =/bin/cat
# 写好之后运行如下命令
$ supervisorctl update
```
- 5、使用`supervisorctl`去管理进程：

```shell
supervisor> stop foo # 停止foo这个进程
supervisor> start fool # 启动foo这个进程
supervisor> stop all # 停止所有的进程
supervisor> start all #　启动所有进程
supervisor> status #查看进程状态
supervisor> exit # 退出supervisorctl
```