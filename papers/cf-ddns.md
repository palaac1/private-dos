## 自建CloudFlare-DDNS配置教程

Shell 脚本
> 获取脚本
得到 API 后，在 VPS 中下载脚本到/usr/local/bin 目录，把脚本命名为 cf-ddns.sh，并修改脚本的权限：
```bash
curl https://gist.githubusercontent.com/benkulbertis/fff10759c2391b6618dd/raw > /usr/local/bin/cf-ddns.sh && chmod +x /usr/local/bin/cf-ddns.sh
```
一般系统都会带有 curl，但如果出错，就需要先安装 curl，具体安装方法可以谷歌或百度一下。

> 配置
打开脚本进行配置：
```bash
 vi /usr/local/bin/cf-ddns.sh
```
找到以下内容并修改：
```
auth_email="user@example.com" # CF邮箱

auth_key="c2547eb745079dac9320b638f5e225cf483cc5cfdda41" # 在 cloudflare 帐户设置中找到全局key

zone_name="example.com" # @域名

record_name="www.example.com" # DDNS 的域名
```

其中，在 `auth_email` 中填入 `CloudFlare` 账号的邮箱，在 `auth_key` 输入前面获取的 `API`，`zone_name` 填入域名 `123.net`，`record_name` 填入 DDNS 的域名 `ddns.123.net`。

修改完后，保存退出。

 输入
```bash
 bash /usr/local/bin/cf-ddns.sh
```
 运行脚本，如果提示`IP changed to: X.X.X.X`，表明配置成功。
> crontab 定时运行
脚本配置成功后，需要让它定时运行。这里设置每 10 分钟运行一次 cf-ddns.sh 脚本。

 输入 `crontab -e`，然后会弹出 `vim` 编辑界面，在里面添加一行：
```bash
*/10 * * * * /usr/local/bin/cf-ddns.sh >/dev/null 2>&1
```

保存并退出。输入`service crond status`，可以看到 `contab` 的运行状态。
