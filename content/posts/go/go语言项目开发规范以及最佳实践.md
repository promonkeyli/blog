+++
date = '2025-12-01T21:24:51+08:00'
draft = false
title = 'go语言项目开发规范以及最佳实践'
summary = 'go语言项目开发规范以及最佳实践'
categories = [ 'go' ]
tags = [ 'go']
+++
本文档基于 Go [社区事实标准](https://github.com/golang-standards/project-layout)（Standard Go Project Layout）及 Uber Go Style Guide 整理，适用于企业级项目开发。

---

## 一、 项目目录结构最佳实践

### 1. 标准项目结构 (Standard Layout)
适用于中大型项目、微服务或需要长期维护的系统。

```text
my-project/
├── cmd/                    # 项目的主入口
│   ├── app-server/         # 主应用入口 (main.go 所在)
│   └── app-cli/            # 命令行工具入口
├── internal/               # 私有代码（核心业务逻辑）
│   ├── handler/            # HTTP/RPC 接入层
│   ├── service/            # 业务逻辑层
│   ├── repository/         # 数据访问层 (DB/Cache)
│   ├── model/              # 内部数据模型
│   └── pkg/                # 仅供本项目使用的内部公共包
├── pkg/                    # 公共代码（可被外部项目引用）
│   └── utils/              # 通用工具库
├── api/                    # API 协议定义
│   ├── openapi/            # Swagger/OpenAPI yaml
│   └── protobuf/           # gRPC proto 文件
├── configs/                # 配置文件模板 (config.yaml.example)
├── scripts/                # 构建、安装、分析脚本 (Makefile, shell)
├── build/                  # 打包生成的文件
│   ├── package/            # Dockerfile 配置
│   └── ci/                 # CI 配置文件
├── test/                   # 额外的外部测试数据或集成测试
├── go.mod                  # 依赖管理
├── go.sum
├── Makefile                # 统一的管理入口
└── README.md
```

#### 关键目录说明
*   **`/cmd`**: 这里的 `main.go` 应非常简短，仅负责初始化依赖（DB、Config）并启动服务。**严禁在此编写业务逻辑**。
*   **`/internal`**: Go 编译器强制特性。此目录下的代码**只能被父级目录及其子目录引用**，有效防止模块间乱引用。
*   **`/pkg`**: 只有当你希望代码被**其他项目**引用时才放在这里。否则请优先使用 `/internal`。
*   **`/configs`**: 通常只放默认配置或示例配置。**严禁提交包含真实密码的配置文件**。

### 2. 扁平化结构 (Flat Layout)
适用于简单的微服务、HTTP 服务或小型工具。

```text
simple-service/
├── main.go
├── service.go
├── user.go
├── config.go
├── go.mod
└── go.sum
```
> **建议**：当文件数量增多时，应及时重构为标准结构。

---

## 二、 Go 编码规范最佳实践

### 1. 命名规范 (Naming)

| 类型 | 规范 | 示例 |
| :--- | :--- | :--- |
| **包名 (Package)** | 简短、全小写、单数、避免下划线 | `net/http` (Good), `net_http` (Bad) |
| **变量/常量** | CamelCase (驼峰)，首字母大小写控制权限 | `userCount`, `ExportedVar` |
| **缩写** | 专有名词缩写必须全大写或全小写 | `ServeHTTP` (Good), `ServeHttp` (Bad); `ID` (Good), `Id` (Bad) |
| **接口 (Interface)** | 单方法接口以 `er` 结尾 | `Reader`, `Writer`, `Formatter` |
| **Getter/Setter** | 读取器不加 Get 前缀 | `obj.Owner()` (Good), `obj.GetOwner()` (Bad) |

### 2. 错误处理 (Error Handling)

*   **不要忽略错误**：即使不处理，也必须记录日志。
*   **错误包装**：使用 `%w` 保留原始错误堆栈（Go 1.13+）。
    ```go
    fmt.Errorf("failed to query user: %w", err)
    ```
*   **尽早返回 (Guard Clauses)**：将正常逻辑放在最左侧（最小缩进），错误处理尽早 Return。
    ```go
    // Good
    if err != nil {
        return err
    }
    // 正常逻辑继续...
    ```

### 3. 依赖注入与全局变量

*   **避免全局状态**：尽量少用全局变量（如 `var globalDB *sql.DB`）。
*   **构造函数注入**：明确结构体的依赖关系。
    ```go
    func NewService(r Repository, l Logger) *Service {
        return &Service{repo: r, log: l}
    }
    ```

### 4. 并发规范

*   **Context 传递**：函数的第一个参数通常是 `ctx context.Context`，用于控制超时和取消。
*   **Goroutine 生命周期**：启动 Goroutine 前必须清楚它何时退出，防止内存泄漏。
*   **Panic**：禁止在业务代码中使用 `panic`，除非是启动时不可恢复的错误（如配置加载失败）。

---

## 三、 架构设计模式 (Architecture)

推荐在 `/internal` 中采用 **Clean Architecture (整洁架构)** 分层：

1.  **Transport Layer (Handler)**: 处理 HTTP/gRPC 请求，解析参数 -> 调用 Service -> 返回 DTO。
2.  **Service Layer (Business Logic)**: 核心业务逻辑，定义 Repository 接口。
3.  **Repository Layer (Data Access)**: 实现接口，操作数据库/缓存。
4.  **Model/Entity**: 纯粹的数据结构定义。

**依赖方向**：Handler -> Service -> Repository (Interface)

---

## 四、 工具链与自动化

规范必须依靠工具强制执行。

1.  **Linter (必装)**: 使用 **`golangci-lint`**。
2.  **格式化**: 使用 `gofmt` 或 `goimports`（自动管理 import）。
3.  **Makefile**: 封装常用命令。

```makefile
.PHONY: build test lint

build:
    go build -o bin/app ./cmd/app-server

test:
    go test -v ./...

lint:
    golangci-lint run
```
