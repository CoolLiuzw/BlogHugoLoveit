# 如何在服务器重启后自动挂载Rclone-脚本


<!--more-->

先把rclone的可执行文件复制到/usr/bin：

```
cp /root/rclone-v1.39-linux-amd64/rclone /usr/bin/rclone
```

新建一个rclone.service文件：

```
vi /usr/lib/systemd/system/rclone.service
```

写入：

```
[Unit]
Description = rclone
    
[Service]
User = root
ExecStart = /usr/bin/rclone mount gdrive:/googledrive-path/ /local-path --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000
Restart = on-abort
    
[Install]
WantedBy = multi-user.target
```

重载daemon，让新的服务文件生效：

```
systemctl daemon-reload
```

现在就可以用systemctl来启动rclone了：

```
systemctl start rclone
```

设置开机启动：

```
systemctl enable rclone
```

停止、查看状态可以用：

```
systemctl stop rclone
systemctl status rclone
```

重启你的VPS，然后查看一下rclone的服务起来没，接着查看一下盘子挂上去没：

```
reboot
systemctl status rclone
df -h
```
