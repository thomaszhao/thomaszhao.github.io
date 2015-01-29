---
title: 适合程序员的Blog -- 基于github pages + jekyll + markdown打造自己的blog
tagline: ""
last_updated: 2015-1-29
category: myblog
layout: post
tags : [jekyll, jekyllbootstrap, bootstrap, markdown, github pages, vim markdown插件]
---
{% include JB/setup %}

我的这个blog([Thomas's blog](http://www.thomaszhao.cn/))是基于[github pages](https://pages.github.com/)搭建的。
本文记载这个blog搭建、配置过程。
欢迎[fork](https://github.com/thomaszhao/thomaszhao.github.io)或基于本模板搭建你自己的blog。

<!-- more -->

总体概述
===============================================================================

## 目标

我想要的blog的效果是这个网站这样的效果: [getbootstrap](http://getbootstrap.com/getting-started/)

* 整体结构简洁，外观看起来像个技术`blog`或`wiki`；
* 右侧有目录树，便于查看；
* 代码简单、清晰，这样我可以按照自己的想法进行定制化修改；
* 文章要用`markdown`编写；
* 有评论，便于分享和讨论；

## 涉及到的技术

技术选型的原因及可替代方案[见后文](#alternative-solutions)。

* blog搭建在[github pages](https://pages.github.com)上。
* 使用github原生的[jekyll](https://help.github.com/articles/using-jekyll-with-pages/)而不是手动上传静态html。
* Jekyll的markdown引擎使用的是[kramdown](http://kramdown.gettalong.org/options.html).
* 模板选用[jekyllbootstrap](http://jekyllbootstrap.com)，样式使用默认的`bootstrap-3`的模板，后续加了些自己的修改。
* 页面开发主要使用`jquery` + [Twitter推出的bootstrap](http://getbootstrap.com).
* 评论插件使用了[多说](http://duoshuo.com/)，没有用[disqus](https://disqus.com/)。
* 站长统计使用的[Google Analytics](https://www.google.com/analytics/web/?)。
* 顺便学习/温习了一下`jquery` `html`。

搭建过程和详细步骤
==============================================================================

以下是我的搭建过程，部分功能会开新的文章写。事实上我不会写的太详细。。。。。。
网上详细搭建的文章有很多，需要的话自己搜一下关键字`jekyll github pages`.

配置`jekyll-bootstrap`
-------------------------------------------------------------------------------

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
jekyll serve -H 0.0.0.0
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


配置`多说`评论
-------------------------------------------------------------------------------

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

增加`old_id`的支持：
-------------------------------------------------------------------------------

如果手贱改了文件名，而`评论`的依据是page.id，那么如何能让旧的`评论`也还在？
解决方案：在yaml头中增加`old_id`，comments的依据是`page.old_id`，如果不存在，才是`page.id`。
代码见：
https://github.com/thomaszhao/thomaszhao.github.io/commit/665bca6b7ab7b2dd304b0447420128e976e12d85


增加TOC(Table of Content，或者叫 侧边导航栏、目录导航栏？)
-------------------------------------------------------------------------------

### 实现方法：

常见的TOC有2种实现方法：

* 在服务器端生成的目录树；
* 在浏览器端通过js动态生成目录树；

常见的目录树是在服务器端生成的，getbootstrap官网的目录树也是在服务器端生成的。
如果jekyll选用`kramdown`作为`markdown`引擎，可以通过指令`{:toc}`来生成一个目录树的。
但是生成的目录树不是很方便定制，

我用的是js生成。总体思路是：读取页面中文章内容部分的h1、h2、h3标签，然后生成目录树，放到指定位置。

* 生成目录树部分：jQuery读取文章内容部分的`h1~h3`，生成`ul`列表，赋值给`TOC`的容器。不需多言。
* 放到指定位置、跟随文章阅览时触发高亮：抄的[bootstrap文档官网](http://getbootstrap.com/getting-started/)的实现和样式，基于[bootstrap的affix.js特性](http://getbootstrap.com/javascript/#affix)。

直接看代码吧，后续有时间再整理：

```
    https://github.com/thomaszhao/thomaszhao.github.io/blob/master/assets/themes/bootstrap-3/css/style.css
    https://github.com/thomaszhao/thomaszhao.github.io/blob/master/assets/themes/bootstrap-3/js/application.js
```


**参考资料**:
目录导航的样式摘抄自bootstrap的官网文档部分：（但是他是服务器端手写的side nav）

```
http://getbootstrap.com/getting-started/
https://github.com/twbs/bootstrap/blob/master/docs/_layouts/default.html#L33
```

js动态生成目录树的代码摘抄自以下2个：

```
    http://www.pchou.info/web-build/2013/01/09/build-github-blog-page-06.html
    https://github.com/PChou/PChou.github.io
    http://www.cnblogs.com/xdp-gacl/p/3718879.html
```
(方法非常相似，cnblog使用的是html的语法，pchow的用的是jquery的语法)



和github尽量相似的样式:
-------------------------------------------------------------------------------

我个人非常喜欢github在预览`markdown`文件时的各种样式，所以我的blog会有很多模仿的痕迹。

### 兼容`GFM`的`markdown`语法

```
$ vi _config.yml

markdown:    kramdown                                                           
kramdown:                                                                       
   input: GFM
   hard_wrap: false
```

### 和`github`一模一样的`样式`

**CSS文件**: 我拷贝并[截取了一份github自己的`css`文件](https://gist.github.com/thomaszhao/9e6f4076b5a42e65cb00)，并把它[直接用在了我的blog里](https://github.com/thomaszhao/thomaszhao.github.io/blob/master/assets/themes/bootstrap-3/css/style.css)。

**鼠标移到标题时右侧出现链接符号**:是用js给`h1/h2/h3`增加一个链接符号，[详见代码](https://github.com/thomaszhao/thomaszhao.github.io/commit/f82e5fd578eb0a3a6bf869d5cc8019204175dee4)。 


站内搜索
------------------------------------------------------------------------------

站内搜索本身应该用[google的自定义搜索](https://www.google.com/cse/all),
但是无奈身在天朝，只能将就着用[Baidu的站内搜索了](http://zhanzhang.baidu.com/guide/index)。

Baidu提供的[`站内搜索`](http://zhanzhang.baidu.com/guide/index)的js代码不知道为什么不好用，
我直接用的他的[`搜索代码`](http://zhanzhang.baidu.com/tools/code)，效果半斤八两，
收录速度相比google都差太多。

直接看代码：

    https://github.com/thomaszhao/thomaszhao.github.io/commit/a8b8772f2bd69013714ef098fd71d5c2b84020c8

其他配置
------------------------------------------------------------------------------

#### 配置谷歌站长分析(google analytics)

#### 配置分享条

我也直接用的duoshuo的插件，如果需要的话请直接看代码:

    https://github.com/thomaszhao/thomaszhao.github.io/commit/97487a289ea97be2630a4e3ec399426b8805d096
    https://github.com/thomaszhao/thomaszhao.github.io/blob/master/_includes/JB/sharing
    https://github.com/thomaszhao/thomaszhao.github.io/blob/master/_includes/JB/sharing-providers/duoshuo

#### 调整：Permalinks：文章的url，我希望和文件目录一致

jekyllbootstrap默认的Permalinks的格式如下：/:categories/:year/:month/:day/:title
我把categories去掉了。
这样我的文章只要文件名不改变，改变目录、分类等对评论、统计就都没有影响。


#### 语法高亮

github pages集成的jekyll默认高亮是`Pygments`高亮，使用这个需要破坏markdown的语法。
所以通常的解决方案是使用js高亮，有很多的选择。
我用的是`google-code-prettify`，安装方法直接参考[google-code-prettify官网文档](https://code.google.com/p/google-code-prettify/wiki/GettingStarted#Serving_your_own_JS_&_CSS)

具体方法[直接看我的修改记录](https://github.com/thomaszhao/thomaszhao.github.io/commit/54db568cbfd62e220e9f9a69d47f8485719040d7).



我的工作流程
==============================================================================

各种常用工具
----------------------------------------------------------------------------

* **git**: 我是个地地道道的码农，对git非常熟悉。
* **vim markdown插件**: 我用的[这个vim-markdown插件](https://github.com/plasticboy/vim-markdown/)，很好很强大，而且支持`GFM`、支持`yaml`语法.

关于目录
-----------------------------------------------------------------------------

我在js代码里写死的，TOC展示`h1 h2 h3`三层目录，其中`h1`加粗。
所以平时写作，需要展现在TOC里的就用`h1 h2 h3`目录，不需要写的可以用`h4`等更深的目录层级。

Tips
------------------------------------------------------------------------------

这个tips框是我在github的code的基础上设置的css样式，直接看代码:

``` tips
https://github.com/thomaszhao/thomaszhao.github.io/commit/7c38b0213d6d9fa240a898f58459d29f3d7d6783
```

使用草稿
------------------------------------------------------------------------------

jekyll本身是自带[草稿功能](http://jekyllrb.com/docs/drafts/)的，
把文章放到`_drafts`目录下，并且文件名中没有时间标识的属于草稿。
我们想要的效果是在**本地预览**时**显示**草稿，在**正式发布**时**不显示**草稿。
配置方法很容易，在`jekyll`启动时增加参数`--draft`:

    jekyll serve -H 192.168.192.128 --draft

备注：如果你想在发布时也显示草稿文章，可以在`_config.yml`中配置`show_drafts: true`。

同时我做了一个小小的优化，页面显示时在草稿加一个`[draft]`的标记，
详见需求[给draft文章加上标记](https://github.com/thomaszhao/thomaszhao.github.io/issues/4)


非公开的隐私文章
------------------------------------------------------------------------------

github是全公开的（除非你是付费用户），但是如果我有些文章不想公开怎么办？
我目前的解决方案是创建一个新的分支(`private`)，
这个`private`分支的代码保存到其他的允许私有库的git托管服务器上，我用的是
[gitlab](https://gitlab.com/)。

具体操作如下：

```
创建一个新的分支`private`:
  git checkout -b private
将这个分支`private`托管到`gitlab`服务器上：
  git remote add -t private gitlab git@gitlab.com:<your scope>/thomaszhao.github.io.git
  git push -u gitlab private
```

当然，你也可以把master分支也同时备份到gitlab上，但是推送时就需要两个都推送了，还是算了。

当前的分支和关系：

|  local branch      | remote repository server |    remote branch |
| -------------------|-----------------------   | ---------------  |
| master             | origin(public)           | master           |
| `private`          | `gitlab (private)`       | `private`        |

``` tips
Tips: 如果我本地换机器了，需要从新下载整套新的代码，需要的git命令如下：

克隆master分支：
  git clone git@github.com:thomaszhao/thomaszhao.github.io.git
克隆private分支：
  git remote add gitlab git@gitlab.com:<your scope>/thomaszhao.github.io.git
  git fetch
  git checkout -b private gitlab/private
```

这样master分支是跟踪github的master分支，`private`分支是跟踪`gitlab`的`private`分支。

日常的工作流程是：

* 想正常发布的文章，放到master分支里，正常使用；
* 想保密的文章，切换到`private`分支，写在`privates/_posts/`目录下，这样可以在本地预览，并推送到私有分支；
* 注意: 只能把代码从master分支`merge`到`private`分支，不能反向merge。


其他可用的方案 Alternative Solutions
===============================================================================

能够实现类似功能、效果的博客系统其实是非常多的，方法也是各式各样，例如:[cnblogs](http://www.cnblogs.com/)、[wordpress](https://wordpress.org/)等等。
但我经过一定的思考，还是选择了`github pages`，也许是对`git`的过于喜爱吧。

### Why Github Pages

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
- 现成的blog系统都是固定的格式，很难按照自己的意思改些东西，仅限于几个模板，没意思~
- 想要个独立的域名
- 不想花钱去买公有ip/服务器。
- 有很多可以达成我的要求，但我就是先看上github pages了


### hexo, Octopress or jekyll？

使用github pages来搭建博客，可以有多种模板/原型来使用的，例如：

[hexo](http://hexo.io/)：

* 分分钟搭建博客
* 有非常多的模板，自带各种强大的插件。
* 但是他没用jekyll，而是自己完成了一套markdown转换成html的功能，所以是直接上传全部静态页面。

[jekyllbootstrap](http://jekyllbootstrap.com)

* 也是分分钟搭建博客
* 有多种模板(但是数量上没法和hexo比较)
* 定制化很少，基本保持jekyll的原滋原味

[Octopress](http://octopress.org/)

* 也是分分钟搭建博客;
* 用的人也是很多;

或者直接使用原生的jekyll自己搭建。

* 我对html之类的并不太熟悉，还是弄个模板吧。

我最终选定的[jekyllbootstrap](http://jekyllbootstrap.com)来作为初始模板，
后期经过了自己的改版，也满足了我深度定制的小欲望。

### 选用哪个样式？

jekyllbootstrap也有很多的样式可以选择，我开始选择的是[The-program](http://themes.jekyllbootstrap.com/preview/the-program/)模板，感觉很IT。但后来发现这个模板的代码写的太浮肿了...。
于是采用的是默认的`bootstrap-3`的模板，外加一些自己的小修改。后续进行不断的调优。

BTW: 话说基于[bootstrap](http://getbootstrap.com)的web开发竟然变得如此优雅，想当年我们画表格的日子简直是弱爆了。

### 评论插件、站长统计

用`disqus`还是`多说`？

* disqus: 匿名发文看起来更有好些；国际化看起来更好一些；功能比多说要强大很多。
* 多说：可以和微博互动。

还是使用国内的吧，速度快点，被屏蔽的风险也要小一点。

google的站长统计速度看起来并不是特别慢，所以还是用了Google Analytics.


附录
==============================================================================

常用站点
------------------------------------------------------------------------------

* bootstrap:         [http://getbootstrap.com/](http://getbootstrap.com/)
* bootstrap中文版：  [http://v3.bootcss.com/](http://v3.bootcss.com/)
* 
* Jekyll:            [http://jekyllrb.com/](http://jekyllrb.com/)
* Jekyll中文:        [http://jekyllcn.com/](http://jekyllcn.com/)
* liquid:            [http://docs.shopify.com/themes/liquid-documentation/basics](http://docs.shopify.com/themes/liquid-documentation/basics)
* 
* [Markdown Basics](https://help.github.com/articles/markdown-basics/)
* [GitHub Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/)

我的github pages的目录结构:
------------------------------------------------------------------------------

```
.
|-- assets                # css、js等资源文件，会直接发布到站点中
|   `-- themes
|       |-- bootstrap-3
|          |-- bootstrap-3.3.1
|          |   |-- css
|          |   |-- fonts
|          |   `-- js
|          |-- css
|          |   |-- bs-sticky-footer.css
|          |   `-- style.css
|          |-- jquery-1.11.2.min.js
|          `-- js
|              `-- application.js
|-- atom.xml
|-- changelog.md
|-- CNAME                 # github pages使用，CNAME文件
|-- _config.yml           # Jekyll使用，核心配置文件
|-- _drafts               # 草稿放到这里
|-- favicon.ico
|-- History.markdown
|-- images                # 图片目录
|   `-- 1.jpg
|-- _includes             # 页面模板，代码中出现include引用时，jekyll会来这个目录找
|   |-- JB
|   |   |-- analytics
|   |   |-- analytics-providers
|   |   |   |-- google
|   |   |-- categories_list
|   |   |-- comments
|   |   |-- comments-providers
|   |   |   |-- duoshuo
|   |   |-- liquid_raw
|   |   |-- pages_list
|   |   |-- posts_collate
|   |   |-- setup
|   |   |-- sharing
|   |   |-- sharing-providers
|   |   |   `-- duoshuo
|   |   |-- sidebar
|   |   `-- tags_list
|   `-- themes
|       |-- bootstrap-3
|           |-- default.html   #页面的模板文件
|           |-- page.html
|           |-- post.html
|           `-- settings.yml
|-- jekyll_serve.sh
|-- _layouts            #Jekyll使用， jekyll默认会读取这个目录的文件来作为转换模板
|   |-- default.html    # 我的这几个文件是直接去引用_include/thems/bootstrap-3目录下的同名文件
|   |-- page.html
|   `-- post.html
|-- _plugins
|   `-- debug.rb
|-- _posts              # 我的日志文章所在
|   `-- template.md
|-- Rakefile
|-- README.md
|-- rss.xml
|-- _site               # Jekyll使用，站点发布的目录,包含在.gitignore文件中
|-- sitemap.txt
|-- index.md
|-- pages.html
|-- 404.html
|-- archive.html
|-- categories.html
`-- tags.html

```

其他tips
----------------------------------------------------------------------------

### 如何输出转义字符？

{% raw %} 
如何让liquid代码不被解析？jekyll + liquid中，如果你需要输出`{% xxx %}`或`{{ xxx }}`时，因为这是liquid的特殊含义字符，所以会被jekyll解析，产生错误。如何正确的输出呢？

答案: 使用 `{% raw %}` `{ % endraw %}` (去掉`{`和`%`之间的空格，这个endraw我也没办法输出。。。)
{% endraw %}

参考：

    http://docs.shopify.com/themes/liquid-documentation/tags/theme-tags#raw




