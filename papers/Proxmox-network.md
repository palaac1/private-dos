# Proxmox VE 7添加虚拟机和网卡
- 创建一个网桥

登录8006面板地址后进入网络删除eno1的网卡信息<br>
并创建一个名为vmbr0的Linux Bridge，并将您的第一个网络接口添加到它。<br>
- 例子：<br>
![写入网卡信息](images/image_1647529185133.png ':size=500')


- 创建多个网卡 ，类推`vmbr1` `vmbr2`，写入IP，不用写网关

![写入多个网卡](images/image_1647529185134.png ':size=500')


- ISO文件目录
下载的ISO文件放入服务器/var/lib/vz/template/iso目录

```bash
cd /var/lib/vz/template/iso
```

例子：下载debian11

```bash
wget -N --no-check-certificate https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-11.5.0-amd64-netinst.iso
```

> 创建独立IP的虚拟机

- `常规`<br>

![常规](images/image_1647529185135.png ':size=500') <br>

- `操作系统`<br>

![操作系统](images/image_1647529185136.png ':size=500') <br>

- `CPU类型`<br>

![CPU类型](images/image_1647529185137.png ':size=500') <br>

- `网络选择vmbr0,独立IP是直接桥接的`<br>

![网络选择vmbr0](images/image_1647529185138.png ':size=500') <br>

----

> 创建NAT网络的虚拟机

先创建vmbr1网卡只要填写IP/24

![创建vmbr1](images/image_1647529185139.png ':size=500') <br>

那么内网网关就是`10.0.0.254/24`，掩码 `/24` 即 `255.255.255.0`<br>


其他重复上述教程。网卡要选择vmbr1<br>

![网络选择vmbr1](images/image_1647529185140.png ':size=500') <br>

安装系统后设置IP。
```
IP：10.0.0.2   // D段2到253都可以  /24
网关：10.0.0.254
掩码：255.255.255.0
```

> NAT内网转发。<br>
在vmbr1网卡信息添加上转发 <br>

```bash
vim /etc/network/interfaces
``` 
```bash
	post-up echo 1 > /proc/sys/net/ipv4/ip_forward
	post-up iptables -t nat -A POSTROUTING -s '10.0.0.0/24' -o vmbr0 -j MASQUERADE	
	post-down iptables -t nat -D POSTROUTING -s '10.0.0.0/24' -o vmbr0 -j MASQUERADE	
	post-up iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport 2022 -j DNAT --to 10.0.0.102:22        #外网端口2022，内网22
	post-down iptables -t nat -D PREROUTING -i vmbr0 -p tcp --dport 2022 -j DNAT --to 10.0.0.102:22        #小鸡IP自己改
```
如图<br>

![interfaces](images/image_1647529185141.png ':size=500') <br>

说明：

`ip_forward` 一行表示开启 IPv4 转发，这个是内核参数，将 Linux 当作路由器用的参数。<br>
一般来说，一个路由器至少要有两个网络接口，一个 WAN，一个 LAN，为了让 LAN 和 WAN 流量相同，需要内核上的路由<br>
`post-up` 和 `post-down` 分别表示网卡启用和禁用之后，执行后面的命令<br>
`iptables` 行表示，开启防火墙转发，-A 表示添加规则，配置一条 NAT 规则，源地址为 10.0.0.0/24 的流量，转发到 vmbr0 接口。<br>
MASQUERADE 对 IP 地址数据包进行改写<br>
网卡关闭后 -D 删除这条规则<br>
最后 2 行是把虚拟机 10.0.0.102 上的 22 端口 NAT 到宿主机的 2022 端口，<br>
这样使得外部的网络可以通过 Proxmox VE 宿主机的 2022 端口访问虚拟机的 22 端口，<br>
可以直接使用上面的配置，或者手动执行下面的命令：<br>

  `iptables -t nat -A PREROUTING -i vmbr0 -p tcp --dport 2022 -j DNAT --to 10.0.0.102:22`
  
最后两行配置在网络配置中是为了让系统重启之后配置依然生效。否则 iptables 的转发就可能丢失。<br>

启用网桥：<br>

`sudo ifup vmbr1`

显示信息：<br>

`ip address show dev vmbr1`

查看 iptables 配置是否生效：<br>

`iptables -L -t nat`

结果：<br>
```
root@pve:~# ip address show dev vmbr1
9: vmbr1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN group default qlen 1000
    link/ether 62:32:cf:9d:5d:f9 brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.1/24 scope global vmbr1
       valid_lft forever preferred_lft forever
    inet6 fe80::6032:cfff:fe9d:5df9/64 scope link 
       valid_lft forever preferred_lft forever
```

- 重启网络：

```bash
sudo systemctl restart networking
systemctl status networking.service
```

通过上面的方式创建了 vmbr1 的网桥之后，再创建新的虚拟机就可以使用这个新的子网。因为这里没有配置 DHCP，所以虚拟机需要设定静态 IP 地址。<br>

在我的真实操作中，我发现无论我怎么重启 `systemctl restart networking` 都无法使得 Proxmox VE 的网络配置生效，最后不得不重启服务器才可以。<br>


- 方便管理端口转发的脚本<br>

```bash
wget -N --no-check-certificate "https://testingcf.jsdelivr.net/gh/ToyoDAdoubi/doubi/iptables-pf.sh" && chmod +x iptables-pf.sh && ./iptables-pf.sh
```

----
> 单网卡如何添加多个IP。

  * [Debian单网卡绑定多IP地址](/papers/debian-multi-ip.md)  
  
