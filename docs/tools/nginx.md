# Nginx

- 反向代理

- 负载均衡
- 动静分离

## 主要应用

- 静态网站部署
- 负载均衡
- 静态代理
- 动静分离
- 虚拟主机

## 基本概念

### 正向代理和反向代理

- 反向代理隐藏真正的服务器，不处理用户请求，只是将请求转发给后台服务器进行处理
  - 如拨打客服总机，会转到其他各个客户后进行沟通
- 正向代理隐藏真正的客户端，代理客户端去访问服务器，请求并返回结果给客户端
  - VPN
  - 跳板
- 正向代理代理对象是客户端，反向代理代理对象是服务端

## Nginx目录结构

- `conf`：存放nginx配置
- `html`：存放默认解析的静态文件目录
- `logs`：存放日志文件
- `sbin`：nginx可执行文件目录

## Nginx常用命令

```sh
cd /usr/local/nginx/sbin/

# 普通启动
./nginx
# 指定配置文件启动 -c是指定配置文件，必须为绝对路径
./nginx -c /usr/local/conf/nginx.conf

# 停止
./nginx -s stop
# 安全退出
./nginx -s quit
# 重新加载配置文件
./nginx -s reload
# 查看nginx进程
ps aux|grep nginx
```

## Nginx配置

### 基础配置

```conf
# 配置worker进程运行用户 nobody也是一个linux用户，一般用于启动程序，没有密码
# user	nobody;

# 配置worker进程的数目，根据硬件调整，通常等于CPU数量或者CPU数量的2倍
worker_processes	6;

# 配置全局错误日志及类型，[debug | info | notice | warn | error | crit]，默认是error
error_log 	logs/error.log;  
#error_log	logs/error.log  notice;
#error_log	logs/error.log  info;

# 配置进程pid文件
pid		logs/nginx.pid;  

# 配置工作模式和连接数
events {
	# 配置每个worker进程连接数上限，nginx支持的总连接数就等于worker_processes * worker_connections
    worker_connections	1024;  
}

# 配置http服务器,利用它的反向代理功能提供负载均衡支持
http {
    # 配置nginx支持哪些多媒体类型，可以在conf/mime.types查看支持哪些多媒体类型
	include       mime.types;  
    # 默认文件类型 流类型，可以理解为支持任意类型
    default_type  application/octet-stream;  
    # 配置日志格式 
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    # 配置access.log日志及存放路径，并使用上面定义的main日志格式
    # access_log  logs/access.log  main;

	# 开启高效文件传输模式
    sendfile        on;
    # 防止网络阻塞
    # tcp_nopush     on;  

    # keepalive_timeout  0;
    # 长连接超时时间，单位是秒
    keepalive_timeout  65;  

	# 开启gzip压缩输出，能够提高响应速度
    # gzip  on;  

    # 配置虚拟主机，可以有多个server，但每个server的listen和server_name不能同时一样
    server {
    	# 配置监听端口
        listen       80;  
        # 配置服务名
        server_name  localhost;  
		# 配置字符集 默认utf-8
        #charset koi8-r;
		# 配置本虚拟主机的访问日志
        #access_log  logs/host.access.log  main;  

		#默认的匹配斜杠/的请求，当访问路径中有斜杠/，会被该location匹配到并进行处理
        location / {
	    	# root是配置服务器的默认网站根目录位置（本地磁盘根路径），默认为nginx安装主目录下的html目录
            root   html;  
	    	#配置首页文件的名称
            index  index.html index.htm;  
        }		

		# 配置404页面
        #error_page  404              /404.html;  
        
        # redirect server error pages to the static page /50x.html
        #error_page   500 502 503 504  /50x.html;  #配置50x错误页面
        
        #精确匹配
        location = /50x.html {
            root   html;
        }

		#PHP 脚本请求全部转发到Apache处理
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

		#PHP 脚本请求全部转发到FastCGI处理
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

		#禁止访问 .htaccess 文件
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }

	
	#配置另一个虚拟主机
    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

	
	#配置https服务，安全的网络传输协议，加密传输，端口443，运维来配置
	#
    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
}
```



