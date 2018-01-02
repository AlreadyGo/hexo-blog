---
title: 设置npm代理
date: 2017-01-22 01:07:17
tags: [js,npm]
---
### 配置镜像
#### by config command
 > npm config set registry http://registry.cnpmjs.org
    npm info underscore （如果上面配置正确这个命令会有字符串response）

#### 命令行指定
> npm --registry http://registry.cnpmjs.org info underscore

#### 编辑 ~/.npmrc 加入下面内容
> registry = http://registry.cnpmjs.org

#### 设置代理
> npm config set proxy http://server:port
  npm config set https-proxy http://server:port
  
#### 如果需要认证的话可以这样设置：
>npm config set proxy http://username:password@server:port
npm confit set https-proxy http://username:password@server:port

#### 如果代理不支持https的话需要修改npm存放package的网站地址。
> npm config set registry "http://registry.npmjs.org/"


### 使用nrm快速切换npm源
> nrm 是一个 NPM 源管理器，允许你快速地在如下 NPM 源间切换

#### 列表项目
* npm
* cnpm
* strongloop
* enropean
* australia
* nodejitsu
* taobao

#### Install
> sudo npm install -g nrm

#### 列出可用的源：

 > ~  nrm ls
  npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  taobao - http://registry.npm.taobao.org/
  eu ----- http://registry.npmjs.eu/
  au ----- http://registry.npmjs.org.au/
  sl ----- http://npm.strongloop.com/
  nj ----- https://registry.nodejitsu.com/
  pt ----- http://registry.npmjs.pt/
  
#### 切换：
>~  nrm use taobao
   Registry has been set to: http://registry.npm.taobao.org/
   
#### 增加源：
> nrm add <registry> <url> [home]

#### 删除源：
> nrm del <registry>

#### 测试速度：
> nrm test