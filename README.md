
# README

## Rancher安装
#### 说明
| 项目    | 版本         |  
| :------ | :----------- | 
| OS      | Ubuntu 16.04 |   
| Kernel  | 3.13.0       |   
| Docker  | 17.06.2-ce   | 
| Rancher | 2.0.2        | 


#### Docker安装
```shell
# Docker内核配置，修改网络防止Docker网络与服务器网络冲突
vim daemon.json 
{
  "bip": "192.168.20.1/24"
} 

mkdir /etc/docker 
cp daemon.json /etc/docker
curl -fsSL https://releases.rancher.com/install-docker/17.06.sh -o get-docker.sh

sh get-docker.sh 

# 添加用户到Docker组
usermod -aG docker ${USER}
service docker restart

# 安装docker-compose
cp docker-compose /usr/local/bin/docker-compose
chmod a+x /usr/local/bin/docker-compose

# Docker与Rancher的兼容BUG，修改根路径为共享模式
mount --make-shared /

docker --version
Docker version 17.06.2-ce, build cec0b72

```

#### Rancher安装
```shell
# 创建网络
docker network create -d bridge --subnet 192.168.0.0/24  rancher_network
47a4da5905c35fdf283609cb3d844757ee6df71f79e0867f68a0707d4e9734ce

# 启动Rancher容器
docker run -d --restart=unless-stopped -p 80:80 -p 443:443 -p 8080:8080 rancher/rancher:v2.0.2

# 通过web访问 
## 首次登陆设置密码为123456 
## 默认用户名admin
http://172.17.20.35/

```

#### Gitlab安装
```shell
docker run --detach \
--hostname gitlab.ld.com \
--publish 433:443 --publish 80:80 --publish 8022:22 \
--name gitlab \
--restart always \
--volume /srv/gitlab/config:/etc/gitlab \
--volume /srv/gitlab/logs:/var/log/gitlab \
--volume /srv/gitlab/data:/var/opt/gitlab \
gitlab/gitlab-ce:latest
```