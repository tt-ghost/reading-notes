# supervisor常用操作

>  supervisor 由 C/S 两部分组成：
>  supervisord(server 部分)：主要负责管理子进程，响应客户端命令以及日志的输出等；
>  supervisorctl(client 部分)：用户可以通过它与不同的 supervisord 进程联系，获取子进程的状态等。

## 安装

```
yum install epel-release
yum install -y supervisor
# 安装完后默认没有配置文件，可以通过以下命令生成
echo_supervisord_conf > /etc/supervisord.conf
```

## 配置

- 默认路径
    + 默认配置文件：`/etc/supervisord.conf`
    + 进程管理配置文件在：`/etc/supervisord.d/`目录下
    + 默认日志文件：`/tmp/supervisord.log`

- 查看配置

```
echo_supervisord_conf
```

- 启动

```
supervisord -c /etc/supervisor/supervisord.conf
```

- 进程控制

```
# 进入交互式操作
supervisorctl

# bash 操作
supervisorctl status
supervisorctl stop tomcat
supervisorctl start tomcat
supervisorctl restart tomcat
supervisorctl reread # 只载入最新的配置文件, 并不重启任何进程;
supervisorctl reload # 载入最新的配置文件，停止原来的所有进程并按新的配置启动管理所有进程;
supervisorctl update # 根据最新的配置文件，启动新配置或有改动的进程，配置没有改动的进程不会受影响而重启;
```

## 开机启动

### 配置systemctl服务

- 进入`/lib/systemd/system`目录，并创建`supervisor.service`文件

```
# supervisord service for systemd (CentOS 7.0+)
# by ET-CS (https://github.com/ET-CS)
[Unit]
Description=supervisor
After=network.target

[Service]
Type=forking
ExecStart=/usr/bin/supervisord -c /etc/supervisor/supervisord.conf
ExecStop=/usr/bin/supervisorctl $OPTIONS shutdown
ExecReload=/usr/bin/supervisorctl $OPTIONS reload
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```

- 设置开机启动

```
systemctl enable supervisor.service
systemctl daemon-reload
```

- 修改权限

```
chmod 766 supervisor.service
```

### 配置service类型服务

- 新建`/etc/rc.d/init.d/supervisor`

```
#!/bin/bash
#
# supervisord   This scripts turns supervisord on
#
# Author:       Mike McGrath <mmcgrath@redhat.com> (based off yumupdatesd)
#
# chkconfig:    - 95 04
#
# description:  supervisor is a process control utility.  It has a web based
#               xmlrpc interface as well as a few other nifty features.
# processname:  supervisord
# config: /etc/supervisor/supervisord.conf
# pidfile: /var/run/supervisord.pid
#

# source function library
. /etc/rc.d/init.d/functions

RETVAL=0

start() {
    echo -n $"Starting supervisord: "
    daemon "supervisord -c /etc/supervisor/supervisord.conf "
    RETVAL=$?
    echo
    [ $RETVAL -eq 0 ] && touch /var/lock/subsys/supervisord
}

stop() {
    echo -n $"Stopping supervisord: "
    killproc supervisord
    echo
    [ $RETVAL -eq 0 ] && rm -f /var/lock/subsys/supervisord
}

restart() {
    stop
    start
}

case "$1" in
  start)
    start
    ;;
  stop) 
    stop
    ;;
  restart|force-reload|reload)
    restart
    ;;
  condrestart)
    [ -f /var/lock/subsys/supervisord ] && restart
    ;;
  status)
    status supervisord
    RETVAL=$?
    ;;
  *)
    echo $"Usage: $0 {start|stop|status|restart|reload|force-reload|condrestart}"
    exit 1
esac

exit $RETVAL
```

- 配置开机启动

```
chmod 755 /etc/rc.d/init.d/supervisor
chkconfig supervisor on
```

注意：
- Supervisor只能管理非daemon的进程，必须以前台方式运行
- nginx 配置中加如下字段

```
daemon off;
```



