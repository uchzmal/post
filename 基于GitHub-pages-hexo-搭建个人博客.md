---
title: 基于GitHub pages +hexo 搭建个人博客
date: 2023-08-14 17:11:34
tags: 教程
---
### 准备工作
 ~~一个[GitHub](https://github.com)账号~~  
 *一定的基础，包括但不限于稍微熟悉linux或windows cmd命令行*  
 *一些Bing/百度/Google的能力*
<!--more-->
#### 前言 *~~（废话）~~* 

##### GitHub Pages 是什么？

[GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages) 是由 GitHub 官方提供的一种免费的静态站点托管服务，让我们可以在 GitHub仓库里托管和发布自己的静态网站页面。

##### Hexo 是什么？

[Hexo](https://hexo.io/zh-cn/) 是一个快速、简洁且高效的静态博客框架，它基于 Node.js 运行，可以将我们撰写的 [Markdown 文档](https://markdown.com.cn/)解析渲染成静态的 HTML 网页。

### 开始

#### 注意事项

- 输入代码时，核对准确，最好切换成英文输入法；  
- 将文中的 “用户名” 和 “邮箱” 替换为自己的 GitHub 账户名和绑定的邮箱；  
- 统一使用 Git Bash 进行操作（支持 Win、Mac）~~cmd也可以但只有windows有~~；  
- 小白请严格按步骤进行，不要跳！

#### 环境搭建

Hexo 基于 [Node.js](https://nodejs.org/zh-cn)，~~*搭建过程中还需要使用 npm（Node.js 已带）*~~ 和 [Git](https://nodejs.org/zh-cn)，因此先搭建本地操作环境，安装 [Node.js](https://nodejs.org/zh-cn) 和 [Git](https://nodejs.org/zh-cn)。

##### 如何检查自己有没有安装环境
依次输入`node -v` ，`npm -v`，`git --version` *~~每一次都要回车~~*  
如果返回不为空或者报错就说明安装成功了  
 
![示例](检查安装.png)
#### 通过ssh连接GitHub

~~*任意目录空白处*~~  右键 -> Git Bash Here，设置用户名和邮箱：
```bash
git config --global user.name "你的GitHub 用户名"
git config --global user.email "你绑定的GitHub 邮箱"
```
```创建ssh秘钥
ssh-keygen -t rsa -C "你绑定的GitHub 邮箱"
```
#### 添加秘钥  
进入 [C:\Users\用户名\.ssh] 目录（要勾选显示“隐藏的项目”），用记事本打开公钥 id_rsa.pub 文件并复制里面的内容。  
登陆 GitHub ，进入 Settings 页面，选择左边栏的 SSH and GPG keys，点击 New SSH key。  
Title 随便取个名字，粘贴复制的 id_rsa.pub 内容到 Key 中，点击 Add SSH key 完成添加。
![](ssh.png)

##### 验证连接
打开 Git Bash，输入 `ssh -T git@github.com` 出现 “Are you sure……”，输入 yes 回车确认。
![](ssh成功.png)

### 创建GitHub仓库     
#### *~~不然怎么放文件~~*
GitHub 主页右上角加号 -> New repository：  
Repository name 中输入 `用户名.github.io`  
勾选 “Initialize this repository with a README”  
Description 选填  
##### 填好后点击 Create repository 创建。
创建后默认自动启用 HTTPS，博客地址为：https://用户名.github.io

### 本地安装 Hexo 博客程序
```bash
npm install hexo-cli -g --registry https://registry.npm.taobao.org install express
```  
Mac 用户需要管理员权限（sudo），运行这条命令：
```bash
sudo npm install hexo-cli -g --registry https://registry.npm.taobao.org install express
```
安装时间可能会有点久，请耐心等待。
## Hexo 初始化和本地预览
### 初始化并安装所需组件：
*先找一个你打算存放本地文件的目录(要是空的哦)~~方便删库跑路~~
在此目录右键打开Git bash*
```bash
hexo init      # 初始化
npm install    # 安装组件
```
完成后依次输入下面命令，启动本地服务器进行预览：
```bash
hexo g   # 生成页面
hexo s   # 启动预览
```
访问 `http://localhost:4000`，出现 Hexo 默认页面，本地博客安装成功！

这个文件夹就是网站的根目录  
它长这样
![根目录](根目录.png)

Tips：如果出现页面加载不出来，可能是端口被占用了。Ctrl+C 关闭服务器，运行`hexo server -p 5000`(当然你也可以改成其他你喜欢的，~~比如114514~~，打算之后你的访问就要把上面的4000改成你改后的端口号) 更改端口号后重试。

#### 新建完成后，指定文件夹的目录如下：
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
具体说明在[这里](https://hexo.io/zh-cn/docs/setup)
## 部署 Hexo 到 GitHub Pages
本地博客测试成功后，就是上传到 GitHub 进行部署，使其能够在网络上访问。~~*先不要管它丑不丑的事*~~    
首先安装 hexo-deployer-git：  
```bash
npm install hexo-deployer-git --save
```
然后修改根目录下 _config.yml 文件末尾的 Deployment 部分，修改成如下：  
```yml
deploy:
  type: git
  repository: git@github.com:用户名/用户名.github.io.git
  branch: main
```
完成后运行 `hexo d` 将网站上传部署到 GitHub Pages。  
完成后访问我们的 GitHub 域名 `https://用户名.github.io` 就可以看到 Hexo 网站了。（国内可能还要等上几分钟）

### 更换主题
没错hexo有非常丰富的[主题库](https://hexo.io/themes/) *~~不然默认页面这么丑谁用得下去~~*

[主题](https://hexo.io/zh-cn/docs/themes)的使用非常简单，在你找到一个喜欢的主题后，这个主题的介绍一般都会有详细的使用方法，这里就不详细讲了。可以参照官方的[主题](https://hexo.io/zh-cn/docs/themes)文档
# 后期使用
## 发布文章
进入博客所在根目录，右键打开 Git Bash Here，创建博文：  
```bash
hexo new "My New Post"  #引号里面是文章的标题
```
然后 source 文件夹中会出现一个 My New Post.md 文件，就可以使用 [Markdown](https://markdown.com.cn/) 编辑器在该文件中撰写文章了。

写完后运行下面代码将文章渲染并部署到 GitHub Pages 上完成发布。以后每次发布文章都是这两条命令。

```bash
hexo g   # 生成页面
hexo d   # 部署发布(依赖hexo-deployer-git)
```

也可以不使用命令自己创建 .md 文件，只需在文件开头手动加入如下格式 Front-matter 即可，写完后运行 hexo g 和 hexo d 发布。

```md
---
title: Hello World # 标题
date: 2023/8/15 hh:mm:ss # 时间
categories: # 分类
- Diary
tags: # 标签
- 第一个标签
- ...
---
摘要
<!--more-->
正文
```
## 网站设置
包括网站名称、描述、作者、链接样式等，全部在网站根目录下的 _config.yml 文件中，参考[官方文档](https://hexo.io/zh-cn/docs/configuration)按需要编辑。  
注意：所有冒号后都有一个空格！

## 常用命令
```bash
hexo new "name"       # 新建文章
hexo new page "name"  # 新建页面
hexo g                # 生成页面
hexo d                # 部署
hexo g -d             # 生成页面并部署
hexo s                # 本地预览
hexo clean            # 清除缓存和已生成的静态文件
hexo help             # 帮助
```
# 其他设置
## Hexo 设置显示文章摘要，首页不显示全文
Hexo 主页文章列表默认会显示文章全文，浏览时很不方便，可以在文章中插入 `<!--more-->` 进行分段。  
该代码前面的内容会作为摘要显示，而后面的内容会替换为 `Read More` 隐藏起来。   
## 设置网站图标
进入 themes/主题 文件夹，打开 _config.yml 配置文件，找到 favicon 修改，一般格式为：`favicon: 图标地址`。（不同主题可能略有差别）

[def]: 根目录.png