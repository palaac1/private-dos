> Linux系统设置全局代理（http代理，socks代理）

- 临时

```bash
export http_proxy=http://ip:port
export https_proxy=http://ip:port
```

- 永久

```bash
vim /etc/profile
```
- 写入

```bash
export http_proxy=http://ip:port
export https_proxy=http://ip:port
```

- 激活

```bash
source /etc/profile
```

- 而对于要取消设置可以使用如下命令，其实也就是取消环境变量的设置：

```bash
unset http_proxy
unset https_proxy
unset ftp_proxy
unset no_proxy
```