---
title: Hexo使用简介
date: 2023-05-01 21:19:51
categories:
- index
tags:
- 第一个
- 技术
---

## Hexo的使用

### GitHub Page

Github Page可以方便的部署静态网站，当需要设置个人主页时，可以建立一个新的库，库的名字时``username.github.io``,其中username是用户的用户名。在库的setting页面上设置setting page,让其source指向master节点。


<!--more-->
### 创建一个Hexo项目

Hexo是依赖Webpack的一个框架，因此是依赖Node.js的。当Node.js已经安装后，我们使用``npm install -g hexo-cli``命令安装hexo脚手架。完成之后，我们可以使用脚手架新建一个Hexo项目。具体的命令为``hexo init blog``。



### Hexo项目的本地运行

如果想在本地预览Hexo的效果，可以在命令行输入``hexo server``命令在本地部署网页，这样就可以在本地预览效果。



### Hexo的部署

当拥有了一个Hexo项目之后，我们就可以将他部署到GitHub Page上了。首先，在该项目中使用``npm install hexo-deployer-git --save``下载git插件，然后，修改项目根目录下的 ``_config.yml`` 文件。我们修改该文件末尾的deploy节点信息，具体代码如下：

```json
deploy:
  type: git
  repository: https://github.com/username/username.github.io.git
  branch: master
```

其中respository表示需要部署的库的地址。branch表示需要上传的代码的分支是master。

将修改之后的配置文件保存之后，我们使用``hexo generate``或``hexo g``命令生成静态文件，然后使用``hexo deploy``命令将文件部署到GitHub上。



### 更改Blog的主题

[Hexo](https://hexo.io/themes/)提供了很多开源的主题，这些主题通过git保存到项目的库中以后可以通过更改_config.yml中``theme:``项来改变配置。



### 创建新的页面

新的页面的创建可以由指令``hexo new pagename``来创建，创建之后，会自动生成一个md文件。如果需要生成一个新的文件夹，则只需要将命令改写成``hexo new page dirname``就可以了。对于每一个md文件的头部需要生成一些关键的信息。具体的格式如下：

```markdown
---
title: "Hello World"
date: 2022-05-15 10:00:00
categories:
- 技术
tags:
- Hexo
- Blog
---
```

其中tags可以有多个而categories只能有一个。