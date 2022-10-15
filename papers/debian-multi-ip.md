> Debian单网卡绑定多IP地址

debian系统下只要简单的修改下/etc/network/interfaces文件就可以了,比如在eth0接口上绑定更多的IP地址,只需在eth0下面添加如下行:
```
auto eth0:0
iface eth0:0 inet static
address 192.168.1.3
netmask 255.255.255.0
auto eth0:1
iface eth0:1 inet static
address 192.168.1.4
netmask 255.255.255.0
```
这样就添加了另外两个IP,192.168.1.3和192.168.1.4。注意，新增ip不要写gateway（网关），如果还要继续添加ip，同理再在下面添加eth0:1、eth0:2….

保存文件，并执行如下命令刷新网络

`# /etc/init.d/networking restart`

或者：
```bash
sudo /etc/init.d/networking restart
```