[https://blog.csdn.net/czj981014/article/details/116766286](https://blog.csdn.net/czj981014/article/details/116766286)
[https://www.w3cschool.cn/docker/docker-install-nginx.html](https://www.w3cschool.cn/docker/docker-install-nginx.html)
# 安装
1. 下载docker-ce的repo
curl https://download.docker.com/linux/centos/docker-ce.repo -o /etc/yum.repos.d/docker-ce.repo
2. 安装依赖
yum install https://download.docker.com/linux/fedora/30/x86_64/stable/Packages/containerd.io-1.2.6-3.3.fc30.x86_64.rpm
3. 安装docker-ce
dnf -y install docker-ce docker-ce-cli --nobest
4.启动 docker
systemctl start docker
5.查看 docker 版本信息
docker -v
6.设置开机自动启动
systemctl enable --now docker



仓库

# 镜像
docker search rabbitmq
docker pull redis:latest
docker images
docker rmi redis:latest

# 容器
docker run -d redis 其中-d 表示在后台运行
docker stop 容器id

docker ps
docker ps -a
docker rm 容器id 
docker exec -it 容器id bash

https://www.w3cschool.cn/docker/docker-image-usage.html

Nginx
docker run -p 80:80 --name nginx01 -d nginx
docker run -p 80:80 --name nginx01 -v /usr/local/nginx:/etc/nginx -d nginx
docker run -p 80:80 --name nginx01 -v /usr/local/nginx:/etc/nginx -d --privileged=true nginx

docker run -p 80:80 --name nginx01 -v /usr/local/nginx/www:/www -v /usr/local/nginx/nginx.conf:/etc/nginx/nginx.conf -v /usr/local/nginx/logs:/wwwlogs  -d nginx
