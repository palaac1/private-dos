> Linux dd常用系统

安装依赖&必要依赖

- Debian/Ubuntu:

```bash
apt-get update && apt-get -y install vim dnsutils curl sudo wget iperf3 git screen unzip
```

- RedHat/CentOS:

```bash
yum install -y xz openssl gawk file vim curl sudo wget screen
```

----
- 甲骨文ARM dd系统

```bash
bash <(wget --no-check-certificate -qO- 'https://raw.githubusercontent.com/MoeClub/Note/master/InstallNET.sh') -d 11 -v 64 -p 密码
```

----
- 国外机DD

```bash
wget --no-check-certificate -O AutoReinstall.sh https://git.io/AutoReinstall.sh && bash AutoReinstall.sh
```

- `Pwd@CentOS`
- `Pwd@Linux`

----
- 国内机DD - 163源

```bash
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -d 11 -v 64 -a -p 密码 --mirror 'http://mirrors.163.com/debian/'
```

----
- 非DHCP网卡,手动添加参数

```bash
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -d 11 -v 64 -a -p 密码 --ip-addr X.X.X.X --ip-mask X.X.X.X --ip-gate X.X.X.X --mirror 'http://mirrors.163.com/debian/'
```

- 将`X.X.X.X`替换为自己的网络参数.
- `--ip-addr :IP Address/内网IP地址`
- `--ip-mask :Netmask   /子网掩码`
- `--ip-gate :Gateway   /网关`

----
- 开机设置root密码，ssh上去除`#!/bin/bash`可更为root登录

```bash
#!/bin/bash
echo root:密码 |sudo chpasswd root
sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config;
sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config;
sudo reboot
```

