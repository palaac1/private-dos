> Docker 安装
- 适用于debian 10和debian 11

```bash
apt-get update && apt-get -y install vim dnsutils curl sudo wget iperf3 git screen unzip
```

```bash
docker version > /dev/null || curl -fsSL get.docker.com | bash
```

----
----

> Docker Compose 安装

Linux 上我们可以从 Github 上下载它的二进制包来使用，最新发行的版本地址：https://github.com/docker/compose/releases

运行以下命令以下载 Docker Compose 的当前稳定版本：

```bahs
sudo curl -L "https://github.com/docker/compose/releases/download/v2.11.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

要安装其他版本的 `Compose`，请替换 `v2.11.2`。


Docker Compose 存放在 GitHub，不太稳定。

你可以也通过执行下面的命令，高速安装 Docker Compose。

```bash
curl -L https://get.daocloud.io/docker/compose/releases/download/v2.11.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

将可执行权限应用于二进制文件：

```bash
sudo chmod +x /usr/local/bin/docker-compose
```
- 创建软链：

```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

- 测试是否安装成功：

```bash
docker-compose version
```
提示：则成功
`cker-compose version 1.24.1, build 4667896b`

