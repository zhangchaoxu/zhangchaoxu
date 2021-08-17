# Linux Cheat Sheet

### 查看centos版本
```
cat /etc/redhat-release
```

### 删除大文件
直接删除大文件,可能会因为文件被程序占用,文件被删除了,但是磁盘空间继续占用    
需要用一个空文件来替换
```
touch ./blank.txt 
rsync -a --delete-before --progress --stats ./blank.txt ./nohup.out
rm ./blank.txt
```

### 查看磁盘占用
```
df -h
```

### 查看本地IP
```
ip addr
```

### 查看出口IP
```
curl ifconfig.me
```

### 安装软件
比如cronolog
```
yum install -y cronolog httpd
```

### 启动java springboot程序
```
nohup java -Dspring.profiles.active=prod -jar api.jar --server.port=8080 --server.servlet.context-path=/api 2>&1 | cronolog log.%Y-%m-%d.out >> /dev/null &
```

### 开机启动
```
1. 编辑/etc/rc.d/rc.local,加入需要执行的命令
2. 添加权限chmod +x /etc/rc.d/rc.local
3. 重启reboot
4. 启动日志可以查看这里/var/log/boot.log
```

