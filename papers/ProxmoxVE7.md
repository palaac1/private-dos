> Debian 11 Bullseye 上安装 Proxmox VE 7

- 步骤
1. 先安装debian 11系统<br>
2. 必须添加 /etc/hosts 条目<br>
3. 设置Debian 11国内源<br>
4. 添加 Proxmox VE 存储库<br>
5. 添加 Proxmox VE 存储库密钥<br>
6. 安装 Proxmox VE 包<br>
7. 创建一个网桥<br>

- 添加 `/etc/hosts` 条目

```bash
vim /etc/hosts
```

参考<br>

```
127.0.0.1       localhost

172.168.0.3     pve.qu1u1.cn     //你的ip和域名
 
# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

- 使用hostname命令测试您的设置是否正常

```
hostname --ip-address
172.168.0.3 # should return your IP address here
```

设置Debian 11国内源

```
vim /etc/apt/sources.list    //修改国内源
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
```

- 添加 Proxmox VE 存储库<br>
中科大源：https://mirrors.ustc.edu.cn/proxmox/debian<br>
清华源：https://mirrors.tuna.tsinghua.edu.cn/proxmox/debian<br>

```bash
echo "deb [arch=amd64] https://mirrors.ustc.edu.cn/proxmox/debian bullseye pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list
```

- 备用

```bash
cd /etc/apt/sources.list.d/ && wget -O 'pve-install-repo.list' 'https://pan.geekrabbit.top/?explorer/share/fileOut&shareID=7tywdr6g&path=%7BshareItemLink%3A7tywdr6g%7D%2Flinux%E8%BD%AF%E4%BB%B6%E5%8C%85%2Fpve7%E5%AD%98%E5%82%A8%E5%BA%93%2Fpve-install-repo.list'
```

- 添加 Proxmox VE 存储库密钥

```bash
wget https://enterprise.proxmox.com/debian/proxmox-release-bullseye.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg
```

`//文件下载到/etc/apt/trusted.gpg.d/`

- 备用

```bash
cd /etc/apt/trusted.gpg.d/ && wget -O 'proxmox-release-bullseye.gpg' 'https://pan.geekrabbit.top/?explorer/share/fileOut&shareID=7tywdr6g&path=%7BshareItemLink%3A7tywdr6g%7D%2Flinux%E8%BD%AF%E4%BB%B6%E5%8C%85%2Fpve7%E5%AD%98%E5%82%A8%E5%BA%93%2Fproxmox-release-bullseye.gpg'
```

```
sha512sum /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg   //校验
7fb03ec8a1675723d2853b84aa4fdb49a46a3bb72b9951361488bfd19b29aab0a789a4f8c7406e71a69aabbc727c936d3549731c4659ffa1a08f44db8fdcebfa  //校验码
```

- 更新存储库和系统

```bash
apt update && apt full-upgrade
```


安装 Proxmox VE 包

```bash
apt install proxmox-ve postfix open-iscsi
```

最后，重新启动系统。

推荐：删除 `os-prober` 包

```bash
apt remove os-prober
```

> 创建一个网桥

- 登录后，创建一个名为vmbr0的Linux Bridge，并将您的第一个网络接口添加到它。

![写入网卡信息](images/image_1647529185133.png ':size=500')