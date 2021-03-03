<!-- ---
title: 使用hexo+github搭建免费个人博客
date: 2019-03-28 23:03:08
categories:
- 随笔
- 博客
tags: 
- 博客
--- -->

​	玩博客有三种方式，一是在平台写，但是限制多，改个样式。二是自己购买域名和`vps`，自己搭建后台，使用`tychpeo/wordpress`之类的框架搭建博客系统，创建一个完完整整属于自己的博客，但是过不了多会“疲”了，因为维护起来实在太麻烦了，比如换个`ip`，你就要迁移整个系统，还有数据备份，实践操作过程中，麻烦程度可能远超想象，再者`vps`也是要钱的。

​	最后，就发展到第三个阶段，基于一些免费云空间来搭建静态网页，既可免费，迁移也方便，整个系统都可以进行git托管，通过框架工具，使用Markdown自由写作，一条指令完成页面生成与部署，这才是真正便捷实用的方式。

<!--more-->

# 前言

使用`github pages`服务搭建博客的好处有：

```
全是静态文件，访问速度快；
免费方便，不用花一分钱就可以搭建一个自由的个人博客，不需要服务器不需要后台；
可以随意绑定自己的域名，不仔细看的话根本看不出来你的网站是基于github的；
数据绝对安全，基于github的版本管理，想恢复到哪个历史版本都行；
博客内容可以轻松打包、转移、发布到其它平台；
```

## 准备工作

在开始之前准备：

- `github`账号；
- 安装了`node.js`、`npm`，并了解相关基础知识；
- 安装了`git for windows`（或者其它git客户端）

## 什么是Node.js

​	`Node.js`是2009年 Ryan Dahl 开发的一个JavaScript运行环境，实际上是对`Chrome V8`引擎进行了封装，`Node.js`的出现，使得JavaScript可以运行在服务端平台，可以使用JavaScript来开发后端。

## 什么是NPM

`NPM`是随同 `Node.js` 一起安装的 `包管理工具` ，能解决 `Node.js` 代码部署上的问题，常见的使用场景有以下几种：

- 允许用户从`NPM服务器`下载别人编写的第三方包到本地使用。
- 允许用户从`NPM服务器`下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到`NPM服务器`供别人使用。

可以简单的理解 `NPM `为后端的 `maven `(可以先这么理解)

## http-server NPM模块

`http-server`是个开源的`npm模块`，官方`github`仓库：https://github.com/indexzero/http-server

1.环境安装

​	到[node.js中文官网](http://nodejs.cn/download/)下载对应系统版本的安装包，安装完成后,打开电脑的`CMD命令窗口`（windows下，WIN键+R后输入`CMD`，然后回车），输入 `node -v` 来查看`node.js`安装的版本，输入 `npm -v` 查看`npm`的版本：

![](https://i.loli.net/2019/03/09/5c83c4cc32b31.jpg)

2.安装 `http-server` 为全局模块

有了`node.js`环境后，我们可以使用 `npm install [模块名称]` 来安装想安装的模块。

```cmd
npm install http-server -g  
```

`cmd命令`窗口执行上边命令，其中 `-g` 表示全局安装的意思。

安装完成后，输入 `http-server -v` 来检测是否安装成功。

安装下载较慢同学可以切换淘宝镜像后重新执行安装命令：

```cmd
npm config set registry http://registry.npm.taobao.org/
npm install http-server -g
```

![](https://i.loli.net/2019/03/09/5c83c7bedc0b6.jpg)



## 搭建github博客

### 创建仓库

​	新建一个名为`你的用户名.github.io`的仓库，比如说，我的`github`用户名是`huangtiancai`，那么你就新建`huangtiancian.github.io`的仓库（必须是你的用户名，其它名称无效），将来你的网站访问地址就是 http://huangtiancai.github.io 了

​	由此可见，每一个`github`账户最多只能创建一个这样可以直接使用域名访问的仓库。

​	创建成功后，默认会在你这个仓库里生成一些示例页面，以后你的网站所有代码都是放在这个仓库里啦。

### 绑定域名

​	当然，你不绑定域名肯定也是可以的，就用默认的 `xxx.github.io` 来访问，如果你想更个性一点，想拥有一个属于自己的域名，那也是OK的

注意：

- `IP`和域名绑定有两种情况：带`www`和不带`www`的
- 域名配置最常见有2种方式，`CNAME`和`A记录`，`CNAME`填写`域名`，`A记录`填写`IP`

由于不带`www`方式只能采用A记录，所以必须先ping一下`你的用户名.github.io`的`IP`，然后到你的域名`DNS`设置页，将A记录指向你ping出来的`IP`，将`CNAME`指向`你的用户名.github.io`，这样可以保证无论是否添加`www`都可以访问

***A记录（`IP`映射）***

  ```
  ip:149.129.77.73
  域名：huangtiancai.xyz
  格式：
  	记录类型：A-将域名指向一个IPV4地址
  	主机记录：www.huangtiancai.xyz
  	记录值：149.129.77.73
  	
  	记录类型：A-将域名指向一个IPV4地址
  	主机记录：@.huangtiancai.xyz
  	记录值：149.129.77.73
  	
  注意：
      主机记录就是域名前缀，常见用法有：
      www：解析后的域名为www.aliyun.com。
      @：直接解析主域名 aliyun.com。
      *：泛解析，匹配其他所有域名 *.aliyun.com。
      mail：将域名解析为mail.aliyun.com，通常用于解析邮箱服务器。
      二级域名：如：abc.aliyun.com，填写abc。
      手机网站：如：m.aliyun.com，填写m。
      显性URL：不支持泛解析（泛解析：将所有子域名解析到同一地址）
  ```

  ![A记录解析配置](https://i.loli.net/2019/03/19/5c8fccbcd11a6.jpg)

解析成功后如图：

![](https://i.loli.net/2019/03/19/5c8fce63c0e3b.jpg)

***`CNAME`***

```
ip:185.199.110.153
域名：huangtiancai.cn
格式：
	记录类型：CNAME-将域名指向另外一个域名
	主机记录：blog.huangtiancai.cn
	记录值：huangtiancai.github.io（这是github生成的一个二级域名）
```

![](https://i.loli.net/2019/03/19/5c8fcf4157e7e.jpg)



解析成功后如图：

![](https://i.loli.net/2019/03/19/5c8fd0f2eceb3.jpg)



然后到你的`github`项目根目录新建一个名为`CNAME`的文件（无后缀），里面填写你的域名

注意：

>- 如果你填写的是没有`www`的，比如` huangtiancai.xyz`，那么无论是访问 [http://www.huangtiancai.xyz](http://www.huangtiancai.xyz) 还是 [http://huangtiancai.xyz](http://huangtiancai.xyz) ，都会自动跳转到 [http://huangtiancai.xyz](http://huangtiancai.xyz)
>- 如果你填写的是带`www`的，比如 `www.huangtiancai.xyz` ，那么无论是访问 [http://www.huangtiancai.xyz](http://www.huangtiancai.xyz/) 还是 [http://huangtiancai.xyz](http://huangtiancai.xyz) ，都会自动跳转到 [http://www.huangtiancai.xyz](http://www.huangtiancai.xyz)
>- 在绑定了新域名之后，原来的`你的用户名.github.io`并没有失效，而是会自动跳转到你的新域名。



## 配置SSH key

>SSH 为 **Secure Shell** 的缩写，利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。简单说，SSH是一种**网络协议**，用于计算机之间的**加密登录**。
>
>SSH之所以能够保证安全，原因在于它采用了**公钥加密**。
>
>**过程：**
>
>（1）远程主机收到用户的登录请求，把自己的公钥发给用户。
>
>（2）用户使用这个公钥，将登录密码加密后，发送回来。
>
>（3）远程主机用自己的私钥，解密登录密码，如果密码正确，就同意用户登录。

因为你提交代码肯定要拥有你的`github`权限才可以，但是直接使用用户名和密码太不安全了，所以我们使用ssh key来解决本地和服务器的连接问题。

用git bash执行如下命令：

```
$ cd ~/. ssh #检查本机已存在的ssh密钥
```

如果提示：No such file or directory 说明你是第一次使用git。

```
ssh-keygen -t rsa -C "邮件地址"
```

然后连续3次回车，最终会生成一个文件在用户目录下，打开用户目录，找到`.ssh\id_rsa.pub`文件，记事本打开并复制里面的内容，打开你的`github`主页，进入个人设置` -> SSH and GPG keys -> New SSH key`：

将刚复制的内容粘贴到key那里，title随便填，保存。

![](https://i.loli.net/2019/03/29/5c9d87c849039.jpg)

## 测试是否成功

```
$ ssh -T git@github.com # 注意邮箱地址不用改
```

如果提示`Are you sure you want to continue connecting (yes/no)?`，输入yes，然后会看到：

>`Hi huangtiancai! You’ve successfully authenticated, but GitHub does not provide shell access`.

看到这个信息说明SSH已配置成功！

此时你还需要配置：

````
$ git config --global user.name "huangtiancai"// 你的github用户名，非昵称
$ git config --global user.email  "xxx@qq.com"// 填写你的github注册邮箱
````



# 使用hexo写博客

## hexo简介

`Hexo`是一个简单、快速、强大的基于 `Github Pages` 的博客发布工具，支持Markdown格式，有众多优秀插件和主题,由中国台湾[`tommy351`](https://link.jianshu.com/?t=http://twitter.com/tommy351)开发.

官网： [http://hexo.io](http://hexo.io/)
`github`: <https://github.com/hexojs/hexo>

## 原理

​	由于`github pages`存放的都是静态文件，博客存放的不只是文章内容，还有文章列表、分类、标签、翻页等动态内容，假如每次写完一篇文章都要手动更新博文目录和相关链接信息，相信谁都会疯掉，所以`hexo`所做的就是将这些`md`文件都放在本地，每次写完文章后调用写好的命令来批量完成相关页面的生成，然后再将有改动的页面提交到`github`。

## 注意事项

>1. 很多命令既可以用Windows的cmd来完成，也可以使用git bash来完成，但是部分命令会有一些问题，为避免不必要的问题，建议全部使用git bash来执行；
>2. `hexo`不同版本差别比较大，网上很多文章的配置信息都是基于2.x的，所以注意不要被误导；
>3. `hexo`有2种`_config.yml`文件，一个是根目录下的全局的`_config.yml`，一个是各个`theme`下的；

## 安装

```
$ npm install -g hexo   //-g 是全局安装
```

## 初始化

在电脑的某个地方新建一个文件夹（名字可以随便取），比如我的是`c:\myblog`，由于这个文件夹将来就作为你存放代码的地方，所以最好不要随便放。

```
$ cd /c/myblog/
$ hexo init
```

`hexo`会自动下载一些文件到这个目录，包括node_modules

````
$ hexo g # 生成
$ hexo s # 启动服务
````

执行以上命令之后，`hexo`就会在public文件夹生成相关`html`文件，这些文件将来都是要提交到`github`去的：

`hexo s`是开启本地预览服务，打开浏览器访问 [http://localhost:4000](http://localhost:4000/) 即可看到内容，很多人会碰到浏览器一直在转圈但是就是加载不出来的问题，一般情况下是因为端口占用的缘故

第一次初始化的时候`hexo`已经帮我们写了一篇名为 Hello World 的文章，默认的主题比较丑，打开时就是这个样子：

![](https://i.loli.net/2019/03/29/5c9d8c9e87367.png)

## 修改主题

替换一个好看点的主题。这是：[官方主题](https://hexo.io/themes/)

我选的主题：[Cafe 主题](https://github.com/giscafer/hexo-theme-cafe/)

 （1）安装

```
$ git clone https://github.com/giscafer/hexo-theme-cafe.git themes/cafe
```

或者直接到[releases](https://github.com/giscafer/hexo-theme-cafe/releases)下载最新源码文件，重命名为`cafe`放到博客themes目录下

（2）使用主题

修改博客配置文件 `_config.yml` 主题属性 theme 为 `cafe`.然后重新执行`hexo g`来重新生成。

（3）更新升级

```
cd themes/cafe
git pull
```

（4）主题配置

主题 `themes/cafe/_config.yml` 文件内容参考说明配置

配置教程具体见[_config.yml文件](https://github.com/giscafer/hexo-theme-cafe/blob/master/_config.yml)注释说明

`Cafe` 主题提供以下内置 widgets(Widgets：小部件，小插件）

- social # 社交账号链接
- category # 归类
- tag # 标签
- tagcloud # 云标签
- archives # 归档
- recent_posts # 最新文章
- friendly_link # 友情链接

## 上传到github

如果你一切都配置好了，发布上传很容易，一句`hexo d`就搞定，当然关键还是你要把所有东西配置好。

首先，`ssh key`肯定要配置好。

其次，配置`_config.yml`中有关deploy的部分：

```
deploy:
  type: git
  repository: git@github.com:huangtiancai/huangtiancai.github.io.git
  branch: master
```

还需要安装一个插件：

````
npm install hexo-deployer-git --save
````

打开你的git bash，输入`hexo d`就会将本次有改动的代码全部提交，没有改动的不会：

## 保留CNAME、README.md等文件

由于`hexo`默认会把所有`md`文件都转换成`html`，包括`README.md``，所有需要每次生成之后、上传之前，手动将README.md`复制到public目录，并删除`README.html`。

## 常用hexo命令

````
hexo new "postName" #新建文章 
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
````

缩写：

```
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

组合命令：

```markup
hexo s -g #生成并本地预览
hexo d -g #生成并上传
```

正常情况用的最多的就两个命令：

```
hexo new "postName" #新建文章 会自动生成创建时间当然你也可以直接自己新建md文件，用这个命令的好处是帮我们自动生成了时间。
hexo d -g #生成并上传
```



一般完整格式如下:

```markdown
---
title: postName #文章页面上的显示名称，一般是中文
date: 2013-12-02 15:30:16 #文章生成时间，一般不改，当然也可以任意修改
categories: 默认分类 #分类
tags: [tag1,tag2,tag3] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面
---

以下是正文
```

## 写博客工具

1.推荐使用：[typora](https://www.typora.io/)

可以导出`PDF`、`Word(.docx)`、`HTML`等格式，作为日常的文本编辑器也什么方便！

![](https://i.loli.net/2019/03/29/5c9d9d75b002e.png)

2.notepad++

​	notepad（记事本）是代码编辑器或WINDOWS中的小程序，用于文本编辑，在文字编辑方面与Windows写字板功能相当。是一款开源、小巧、免费的纯文本编辑器。

​	支持的插件非常多！非常强大,当然是支持Markdown语法插件

访问官网下载：[notepad++ Home](https://notepad-plus-plus.org/)

![](https://i.loli.net/2019/03/29/5c9db7d98f22d.gif)

3.vscode

