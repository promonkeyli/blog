+++
date = '2025-10-02T08:32:35+08:00'
draft = false
title = 'Go语言中的循环'
summary = 'Go 语言中的循环记录'
categories = [ 'go' ]
tags = [ 'go' ]
+++

> `for` 是 Go 唯一的循环结构

### 基本语法

```go
//1. 基本循环
	var i = 0
	for i <= 3 {
		fmt.Println(i)
		i++
	}
	for j := 0; j < 5; j++ {
		s := fmt.Sprintf("当前的值是%v", j)
		fmt.Println(s)
	}

	// 2.无限循环
	for {
		fmt.Println("无限循环...")
	}

	// 3.for ... range 用来遍历数组、切片、字符串、map、channel。
	for k := range 3 {
		fmt.Println("我是range循环", k)
	}
```

### 循环控制

* `break`：跳出循环，对于双循环，只是跳出本层循环
* `continue`：跳过本次循环
* `goto`：不推荐，一般用 `break`/`continue` 就够了

```go
// break
for i := 0; i < 3; i++ {
    for j := 0; j < 3; j++ {
        if j == 1 {
            break // 只跳出 j 这一层
        }
        fmt.Println(i, j)
    }
}

// break outer 
outer: // 定义标签
for i := 0; i < 3; i++ {
    for j := 0; j < 3; j++ {
        if j == 1 {
            break outer // 跳出外层循环
        }
        fmt.Println(i, j)
    }
}

// continue
for i := 0; i < 5; i++ {
    if i == 2 {
        continue // 跳过本次循环后续代码，直接进入下一次循环
    }
    fmt.Println("i =", i)
}
```

