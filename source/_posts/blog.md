---
title: 'hexo blog'
date: 2017-05-22 08:21:48
tags: [hexo,blog]
---
### 安装
> $ npm install hexo-cli -g

### 初始化
> $ hexo init blog
  $ cd blog
  
### 下载依赖
> $ npm i
  
### 修改配置文件_config.yml
```
  # Hexo Configuration
  ## Docs: https://hexo.io/docs/configuration.html
  ## Source: https://github.com/hexojs/hexo
    
  # 自定义网站标题,作者,语言(跟主题有关),我这边主题支持zh-Hans简体中文
  title: AlreadyGo
  subtitle:
  description:
  author: Hui Zhou
  language: zh-Hans
  timezone:
  
  ## 自定义url
  url: http://alreadygo.github.io
  root: /
  permalink: :year/:month/:day/:title/
  permalink_defaults:
  
  # Directory source为源码目录,public为生成物目录
  source_dir: source
  public_dir: public
  tag_dir: tags
  archive_dir: archives
  category_dir: categories
  code_dir: downloads/code
  i18n_dir: :lang
  skip_render:
  
  # Writing
  new_post_name: :title.md # File name of new posts
  default_layout: post
  titlecase: false # Transform title into titlecase
  external_link: true # Open external links in new tab
  filename_case: 0
  render_drafts: false
  post_asset_folder: false
  relative_link: false
  future: true
  highlight:
    enable: true
    line_number: true
    auto_detect: false
    tab_replace:
    
  # Home page setting
  # path: Root path for your blogs index page. (default = '')
  # per_page: Posts displayed per page. (0 = disable pagination)
  # order_by: Posts order. (Order by date descending by default)
  index_generator:
    path: ''
    per_page: 10
    order_by: -date
    
  # Category & Tag
  default_category: uncategorized
  category_map:
  tag_map:
  
  # Date / Time format
  ## Hexo uses Moment.js to parse and display date
  ## You can customize the date format as defined in
  ## http://momentjs.com/docs/#/displaying/format/
  date_format: YYYY-MM-DD
  time_format: HH:mm:ss
  
  # Pagination
  ## Set per_page to 0 to disable pagination
  per_page: 10
  pagination_dir: page
  
  # Extensions
  ## Plugins: https://hexo.io/plugins/
  ## https://github.com/iissnan/hexo-theme-next
  theme: next
  
  # Deployment
  ## Docs: https://hexo.io/docs/deployment.html
  ### npm i hexo-deployer-git --save
  #### 修改自定义部署git仓库及分支
  deploy:
    type: git
    repo: https://github.com/AlreadyGo/alreadyGo.github.io.git
    branch: master
```
### 生成static文件
> $ hexo generate

### 删除static文件
> $ hexo clean 

### 在本地运行
> $ hexo server

### 部署到github
> $ hexo deploy

### 常见插件安装
https://hexo.io/plugins/
这边安装了几个:
> $ npm i hexo-deployer-git --save  //git部署
  $ npm i hexo-wordcount --save //post字数统计
  $ npm i hexo-generator-searchd --save //本地搜索
  