+++
date = '2025-09-01T22:56:08+08:00'
draft = false
title = 'mac 安装 Homebrew 并使用国内镜像源'
summary = 'mac安装 Homebrew 以及常见镜像源配置记录'
categories = [ 'mac' ]
tags = [ 'mac' ]
+++
### Homebrew是mac上比较流行的包管理工具，本文列出了mac上两种不同shell下配置国内常用镜像源的方法

* bash
```shell
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.bash_profile
echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"' >> ~/.bash_profile
echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"' >> ~/.bash_profile
echo 'export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"' >> ~/.bash_profile
echo 'export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles"' >> ~/.bash_profile
```

* zsh
```shell
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"' >> ~/.zprofile
echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"' >> ~/.zprofile
echo 'export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"' >> ~/.zprofile
echo 'export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles"' >> ~/.zprofile
```
#### homebrew国内常用源
* 清华源: https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/
* 阿里源: https://developer.aliyun.com/mirror/homebrew/

#### 常见问题
```shell
# 查看 Homebrew 版本
brew --version
# 更新 Homebrew
brew update

```
