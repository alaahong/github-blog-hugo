---
title: "New Blog With Hugo"
date: 2020-04-06T14:54:53+08:00
draft: false
---


最近翻译[Selenium官方文档](https://www.selenium.dev/documentation/)的缘故，接触了[Hugo](https://gohugo.io/)，原本的博客有些年久失修，决定用Hugo重新做一版.


# 安装Hugo

此段详见[官网教程](https://gohugo.io/getting-started/installing/)，Windows上建议通过[Chocolatey](https://chocolatey.org/)安装

```powershell
> choco install hugo-extended -confirm
Chocolatey v0.10.15
Installing the following packages:
hugo-extended
By installing you accept licenses for the packages.

hugo-extended v0.68.3 [Approved]
hugo-extended package files install completed. Performing other installation steps.
Downloading hugo-extended 64 bit
  from 'https://github.com/gohugoio/hugo/releases/download/v0.68.3/hugo_extended_0.68.3_Windows-64bit.zip'
Progress: 100% - Completed download of C:\Users\Ian\AppData\Local\Temp\chocolatey\hugo-extended\0.68.3\hugo_extended_0.68.3_Windows-64bit.zip (36.25 MB).
Download of hugo_extended_0.68.3_Windows-64bit.zip (36.25 MB) completed.
Hashes match.
Extracting C:\Users\Ian\AppData\Local\Temp\chocolatey\hugo-extended\0.68.3\hugo_extended_0.68.3_Windows-64bit.zip to C:\ProgramData\chocolatey\lib\hugo-extended\tools...
C:\ProgramData\chocolatey\lib\hugo-extended\tools
 ShimGen has successfully created a shim for hugo.exe
 The install of hugo-extended was successful.
  Software installed to 'C:\ProgramData\chocolatey\lib\hugo-extended\tools'

Chocolatey installed 1/1 packages.
 See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
```

注意安装扩展Hugo，否则在某些主题下可能遇到[编译错误](https://gohugo.io/troubleshooting/faq/#i-get-tocss--this-feature-is-not-available-in-your-current-hugo-version).

```bash
>hugo server
Building sites … WARN 2020/04/06 14:18:29 found no layout file for "HTML" for kind "home": You should create a template file which
matches Hugo Layouts Lookup Rules for this combination.
```



# 制作站点

在Github上创建存放源码的repo

![image](/images/2020-04-06_15_04_34-Window.png)

![image](/images/2020-04-06_15_07_07-Window.png)



Clone到本地，并创建Hugo站点，如果是存量项目，可能会遇到需要强制创建的参数

```
>hugo new site .
Error: alaahong.github.io already exists and is not empty. See --force.

>hugo new site . --force
Congratulations! Your new Hugo site is created in alaahong.github.io.

```

选择一个主题，我使用了一个比较简洁的[Coder](https://themes.gohugo.io/hugo-coder/)

```
git submodule add https://github.com/luizdepra/hugo-coder.git themes/hugo-coder
```

在配置项选择指定的主题

```
theme = "hugo-coder"
```

hugo一下

```
>hugo
Building sites …
                   | EN
-------------------+-----
  Pages            |  7
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  0
  Processed images |  0
  Aliases          |  2
  Sitemaps         |  1
  Cleaned          |  0

Total in 119 ms

```

本地预览一下 

```
hugo server
```

![image](/images/2020-04-06_16_37_18-手中执剑,方能保护所爱之人.png)



# 增加评论

[官方推荐](https://gohugo.io/content-management/comments/)的是Disqus，这里采用的是[utteranc](https://utteranc.es/)

首先建立一个评论专用的repo

![image](/images/2020-04-06_21_10_54-Window.png)

新建评论标签

![image](/images/2020-04-06_21_12_20-Window.png)

添加配置

```
[params.utteranc]
    enable = true
    repo = "alaahong/hugoblogcomments"    # 存储评论的Repo，格式为 owner/repo
    issueTerm = "pathname"  #表示你选择以那种方式让github issue的评论和你的文章关联。
    theme = "github-light" # 样式主题，有github-light和github-dark两种
    label = "BlogComments"  # Github Issue的标签,要先在repo里面建立这个标签
```

效果如下

![image](/images/2020-04-06_21_13_43-Window.png)

# TOC目录

官方有实现[TOC](https://gohugo.io/content-management/toc/)，但是网上资料比较旧，涉及这个的不太多。将以下内容添加到single.html里合适的位置即可

```
    <aside>
        {{ .TableOfContents }}
    </aside>
```

[这里](https://gohugo.io/getting-started/configuration-markup/#table-of-contents)有对应的配置项

# 编译部署

推荐将github的静态页资源作为子模块

```shell
git submodule add -b master https://github.com/alaahong/alaahong.github.io.git public
```

官网给了linux环境下的[一键部署](https://gohugo.io/hosting-and-deployment/hosting-on-github/)，仿照着这里提供一个windows下的脚本

```powershell
hugo
cd public
git add --all
git commit -m "Publishing to github"
git push
cd ..
```

保存以上内容到deploy.cmd，每次修改后执行此脚本，会自动将public下的全部新增未忽略文件推送到github.