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

屏蔽单个IP的命令是 deny 123.45.6.7 #封整个段即从123.0.0.1到123.255.255.254的命令 deny 123.0.0.0/8 #封IP段即从123.45.0.1到123.45.255.254的命令 deny 124.45.0.0/16 #封IP段即从123.45.6.1到123.45.6.254的命令是 deny 123.45.6.0/24

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

# nginx 缓存配置

```nginx
http{  
    ......  
    proxy_cache_path /data/nginx/tmp-test levels=1:2 keys_zone=tmp-test:100m inactive=7d max_size=1000g;  
}
```

 proxy_cache_path 缓存文件路径

 levels 设置缓存文件目录层次；levels=1:2 表示两级目录

 keys_zone 设置缓存名字和共享内存大小

 inactive 在指定时间内没人访问则被删除

max_size 最大缓存空间，如果缓存空间满，默认覆盖掉缓存时间最长的资源

```nginx
  proxy_cache tmp-test;  
  proxy_cache_valid  200 206 304 301 302 10d;  
  proxy_cache_key $uri; 
```

 Proxy_cache  tmp-test  使用名为tmp-test的对应缓存配置

 proxy_cache_valid  200 206 304 301 302 10d; 对httpcode为200…的缓存10天

 proxy_cache_key  $uri  定义缓存唯一key,通过唯一key来进行hash存取

# 限制客户端的访问频次和访问次数

**NginxHttpLimitConnModule**，可以根据设定的条件来限定客户端（单一ip）的**并发访问**，但是并不是所有的访问都会被计数，只有那些正在被处理的的请求（这些请求的头信息已

经被完全读入），所在的访问才会被计数。

**NginxHttpLimitReqModule**，可以根据设定的条件来限定客户端（单一ip）的访问频率

**1.NginxHttpLimitConnModule**

```nginx
http{

limit_conn_zone $binary_remote_addr  zone=one:10m;

server {

limit_conn test 1;

location =/1.html{

root html;

}

}

}
```

（1）limit_conn_zone $binary_remote_addr  zone=one:10m;

第一个参数$binary_remote_addr ：表示以客户端ip作为键值来进行限制

第二个参数zone=one:10m：表示生成一个大小为10M，名字为one的存储区域，用来存储访问次数

（2）limit_conn test 1;

表示在test存储区内，限制客户端ip只能访问一次，若超过访问限制，则返回503错误。

**2.NginxHttpLimitReqModule**

```nginx
http{

limit_req_zone  $binary_remote_addr  zone=two:10m   rate=5r/s;

server {

limit_req zone=two burst=5 nodelay;

location =/1.html{

root html;

}

}

}

```

limit_req_zone  $binary_remote_addr  zone=two:10m   rate=5r/s;

第一个参数$binary_remote_addr：表示以客户端ip作为键值来进行限制

第二个参数zone=two:10m ：表示生成一个大小为10M，名字为two的存储区域，用来存储访问频率

第三个参数 rate=5r/s：表示限定客户端的访问频率为每秒5次

（2）limit_req zone=two burst=5 nodelay;

第一个参数zone=two：表示使用存储区域two来限制

第二个参数burst=5：表示设定一个缓存区域，当有大量请求时，超过了访问频次限制的请求会放在这个缓冲区域内

第三个参数nodelay：表示当超过访问次数并缓冲也满的情况下，直接放回503错误，若不设置，这些多余的请求会延迟处理