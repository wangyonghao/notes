## 打造Mac工作环境-日常篇

## 前言

写这篇文章的主旨是：记录在工作中使用Mac时使用高频率的软件的安装及配置过程，逐渐积累打磨出适合自己工作环境。

本人使用笔记本是`MBP 2019`款，操作系统是`maxOS Catalina 10.15.7`，网络环境是中国大陆。

指导原则：

1. **节省成本**
2. **自动化**，安装方式优先顺序：`自动化 > AppStore >安装包`
3. **不重复**
4. **无国界**，部分软件官网在国外。

文章分为日常篇和开发篇两部分：

- **日常篇** - 包含系统配置和日常软件两部分。
- **开发篇** - 如其名，与开发相关的软件都在这里。



## 系统配置

### 配置 Launchpad

原生的配置让 Launchpad 稍许拥挤, 将 Launchpad应用排列方式改为8列7行。

```shell
defaults write com.apple.dock springboard-columns -int 8; 
defaults write com.apple.dock springboard-rows -int 7; 
defaults write com.apple.dock ResetLaunchPad -bool TRUE;
killall Dock
```



## 常用软件

这里列举一下日常篇包含的软件：

- OhMyZsh
- Homebrew
- iTerm2
- Typora
- Sublime Text 3
- Alfred 4
- CleanMyMac 4
- Shiftit
- HazeOver
- Xmind
- Spark
- Qspace
- FreeDownloadManager
- WeChat
- DingTalk
- IINA
- 

## 开始

### [OnMyZsh](https://github.com/ohmyzsh/ohmyzsh)

官方提供的[安装命令](https://ohmyz.sh/#install)已不能使用，会报 443: Connection refused 错误，使用下面的命令进行安装

```bash
# 克隆 ohmyzsh 仓库
git clone https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh
# 可选，备份 zsh 配置文件
cp ~/.zshrc ~/.zshrc.orig
# 创建新的 zsh 配置文件
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
# 将 zsh 设置默认shell程序
chsh -s $(which zsh)
```

插件

[autojump](https://github.com/wting/autojump): 终端中一键直达目录, 命令行中切换目录是最常用的操作, 只要正常 cd 过目录, 下次只要记住目录名字, 就可以直接进去, 支持模糊匹配, 用过一次, 无法离开

```shell
brew install autojump
```

[Zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) : 终端历史操作记录自动补全

```
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
```



禁止自动更新

```shell
echo "DISABLE_UPDATE_PROMPT=true" >> ~/.zhrc
echo "DISABLE_AUTO_UPDATE=true" >> ~/.zhrc
```

相关命令

```shell
# 更新
omz update
# 卸载
uninstall_oh_my_zsh
```



[Mac 小记 — iTerm2、Zsh、Homebrew](https://www.cnblogs.com/youclk/p/8125305.html)

### Homebrew

安装参考[mac下国内安装Homebrew教程](https://www.cnblogs.com/xibushijie/p/13335988.html)

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

禁止HomeBrew自动更新，手动更新执行 ` brew  update`

```

echo "export HOMEBREW_NO_AUTO_UPDATE=1" >> ~/.zshrc
source ~/.zshrc
```





有了 Homebrew 后面安装就简单多了，这里汇总一下我使用`brew`命令安装的程序

```shell
# 命令行终端
brew install --cask iterm2
# 文本编辑工具
brew install --cask sublime-text
# MarkDown写作
brew install --cask typora
# 窗口管理
brew install --cask shiftit
# 微信
brew install --cask wechat
# 视频
brew install iina

```

### Alfred

workflow 强烈推荐 [zenorocha/alfred-workflows · GitHub](https://link.zhihu.com/?target=https%3A//github.com/zenorocha/alfred-workflows)



### [Sublime Text](http://www.sublimetext.cn/)

 一款用于代码、标记和散文的精致文本编辑器

#### 安装

```bash
brew install --cask sublime-text
```

#### 插件

- [Package Control](http://packagecontrol.cn/installation) - 插件管理器，按下`Ctrl+Shift+P` > `Install Package  `来查找、安装其他插件。
- [Emmet](http://emmet.io/)  -  高效地编写HTML和CSS插件。
- [Trailing Spaces](https://github.com/SublimeText/TrailingSpaces) - 高亮尾部的空格，并在瞬间将其删除。

#### 

