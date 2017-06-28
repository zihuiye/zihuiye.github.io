---
layout: post
title:  "在ubuntu上用nginx和gunicorn部署Django"
date:   2017-06-27 23:24:00 +0800
categories: Django ubnutu nginx gunicorn
---

## 在ubuntu上用nginx和gunicorn部署Django

想做这个真是进了很多坑，但是要做部署真是的是绕不过去的，所以硬着头皮就上了。结果还是能跑起来就写个文档记录一下。

### 部署环境

- 首先安装的是nginx，因为这个最简单，网上下载源码，编译安装就完成了。  
安装完成之后就是对nginx进行配置。我的配置是：

``` 
server_name musicmap.zihuiye.com;

	location /static {
		alias /home/zihuiye/django-project/musicmap/static;
	}

	location /media {
                alias /home/zihuiye/django-project/musicmap/media;
        }


	location / {

		proxy_pass http://127.0.0.1:8080;
		proxy_set_header Host $host;
		proxy_set_header X-Fowarded-For %proxy_add_x_forward_for;
		proxy_set_header X-Real-IP $remote_addr;
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		#try_files $uri $uri/ =404;
	}
```
