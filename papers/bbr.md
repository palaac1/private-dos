> 网络优化脚本-1
- neko脚本

- Linux网络优化: http://sh.nekoneko.cloud/tools.sh

```bash
wget http://sh.nekoneko.cloud/tools.sh -O tools.sh && bash tools.sh
```


- BBR一键安装: http://sh.nekoneko.cloud/bbr/bbr.sh

- DD一键脚本: http://sh.nekoneko.cloud/DD/AutoReinstall.sh

- Gost一键脚本: http://sh.nekoneko.cloud/EasyGost/gost.sh

- Ehco一键脚本: http://sh.nekoneko.cloud/ehco.sh/ehco.sh

- 一键Brook转发: http://sh.nekoneko.cloud/brook-pf/brook-pf.sh

- 自定义隧道后端一键安装: http://sh.nekoneko.cloud/land.sh

----
----

> 网络优化脚本-2

- ylx.me脚本

- 预先准备

centos：
```bash
yum install ca-certificates wget -y && update-ca-trust force-enable
```

```bash
yum install -y vim dnsutils curl sudo wget iperf3 git screen unzip
```

debian/ubuntu：
```bash
apt-get install ca-certificates wget -y && update-ca-certificates
```

```bash
apt-get update && apt-get -y install vim dnsutils curl sudo wget iperf3 git screen unzip
```

- 不卸载内核版本

```bash
wget -O tcpx.sh "https://github.com/ylx2016/Linux-NetSpeed/raw/master/tcpx.sh" && chmod +x tcpx.sh && ./tcpx.sh
```

```bash
wget -N --no-check-certificate "https://github.000060000.xyz/tcpx.sh" && chmod +x tcpx.sh && ./tcpx.sh
```

- 卸载内核版本

```bash
wget -O tcp.sh "https://github.com/ylx2016/Linux-NetSpeed/raw/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh 
```

```bash
wget -N --no-check-certificate "https://github.000060000.xyz/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```

提示：目前脚本对CN地址作了特殊处理，如果非CN地址被MAXMIND识别为CN，那可能造成处理的链接返回503无法通过链接检测