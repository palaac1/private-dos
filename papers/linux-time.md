> 同步时区和修改时间为24H

- 同步时间

`CentOS 7`

```bash
yum install -y ntp
systemctl enable ntpd
ntpdate -q 0.rhel.pool.ntp.org
systemctl restart ntpd
```

`Debian 11 / Ubuntu 22`

```bash
apt-get install -y ntp
systemctl enable ntp
systemctl restart ntp
```


- 修改为香港时间

```bash
sudo timedatectl set-timezone Asia/Hong_Kong
```
- 修改为24小时制

```bash
localectl set-locale LC_TIME=en_GB.UTF-8
```
 
- 修改后记得重启 `reboot`