# mysql的坑



### error: 1366 Incorrect string value: '\xF0\x9F\x8D\xB5'

原来是这些人的昵称、留言内容里有emoj表情，而这些表情是按照四个字节一个单位进行编码的，而我们通常使用的utf-8编码在mysql数据库中默认是按照3个字节一个单位进行编码的，正是这个原因导致将数据存入mysql数据库的时候出现错误

(1) ：修改数据库配置

```		mysql

[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
init_connect='SET NAMES utf8mb4’
```

(2)：修改mysql数据库的编码为uft8mb4

执行命令：`ALTER DATABASE 数据库名称 CONVERT TO CHARACTER SET utf8mb4`;

(3)：修改数据表的编码为utf8mb4

执行命令：`ALTER TABLE 表名称 CONVERT TO CHARACTER SET utf8mb4`;

(4)：修改连接数据库的连接代码

如果默认连接有设置编码`jdbc:mysql://127.0.0.1:3306/test?characterEncodeing=UTF-8`，要去除`characterEncodeing=UTF-8`。

