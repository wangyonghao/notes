### Git常用命令

整理记录一些实用但不太容易记的Git命令

![img](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png)

专用名词

Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库



## 查看信息

```shell 
# 查看将要Push的内容
git log --stat -1
# 显示指定文件是什么人在什么时间修改过
$ git blame [file]
```



### 参考

1. https://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html