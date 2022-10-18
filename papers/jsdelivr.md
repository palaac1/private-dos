> jsdelivr 加速 GitHub 仓库指定文件<br>

- 各个加速域名<br>
`testingcf.jsdelivr.net`<br>
`gcore.jsdelivr.net`<br>
`fastly.jsdelivr.net`<br>
`originfastly.jsdelivr.net`<br>
`quantil.jsdelivr.net`<br>


- 使用方法：[https://cdn.jsdelivr.net/gh/你的用户名/你的仓库名@发布的版本号/文件路径](/papers/jsdelivr)<br>
- 例子：<br>
- 读取单个文件<br>
```https
https://cdn.jsdelivr.net/gh/palaac1/private-dos@master/papers/LinuxMirrors.md
```
- 读取目录 ,`master`可以改为其他版本<br>
```https
https://cdn.jsdelivr.net/gh/palaac1/private-dos@master/
```
----
----
- 加载任何 GitHub 版本、提交或分支<br>
- 注意：我们建议对支持它的项目使用 npm

https://cdn.jsdelivr.net/gh/user/repo@version/file

- 加载 jQuery v3.2.1

https://cdn.jsdelivr.net/gh/jquery/jquery@3.2.1/dist/jquery.min.js

- 使用版本范围而不是特定版本

https://cdn.jsdelivr.net/gh/jquery/jquery@3.2/dist/jquery.min.js<br>
https://cdn.jsdelivr.net/gh/jquery/jquery@3/dist/jquery.min.js

- 完全省略版本以获取最新版本<br>
- 你不应该在生产中使用它

https://cdn.jsdelivr.net/gh/jquery/jquery/dist/jquery.min.js

- 将“.min”添加到任何 JS/CSS 文件以获得缩小版本<br>
- 如果一个不存在，我们会为你生成

https://cdn.jsdelivr.net/gh/jquery/jquery@3.2.1/src/core.min.js

- 在末尾添加 / 以获取目录列表

https://cdn.jsdelivr.net/gh/jquery/jquery/