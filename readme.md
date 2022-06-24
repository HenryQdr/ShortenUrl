现在很多平台分享出来的链接都使用了一种短链接（Short URL）的技术，例如新浪的 http://t.cn，Telegram 的 http://t.me，Twitter 的 http://t.co。

这些链接的后缀往往非常简短，只有几个随机的字符或者数字。可以设置为自增长，也可以通过 Hash 算法生成，只要唯一即可。然后在服务器的数据库中，通过唯一的随机码，找到对应的网址进行重定向。

因此，如果我们需要搭建自己的短链接服务，通常都需要有一台独立的服务器或者数据库。不过在 GitHub 上有人提供了一种思路，可以使用 Github Pages 来免费构建自己的短链接服务。

Step 1

首先在 GitHub 上新建一个仓库，当做数据库用来存储链接，笔者这里命名为 gh-pages-url-shortener-db。


Step 2

Fork 这个 link 仓库，打开 404.html，修改文件里的 GITHUB_ISSUES_LINK 字段，将这个 value 指向自己上一步新建的 gh-pages-url-shortener-db 仓库。

// 注意将{username}和{dbname}替换为自己的用户名和仓库
var GITHUB_ISSUES_LINK = "https://api.github.com/repos/{username}/{dbname}/issues/";
Step 3

最后在 Settings->GitHub Pages->Source 设置里配置一下 GitHub Pages 分支即可。


现在来测试一下！

在第一步建立的 gh-pages-url-shortener-db 仓库里开一个 issues，标题就是需要转换的长链接。

例如我这里使用一个带有中文转译后的链接，这个链接会定位到百度百科的科比·布莱恩特词条。


现在，通过在浏览器中输入网址 https://mayandev.top/link/1，就可以跳转到上面的百度百科的词条了。

动图封面
为了避免邮件打扰，建议关闭此仓库的通知功能。


觉得手动创建 issues 太麻烦？

GitHub 提供命令行的 CLI 工具，可以让我们通过终端来操作 issues。

你也可以通过我编写的命令行工具进行短链转换。


只需要进行以下的一些设置，即可完成短链接转换的操作。

# 安装工具
npm install gh-short-url -g
# 设置用户名以及域名
shorten config --database=${repo-name} --user=${username} --pages=${domain}
# shorten it！
shorten https://en.wikipedia.org/wiki/Kobe_Bryant#Basketball_legacy/
我将工具开源在了 GitHub 中的 gh-short-url 仓库中，欢迎 star ！感谢原 Repo 作者提供的灵感。

How does this Work？

为什么可以通过 GitHub Pages 实现短链？原 Repo 的作者提到：

404.html handles all requests
Small javascript snippet fetches a JSON representation of the GitHub issue via the JSON API, and redirects to the issue title, as a URL.
Profit?
真正的秘密藏在了 404.html 中，感兴趣的读者可以自行阅读。

完
