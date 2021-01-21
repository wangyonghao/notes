# 打造Mac工作环境-开发篇

## 前言

写这篇文章的主旨是：记录在工作中使用Mac时使用高频率的软件的安装及配置过程，逐渐积累打磨出适合自己工作环境。

本人使用笔记本是`MBP 2019`款，操作系统是`maxOS Catalina 10.15.7`，网络环境是中国大陆。

指导原则：

1. **节省成本**
2. **自动化**，安装方式优先顺序：`自动化 > AppStore >安装包`
3. **不重复**
4. **无国界**，部分软件官网在国外。

文章分为日常篇和开发篇两部分：

- **日常篇** - 包含系统增强工具和办公软件。

- **开发篇** - 如其名，与开发相关的软件都在这里。

## 准备

这里列举一下日常篇包含的软件：

- Java -  Java工程师必备
- Intellij Idea -  Java工程师必备
- Maven - 构建工具
- Docker - 虚拟容器
- Postman - API测试工具
- StarUML - UML工具
- Dash - 文档API查询工具

```shell
# 版本管理
brew install git
# 项目管理工具
brew install maven
# Java运行环境
brew install java11
# API测试工具
brew install --cask postman
# 虚拟容器
brew install --cask docker
# 集成开发工具
brew install --cask intellij-idea
# UML工具
brew install --cask staruml
```

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

### 常用APP

```bash
# 常用
brew install wechat
# 窗口管理工具
brew install shiftit 
brew install free-download-manager 

# 接口测试工具
brew install postman
# Markdown文档编辑
brew install typora
# UML工具
brew install staruml
# 开发工具
brew install intellij-idea
```



#### 安装插件

- Lombox

- FindBugs-IDEA

- **Mybatis Log Plugin** 

  MyBatis Log Plugin 这款插件是直接将Mybatis执行的sql脚本显示出来，无需处理，可以直接复制出来执行的 。`Tools -- >  Mybatis Log Plugin` 打开其日志框，注意其转换的SQL不是输出到IDE的控制台!!!

- 

  

[强迫症的 Mac 设置指南](https://github.com/macdao/ocds-guide-to-setting-up-mac)





Redis deskstop manager  编译

https://blog.csdn.net/zhangatle/article/details/101671697





前端开发

```
brew install yarn

```

