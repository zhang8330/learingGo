# Go语言学习

## 概述

​	Go语言（或 Golang）是Google开发的开源编程语言。Go语言在多核并发上拥有原生的设计优势，Go语言从底层原生支持**并发**。它的主要目标是“**兼具Python 等动态语言的开发速度和C/C++等编译型语言的性能与安全性**”。

​	很多公司，特别是中国的互联网公司，即将或者已经完成了使用 Go 语言改造旧系统的过程。经过 Go 语言重构的系统能使用**更少的硬件资源获得更高的并发和I/O吞吐表现**。充分挖掘硬件设备的潜力也满足当前精细化运营的市场大环境。

​	Go语言的并发是基于 `goroutine` 的，`goroutine` 类似于线程，但并非线程。可以将 `goroutine` 理解为一种虚拟线程。Go 语言运行时会参与调度 `goroutine`，并将 `goroutine` 合理地分配到每个 CPU 中，最大限度地使用CPU性能。开启一个`goroutine`的消耗非常小（大约2KB的内存），你可以轻松创建数百万个`goroutine`。

### goroutine的特点

1.  `goroutine`具有可增长的分段堆栈。这意味着它们只在需要时才会使用更多内存。
2.  `goroutine`的启动时间比线程快。
3. `goroutine`原生支持利用channel安全地进行通信。
4. `goroutine`共享数据结构时无需使用互斥锁。

### go适合做什么

- 服务端开发
- 分布式系统，微服务
- 网络编程
- 区块链开发
- 内存KV数据库，例如boltDB、levelDB
- 云平台

## Go语言主要特征

1. 自带GC（Garbage Collection）：Go语言内置了一个自动管理内存的工具，能够自动识别并释放不再使用的内存资源。
2. 静态编译：
   - 生成单个独立的可执行文件，不再依赖外部库。
   - 支持交叉编译-即可以在一个平台上编译适用于另一个平台的二进制文件。
   - 减少运行时依赖，运行时不需要额外的动态链接库。
   - 运行效率高，Go的静态编译程序通常具有较快的启动速度和较高的运行效率。
3. 支持并发
4. 丰富的内置类型
5. 函数多返回值
6. 错误处理
7. 匿名函数和闭包
8. 类型和接口
9. 反射
10. 语言交互性

## 内置类型

### 值类型

``` go
bool
int(32 or 64), int8, int16, int32, int64
uint(32 or 64), uint8(byte), uint16, uint32, uint64
float32, float64
string
complex64, complex128 --复数(32+32, 64+64)
array    -- 固定长度的数组
```

### 引用类型：（指针类型）

``` go
slice   -- 序列数组(最常用)
map     -- 映射
chan    -- 管道
```

## 内置函数

Go 语言拥有一些不需要进行导入操作就可以使用的内置函数。

```go
append          -- 用来追加元素到数组、slice中,返回修改后的数组、slice
close           -- 主要用来关闭channel
delete          -- 从map中删除key对应的value
panic           -- 停止常规的goroutine  （panic和recover：用来做错误处理）
recover         -- 允许程序定义goroutine的panic动作
real            -- 返回complex的实部   （complex、real imag：用于创建和操作复数）
imag            -- 返回complex的虚部
make            -- 用来分配内存，返回Type本身(只能应用于slice, map, channel)
new             -- 用来分配内存，主要用来分配值类型，比如int、struct。返回指向Type的指针
cap             -- capacity是容量的意思，用于返回某个类型的最大容量（只能用于切片和 map）
copy            -- 用于复制和连接slice，返回复制的数目
len             -- 来求长度，比如string、array、slice、map、channel ，返回长度
print、println  -- 底层打印函数，在部署环境中建议使用 fmt 包
```

## 内置接口error

```go
type error interface { //只要实现了Error()函数，返回值为String的都实现了err接口

        Error()    String

}
```

## init函数和main函数

### init函数

init函数用于包的初始化，该函数是go语言的一个重要特征。

特征：

1. init函数是用于程序执行前做包的初始化函数
2. 每个包可以有多个init函数
3. 包的每个源文件也可以拥有多个init函数
4. 同一个包的init函数执行顺序没有明确定义
5. 不同包的init函数按照包导入的依赖关系决定该初始化函数的执行顺序
6. init函数不能被其他函数调用，是在main函数执行前，自动被调用

### main函数

Go语言默认的入口函数：func main()

```go
 func main(){
        //函数体
}
```

### 二者异同

相同点：

- 两个函数在定义时不能有任何的参数和返回值，且Go程序自动调用

不同点：

- init可以用于任意包，且可以有多个
- main只能用于main包，只能有一个

### 执行顺序

在同一个`go`文件的`init()`执行顺序为从上到下

在同一个`package`中不同文件是按照文件名字符串比较“从小到大”顺序调用`init()`函数

对于不同的`package`，如果不相互依赖，则按照main包中“先`import`的后调用”的顺序调用其包中的`init()`，如果`package`存在相互依赖，则先调用最早被依赖的的package中的`init()`，最后调用`main`函数

如果`init`函数中使用了`println()`或者`print()`你会发现在执行过程中这两个不会按照你想象中的顺序执行。这两个函数官方只推荐在测试环境中使用，对于正式环境不要使用。

## 运算符

算数运算符、关系运算符、逻辑运算符、位运算符、赋值运算符

### 算数运算符

\+ - * / %

### 关系运算符

==  !=  >  >=   <  <=      会返回True    或   False

### 逻辑运算符

 &&    ||    ！

### 位运算符

- &：参与运算的两数各对应的二进制相与
- |：……相或
- ^：……相异或，两位不一样的为1
- <<：左移n位，乘以2的n次方
- \>>：右移n位，除以2的n次方

### 赋值运算符

= += -= /= *= %= <<= >>= &= |= ^=

## 下划线

"_"是特殊标识符，用来忽略结果。

### 下划线在import中

在Golang里，import的作用是导入其他package。

> > import 下划线（如：import _ hello/imp）的作用：当导入一个包时，该包下的文件里所有init()函数都会被执行，然而，有些时候我们并不需要把整个包都导入进来，仅仅是是希望它执行init()函数而已。这个时候就可以使用 import _ 引用该包。即使用【import _ 包路径】只是引用该包，仅仅是为了调用init()函数，所以无法通过包名来调用包中的其他函数。

## 变量

### 声明

``` go
var 变量名 变量类型

var name string
var age int
var isOk bool


var(
	a string
    b int
    c bool
    d float32
)
```

### 变量的初始化

#### 默认值

整型和浮点型变量默认为0，字符串变量的默认值为空字符串，布尔型变量默认为false，切片、函数、指针变量默认为nil

``` go
var name string = "aaa"
var sex int = 1
var name, sex = "abc", 1//有时候会省略变量类型，编译器根据等号右边的值来推导变量的类型完成初始化

```

### 短变量声明

在函数内部，可以使用:=方式声明并初始化变量

``` go
package main

import (
    "fmt"
)

var m = 100

func main(){
    n := 10
    m := 200
    fmt.Println(m, n)
}
```

### 匿名变量

在使用多重赋值时，如果想要忽略某个值，可以使用`匿名变量(anonymous variable)`。匿名变量用一个下划线_表示，如：

```go
func foo() (int, string){
    return 10, "Q1mi"
}

func main(){
    x, _ := foo()
    _, y := foo()
    fmt.Println("x=", x)
    fmt.Println("y=", y)
}
```

匿名变量不占用命名空间，不会分配内存，所以匿名变量之间不存在重复声明。

**注意事项：**

- 函数外的每个语句都必须以关键字开始（var、const、func等）
- := 不能使用在函数外
- _多用于占位，表示忽略值。

### 常量

相对于变量，常量是恒定不变的。

```go
const pi = 3.1415
const e = 2.718

const (
	n1 = 1
    n2
    n3
)//n1,n2,n3都为1
```

### iota

`iota`是`go`语言的常量计数器，只能在常量的表达式中使用。

`iota`在`const`关键字出现时将被重置为`0`。`const`中每新增一行常量声明将使`iota`计数一次。`iota`可理解为`const`语句块中的行索引。

``` go
//a.go
const (
        n1 = iota //0
        n2        //1
        n3        //2
        n4        //3
    )
//b.go
const (
        n1 = iota //0
        n2 = 100  //100
        n3 = iota //2
        n4        //3
    )
const n5 = iota //0
//c.go
const (
        _  = iota
        KB = 1 << (10 * iota)
        MB = 1 << (10 * iota)
        GB = 1 << (10 * iota)
        TB = 1 << (10 * iota)
        PB = 1 << (10 * iota)
    )

//d.go
const (
        a, b = iota + 1, iota + 2 //1,2
        c, d                      //2,3
        e, f                      //3,4
    )

//iota自增在每一行开始时发生。
```

## 基本类型

### 介绍

类型（字节数，默认值）：

- 布尔值：bool（1，false）
- 整形：int和uint（4或8，0）、int8和uint8（1，0）、int16和uint16（2，0）、int32和uint32（4，0）、int64和uint64（8，0）
- 浮点型：float32（4，0.0）、float64（8，0.0）
- 复数：complex64（8，）、complex128（16，）
- byte（1，0）、rune（4，0）、uintptr（4或8，）、array、struct、string（，“”）、slice（，nil）、map（，nil）、channel（，nil）、interface（，nil）、function（，nil）。

### 转义符

- \r：回车符（返回行首）
- \n：换行符（直接跳到下一行的同列位置）
- \t：制表符
- '：制表符
- "：双引号
- \\：反斜杠

```go
//打印文件路径
package main
import (
    "fmt"
)
func main() {
    fmt.Println("str := \"c:\\pprof\\main.exe\"")
}
//定义多行字符串
    s1 := `第一行
    第二行
    第三行
    `
    fmt.Println(s1)
```

### 字符串的常用操作

| 方法                               | 介绍           |
| ---------------------------------- | -------------- |
| len(str)                           | 求长度         |
| +或fmtSprintf                      | 拼接字符串     |
| strings.Split                      | 分割           |
| strings.Contains                   | 判断是否包含   |
| strings.HasPrefx,string.HasSuffix  | 前缀/后缀判断  |
| strings.Index(),stirng,LastIndex() | 子串出现的位置 |
| strings.Join(a[]string,sep string) | join操作       |

### byte和rune类型

组成每个字符串的元素叫做“字符”，可以通过遍历或者单个获取字符串元素获得字符。字符用单引号‘包裹，如：

``` go
var a := 'a'
var x := '好'
```

Go 语言的字符有以下两种：

```
uint8类型，或者叫 byte 型，代表了ASCII码的一个字符。

rune类型，代表一个 UTF-8字符。
```

当需要处理中文、日文或者其他复合字符时，则需要用到`rune`类型。`rune`类型实际是一个`int32`。
Go 使用了特殊的 `rune` 类型来处理 `Unicode`，让基于 `Unicode`的文本处理更为方便，也可以使用 `byte` 型进行默认字符串处理，性能和扩展性都有照顾

```
    // 遍历字符串
    func traversalString() {
        s := "pprof.cn博客"
        for i := 0; i < len(s); i++ { //byte
            fmt.Printf("%v(%c) ", s[i], s[i])
        }
        fmt.Println()
        for _, r := range s { //rune
            fmt.Printf("%v(%c) ", r, r)
        }
        fmt.Println()
```

因为UTF8编码下一个中文汉字由`3~4`个字节组成，所以我们不能简单的按照字节去遍历一个包含中文的字符串，否则就会出现上面输出中第一行的结果。

字符串底层是一个byte数组，所以可以和[]byte类型相互转换。字符串是不能修改的 字符串是由byte字节组成，所以字符串的长度是byte字节的长度。 rune类型用来表示utf8字符，一个rune字符由一个或多个byte组成。

### 修改字符串

要修改字符串，需要先将其转换成`[]rune或[]byte`，完成后再转换为`string`。无论哪种转换，都会重新分配内存，并复制字节数组。

```go
func changeString(){
    s1 := "hello"
    //强制类型转换
    byteS1 := []byte(s1)
    byteS1[0] = 'H'
    fmt.Println(string(byteS1))
    
    s2 := "博客"
    runeS2 := []rune(s2)
    runeS2[0] = '狗'
    fmt.Println(string(runeS2))
}
```

### 类型转换

Go语言中只有强制类型转换，没有隐式类型转换。该语法只能在两个类型之间支持相互转换的时候使用。

强制类型转换的基本语法如下：

```
T(表达式)
```

其中，T表示要转换的类型。表达式包括变量、复杂算子和函数返回值等.

比如计算直角三角形的斜边长时使用math包的Sqrt()函数，该函数接收的是float64类型的参数，而变量a和b都是int类型的，这个时候就需要将a和b强制类型转换为float64类型。

```go
func sqrtDemo() {
    var a, b = 3, 4
    var c int
    // math.Sqrt()接收的参数是float64类型，需要强制转换
    c = int(math.Sqrt(float64(a*a + b*b)))
    fmt.Println(c)
}
```

## 数组

``` go
var a [len]int;
for i := 0;i < len(a); i++{
}
for index, v := range a{
}

```

指针数组：[n]\*T，数组指针：\*[n]T

### 初始化

```go
//全局
var arr0 [5]int = [5]int{1, 2, 3}//未初始化的元素值为0
var arr1 = [5]int{1, 2, 3, 4, 5}
var arr2 = [...]int{1, 2, 3, 5, 6}
var str = [5]string{3: "hello world", 4:"tom"}
//局部
a := [3]int{1, 2}
b := [...]int{1, 2, 3, 4}
c := [5]int{2: 100, 4: 200}
d := [...]struct {
    name string
    age uint8
}{
    {"user1", 10},
    {"user2", 20},//最后一行也要加逗号
}
```

### 多维数组

```go
//全局
var arr0 [5][3]int
var arr1 [2][3]int = [...][3]int{{1, 2, 3}, {7, 8, 9}}
//局部
a := [2][3]int{{1, 2, 3}, {4, 5, 6}}
b := [...][2]int{{1, 1}, {2, 2}, {3, 3}}//第二维度不能用...
```

> 值拷贝行为会造成性能问题，通常会建议使用 slice，或数组指针。

内置函数 len 和 cap 都返回数组长度 (元素数量)。

```go
package main

func main() {
    a := [2]int{}
    println(len(a), cap(a)) 
}
//2 2
```

#### 多维数组遍历：

```go
package main

import (
    "fmt"
)

func main() {

    var f [2][3]int = [...][3]int{{1, 2, 3}, {7, 8, 9}}

    for k1, v1 := range f {
        for k2, v2 := range v1 {
            fmt.Printf("(%d,%d)=%d ", k1, k2, v2)
        }
        fmt.Println()
    }
}
```

输出结果：

```
    (0,0)=1 (0,1)=2 (0,2)=3 
    (1,0)=7 (1,1)=8 (1,2)=9 
```
