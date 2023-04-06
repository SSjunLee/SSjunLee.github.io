---
title: Hexo 博客搭建

categories: 
- js

tags:
- Hexo
---

## 安装配置Hexo


``` bash

npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server

```

此时可以把基本环境跑起来，有两个重要的目录，source目录存放博客页面，source/_posts目录是博客的内容，博客基本都是md文档。还有重要的站点配置文件 __config.yaml。


## 安装next主题

先在github上把主题下载下来放到themes 目录，把名字改成next，在站点配置文件中修改。

``` yaml

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
# theme: landscape
theme: next

```

### 修改布局

在主题配置文件_config.yaml中修改布局

``` yaml


# ---------------------------------------------------------------
# Scheme Settings
# ---------------------------------------------------------------

# Schemes
#scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini

```


但目前很多页面是没法点击的，需要创建这些页面。

``` yaml 

# Usage: `Key: /link/ || icon`
# Key is the name of menu item. If the translation for this item is available, the translated text will be loaded, otherwise the Key name will be used. Key is case-senstive.
# Value before `||` delimiter is the target link, value after `||` delimiter is the name of Font Awesome icon.
# When running the site in a subdirectory (e.g. yoursite.com/blog), remove the leading slash from link value (/archives -> archives).
# External url should start with http:// or https://
menu:
  home: / || fa fa-home
  about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat

# Enable / Disable menu icons / item badges.
menu_settings:
  icons: true # 是否显示各个页面的图标
  badges: true # 是否显示分类/标签/归档页的内容量

```


主题的配置文件中有这些页面的路由,我们配置上，然后运行

```bash
hexo new page "名字"
```
分别将tags，categories，archives，about创建出来,并设置上配置信息,如分类页面 source/categories/index.md

``` 
---
title: 文章分类
date: 2023-04-06 12:01:52
type: "categories"
---

```


### 加入搜索

```bash
npm install hexo-generator-searchdb --save
```
修改站点配置
``` yaml
# Search
search:
    path: search.xml # 支持json和xml，可自行设定，索引文件的路径，相对于站点根目录
    field: post # 搜索范围，默认是 post，还可以选择 page、all，设置成 all 表示搜索所有页面
    format: html
    limit: 10000 
```

修改主题配置

```yaml
# Local Search
# Dependencies: https://github.com/theme-next/hexo-generator-searchdb
local_search:
  enable: true
  # If auto, trigger search by changing input.
  # If manual, trigger search by pressing enter key or search button.
  trigger: auto
  # Show top n results per article, show all results by setting to -1
  top_n_per_article: 1
  # Unescape html strings to the readable one.
  unescape: false
  # Preload the search data when the page loads.
  preload: false
```


### 代码块

主题配置文件

```yaml
codeblock:
  # Code Highlight theme
  # Available values: normal | night | night eighties | night blue | night bright | solarized | solarized dark | galactic
  # See: https://github.com/chriskempson/tomorrow-theme
  highlight_theme: night bright
  # Add copy button on night eighties
  copy_button:
    enable: true
    # Show text copy result.
    show_result: false
    # Available values: default | flat | mac
    style: mac 

```


### 加妹子

```bash
npm install -save hexo-helper-live2d
```

站点配置

``` yaml 
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  log: false
  model:
    use: live2d-widget-model-shizuku
  display:
    position: right
    width: 150
    height: 300
  mobile:
    show: true
```

建立目录 live2d_models/shizuku，在下面建立shizuku.model.json

``` bash 
npm install --save live2d-widget-model-shizuku
```

## 集成gitpages

创建一个github仓库，名字为`名字.github.io` ,并提交代码，下载hexo同步插件。

```bash
npm install hexo-deployer-git --save
```

在站点配置中配置

```yaml
deploy:
  type: git
  repository: 地址
  branch: gh-pages
```


```bash
# 清理缓存
hexo clean

# 生成静态文件
hexo g  

# 部署
hexo deploy
```