# Personal Website

在尝试利用https://github.com/kitian616/jekyll-TeXt-theme搭建个人博客网站时，由于完全不会Ruby，发现看代码效率很低，于是从Ruby语言开始学起

Ruby: 一种脚本语言

Gem: 表示Ruby中可以调用的软件包，包含打包的Ruby应用程序或库，每个gem都遵循相同的代码组织标准结构

- lib目录包含gem的代码
- test或spec目录包含供开发者测试的框架
- Rakefile。ruby程序会使用它来自动化测试，生成代码和执行其他任务。
- bin目录中的是可执行文件，该文件将在PATH安装gem时加载到用户的文件中 。安装gem时，会自动生成。
- gemspec，其中包含gem有关的信息，例如版本号、作者、联系邮件等

Jekyll: 一种用Ruby写成的静态网页生成工具, 本质上也是一个Gem

Gemfile: Jekyll中用来列出所需Gem的一个文件。

Bundler: Bundler可以安装Gemfile里的Gem。ruby项目依赖于一系列的ruby gems，而bundler是一个很好的管理ruby项目gems的工具。使用bundler可以为你的ruby project提供统一的构建环境。

初次建立一个新的Jekyll project，可以用gem install bundler来安装Bundler.
使用 bundle init来建立一个新的Gemfile
再用bundle install来用Bundler安装Gemlist里的Gem.
如果你修改了Gemlist,需要及时的update Gem: bundle update
编译，在本地运行Jekyll生成的网页：bundle exec jekyll serve





---

layout: "post"

1. how to compile:
   for the first time, use `bundle exec jekyll serve`

after, just use `jekyll serve`

2. how to make draft
   将文件保存在_drafts文件夹中，该文件默认不会出现在网站中，如果想在网站中显示该文件，就用`jekyll serve --draft`
   在_post文件中的文件需要在文件名中显式地写出日期，而draft不需要

3. 新建一个页面，例如about page
   about.md文件储存在根目录下，其本质与其他的blog是相同的

4. 如何给一个post分配永久链接
   _site文件夹里全都是编译后的内容，所生成的网页链接与文件夹内部结构相同。
   如果希望有一个固定链接的话，可以在头文件中写入

```
permalink:"/my-url/"
```

或者如下，这里冒号表示后面的是一个变量

```
categories: jekyll 
permalink: /:categories/:year/:month/:day/:title.html
```

5. 头信息默认值
   在_config.yml末尾添加如下内容

```yml
defaults:
  -
    scope:
      path: "" #如果是空白，就应用于所有的文件，如果有内容，就只应用于对应路径中的文件
      type: "post"
    values:
        layout: "post"
        author: "Me"
      #values
```

请注意，每次修改_config.yml文件都要重新启动jekyll

6. 安装一个主题
   主题一样是在_config.yml文件中定义好的。
   如果要安装一个新的主题
   (1) 进入rubygems.org网站，搜索jekyll-theme，选定theme后，进入根目录下的Gemfile，在如下位置添加

```
gem "minima", "~> 2.5"
gem "theme-name"
```

然后在终端中输入bundle install来安装对应的theme，并在_config.yml中修改所用的theme。
安装后可能会遇到没有对应的layout的问题，每个网站至少都需要post page home三个layout，这时就需要去他的源码看有什么_layouts

7. layout
   我们也可以创建自己的layout，只需要在根目录中新建一个_layouts文件夹，并在里面放入一些html文件。
   layout可以嵌套，只需要在down-level layout中加入

```
---
layout: "up-level layout"
---
```

8. Variables
   content: 当前blog的所有内容
   page: 当前网页
   site.title: 通常是_config.yml文件中定义的一些变量
   其他介绍见jekyll官网

9. include
   根目录下新建文件夹_incldues，这里比较复杂了，暂时不记

10. 遍历，条件语句
    这里主要是介绍html的语法

11. data
    根目录下新建文件夹_data，这里的文件通常储存一些全局变量，然后可以通过site.data.variable来访问

12. 静态文件
    相关文件夹：assets