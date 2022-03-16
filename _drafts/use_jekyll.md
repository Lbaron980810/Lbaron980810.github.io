---
layout: "post"
---
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