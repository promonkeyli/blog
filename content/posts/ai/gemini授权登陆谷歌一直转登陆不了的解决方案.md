+++
date = '2025-11-19T20:59:47+08:00'
draft = false
title = 'gemini授权登陆谷歌一直转登陆不了的解决方案'
summary = 'gemini授权登陆谷歌一直转登陆不了的解决方案'
categories = [ 'ai' ]
tags = [ 'ai' ]
+++

谷歌 gemini code cli 在进行谷歌登录授权时，有时会一直处于登陆中状态，解决方法，可以为终端设置代理 
你需要为当前终端会话设置 https_proxy 环境变量，将 Gemini CLI 的网络请求指向你的本地代理端口，做法如下：

Windows (CMD):
```shell
set https_proxy=http://127.0.0.1:7089
```

Windows (PowerShell):
```shell
$env:https_proxy="http://127.0.0.1:7089"
```

macOS / Linux (Bash/Zsh):
```shell
export https_proxy=http://127.0.0.1:7089
```
注意： 请将 7089 替换为你的代理软件（比如 clashx使用的默认端口是7089）实际使用的 HTTP 代理端口，每一次重新进入命令行都需要先输入上面的shell再使用gemini命令才行，这个配置是临时有效
