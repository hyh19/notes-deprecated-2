[TOC]

# 第11章　Nginx反向代理与负载均衡应用实践

## 11.8　Nginx负载均衡配置实战

### 11.8.1　配置基于域名虚拟主机的Web节点

主机 web01/web02 的配置 `www.conf`
```nginx
server {
    listen 80;
    server_name www.etiantian.org;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
    }
}
```

主机 web01/web02 的配置 `bbs.conf`
```nginx
server {
    listen 80;
    server_name bbs.etiantian.org;
    location / {
        root /usr/share/nginx/html/bbs;
        index index.html index.htm;
    }
}
```

### 11.8.2　Nginx负载均衡反向代理实践

#### 1.小试牛刀实现为www服务代理
#### 2.反向代理多虚拟主机节点服务器企业案例

主机 lb01 的配置 `www+bbs.conf`
```nginx
upstream backend {
    server 128.199.147.250:80 weight=1;
    server 128.199.169.249:80 weight=1;
}

server {
    listen 80;
    server_name www.etiantian.org bbs.etiantian.org;
    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
    }
}
```

配置要点：
- 定义服务器池 `upstream`
- 转发请求到服务器池 `proxy_pass http://backend`
- 请求头带上 `proxy_set_header Host $host`

#### 3.经过反向代理后的节点服务器记录用户IP企业案例

主机 lb01 的配置 `www+bbs.conf`
```nginx
upstream backend {
    server 128.199.232.95:80 weight=1;
    server 128.199.149.40:80 weight=1;
}

server {
    listen 80;
    server_name www.etiantian.org bbs.etiantian.org;
    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
    }
}
```

配置要点：
- 请求头带上 `proxy_set_header X-Forwarded-For $remote_addr`
- 日志格式带上 `$http_x_forwarded_for`

#### 4.与反向代理配置相关的更多参数说明

重构上一小节代码，把代理配置的参数放到一个文件中。

`/etc/nginx/conf.d/proxy.conf`
```nginx
proxy_set_header Host $host;
proxy_set_header X-Forwarded-For $remote_addr;
proxy_connect_timeout 60;
proxy_send_timeout 60;
proxy_read_timeout 60;
proxy_buffer_size 4k;
proxy_buffers 4 32k;
proxy_busy_buffers_size 64k;
proxy_temp_file_write_size 64k;
```

主机 lb01 的配置 `www+bbs.conf`
```nginx
upstream backend {
    server 128.199.232.95:80 weight=1;
    server 128.199.149.40:80 weight=1;
}

server {
    listen 80;
    server_name www.etiantian.org bbs.etiantian.org;
    location / {
        proxy_pass http://backend;
        include /etc/nginx/conf.d/proxy.conf;
    }
}
```

### 11.8.3　根据URL中的目录地址实现代理转发

主机 web01/web02/web03 的配置 `www.conf`
```nginx
server {
    listen 80;
    server_name www.etiantian.org;
    location / {
        root /usr/share/nginx/html/www;
        index index.html index.htm;
    }
}
```

主机 lb01 的配置 `www.conf`

**方案一：用 `location` 指令**
```nginx
upstream static_pools {
    server 128.199.232.95:80 weight=1;
}

upstream upload_pools {
    server 128.199.149.40:80 weight=1;
}

upstream default_pools {
    server 128.199.81.167:80 weight=1;
}

server {
    listen 80;
    server_name www.etiantian.org;

    location /static/ {
        proxy_pass http://static_pools;
        include /etc/nginx/conf.d/proxy.conf;
    }

    location /upload/ {
        proxy_pass http://upload_pools;
        include /etc/nginx/conf.d/proxy.conf;
    }

    location / {
        proxy_pass http://default_pools;
        include /etc/nginx/conf.d/proxy.conf;
    }
}
```

**方案二：用 `if` 语句**
```nginx
upstream static_pools {
    server 128.199.232.95:80 weight=1;
}

upstream upload_pools {
    server 128.199.149.40:80 weight=1;
}

upstream default_pools {
    server 128.199.81.167:80 weight=1;
}

server {
    listen 80;
    server_name www.etiantian.org;

    location / {

        if ($request_uri ~* "^/static/(.*)$") {
            proxy_pass http://static_pools;
        }

        if ($request_uri ~* "^/upload/(.*)$") {
            proxy_pass http://upload_pools;
        }

        proxy_pass http://default_pools;
        include /etc/nginx/conf.d/proxy.conf;
    }
}
```

> 根据HTTP的URL进行转发的应用情况，被称为第7层（应用层）的负载均衡，而LVS的负载均衡一般用于TCP等的转发，因此被称为第4层（传输层）的负载均衡。

### 11.8.4　根据客户端的设备（user_agent）转发实践

主机 web01/web02/web03 的配置同上一小节。

主机 lb01 的配置 `www.conf`
```nginx
upstream web01 {
    server 128.199.232.95:80 weight=1;
}

upstream web02 {
    server 128.199.149.40:80 weight=1;
}

upstream web03 {
    server 128.199.81.167:80 weight=1;
}

server {
    listen 80;
    server_name www.etiantian.org;

    location / {

        if ($http_user_agent ~* "Firefox") {
            proxy_pass http://web01;
        }

        if ($http_user_agent ~* "Chrome") {
            proxy_pass http://web02;
        }

        proxy_pass http://web03;
        include /etc/nginx/conf.d/proxy.conf;
    }
}
```

### 11.8.5　根据文件扩展名实现代理转发

跳过

## 11.9　Nginx负载均衡监测节点状态

跳过

## 11.10　proxy_next_upstream参数补充

跳过

学习结束