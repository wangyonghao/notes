## 如何配置一个高效的 Mac 工作环境

## 准备

**这里先列举一下微服务开发需要安装哪些软件：**

- JDK - Java8
- IDE - Intellij Idea
- 构建工具 - Gradle & Nexus
- 数据库 - Mysql & Navicat
- 版本管理 - Git & GitLab
- 服务容器 - Docker & Registry
- 负载均衡/反向代理工具 - Nginx
- 分布式缓存 - Redis

为减少服务型软件（需要服务启动的软件，比如Nexus、mysql、Redis）的资源占用，这里使用虚拟机运行这些软件。在日常使用Mac时，关闭虚拟机即可。

**最终的软件结构树：**

- 宿主机
  - Java 8
  - Git
  - Gradle
  - Virtualbox
    - Centos
      - Docker
        - Nexus (同时作为Maven私服 和 Docker registry)
        - Mysql
        - Redis
        - Nginx
        - Jenkins 
        - Gitlab（可选，也可以使用GitHub/Gitee）
  - Intellij IDEA

## 安装

工欲善其事，必先利其器。先安装一下**Zsh**和包管理工具**Homebrew**。

[Mac 小记 — iTerm2、Zsh、Homebrew](https://www.cnblogs.com/youclk/p/8125305.html)

### Homebrew

```bash
# 安装Homebrew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

# 阿里云镜像源，解决软件下载速度慢的问题
cd "$(brew --repo)"
git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git

# 清华大学镜像源
cd "$(brew --repo)"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

# 卸载Homebrew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)“

# 还原官方镜像源
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```

### Java8
Mac OSX自带有Java环境，此步可跳过。可在终端中使用`java -version` 查看。
### Git
Mac OSX自带有Git环境，此步可跳过。可在终端中使用`git --version` 查看。
### Gradle 

#### 安装

```bash
brew install gradle
```

#### 优化配置

##### 使用[gdub](https://github.com/dougborg/gdub)简化gradlew调用

```bash
brew install gdub
echo "alias gradle=gw" >> ~/.zshrc
source ~/.zshrc
```


### Virtualbox

#### 安装

```
brew install virtualbox
```

### CentOS

#### 下载镜像

使用[国内镜像](<https://blog.csdn.net/weixin_42430824/article/details/81019039>)下载速度较快，点击这里下载[阿里云CentOS镜像](<https://blog.csdn.net/weixin_42430824/article/details/81019039>)

#### 安装

如果安装镜像，可参考： [在VirtualBox中安装CentOS7详解(Mac版)](<https://blog.csdn.net/ytangdigl/article/details/79736562>)

#### 后台运行虚拟机

```bash
vboxmanage startvm centos --type headless
```

[参考](<https://www.jianshu.com/p/a4c9ec791948>)

#### 配置端口转发，以便使用Mac终端直接访问虚拟机

1. 使用`ip addr`查看网卡，这里是`enp0s3`。

2. 编辑`vi /etc/sysconfig/network-scripts/ifcfg-enp0s3`，修改`ONBOOT=YES`

3. 在虚拟机`Settings - Network - Advanced - Port Forwarding` 设置端口转发:

   `name=ssh, Protocol=TCP, HostPort=22, GuestPort=22`

4. 在Mac终端，使用 `ssh` 登录虚拟机：

  ```bash
  ssh username@localhost
  ```

5. 消除警告信息`-bash:warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory`

   修改`/etc/locale.conf` 为以下内容：

   ```
   LC_ALL=en_US.utf8
   LC_CTYPE=en_US.utf8
   LANG=en_US.utf8
   ```

#### 配置yum源

```
yum install -y wget
cd /etc/yum.repos.d/
mv CentOS-Base.repo CentOS-Base.repo.back
wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo


yum clean all
yum makecache
yum update
```

## Docker

以下命令是在虚拟机中执行的，请先使用`ssh username@localhost`登录虚拟机。

### 安装Docker

```bash
# yum-util提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
yum install -y yum-utils device-mapper-persistent-data lvm2
# 设置阿里云源，
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 设置docker源
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 安装docker-ce
yum install -y docker-ce
# 启动docker
systemctl start docker
# 将docker添加为服务
systemctl enable docker
# 验证安装是否成功(有client和service两部分表示docker安装启动都成功了)
docker version
```

#### [安装docker-compose](<https://github.com/docker/compose/releases>)

```bash
curl -L https://github.com/docker/compose/releases/download/1.24.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

# 验证版本
docker-compose -version
```

#### 配置国内源

```bash
# 使用Docker国内镜像源加速
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://okry75km.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
# 验证
docker info
```

[Docker常用命令](<http://www.docker.org.cn/dockerppt/106.html>)

### 在Docker中安装Nexus3

```bash
#使用 Docker volume 持久化 Nexus 数据
docker volume create nexus-data
# 后台启动 Nexus，
# 参数说明：
#   --restart=always 宕机自动重启
#   -v nexus-data:/nexus-data 挂载卷
docker run -d --name nexus -p 8081:8081 --restart=always -v nexus-data:/nexus-data sonatype/nexus3
```

这里解释一下`docker run`参数的意义：

- `-d` 后台运行镜像
- `--name nexus` 给容器起一个别名
-  `-p 8081:8081`  将镜像的8080端口映射到Docker宿主机的80端口
-  `--restart=always` Docker启动或容器宕机时，自动重启容器
-  `-v nexus-data:/nexus-data `  挂载Docker卷

### 在Docker中安装Jenkins

```bash
docker volume create jenkins-data
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 --restart=always -v jenkins-data:/var/jenkins_home jenkins/jenkins
# 验证
curl localhost:8080
# 初始访问时需要输入密码，下面的命令可以查看Jenkins初始密码
docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

#### 配置加速器

【系统管理】-> 【插件管理】-> 【高级】-> 【升级站点】

 更换升级站点：`http://mirror.xmission.com/jenkins/updates/current/update-center.json`

参考:  [Docker版本Jenkins的使用](<https://www.jianshu.com/p/0391e225e4a6>)

### 在Docker中安装MySQL

```bash
docker volume create mysql-data
docker run -d --name mysql -p 3306:3306 -p 33060:33060 --restart=always -v mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 mysql

# 登录到MySQL
docker exec -it mysql mysql -uroot
```
#### 在 Docker中安装Redis

```
docker volume create redis-data
docker run --name redis -p 6379:6379 --restart=always -v redis-data:/data -d redis redis-server --appendonly yes 
# 登录到 redis
docker run -it --rm redis redis-cli -h redis

```

### 在 Docker中安装ActiveMQ

```
docker volume create activemq-data
docker run -d --name activemq \
					 -p 61616:61616 -p 8161:8161 \
					 --restart=always \
					 -v activemq-data:/opt/activemq/conf \
					 -v activemq-data:/opt/activemq/data \
					 webcenter/activemq
# 登录到 activemq
docker run -it --rm redis redis-cli -h redis



iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 8161 -j ACCEPT
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 61616 -j ACCEPT
```

### 安装Oracle

```shell
docker pull deepdiver/docker-oracle-xe-11g
docker run -d -p 1520:22 -p 1521:1521 deepdiver/oracle-xe-11g oracle11g

docker run -d -p 1520:22 -p 1521:1521 396b3e06a5dc --name oracle11g


# connect info
hostname: localhost
port: 49161
sid: xe
username: system
password: oracle

# Password for SYS & SYSTEM 
oracle
# Login by SSH
ssh root@localhost -p 49160
password: admin
# Connect to ownCloud CI database
username: autotest
password: owncloud
```



### Docker Registry

```
docker run -d -p 5000:5000 -v /root/docker/registry:/var/lib/registry -v /root/certs/:/root/certs  -e REGISTRY_HTTP_TLS_CERTIFICATE=/root/cer
ts/domain.crt -e REGISTRY_HTTP_TLS_KEY=/root/certs/domain.key registry
```

[那么如何登录到docker虚拟机中呢？](<https://www.jianshu.com/p/8c22cdfc0ffd>)





### IDEA

```
brew install intellij-idea-ce
```

#### 安装插件

- Lombox

- FindBugs-IDEA

- **Mybatis Log Plugin** 

  MyBatis Log Plugin 这款插件是直接将Mybatis执行的sql脚本显示出来，无需处理，可以直接复制出来执行的 。`Tools -- >  Mybatis Log Plugin` 打开其日志框，注意其转换的SQL不是输出到IDE的控制台!!!

- 

  

[强迫症的 Mac 设置指南](https://github.com/macdao/ocds-guide-to-setting-up-mac)

# 