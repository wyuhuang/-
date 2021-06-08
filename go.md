Cloning into 'common'...
The authenticity of host 'gitlab.inwesure.com (10.95.16.22)' can't be established.
ED25519 key fingerprint is SHA256:EZQMEWvO3Av2LxX+UHwJWnpnlgqmtKI2RZpfWmvTW44.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])?



# go

## 环境和编译运行

run--编译并运行

```shell
go run 文件名（含后缀）
```



## 基础

### 语言结构

- 包声明
- 引入包
- 函数
- 变量
- 语句 & 表达式
- 注释

```go
package main

import "fmt"

func main() {
   /* 这是我的第一个简单的程序 */
   fmt.Println("Hello, World!")
}
```



main方法是程序入口，main方法必须在main包下



### 变量

**声明**

```go
var v int;
var v1, v2 int;
var (
	v1 int
    v2 float
)
```



**声明并初始化**

```go
var v int = 33
var v = 33 //省略类型，自动推断
v := 33//省略var
```



**赋值与多重赋值**

```go
v = 100

//多重赋值
i, j = 1, 2
i, j = j, i//直接交换变量,可以理解先j,i的值入栈,然后出栈赋值
```



**匿名变量**

```go
func getName() (firstName, lastName, nickName string) {
	return "May", "chan", "cccc"
}
_, _, d := getName();//多返回值类似多重赋值(反向),只想要部分返回值时可以考虑匿名变量,使用下划线作为变量匿名
```



### 常量

**字面常量**

```go
-12
3.14159265358979323846 // 浮点类型的常量
3.2+12i // 复数类型的常量
true // 布尔类型的常量
"foo" // 字符串常量
```

go的常量是无具体类型的(只要大小兼容类型即可赋值),-12可以赋值给long型,而不需要像java使用-12L



**定义常量**

Go的常量定义可以限定常量类型，但不是必需的。如果定义常量时没有指定类型，那么它 与字面常量一样，是无类型常量。

和变量差不多

```go
const Pi float64 = 3.14159265358979323846
const zero = 0.0 // 无类型浮点常量
const (
 size int64 = 1024
 eof = -1 // 无类型整型常量
)
const u, v float32 = 0, 3 // u = 0.0, v = 3.0，常量的多重赋值
const a, b, c = 3, 4, "foo"
// a = 3, b = 4, c = "foo", 无类型整型和字符串常量
```

于常量的赋值是一个编译期行为，所以右值不能出现任何需要运行期才能得出结果的表达 式，比如试图以如下方式定义常量就会导致编译错误：

```go
 const Home = os.GetEnv("HOME") 
```



**预定义常量**

![image-20210528115127173](C:\Users\v_sxhthuang\AppData\Roaming\Typora\typora-user-images\image-20210528115127173.png)

**简写**

![image-20210528115143151](C:\Users\v_sxhthuang\AppData\Roaming\Typora\typora-user-images\image-20210528115143151.png)



**枚举**

使用iota即可

```go
const (
 Sunday = iota
 Monday
 Tuesday
 Wednesday
 Thursday
 Friday
 Saturday
 numberOfDays // 这个常量没有导出
) 
```

同Go语言的其他符号（symbol）一样，以大写字母开头的常量在包外可见。 以上例子中numberOfDays为包内私有，其他符号则可被其他包访问。









### 数据类型

| 1    | **布尔型** 布尔型的值只可以是常量 true 或者 false。一个简单的例子：var b bool = true。 |
| ---- | ------------------------------------------------------------ |
| 2    | **数字类型** 整型 int 和浮点型 float32、float64，Go 语言支持整型和浮点型数字，并且支持复数，其中位的运算采用补码。 |
| 3    | **字符串类型:** 字符串就是一串固定长度的字符连接起来的字符序列。Go 的字符串是由单个字节连接起来的。Go 语言的字符串的字节使用 UTF-8 编码标识 Unicode 文本。 |
| 4    | **派生类型:** 包括：(a) 指针类型（Pointer）(b) 数组类型(c) 结构化类型(struct)(d) Channel 类型(e) 函数类型(f) 切片类型(g) 接口类型（interface）(h) Map 类型 |

没有字符型



**布尔类型**

布尔类型不能接受其他类型的赋值，不支持自动或强制的类型转换。



**整型**

一般用int保证兼容,而不是int8\int16

![image-20210528161204488](C:\Users\v_sxhthuang\AppData\Roaming\Typora\typora-user-images\image-20210528161204488.png)

需要注意的是，int和int32在Go语言里被认为是两种不同的类型，编译器也不会帮你自动 做类型转换,可以强制类型转换(注意精度)



**数值运算**

一致

**比较运算**

一致

两个不同类型的整型数不能直接比较，比如int8类型的数和int类型的数不能直接比较，但 各种类型的整型变量都可以直接与字面常量（literal）进行比较



**位运算**

![image-20210528162417461](C:\Users\v_sxhthuang\AppData\Roaming\Typora\typora-user-images\image-20210528162417461.png)



**浮点型**

- 表示

Go语言定义了两个类型float32和float64，其中float32等价于C语言的float类型， float64等价于C语言的double类型。 在Go语言里，定义一个浮点数变量的代码如下： var fvalue1 float32 fvalue1 = 12 fvalue2 := 12.0 // 如果不加小数点，fvalue2会被推导为整型而不是浮点型

对于以上例子中类型被自动推导的fvalue2，需要注意的是其类型将被自动设为float64， 而不管赋给它的数字是否是用32位长度表示的



- 比较

```go
import "math"
// p为用户自定义的比较精度，比如0.00001
func IsEqual(f1, f2, p float64) bool {
 return math.Fdim(f1, f2) < p
} 
```



**字符串**

Go语言中字符串的声明和初始化非常简单，举例如下： 

```go
var str string // 声明一个字符串变量 
str = "Hello world" // 字符串赋值
ch := str[0] // 取字符串的第一个字符
```

字符串的内容可以用类似于数组下标的方式获取，但与数组不同，字符串的内容不能在初始 化后被修改

```go
str := "Hello world" // 字符串也支持声明时进行初始化的做法
str[0] = 'X' // 编译错误
```

- 操作

![image-20210528164546200](C:\Users\v_sxhthuang\AppData\Roaming\Typora\typora-user-images\image-20210528164546200.png)

- 遍历

```go
func iter(method int) {
	var str string = "hello, 世界"
	if method == 1 { //方法一, 按字节便利
		n := len(str)
		for i := 0; i < n; i++ {
			fmt.Println(i, str[i])
		}
	} else { //方法二,以Unicode字符方式遍历时，每个字符的类型是rune
		for i, ch := range str {
			fmt.Println(i, ch)
		}
	}
}
```

go的字符串以UTF-8编码



## 流程控制

#### if

在if之后，条件语句之前，可以添加变量初始化语句，使用;间隔；

```go 
if e, ok := err.(*os.PathError); ok && e.Err != nil { // 获取PathError类型变量e中的其他信息并处理 }
```



在有返回值的函数中，不允许将“最终的”return语句包含在if...else...结构中， 否则会编译失败. 失败的原因在于，Go编译器无法找到终止该函数的return语句。编译失败的案例如下：

```go
func example(x int) int {
 if x == 0 {
 return 5
 } else {
 return x
 }
} 
```



#### switch

条件表达式不限制为常量或者整数

单个case中，可以出现多个结果选项； 

与C语言等规则相反，Go语言不需要用break来明确退出一个case；

 只有在case中明确添加fallthrough关键字，才会继续执行紧跟的下一个case； 

 可以不设定switch之后的条件表达式，在此种情况下，整个switch结构与多个 if...else...的逻辑作用等同。

```go
switch i { //这里的i是表达式
 case 0:
 fmt.Printf("0")
 case 1:
 fmt.Printf("1")
 case 2:
 fallthrough
 case 3:
 fmt.Printf("3")
 case 4, 5, 6:
 fmt.Printf("4, 5, 6")
 default:
 fmt.Printf("Default")
} 
```









## 数据结构

### 数组

**声明**

```go
[32]byte // 长度为32的数组，每个元素为一个字节
[2*N] struct { x, y int32 } // 复杂类型数组
[1000]*float64 // 指针数组
[3][5]int // 二维数组
[2][2][2]float64 // 等同于[2]([2]([2]float64)) 多维数组
```

```go
var variable_name [SIZE] variable_type
```



**初始化**

```go
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
//不确定长度
var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
或
balance := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}

//如果设置了数组的长度，我们还可以通过指定下标来初始化元素：
//  将索引为 1 和 3 的元素初始化
balance := [5]float32{1:2.0,3:7.0}
```



**访问与遍历**

- 使用下标读写某个元素
- 遍历

```go
for i := 0; i < len(array); i++ {
 fmt.Println("Element", i, "of array is", array[i])
} 
//个关键字range，用于便捷地遍历容器中的元素。当然，数组也是range的支持范围。
//range,可以读取下标及其对应的元素
for i, v := range array {
 fmt.Println("Array element[", i, "]=", v)
} 
```

- 值语义

中数组是一个值类型（value type）。所有的值类型变量在赋值和作为参数传递时都将产生一次复制动作。如果将数组作为函数的参数类型，则在函数调用时该 参数将发生数据复制。



### 切片

#### 概述

切片（slice）是对数组一个连续片段的引用（该数组我们称之为相关数组，通常是匿名的），所以切片是一个引用类型（因此更类似于 C/C++ 中的数组类型，或者 Python 中的 list 类型）。切片提供了一个相关数组的动态窗口(不包括终止索引标识的项)。







#### 创建

**基于数组**

数组切片可以基于一个已存在的数组创建。数组切片可以只使用数组的一部分元素或者整个 数组来创建，甚至可以创建一个比所基于的数组还要大的数组切片

```go
// 先定义一个数组
 var myArray [10]int = [10]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
 // 基于数组创建一个数组切片
var mySlice []int = myArray[:5]//:左边留空表示从第一个元素开始,egmyArray[:]\myArray[1:]
```

持用myArray[first:last]这样的方式来基于数组生成一 个数组切片



**直接创建**

使用make函数

```go
//创建一个初始元素个数为5的数组切片，元素初始值为0：
mySlice1 := make([]int, 5)

//创建一个初始元素个数为5的数组切片，元素初始值为0，并预留10个元素的存储空间：
mySlice2 := make([]int, 5, 10)

//直接创建并初始化包含5个元素的数组切片：
mySlice3 := []int{1, 2, 3, 4, 5}
//当然，事实上还会有一个匿名数组被创建出来，只是不需要我们来操心而已。
```



**追加创建**

```go
mySlice = append(mySlice, 1, 2, 3)
```





**基于切片**

```go
oldSlice := []int{1, 2, 3, 4, 5}
newSlice := oldSlice[:3] // 基于oldSlice的前3个元素构建新数组切片
```



**内容复制**

```go
slice1 := []int{1, 2, 3, 4, 5}
slice2 := []int{5, 4, 3}
copy(slice2, slice1) // 只会复制slice1的前3个元素到slice2中
copy(slice1, slice2) // 只会复制slice2的3个元素到slice1的前3个位置
```





#### 相关操作

**获取长度**

使用len函数

```go
len(mySlice)
```



**获取容量**

```go
 cap(mySlice)
```





**遍历**

for-range循环

```go
for _, v := range myArray {
 fmt.Print(v, " ")
 } 
```

普通for循环

```go
for i := 0; i <len(mySlice); i++ { fmt.Println("mySlice[", i, "] =", mySlice[i])}
```



#### 动态存储

可动态增减元素是数组切片比数组更为强大的功能。与数组相比，数组切片多了一个存储能 力（capacity）的概念

合理地设置存储能力的 值，可以大幅降低数组切片内部重新分配内存和搬送内存块的频率，从而大大提高程序性能.

假如你明确知道当前创建的数组切片最多可能需要存储的元素个数为50，那么如果你设置的 存储能力小于50，比如20，那么在元素超过20时，底层将会发生至少一次这样的动作——重新分 配一块“够大”的内存，并且需要把内容从原来的内存块复制到新分配的内存块，这会产生比较 明显的开销



### map

**声明**

```go
var myMap map[string] PersonInfo
```



**创建**

```go
myMap = make(map[string] PersonInfo)myMap = make(map[string] PersonInfo, 100)//指定初始容量//创建并初始化myMap = map[string] PersonInfo{ "1234": PersonInfo{"1", "Jack", "Room 101,..."},}
```



**赋值**

```go
myMap["1234"] = PersonInfo{"1", "Jack", "Room 101,..."} 
```



**删除**

```go
delete(myMap, "1234")
```

如果“1234”这个键不存在，那么这个调 用将什么都不发生，也不会有什么副作用。但是如果传入的map变量的值是nil，该调用将导致 程序抛出异常（panic）。



**查找**

```go
value, ok := myMap["1234"]if ok { // 找到了 // 处理找到的value} 
```





## 函数

### 函数与闭包

#### 函数定义

```go
func Add(a int, b int) (ret int, err error) { if a < 0 || b < 0 { // 假设这个函数只支持两个非负数字的加法 err= errors.New("Should be non-negative numbers!") return } return a + b, nil // 支持多重返回值} 
```



**参数/返回值列表简写**

```go
func Add(a, b int)(ret int, err error) { // ...} 
```

如果返回值列表中多个返回值的类型相同，也可以用同样的方式合并。 如果函数只有一个返回值，也可以这么写：

```go
func Add(a, b int) int { // ...}
```



**任意类型参数**

```go
func Printf(format string, args ...interface{}) { // ...} 
```





#### 不定参数

```go
func myfunc(args ...int) { for _, arg := range args { fmt.Println(arg) }} 
```

形如...type格式的类型只能作为函数的参数类型存在，并且必须是最后一个参数。它是一 个语法糖（syntactic sugar），即这种语法对语言的功能并没有影响，但是更方便程序员使用。通 常来说，使用语法糖能够增加程序的可读性，从而减少程序出错的机会。

类型...type**本质上是一个数组切片，也就是[]type**，这也是为 什么上面的参数args可以用for循环来获得每个传入的参数。



**不定参数的传递**

```go
func myfunc(args ...int) { // 按原样传递 myfunc3(args...) // 传递片段，实际上任意的int slice都可以传进去 myfunc3(args[1:]...)}
```









#### 匿名函数

在Go里面，函数可以像普通变量一样被传递或使用，这与C语言的回调函数比较类似。不同 的是，Go语言支持随时在代码里定义匿名函数。 匿名函数由一个不带函数名的函数声明和函数体组成:

```go
func(a, b int, z float64) bool { return a*b <int(z)} 
```

匿名函数可以直接赋值给一个变量或者直接执行

```go
f := func(x, y int) int { return x + y} //此时f是一个函数f := func(ch chan int) { ch <- ACK} (reply_chan) // 花括号后直接跟参数列表表示函数调用,执行匿名函数//此时f是匿名函数的返回值,及int类型变量
```



#### 闭包

```go
func bibao() {	f := func() (func()) {		var i = 10		return func() { //f2			i++			fmt.Print(i, ", ")		}	}()  //注意这里有括号,即f赋值的是匿名函数的返回值,即另一个函数f2,此时的f2即为闭包函数,调用f可以实现累加	f()	f()}
```



### 2.6 错误处理

**error接口**

```go
type error interface { Error() string}
```

对于大多数函数，如果要返回错误，大致上都可以定义为如下模式，将error作为多种返回 值中的最后一个，但这并非是强制要求： 

```go
func Foo(param int)(n int, err error) { // ... } 
```

调用时的代码建议按如下方式处理错误情况： 

```go
n, err := Foo(0) if err != nil { // 错误处理 } else { // 使用返回值n }
```

 

**自定义error类型**

```go
type PathError struct { Op string Path string Err error} //实现了Error接口方法func (e *PathError) Error() string { return e.Op + " " + e.Path + ": " + e.Err.Error()} 
```





## 面向对象编程

### 类型系统

#### 为类型添加方法

func (类型实例变量声明) 方法签名

如果方法签名和某个接口的方法相同则"实现"了这个接口

**指针还是值**

go都是值传递,所以

- 如果要改变对象的内容,则使用指针
- 否则(即只读)
  - 如果值所占内存很小使用值传递
  - 否则使用指针传递



#### 值语义和引用语义

**数组**

值语义, b = a赋值时会赋值

b = &a是指针用法,地址赋值

![image-20210607195440652](C:\Users\v_sxhthuang\AppData\Roaming\Typora\typora-user-images\image-20210607195440652.png)

**切片**

`s`如果使用`s...`符号解压缩切片，则可以将切片直接传递给可变参数函数。在这种情况下，不会创建新的切片。

示例

```
package mainimport "fmt"func main() {    //multiParam 可以接受可变数量的参数    names := []string{"jerry", "herry"}    multiParam(names...)}func multiParam(args ...string) {    //接受的参数放在args数组中    for _, e := range args {        fmt.Println(e)    }}
```

还有一种情况就是通过append合并两个slice,

```
    stooges := []string{"Moe", "Larry", "Curly"}    lang := []string{"php", "golang", "java"}    stooges = append(stooges, lang...)    fmt.Println(stooges)//[Moe Larry Curly php golang java]
```



**map**

引用语义,按值语义理解,即复制,但是复制的是包装的底层map实现的指针变量

将它们设计为引用类型而不是统一的值类型的原因 是，完整复制一个channel或map并不是常规需求。



**接口**

同样，接口具备引用语义，是因为内部维持了两个指针，示意为： 

```go
type interface struct {    data *void itab *Itab } 
```



## 结构体



**定义**

```go
type Rect struct {    x, y float64	width, height float64}
```





**初始化**

```go
rect1 := new(Rect)rect2 := &Rect{}rect3 := &Rect{0, 0, 100, 200}rect4 := &Rect{width: 100, height: 200} 
```



## 项目构建

### package

main.go是程序入口,必须在main包下.

go的包不要求和目录同名,即a目录下的main.go文件允许package main



### import



**用法**

1. 导入系统包或第三方软件包

```go
import "fmt"import "github.com/astaxie/beego"  
```

2. 调用导入包的init函数

```go
_ "github.com/go-sql-driver/mysql"
```



3. 别名导入

```go
import f "fmt"  //f可以代替fmt, 比如f.Printlnimport . "fmt"  //包名可以省略, 比如Println
```



**说明**

- import的路径问题

```go
import "./controller" // 相对路径 不推荐import "models/User" // 相对gopath src目录下 推荐
```

- 包的导入过程说明

程序的初始化和执行都起始于main包。如果main包还导入了其它的包，那么就会在编译时将它们依次导入。有时一个包会被多个包同时导入，那么它只会被导入一次（例如很多包可能都会用到fmt包，但它只会被导入一次，因为没有必要导入多次）。

当一个包被导入时，如果该包还导入了其它的包，那么会先将其它包导入进来，然后再**对这些包中的包级常量和变量进行初始化**，接着**执行init函数**（如果有的话），依次类推。等所有被导入的包都加载完毕了，就会开始对main包中的包级常量和变量进行初始化，然后执行main包中的init函数（如果存在的话），最后执行main函数。下图详细地解释了整个执行过程：



### 构建程序

- `go build` 编译并安装自身包和依赖包
- `go install` 安装自身包和依赖包
- `go run` build + run



### 格式化代码

在命令行输入 `gofmt –w program.go `会格式化该源文件的代码然后将格式化后的代码覆盖原始内容（如果不加参数 -w 则只会打印格式化后的结果而不重写文件）；`gofmt -w *.go` 会格式化并重写所有 Go 源文件；`gofmt map1 `会格式化并重写 map1 目录及其子目录下的所有 Go 源文件。

gofmt 也可以通过在参数 -r 后面加入用双引号括起来的替换规则实现代码的简单重构，规则的格式：<原始内容> -> <替换内容>。



### 生成代码文档

go doc 工具会从 Go 程序和包文件中提取顶级声明的首行注释以及每个对象的相关注释，并生成相关文档。

它也可以作为一个提供在线文档浏览的 web 服务器，golang.org 就是通过这种形式实现的。



