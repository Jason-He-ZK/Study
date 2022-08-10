无法访问

```
再配置下防火墙
firewall-cmd --zone=public --add-port=80/tcp --permanent
重启防火墙
systemctl restart firewalld.service
```

启动，关闭，重启

```
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

kill -term 主pid

./nginx -s reload
```

反向代理：接受请求，转发个服务器
正向代理：vpn

核心配置文件：nginx.conf

基本配置：用户，进程数，pid，日志
events 配置：工作模式，连接数（每个进程数）
http 配置：防止网络阻塞，gzip 压缩输出，长连接超时时间，虚拟主机（端口和域名不能完全相同）

```
#配置虚拟主机
server {
	listen       80;  #配置监听端口
	server_name  localhost;  #配置服务名
	#charset koi8-r;  #配置字符集
	#access_log  logs/host.access.log  main;  #配置本虚拟主机的访问日志

	#默认的匹配斜杠/的请求，当访问路径中有斜杠/，会被该location匹配到并进行处理
	location / {
	        #root是配置服务器的默认网站根目录位置，默认为nginx安装主目录下的html目录
		root   html;  
	        #配置首页文件的名称
		index  index.html index.htm;  
	}
	#error_page  404              /404.html;  #配置404页面
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
```

### 主要功能

##### 静态网站部署

```
http://192.168.92.128:80/ = root = /opt/static/ace
http://192.168.92.128:80/ace = root/ace = /opt/static/ace/ace
```

##### 负载均衡

```
upstream www.myweb.com { 轮询
    	server 127.0.0.1:8080; 
    	server 127.0.0.1:9090; 
} 
upstream www.myweb.com { 权重
     	server 127.0.0.1:9100 weight=3; 
      	server 127.0.0.1:9200 weight=1;  
}
upstream www.myweb.com { 哈希
    	ip_hash; 
    	server 127.0.0.1:8080; 
    	server 127.0.0.1:9090; 
}
upstream www.myweb.com { 最小连接
    	least_conn;
    	server 127.0.0.1:8080; 
    	server 127.0.0.1:9090; 
}
upstream www.myweb.com { 
    	server 127.0.0.1:9100;
	#其它所有的非backup机器down的时候，才请求backup机器
    	server 127.0.0.1:9200 backup; 
        #down表示当前的server是down状态，不参与负载均衡
        server 127.0.0.1:9300 down;
}
```

```
upstream www.myweb.com { #指定服务器地址
     	server  127.0.0.1:9100;
      	server  127.0.0.1:9200;
}

server {
        listen       80;  #配置监听端口
        server_name  localhost;  #配置服务名
        location /myweb {
	        proxy_pass http://www.myweb.com;
        }
}
```

##### 静态代理

将静态资源放到 Nginx 中

```
server {
        listen       9200;  #配置监听端口
        server_name  localhost;  #配置服务名
        location ~ .*/(css|js|img|images) {
    	        root   /opt/static;
        }
}
```

##### 动静分离

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038502093-6875bd01-b688-4200-bb88-98beb13f99ae.png#clientId=udcdbd9cb-107f-4&from=paste&height=177&id=u52c287eb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=354&originWidth=595&originalType=binary&ratio=1&size=45401&status=done&style=none&taskId=u126731a0-34d4-483a-808f-2207cc0c9af&width=297.5)；

```
upstream www.myweb.com { 
    server  127.0.0.1:9100 weight=5; 
    server  127.0.0.1:9200 weight=2;  
}
upstream static.myweb.com { 
    server  127.0.0.1:81 weight=1; 
    server  127.0.0.1:82 weight=1;  
}
server {
        listen       9200;  #配置监听端口
        server_name  localhost;  #配置服务名
        location /myweb {
        	proxy_pass http://www.myweb.com;
        }
        location ~ .*/(css|js|img|images) {
    	        proxy_pass http://static.myweb.com;
        }
}
```

##### 虚拟主机

一台机器绑定多个 ip（域名 + 端口）

```
server {
	listen       80;
	server_name  beijing.myweb.com;
	location / {
		proxy_pass http://beijing.myweb.com;
	}
}
server {
	listen       80;
	server_name  nanjing.myweb.com;
	location / {
		proxy_pass http://nanjing.myweb.com;
	}
}
server {
	listen       80;
	server_name  tianjin.myweb.com;
	location / {
		proxy_pass http://tianjin.myweb.com;
	}
}
```

```
修改hosts文件
192.168.208.128 beijing.myweb.com
192.168.208.128 nanjing.myweb.com
192.168.208.128 tianjin.myweb.com
```
