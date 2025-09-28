+++
date = '2025-09-27T21:11:08+08:00'
draft = false
title = 'Go 语言环境安装以及包源配置'
summary = 'Go 语言开发环境配置过程记录（Mac下配置）'
categories = [ 'go' ]
tags = [ 'go' ]
+++

### 1. 简介
Go 语言是一种由 Google 开发的 简洁、高效、并发友好的开源编程语言，适合构建高性能后端和分布式系统。

### 2. 安装：[官方地址](https://go.dev/dl/)

### 3. 配置环境变量
1. 打开你的~/.zshrc或者~/.bash_profile文件（取决于你用的shell），然后添加以下内容：
```shell
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```
2. 保存文件，然后执行source ~/.zshrc或者source ~/.bash_profile，让环境变量生效。

### 4. 验证安装
```shell
go version
```

### 5. 项目管理：Go Modules
多个go项目使用Go Modules进行依赖管理，项目目录下执行如下命令，就会生成一个go.mod文件，记录该项目的依赖
```shell
go mod init <你的模块名>
```

### 5. 卸载
1. 移除安装包
2. 环境变量配置文件里面的内容进行清除，重新source ~/.zshrc使配置生效

### 6. 常用命令
```shell
go mod init <module-name>   # 初始化一个模块（开启 Go Modules）
go mod tidy                 # 自动分析 & 下载缺失依赖，删除无用依赖
go get <pkg>                # 下载/升级依赖
go list -m all              # 查看当前项目依赖列表

go run main.go              # 运行单个文件
go run ./cmd/server         # 运行目录下的 main 包
go build                    # 编译当前项目（生成可执行文件）
go build -o myapp           # 指定输出文件名
go install                  # 编译并安装到 $GOPATH/bin 或 go env GOBIN

go test                     # 运行测试
go test -v                  # 显示详细信息
go test ./...               # 递归运行所有测试
go test -bench=.            # 运行基准测试（性能测试）

go fmt ./...                # 格式化所有代码
go vet ./...                # 静态分析，检查可能的错误
go doc fmt.Println          # 查看某个函数/包的文档
go env                      # 查看 Go 环境变量

go clean -modcache          # 清理依赖缓存（GOPATH/pkg/mod）
go clean -testcache         # 清理测试缓存

# 其他
go help                     # go常见命令查看
go bug                      # go bug反馈
go fix                      # api升级助手，旧的api使用该命令后自动进行升级
go generate                 # 在源码里标记要执行的命令 → go generate 扫描注释并运行 → 自动生成代码文件
go work                     # 管理多模块工作区，方便在本地同时开发多个 Go 模块，避免频繁写
go tool                     # go tool 提供了底层的“专业工具箱”，用于调试、分析、编译细节
go vet                      # Go 的静态检查工具，用来发现可能导致 bug 的代码，而不是格式问题
```
