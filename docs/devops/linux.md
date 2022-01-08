# Linux



## Linux系统的运行级别

Linux系统有7种运行级别(`runlevel`)

**运行级别0**：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动

**运行级别1**：单用户工作这台，root权限，用于系统维护，禁止远程登录

**运行级别2**：多用户状态（没有`NFS`），不支持网络

**运行级别3**：完全的多用户状态（有`NFS`），登录后进入控制台命令行模式

**运行级别4**：系统未使用，保留

**运行级别5**：X11控制台，登录后进入图形GUI模式

**运行级别6**：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动



## 常用服务类相关命令

### service

> 注册在系统中的标准化程序

常用方法

```bash
# CentOS 6
service 服务名 start
service 服务名 stop
service 服务名 restart
service 服务名 reload
service 服务名 status

# CentOS 7
systemclt start 服务名(xxx.service)
systemclt stop 服务名(xxx.service)
systemclt restart 服务名(xxx.service)
systemclt reload 服务名(xxx.service)
systemclt status 服务名(xxx.service)
```

查看服务的方法

```bash
# CentOS 6
# 通过文件查看
ls /etc/init.d
# 通过chkconfig查看
chkconfig --list | grep xxx

# CentOS 7
# 通过文件查看
cd /usr/lib/systemd/system
# 通过systemclt查看
systemctl list-unit-files
systemctl --type service
```

设置自启动

```bash
# CentOS 6
chkconfig --level 5 服务名 on
chkconfig --level 5 服务名 off

# CentOS 7
systemctl enable service_name
systemctl dable service_name
```



