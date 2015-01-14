---
title: 日常使用jekyll blog的Tips
layout: post
category : myblog
tagline: 
tags : [jekyll, myblog]
---
{% include JB/setup %}

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

```shell
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


### Header 3

