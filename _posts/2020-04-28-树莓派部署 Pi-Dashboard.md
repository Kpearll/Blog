---
layout:     post
title:      树莓派部署 Pi-Dashboard
subtitle:   
date:       2020-04-28
author:     Kpearl
header-img: img/raspi-bg1.jpg
catalog: true
tags:
    - Raspberry Pi
---
### 部署 Pi Dashboard(仪表盘)
运行环境需要安装Nginx和PHP
#### 安装 Nginx 和 PHP7
```
#安装软件包
sudo apt-get update
sudo apt-get install nginx php7.3-fpm php7.3-cli php7.3-curl php7.3-gd php7.3-cgi
sudo service nginx start
sudo service php7.3-fpm restart
```
如果安装成功，可通过 ```http://树莓派IP/``` 访问到 Nginx 的默认页进行测试。Nginx 的根目录在 /var/www/html。
#### 修改 Nginx 配置
使 Nginx 能处理 PHP
```
sudo vim /etc/nginx/sites-available/default
```
修改文件内容：
```
location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
```
替换为：
```
location / {
	index  index.html index.htm index.php default.html default.htm default.php;
}
 
location ~\.php$ {
	fastcgi_pass unix:/run/php/php7.3-fpm.sock;
	#fastcgi_pass 127.0.0.1:9000;
	fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	include fastcgi_params;
}
```

```
#重启nginx服务
sudo service nginx restart
```

#### 部署 Pi Dashboard
##### [Pi Dashboard 介绍](http://make.quwj.com/project/10) 
```
#GitHub部署
cd /var/www/html
sudo git clone https://github.com/spoonysonny/pi-dashboard.git
```

### 访问 Pi Dashboard
现在可以通过 ```http://IP地址/pi-dashboard``` 访问部署好了的 Pi Dashboard。