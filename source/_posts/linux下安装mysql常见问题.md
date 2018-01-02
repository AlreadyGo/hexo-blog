---
title: linux下安装mysql常见问题
date: 2017-01-21 01:07:17
tags: [mysql,linux]
---
1.无法远程连接mysql  1130

GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;

2.服务端中文乱码

[mysqld]

character_set_server=utf8

service mysql restart

此时,
<pre>mysql -u root -p</pre>
status

会有类似的结果:Server characterset: utf8