# Docker安装基础设施

# mysql 

```bash
docker pull mysql
docker run -e MYSQL_ROOT_PASSWORD=zaq12wsx --name=mysql -p 3306:3306 -p 33060:33060 -d mysql
```

### 创建一个用户

``` bash
docker exec -it mysql mysql -uroot -p
# 创建数据库
create database owl_rbac default character set utf8 collate utf8_general_ci;
# 创建用户并授权
create user 'dbuser'@'%' identified by 'zaq12wsx';
grant select,insert,update,delete,create on owl_rbac.* to dbuser;
flush privileges;
```

## Redis

### 安装

```
docker pull redis
docker run --name=redis -p 6379:6379 -d redis
```

### 设置密码

```bash
docker exec -it redis redis-cli
# 设置密码
config set requirepass zaq12wsx
# 验证密码
auth zaq12wsx 
```

