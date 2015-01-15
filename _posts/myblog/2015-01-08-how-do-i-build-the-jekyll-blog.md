---
title: 基于github pages + jekyll的博客搭建过程
tagline: ""
excerpt: ""
category: myblog
layout: post
tags : [myblog]
---
{% include JB/setup %}

[Thomas's blog](http://www.thomaszhao.cn/)是基于[github pages](https://pages.github.com/)搭建的。本文记载这个blog搭建的过程。欢迎[fork](https://github.com/thomaszhao/thomaszhao.github.io)或基于本模板搭建你自己的blog。。

目标和技术选型
===============================================================================

## 目标

我想要的blog的效果是这个网站这样的效果: [getbootstrap](http://getbootstrap.com/getting-started/)

* 整体结构简洁，外观看起来像个技术博客
* 右侧有目录树
* 代码简单、清晰、易修改
* 有评论

## Why Github Pages

Github Pages优点：

* 静态网页博客，基于jekyll，并且使用了github自带的CDN
* 可以使用自带域名，
* 无以匹配的开源精神
* github + pages + jekyll可以完全体验DIY的感觉，而且可以做的非常干净、利索，这其实是我做选择的主因

Github Pages缺点：

* CDN、服务器都在米国，随时可能被墙。不过无所谓了，我假设技术博客的读者大多数都会fq。
* 毕竟是静态博客，功能受限。

为什么不用其他现成的blog系统：

- 总是不自觉地去转写有的没的，我现在需要一个blog只写我自己的原创作品。
- 大爱markdown
- 现成的blog系统都是固定的格式，很难按照自己的意思改些东西，仅限于几个模板。
- 想要个独立的域名
- 不想花钱去买公有ip/服务器。
- 有很多可以达成我的要求，但我就是先看上github pages了

## 模板选择

#### hexo还是用jekyll？

使用github pages来搭建博客，可以有多种模板/原型来使用的，例如：

[hexo](http://hexo.io/)：

* 分分钟搭建博客
* 有非常多的模板，自带各种强大的插件。
* 但是他没用jekyll，而是自己完成了一套markdown转换成html的功能，所以是直接上传全部静态页面。

[jekyllbootstrap](http://jekyllbootstrap.com)

* 也是分分钟搭建博客
* 有多种模板(但是数量上没法和hexo比较)
* 定制化很少，基本保持jekyll的原滋原味

或者直接使用原生的jekyll自己搭建。

* 我对html之类的并不太熟悉，还是弄个模板吧。

我最终选定的[jekyllbootstrap](http://jekyllbootstrap.com)。

#### 选用哪个样式？

jekyllbootstrap也有很多的样式可以选择，我开始选择的是[The-program](http://themes.jekyllbootstrap.com/preview/the-program/)模板，感觉很IT。但后来发现这个模板的代码写的太浮肿了...。
于是采用的是默认的`bootstrap-3`的模板，外加一些自己的小修改。后续进行不断的调优。
BTW: 话说基于[bootstrap](http://getbootstrap.com)的web开发竟然变得如此优雅，想当年我们画表格的日子简直是弱爆了。

#### 评论插件、站长统计

用disqus还是多说？

* disqus: 匿名发文看起来更有好些；国际化看起来更好一些；
* 多说：可以和微博互动。

还是使用国内的吧，速度快点。

google的站长统计速度看起来并不是特别慢，所以还是用了Google Analytics.

搭建过程和详细步骤
==============================================================================

以下是我的搭建过程，部分功能会开新的文章写。事实上我不会写的太详细。。。。。。网上详细搭建的文章有很多，需要的话自己搜一下关键字`jekyll github pages`.

## 配置jekyll-bootstrap

参考资料：

* [jekyllbootstrap有很详细的官方手册](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

下载原版的jekyll-bootstrap框架：

```    
    git clone https://github.com/plusjade/jekyll-bootstrap.git thomaszhao.github.io
```

删掉原版的git log（按照git的思维，应该保留的，但是我觉得乱。作为补偿，去原作者的github上点个star好了~

```
    cd thomaszhao.github.io    
    rm -rf .git/
```

初始化自己的git:

```
    git init
    git add .
    git commit -m "original bootstrap with theme the-program"
    git remote add origin git@github.com:thomaszhao/thomaszhao.github.io.git
    git push -u origin master -v
```

以上就是第一次提交之后的版本。可以参考我的github：
    
    https://github.com/thomaszhao/thomaszhao.github.io/tree/63664d96e7eb6c19d579173309384a0a776ed287

准备启动脚本: jekyll可以非常方便的本地预览自己的页面，把以下命令放到`jekyll_server.sh`脚本里：（ip是我自己的服务器的ip）
这样以后直接执行./jekyll_server.sh即可。

```
#!/bin/sh

rm -rf _site/*
jekyll serve -H 192.168.192.128
```

备注：如果jekyll生成文档错误，可以使用如下命令来跟踪出了什么错误：

```
jekyll build --trace
```


然后就该修改自己的信息了，修改一下文件：

```
_config.yml：  一些参数
_includes/themes/the-program/default.html:  去掉twitter之类的信息
index.md:    去掉默认的例子，顺便美化一下
README.md:   写你自己的信息
CNAME:       设置自己的cname
```

git 提交后，你已经有了一个基础的blog系统。接下来就需要各种定制化修改了。


## 使用`多说`评论

jekyllbootstrap默认没有[多说](http://www.duoshuo.com)评论，所以需要自己稍微定制化一下:
增加对多说的支持：

$ vi _includes/JB/comments

{% raw %}
```
+{% when "duoshuo" %}
+  {% include JB/comments-providers/duoshuo %}
{% when "disqus" %}
   {% include JB/comments-providers/disqus %}
```
{% endraw %} 


$ vi $ cat _includes/JB/comments-providers/duoshuo

```
<!-- Duoshuo Comment START -->
<div class="ds-thread" data-thread-key="{{ page.url }}" data-title="{{ page.title }}" data-url="{{site.production_url}}{{ page.url }}"></div>
<script type="text/javascript">
var duoshuoQuery = {short_name:"{{ site.JB.comments.duoshuo.short_name }}"};
(function() {
var ds = document.createElement('script');
ds.type = 'text/javascript';ds.async = true;
ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.unstable.js';
ds.charset = 'UTF-8';
(document.getElementsByTagName('head')[0] 
|| document.getElementsByTagName('body')[0]).appendChild(ds);
})();
</script>
<!-- Duoshuo Comment END -->
```

修改_config.yml：

```
 82   comments :                                                                    
 83     provider : duoshuo                                                          
 84     duoshuo:                                                                    
 85            #this shortname must be exactly the same as your subdomain in duosuo, e.g.: <your    -short-name>.duoshuo.com
 86             short_name: thomaszhao  
```

详细代码请参考我的githu:
    https://github.com/thomaszhao/thomaszhao.github.io/commit/97487a289ea97be2630a4e3ec399426b8805d096

代码如上就可以了，然后就该去多说网站上注册多说的账户，记得子域名一定要和上面的short_name一致。

```
Tips: 多说插件可以定制化他的显示形式，比如每页显示多少评论，你的输入框在上面还是下面：

* 去官网上点击“后台管理” - “设置”，配置比如按最新的顺序显示评论，绑定微博号等等。
```

## 配置谷歌站长分析(google analytics)

## 配置分享条

我也直接用的duoshuo的插件，如果需要的话请直接看代码:
    https://github.com/thomaszhao/thomaszhao.github.io/commit/97487a289ea97be2630a4e3ec399426b8805d096
    https://github.com/thomaszhao/thomaszhao.github.io/blob/master/_includes/JB/sharing
    https://github.com/thomaszhao/thomaszhao.github.io/blob/master/_includes/JB/sharing-providers/duoshuo

## 调整：Permalinks：文章的url，我希望和文件目录一致

jekyllbootstrap默认的Permalinks的格式如下：/:categories/:year/:month/:day/:title
我把categories去掉了。
这样我的文章只要文件名不改变，改变目录、分类等对评论、统计就都没有影响。

## 增加old_id的支持：

如果手贱改了文件名，而comments的依据是page.id，那么如何能让旧的comment也还在？
解决方案：在yaml头中增加old_id，comments的依据是page.old_id，如果不存在，才是page.id
代码见：
https://github.com/thomaszhao/thomaszhao.github.io/commit/665bca6b7ab7b2dd304b0447420128e976e12d85

## 增加目录树(或者叫 侧边导航栏？目录导航栏？)

#### 实现方法：
总体思路：常见的目录树是在服务器端生成的，getbootstrap官网的目录树也是在服务器端生成的。而jekyll服务器端是静态文件托管，所以只能用js动态生成目录树了。总体思路是js代码，读取页面中文章内容部分的h1、h2、h3标签，然后生成目录树，放到指定位置。

直接看代码吧，后续有时间再整理：

```
    https://github.com/thomaszhao/thomaszhao.github.io/blob/master/assets/themes/bootstrap-3/css/style.css#L102
    https://github.com/thomaszhao/thomaszhao.github.io/blob/master/assets/themes/bootstrap-3/js/application.js
```

#### 参考资料：

目录导航的样式摘抄自bootstrap的官网文档部分：（但是他是服务器端手写的side nav）
http://getbootstrap.com/getting-started/
https://github.com/twbs/bootstrap/blob/master/docs/_layouts/default.html#L33
js动态生成目录树的代码摘抄自以下2个：
http://www.pchou.info/web-build/2013/01/09/build-github-blog-page-06.html
https://github.com/PChou/PChou.github.io
http://www.cnblogs.com/xdp-gacl/p/3718879.html
(方法非常相似，cnblog使用的是html的语法，pchow的用的是jquery的语法)

## 语法高亮

github pages集成的jekyll默认高亮是`Pygments`高亮，使用这个需要破坏markdown的语法。
所以通常的解决方案是使用js高亮，有很多的选择。
我用的是`google-code-prettify`，安装方法如下：

```
    https://code.google.com/p/google-code-prettify/wiki/GettingStarted#Serving_your_own_JS_&_CSS

我自己的修改记录：
    https://github.com/thomaszhao/thomaszhao.github.io/commit/54db568cbfd62e220e9f9a69d47f8485719040d7
```

## [ ] 站内搜索

## 其他配置:

### 配置支持github特殊的markdown语法：

```
$ vi _config.yml

markdown:    kramdown                                                           
kramdown:                                                                       
   input: GFM     
```

小结
==============================================================================

花了些时间按自己的心意搭建了一个blog，后续会在上面记录很多的信息。
使用技术：

* `github + pages`负责，`jekyll`负责markdown到静态网页服务器的转换；
* 基于`bootstrap`快速搭建网站，十分方便；
* 使用了`jekyllbootstrap`的模板;
* 顺便学习/温习了一下`jquery` `html`。


