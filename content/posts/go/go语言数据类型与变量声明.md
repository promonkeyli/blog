+++
date = '2025-09-29T20:14:32+08:00'
draft = false
title = 'Go语言数据类型与变量声明'
summary = 'Go 语言数据类型以及变量声明的一些语法记录'
categories = [ 'go' ]
tags = [ 'go' ]
+++

### 变量声明 var、const
```go
var a =  "initial" // 单变量声明，不声明类型go可以进行推断

var b,c int = 1,2 // 多变量声明

var d int // d 的值声明了类型，但是没有初始化，int类型值就是0

var (
    e string
    f int
    g bool
    d float
) // 批量变量声明

e := "apple" // 短变量声明，等价于：var e string = "apple", 短变量声明语法仅在函数中使用，包级作用域需要完整变量声明

const f string = "constant" // 常量声明
```
### 数据类型
### 基本数据类型
* 整型： 有符号整型(int、int8、int16、int32、int64)、无符号整型（uint、uint8（=byte）、uint16、uint32、uint64）
* 浮点: float32、float64 
* 复数型: complex64、complex128
* 布尔型: bool
* 字符串: string
* 字符/字节: byte(uint8、表示原始字节数据)、rune(int32, 主要表示Unicode码点)

### 复合数据类型
* 数组(array): [N]T 固定长度
* 切片(slice): []T 动态长度
* 映射(map): map[K]V 键值对
* 结构体(struct): struct{}
* 通道(chan): chan T 用于goroutine通信
* 指针(pointer): *T

### 特殊类型
* 函数类型: func
* 接口类型: interface
* 类型别名: type
* 自定义类型 type MyStruct struct {}
