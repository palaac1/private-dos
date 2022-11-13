- 获得奇迹塑造者
建议克隆 Wondershaper 的 GitHub 存储库，以便您可以随时提取新的更新（如果可用）。打开一个新终端并使用克隆存储库

``` bash
git clone https://github.com/magnific0/wondershaper.git
```

- 这将在您当前文件夹中的一个名为 wondershaper 的新文件夹中克隆 Wondershaper。现在使用

``` bash
cd wondershaper
```

- 使用奇迹塑造者

- 您可以在不安装的情况下运行 wondershaper（作为具有足够权限的任何用户），此时停止按照说明操作。通过键入显示 wondershaper 使用说明

``` bash
./wondershaper -h
```

- 该程序详细介绍了有关如何使用 Wondershaper 的所有可用选项。接下来是选择一个你想要塑造的界面。您可以通过键入查看所有可用的接口

``` bash
ip addr show
```

- 请注意，在旧系统上，此命令可能不可用。在这种情况下，您应该运行ifconfig。

- 确定要塑造的网络接口。名称因系统而异。

- 在以下示例中，无线接口被限制为4Mbps 的上传和 8Mbps 的下载。

``` bash
./wondershaper -a wlp4s0 -u 4096 -d 8192
``` 

- 如果您收到消息，告诉您RTNETLINK answers: Operation not permitted您的用户帐户没有足够的权限。在这种情况下尝试：

``` bash
sudo ./wondershaper -a wlp4s0 -u 4096 -d 8192
```

- 系统安装（可选）
- 为便于安装而提供的 makefile 文件。Wondershaper 的默认位置在/usr/bin. 如果要安装到系统中，可以运行：

``` bash
sudo make install
```

- 您可以通过输入以下内容来验证是否正确安装了 Wondershaper：

``` bash
which wondershaper
```

- 这应该返回`/usr/bin/wondershaper`。您可以按照“使用 Wondershaper”部分中解释的相同说明进行操作，但现在您可以通过删除./每个命令开头的 来运行系统版本，而不是运行程序的本地版本。例如要再次显示帮助说明，请运行：

``` bash
wondershaper -h
```

- 然而我们使用编译安装的方式安装了之后，是不会在sbin下建立软链的，所以为了方便后续，我们先为它建个软链。

``` bash
ln -s /usr/local/sbin/wondershaper /usr/sbin/wondershaper
```

- 持续使用 Wondershaper（可选）<br>
- 不必像前面显示的那样使用命令行选项来设置速率和界面，<br>
- 而是需要在wondershaper.conf配置文件中设置这些参数。您可以使用您喜欢的文本编辑器（下面示例中的 vim）编辑此文件，如下所示：<br>

``` bash
sudo vim /etc/systemd/wondershaper.conf
```

内容

```
[wondershaper]

# Adapter
IFACE="网卡名字"

# Download rate in Kbps #下载限速
DSPEED="2024000"

# Upload rate in Kbps #上传限速
USPEED="520192"

### Separate items by whitespace:

#HIPRIODST=(IP1 IP2)
HIPRIODST=()

COMMONOPTIONS=()

# low priority OUTGOING traffic - you can leave this blank if you want
# low priority source netmasks
NOPRIOHOSTSRC=(80);

# low priority destination netmasks
NOPRIOHOSTDST=();

# low priority source ports
NOPRIOPORTSRC=();

# low priority destination ports
NOPRIOPORTDST=();

### EOF
```


- 然后执行以下命令，可以让 wondershaper 在每次系统启动时都自动开始服务：

``` bash
开机运行
sudo systemctl enable wondershaper.service
启动
sudo systemctl start wondershaper.service
重启
sudo systemctl restart wondershaper.service

```

- 用法
    wondershaper [-hcs] [-a <adapter>] [-d <rate>] [-u <rate>]
允许使用以下命令行选项：

-h显示帮助

-a <adapter>设置适配器

-d <rate>设置最大下载速率（Kbps）和/或

-u <rate>设置最大上传速率（Kbps）

-p使用 /etc/systemd/wondershaper.conf 中的预设

-f <file>使用替代预设文件

-c清除适配器的限制

-s显示适配器的当前状态

不同的模式是：

    wondershaper -a <adapter> -d <rate> -u <rate>

    wondershaper -c -a <adapter>

    wondershaper -s -a <adapter>
一些例子：

    wondershaper -a eth0 -d 1024 -u 512

    wondershaper -a eth1 -d 94000 -u 94000  # could be used on a 100Mbps link

    wondershaper -a eth1 -u 94000  # only limit upload

    wondershaper -c -a eth0

    wondershaper -p -f foo.conf
	