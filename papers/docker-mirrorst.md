> Docker 拉取镜像速度慢怎么破？

Docker Hub 是我们分发和获取 Docker 镜像的中心，但由于服务器位于海外，经常会出现拉取/上传镜像时速度太慢或无法访问的情况。再加上运营方不断对 Docker Hub 的免费使用进行限制，导致我们在国内使用时总是磕磕绊绊。

如果你在使用 Docker 时也碰到了拉取镜像速度慢或拉取失败的情况，可以尝试改用国内的 Docker Hub 镜像服务器。

> 以下是截止本文发布时仍在向公众免费提供 Docker Hub 镜像的平台：

> - 网易云 https://hub-mirror.c.163.com
> - 百度云 https://mirror.baidubce.com
> - DaoCloud http://f1361db2.m.daocloud.io
> - 阿里云 https://ustc-edu-cn.mirror.aliyuncs.com
> - Github https://ghcr.io

> 1.修改 Docker 镜像服务器的方法
编辑 /etc/docker/daemon.json 配置文件

> 创建配置文件目录
```bash
sudo mkdir /etc/docker
```
> 编辑配置文件，如果文件不存在，以下命令会自动创建。
```bash
sudo nano /etc/docker/daemon.json
```

将配置信息粘贴到配置文件中，配置信息为 json 格式，可以根据实际需要设置多个国内的镜像服务器。
```bash
{
  "registry-mirrors": [
    "https://hub-mirror.c.163.com",
    "https://mirror.baidubce.com"
  ]
}
```
> 2. 重启 Docker 服务
```bash
sudo systemctl daemon-reload 
sudo systemctl restart docker
```
> 3. 检查设置是否生效
```bash
sudo docker info
```
结果中显示了我们设置的镜像服务器地址，则说明设置已经生效，返回的信息类似下面这样：

- `Registry Mirrors:`
- `https://hub-mirror.c.163.com/`