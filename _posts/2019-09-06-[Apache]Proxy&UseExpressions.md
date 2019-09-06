---
layout: post
title: 【Apache】Proxy & Use Expressions To Restricted Access
date: 2019-09-06 20:30
categories: kubenetes minikube
---

# Goal
1. 80 port proxy to 8080 port. If work, will show 'proxy'
2. `http://localhost:8080` can't access, must return 404.
	- Use `<If "%{SERVER_PORT} != '80'"> Redirect 404 / </If>`

# Repo & Env Set
> source code : `https://github.com/jhaoheng/apacheProxy&UseExpressions`

1. `docker-compose up -d` && `docker exec -it ubuntu /bin/bash`
2. `apt-get update`
3. `apt-get install -y libapache2-mod-proxy-html libxml2-dev apache2 build-essential`
4. Proxy Modules
    - `a2enmod proxy proxy_http proxy_ajp rewrite proxy_balancer proxy_connect proxy_html xml2enc`

<!--more-->

# Steps
> ref `./default.conf` & `ports.conf`

## step1 : enable port 8080

1. update `ports.conf` in the '/etc/apache2/ports'
2. `Listen 8080`

## step2 : set VirtualHost

- `echo "proxy" > /var/www/proxy/index.html`
- set VirtualHost
```
<VirtualHost *:8080>
        ServerName localhost
        DocumentRoot /var/www/proxy/
</VirtualHost>
```

## step3 : set proxy

```
# Proxy
ProxyRequests Off
ProxyPass / http://localhost:8080
ProxyPassReverse / http://localhost:8080
ProxyPreserveHost On
```

## step4 : restricted direct connect localhost:8080

> Write below into `<VirtualHost *:8080> ... </VirtualHost>`

```
AllowCONNECT 80
<If "%{SERVER_PORT} != '80'">
        Redirect 404 /
</If>
```

- Expressions in Apache HTTP Server
	- https://httpd.apache.org/docs/current/expr.html

## step5 : restart & check it
- `apache2ctl restart`
- `http://localhost:80`, will show 'proxy'
- `http://localhost:8080`, be restricted to access
