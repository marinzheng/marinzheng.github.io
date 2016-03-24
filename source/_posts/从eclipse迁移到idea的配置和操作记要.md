---
title: 从eclipse迁移到idea的配置和操作记要
date: 2016-03-22 23:29:59
tags: IDEA
categories: 技术
---

IntelliJ IDEA号称是最好用的Java IDE，这两年尝试了好几次，都因为敌不过对Eclipse更加熟悉而放弃。最近几个项目都是基于Maven构建的，Eclipse的目录结构屡屡挑战我的代码洁癖，最后终于挑战成功，在一个不加班的周末，决定彻底倒戈IDEA阵营。

迁移了一个web工程，整个过程没有什么坑点。配置和操作的要点记录如下：

**配置：**

- 2016.1版的IDEA要jdk1.8才能跑，而程序多运行在jdk1.7上，所以需要为IDEA专门新建一个环境变量：`IDEA_JDK_64` 指向1.8版jdk目录；
- IDEA安装目录下的bin目录中有两个文件值得注意：idea64.exe.vmoptions和idea.properties。前者是IDEA运行的jvm参数；后者是IDEA的各种配置项；
- IDEA默认会在用户目录下的.IntelliJIdea目录下新建两个文件夹：config和system。其中config是IDEA的配置目录，system是项目的配置目录。config目录需要进行备份，之后在别的机器上，把这个备份copy进去就不用再重新设置一遍了。备份的过程是把.IntelliJIdea目录从操作系统盘移到别的硬盘，然后把config设成百度云盘的自动备份目录。修改idea.properties中：
```
    idea.config.path=D:/JetBrains/.IntelliJIdea/config
    idea.system.path=D:/JetBrains/.IntelliJIdea/system
```

- 改变IDEA皮肤：`File->Settings->Appearance & Behavior`在`Theme`中选`Darcula`；
- 改变IDEA字体大小：`File->Settings->Appearance & Behavior->Appearance`选中`Override default fonts by(not recommended)`改`Size`；
- 改变编辑区字体大小：首先要点`File->Settings->Editor->Colors & Fonts`新建（`Save As`）一个`Scheme`（不要用`Default`或`Project Default`，要给个性化配置起个名字，以后就用这个名字的配置），然后点`Colors & Fonts`下的`Font`、`Console Font`更改字体大小；
-  关闭未被使用错误：首先要点`File->Settings->Editor->Inspections`新建一个`Profile`（从`Default`上`Copy`），然后点`File->Settings->Editor->Inspections`找Java选`Declaration redundancy`改`Unused declaration`；
- 关闭拼写错误：点`File->Settings->Editor->Spelling`点`Configure 'Spelling' inspection`改`Spelling->Typo`；
- 设置显示行号：`File->Settings->Editor->General->Appearance`选中`show line numbers`；
- 最好勾选`View`菜单中的: 

	![View菜单勾选项](http://marinzheng.github.io/images/viewbar.png) 

  这样会显示工具框，提供很多可点击的快捷方式，如:
	
	打开文件目录： 

	![open快捷方式](http://marinzheng.github.io/images/viewopen.png) 

	setting（idea的配置）： 
	
	![setting 快捷方式](http://marinzheng.github.io/images/viewsetting.png) 

	project structure（项目相关配置，sdk什么的）： 
	
	![project structure快捷方式](http://marinzheng.github.io/images/viewprojectstructure.png) 


另外，IDEA左下角鼠标悬停有提示框，能选很多工具，比如`Maven Projects`工具框，打开后直接点击就可以运行Maven命令了，方便。


**IDEA与Eclipse的几点不同：**

- Eclipse中有workspace的概念，它里面可以放很多project，一个Eclipse可以引入某个workspace里的多个project；IDEA里没有workspace的概念，它的最顶级结构是project，规定的结构是一个project里可以包含一个或多个module（和maven是一样的：project是spring-framework，module有spring-mvc、spring-jpa等）。当project只包含一个module时，一般设定project和module是一个目录，一个名字（比如test） ，这样就和Eclipse的结构看起来差不多了，但其实本质上是不一样的，它是test工程的test模块，而不是某个workspace下的test工程。一个IDEA只能打开一个project中的模块；
- IDEA不用按`Ctrl s`保存，它自动会保存；
- IDEA的普通工程，默认不能自动编译（Eclipse是保存的时候编译，是自动的），需要手动`Compile`（某个类）、`Build`（project或module）、`Make`（增量编译，用的最多）。每回手动点这三个按钮太麻烦，可以配置成：在执行运行命令前自动执行`Make`（比如配置Tomcat运行命令时（`Run->Edit Configurations`中配置）时，自动有`before launch Make`命令），也就是说跟自动编译也差不多。用Maven的时候更不用担心了，Maven工程都是执行Maven的命令去编译、打包、jetty运行什么的，本来也不用IDE去管这些事情。

**快捷键：`File->Setting->Keymap`改为`Eclipse`**

- 文件内搜索和替换：`Ctrl + F`
- 继续向前搜索：`Ctrl + K`
- 继续向后搜索：`Ctrl + Shift + K`
- 类内搜索方法：`Ctrl + O`
- 工程内搜索一切：`Shift Shift`
- 工程内搜索类：`Ctrl + Shift + T`
- 工程内搜索文件：`Ctrl + Shift + R`
- 工程内搜索文字：`Crtl + H`
- 文件内上移下移一行：`Alt + 上下键`
- 关闭当前页：`Ctrl + F4`
- 关闭工具框：`Ctrl + W` （比如Project、Run、Version Control）
- 关闭弹出框：`Esc` （`Ctrl + F`弹出来的搜索框）
- 最近编辑的文件列表：`Ctrl + E`
- 搜索所有引用处：`Ctrl + Alt + H`
- 调出最近复制的N份内容：`Ctrl + Shift + V`
- 格式化代码：`Ctrl + Shift + F`
- 跳到方法定义处：`F3`
- 自动代码生成：`Alt + Insert`
- 快速修复错误：`Alt + Enter` （类似与Eclipse中的 `Ctrl + 1`）
- 补全当前行：`Ctrl + Shift + Enter ` （最常用的场景时补全当前行后的；号并将光标定位到下一行）
- 定位到接口实现类中的方法：`Ctrl + T`
- 跳到指定行：`Ctrl + L`
- 删除当前行：`Ctrl + D`
- 撤销：`Ctrl + Z`
- 复原：`Ctrl + Y`
- 全选：`Ctrl + A`
- 剪切：`Ctrl + X`
- 复制：`Ctrl + C`
- 粘贴：`Ctrl + V`
- 注释：`Ctrl + /`
- 返回刚才编辑：`Alt + 左右键`

**代码模板：输完按`Tab`**

- `sout`：System.out.println();
- `5.sout`：System.out.println(5);
- `fori`：for (int i = 0; i < ; i++) {}
- `5.fori`：for (int i = 0; i < 5; i++) {}
- `list对象引用.fori`：for (int i = 0; i < list.size(); i++) {}
- `list对象引用.for`：for(Oject o : list) {}
- `ifn`：if方法体
- `else ifn`：else if方法体
- `psvm`：main方法
- `psf`：public static final
- `st`：String
