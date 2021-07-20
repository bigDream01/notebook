### 连接问题

可以通过关闭防火墙

```shell
systemctl stop firewalld
```

或者开放端口号来开启连接

```shell
firewall-cmd --add-port=3306/tcp --permanent
```

### 启动问题

> 一般设置的是自启动

添加开机启动RabbitMQ服务

```shell
chkconfig rabbitmq-server on
```

启动服务

```shell
/sbin/service rabbitmq-server start
```

查看服务状态

```shell
/sbin/service rabbitmq-server status
```

