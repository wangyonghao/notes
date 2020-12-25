### MAC 工具

## 前言

本文的主旨是打造一个高效的 Mac 工作环境 。在借鉴了不少前人经验的同时，也希望分享一些个人心得。

本文借鉴前人经验结合个人心得逐步 [打造一个高效的 Mac 工作环境](https://github.com/macdao/ocds-guide-to-setting-up-mac)，收集效率的方法和实用的软件。

一个高效的 Mac 工作环境的特点：

- **自动化**，简化操作，提高效率
- **统一**
- **够用**就好，如果系统本身已经满足了我的需求，我不会再使用第三方工具。
- 效率，一切都是为了效率。

记录一些工具的设置



个人工作习惯：不使用鼠标，键盘党，轻微强迫症患者



## MacOS篇

介绍一些系统本身的一些设置。

### 

### 在日历中显示法定假日

打开日历应用，点击 `File -> New Calendar Subscriber...` 打开新建日历订阅窗口。

1.  在订阅栏输入以下[订阅地址]( <https://www.jianshu.com/p/c0c78f8305f9>)：

    ```
    webcal://p10-calendars.icloud.com/published/2/MTI3Njk0MTQxNzEyNzY5NFRvxM53AOH-Px17vHeUETlZdUggoyEt2KnFiIqHg40FkRXfcQJjYoa2dULRarI9z4UlbHxK-kLOohfiRVSSP7Q
    ```

2. 勾选移除`提醒` 和`附件`，忽略订阅中的广告。

3. 点击确定，等待导入完成。 <https://www.jianshu.com/p/c0c78f8305f9>

### 在Finder中显示文件完整路径

1. 使用【访达】的【显示】设置显示完整路径

2. 使用终端命令行`defaults write com.apple.finder _FXShowPosixPathInTitle -bool TRUE;killall Finder`显示完整路径，建议直接复制粘贴。如果需要取消路径显示，在终端命令行输入指令`defaults delete com.apple.finder _FXShowPosixPathInTitle;killall Finder`，即可恢复默认显示，建议直接复制粘贴。

### 禁止Mac OS X系统产生.DS_Store文件的方法

打开 “终端” ，复制黏贴下面的命令，回车执行，重启Mac即可生效。

```bash 
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE
```

恢复`.DS_store`生成

```bash
defaults delete com.apple.desktopservices DSDontWriteNetworkStores	
```

已经生成的文件还需要你手动删除：

```bash
sudo find . -name ".DS_Store" -depth -exec rm -f {} \;
```

### 

## 日常工具篇

介绍一些日常使用的工具，与开发没有直接关系的第三方软件及其设置。

### [Homebrew](http://brew.sh/)

包管理工具

执行`install`的时候经常会执行更新，有时候会比较慢，我们可以设置环境变量`HOMEBREW_NO_AUTO_UPDATE`关闭更新：

```bash
echo "export HOMEBREW_NO_AUTO_UPDATE=1" >> ~/.zshrc
source ~/.zshrc
```

### [iTerm2](https://iterm2.com/)

终端工具

#### 参考资料

- [Iterm2](https://github.com/macdao/ocds-guide-to-setting-up-mac#iterm2)
- [Mac 小记 — iTerm2、Zsh、Homebrew](https://www.cnblogs.com/youclk/p/8125305.html)
- [MAC下使用iTerm2和zsh](https://blog.csdn.net/u014102846/article/details/77964493)
- [终端配色](<https://sspai.com/post/53008>)

### Alfred

[5分钟上手Mac效率神器Alfred以及Alfred常用操作](https://www.jianshu.com/p/e9f3352c785f)

### 

基于MPV的一个高颜值有潜力的现代播放器

**Cheapsheet**

安装了这个后，在任何软件中按住Command键数秒后就会弹出该应用以及当前系统可用的所有快捷键列表，是为「cheat sheet」。

```
brew install iina
brew cask install cheapsheet
```

## 开发工具篇

- Dash

  文档API查询工具


参考：

- [一些口碑甚好的Mac Apps使用感受](<https://www.jianshu.com/p/2f8fb07101c1>)

  

Gitbook + Typora + git

#### 取消检查更新

`Preferences > Settings`在`Preferences.sublime-settings - User`文件添加`"update_check": false`



## 参考

- [强迫症的 Mac 设置指南](https://github.com/macdao/ocds-guide-to-setting-up-mac#功能键)