---
title: hexo博客创建和日常操作记要
date: 2016-03-04 12:03:37
tags: hexo
categories: 技术 
---

github是搭建静态博客的一件多快好省的利器，非常经济实惠，非常程序员。

目前，有很多现成的工具可以帮助我们很轻松的完成这件事情。在这些工具的选择上，我比较挑剔两点：

1. 工具的安装、配置及日常操作是否简易；
2. 配套的主题是否有中意可用的。

第一个挑剔点主要基于：懒！不想花过多时间研究和记忆这个，最好拿来即用；第二个挑剔点主要基于我对前端不老熟的，主题改起来费劲。尝试过jekyll、hexo和hugo三种在github上搭建静态博客的工具。感觉jekyll配置太复杂，hugo主题太少且没有中意的，所以最后选择了hexo。

----------

hexo是一台湾同胞研发的一款基本于nodejs的github静态博客创建和日常操作工具（[https://hexo.io/](https://hexo.io/ "hexo官网")）。利用hexo搭建一个高可用的github静态博客，原理和过程是：**在github上创建一个以自己名字（比如是xxx）开头项目：xxx.github.io（github会对这个项目提供web支持，即可以通过浏览器访问），在本地生成博客（文章、样式等），将本地生成的博客推送到xxx.github.io上。**要点无外乎：**安装必要工具、创建github远程博客、使用hexo生成本地博客、使用hexo将本地博客的内容推送到github远程博客、使用hexo将本地博客的源文件备份到github库，以便在多个电脑上操作。**以下是我本次搭建这个博客的流水帐记要：

- ***安装工具（64位win7系统）***：
 
	- node-v5.7.1-x64.msi （[https://nodejs.org/en/](https://nodejs.org/en/ "node下载地址")）
	- Git-2.6.2-64-bit.exe （[http://git-scm.com/download/](http://git-scm.com/download/ "git下载地址")）
	- TortoiseGit-1.8.15.0-64bit.msi （[https://tortoisegit.org/download/](https://tortoisegit.org/download/ "tortoisegit下载地址")）
	
以上软件版本都用最新就好。前几天为调试pomelo代码，把node-v5.7.1-x64.msi卸了，装了一个node-v0.10.13-x64.msi，回过头来再用hexo命令，发现不能用了，说node版本太低。如果习惯用命令行操作git的话，不装TortoiseGit也行，反正我是不习惯。

- ***创建github远程博客***

github用户名是xxx，就创建一个xxx.github.io的项目。项目创建好之后，再顺便建一个backup分支，用于备份博客源文件（不是之后有把本地博客推送到github上的步骤吗？为什么这里还要建分支用于备份？——这是因为推送本地博客到github上，并不是推送的源文件，而是根据源文件生成的博客显示内容，所以为保证多电脑使用，要有备份源文件步骤，这里先建好backup分支）。

![github上创建backup分支](http://marinzheng.github.io/images/backup.png)


- ***生成本地博客***

node有一套自己的包管理工具，叫npm（装node的时候已自动装好），只要把工具和包上传到npm服务器上，就可以用npm的命令下载到本地安装使用了，跟maven远程仓库差不多。hexo及其配套的一些工具都已上传到了npm服务器，按照hexo官方文档（[https://hexo.io/zh-cn/docs//](https://hexo.io/zh-cn/docs/ "hexo官方文档")）上的步骤，非常容易地就能创建一个本地博客：

```
    $ cd e:                    #-- 命令行进入本地磁盘e盘
    $ npm install hexo-cli -g  #-- 通过npm命令安装hexo
    $ hexo init xxx.github.io  #-- 通过hexo命令生成本地博客项目
    $ cd xxx.github.io         #-- 进入本地博客目录
    $ npm install              #-- 通过npm命令安装项目依赖
    $ hexo g                       通过hexo命令将本地博客项目源文件转化为
							   #-- 博客文件（每回修改源文件，都得运行这个命令）
    $ hexo s -p5000            #-- 通过hexo命令启动本地博客
```

此时，本地博客就创建好并启动起来了，可以通过`http://localhost:5000`访问了。按理说运行`hexo s`就行了，不用加端口，它就可以启动一个监听4000端口的web服务，但是我这儿不行。看了下我的hexo的版本是`3.2.0 released`，网上有人说hexo在`3.0`之后改动挺大，分出了很多模块，要单独安装，这种情况要装server模块什么的，太麻烦了，反正现在这种运行方法还行，运行的时候加一个端口就行了，也不费事，我就没装（但是之后发现git模块非装不可，不然不能推送到github远程库）。

关于本地博客的具体配置项，可以参考上述hexo官方文档或百度，主要是修改博客根目录下的`_config.yml`文件，这里就不详细介绍每一项了（我也没仔细看）。比较关键的是修改语言和主题：

1. 将语言修改为中文：修改`_config.yml`文件中的`language: zh-CN`
2. 下载和替换`maupassant`主题：

下载：在本地`xxx.github.io`目录下：

```
    $ git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant  #-- 通过git命令下载
    $ npm install hexo-renderer-jade --save                                        #-- 安装主题必要插件
    $ npm install hexo-renderer-sass --save                                        #-- 安装主题必要插件
```

替换：修改`_config.yml`文件中的`theme: maupassant`。

```
    $ hexo g         #-- 转化
    $ hexo s -p5000  #-- 启动
```

此时再访问`http://localhost:5000`，验证主题是否有变化。

关于`maupassant`主题的配置项，可以参考主题作者的博客：[https://www.haomwei.com/technology/maupassant-hexo.html](https://www.haomwei.com/technology/maupassant-hexo.html "maupassant主题博客")，主要是修改本地博客根目录下`themes/maupassant/_config.yml`文件。


- ***将本地博客的内容推送到github远程博客***

在`_config.yml`文件中配置（如果没有，就在最后添加）：

```
    deploy:
    	type: git
    	repository: git@github.com:xxx/xxx.github.io.git
    	branch: master
```

**！这里一定要用`ssh`仓库地址（`git@`），我用`https`的没成功。**

在本地`xxx.github.io`目录下：

```
	$ npm install hexo-deployer-git --save  #-- 通过npm命令安装hexo的git模块
	$ hexo d                                #-- 通过hexo命令将本地博客内容摄像头到github远程博客
```

等几分钟，浏览器访问`http://xxx.github.io`，验证推送是否生效。

- ***将本地博客的源文件备份到github库***

github备份源文件非常重要。一是源文件丢了，生成的静态页面不好往回“反编译”，费时；二是程序员，单位三台电脑，家里两台电脑，抓起哪个来用哪个，用U盘来回来去倒，费事。hexo并没有提供备份源文件的命令，之前一直为这个事儿苦恼，也试过用百度云盘什么的同步，但终究太low。好在人间自有大神在：[https://github.com/coneycode/hexo-git-backup](https://github.com/coneycode/hexo-git-backup "hexo-git-backup")。这插件太好了，自从用了这个插件之后，腿也不疼了，腰也不喘了。。。

具体安装配置如下：

安装：在本地`xxx.github.io`目录下：

```
	$ npm install hexo-git-backup --save  #-- 通过npm命令安装hexo的备份模块
```

配置：在`_config.yml`文件最后添加：

```
	backup:
    	type: git
		theme: maupassant
    	repository:
       		github: git@github.com:xxx/xxx.github.io.git,backup
```

`xxx.github.io.git`库的`backup`分支之前已经建好，详见前文。如果事先没有建好的话，也可以此时创建，只是要比事先建好多一个步骤：清空分支代码。

操作：在本地`xxx.github.io`目录下：

```
	$ hexo b  #-- 通过hexo命令将本地博客的源文件备份到github库
```

----------

好了，博客的搭建到这儿就完成了。之后就是日常使用了，也是一些流水帐：

在一台新电脑或目录里（已安装必要工具）：

用TortoiseGit或命令行下载仓库：`https://github.com/xxx/xxx.github.io.git`，并切换到`backup`分支。在刚生成的本地`xxx.github.io`目录下：
	
```
	$ npm install hexo-cli -g         	     #-- 安装hexo（只需运行一次）
	$ npm install hexo-deployer-git --save   #-- 安装hexo的git模块（只需运行一次）
	$ npm install hexo-git-backup --save     #-- 安装hexo的备份模块（只需运行一次）
	$ npm install hexo-renderer-jade --save  #-- 安装主题插件（只需运行一次）
	$ npm install hexo-renderer-sass --save  #-- 安装主题插件（只需运行一次）
	$ npm install hexo-git-backup --save     #-- 安装hexo的备份模块（只需运行一次）
	$ npm install                            #-- 安装其它依赖（只需运行一次）
```

日常操作如下：

```
	$ hexo n "文章标题"  #-- 新建文章。文件建在xxx.github.io/source目录下，想编辑或修改，在这里找
	$ hexo g            #-- 将编辑的源文件转化成博客文件
	$ hexo s -p5000     #-- 启动本地博客服务器，监听5000端口
	$ hexo d            #-- 发布本地博客到github远程博客
	$ hexo b            #-- 备份源文件到github远程仓库
```

----------

总结：

1. 总体来说挺简单的；
2. hexo在3.0之后改变这么大，那么在之后4.0、5.0、...、100.0的版本中，改变是不是更大，我是不是要在安装npm包的时候，指定版本号？
3. Now!