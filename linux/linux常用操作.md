# linux 常用操作

```shell
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 3000
sudo iptables-save
```

## mac系统普通用户如何创建管理员账户

- 开机的时候按住 `command+s`
- 出现命令行终端的时候按照以下顺序输入命令： 
```shell
/sbin/mount -uaw
rm var/db/.applesetupdone
reboot
```

## SCP传文件失败

```
bash: scp: command not found
lost connection
```
解决方式：
```shell
brew -y install openssh-clients
```