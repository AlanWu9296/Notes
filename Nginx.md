# Template

```nginx
server {
  server_name your.domain.com;
  listen 80;

  location / {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_set_header X-NginX-Proxy true;
    proxy_pass http://127.0.0.1:3000;
    proxy_redirect off;
  }
}
```

# 访问控制 allow/deny

```nginx
location /nginx-status {
  stub_status on;
  access_log off;
#  auth_basic   "NginxStatus";
#  auth_basic_user_file   /usr/local/nginx-1.6/htpasswd;

  allow 192.168.10.100;
  allow 172.29.73.0/24;
  deny all;
}
```

from top to bottom, if any rule is matched then it stops

httpd-devel 工具的 htpasswd 来为访问的路径设置登录密码：

```nginx
# htpasswd -c htpasswd admin
New passwd:
Re-type new password:
Adding password for user admin

# htpasswd htpasswd admin    //修改admin密码
# htpasswd htpasswd sean    //多添加一个认证用户
```

# nginx.conf配置文件

Nginx配置文件主要分成四部分：**main（全局设置）**、**server（主机设置）**、**upstream（上游服务器设置，主要为反向代理、负载均衡相关配置）**和 **location（URL匹配特定位置后的设置）**，每部分包含若干个指令。main部分设置的指令将影响其它所有部分的设置；server部分的指令主要用于指定虚拟主机域名、IP和端口；upstream的指令用于设置一系列的后端服务器，设置反向代理及后端服务器的负载均衡；location部分用于匹配网页位置（比如，根目录“/”,“/images”,等等）。他们之间的关系式：server继承main，location继承server；upstream既不会继承指令也不会被继承。它有自己的特殊指令，不需要在其他地方的应用。