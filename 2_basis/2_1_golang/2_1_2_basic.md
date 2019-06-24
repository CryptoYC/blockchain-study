# Golang指北

## 欢迎

欢迎来到Evolab的Golang指北！首先向大家介绍一下Golang的特性：静态强类型、编译型、并发型编程语言，支持面向对象（~~Golang是面向对象语言~~），自带垃圾回收功能。

像所有语言一样，我们先用代码来欢迎你！

```
package main

import "fmt"

func main() {
	fmt.Println("Hello, world~")
}
```

## 包

可以看到我们之前欢迎大家的代码第一行:`package main`
它表明我们目前在main包，也是我们程序开始运行的地方，更确切一点，程序从main包的`func main() {}`开始。

接下来是`import "fmt"`，这也是包的一种使用方式，即引用。
一般来说，我们会引用Golang自身提供的一些工具包来实现自己的程序，如"fmt"、"io/ioutil"、"log"、"math"、"os"、"path/filepath"、"strings"、"unicode"、"unsafe"等实用包文件。
（使用Golang提供的包中的函数而未引用时，可以快捷键自动导入！）

最后就是我们刚刚导入的"fmt"包中的打印函数了，包名+"."（指针操作符）+函数即可。

### 分组导入

看完前两行，我们再看一段程序：
```
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}
```
此代码用圆括号组合了导入，这是“分组”形式的导入语句（推荐）。 

结果

Now you have 2.6457513110645907 problems.

~~当然你也可以编写多个导入语句~~
```
import "fmt"
import "math"
```

### 导出名

在 Go 中，如果一个名字以大写字母开头，那么它就是已导出的。例如，Pizza 就是个已导出名，Pi 也同样，它导出自 math 包。 
pizza 和 pi 并未以大写字母开头，所以它们是未导出的。 

在导入一个包时，你只能引用其中已导出的名字。任何“未导出”的名字在该包外均无法访问。 
执行代码，观察错误输出。 

然后将 math.pi 改名为 math.Pi 再试着执行一次。 

结果

./*.go:9:14: cannot refer to unexported name math.pi

./*.go:9:14: undefined: math.pi

## 函数

函数可以没有参数或接受多个参数。 
在本例中，add 接受两个 int 类型的参数。 
注意类型在变量名之后。 

（参考 [这篇关于 Go 语法声明的文章](https://blog.go-zh.org/gos-declaration-syntax)了解这种类型声明形式出现的原因。） 

```
package main

import "fmt"

func add(x int, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}
```
结果

55

当连续两个或多个函数的已命名形参类型相同时，除最后一个类型以外，其它都可以省略。 
```
package main

import "fmt"

func add(x, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}

```

结果

55

本例中`x int, y int`被缩写为 `x, y int`

### 多值返回

函数可以返回任意数量的返回值。 
```
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}

```

结果

world hello

swap 函数返回了两个字符串。

### 命名返回值
Go 的返回值可被命名，它们会被视作定义在函数顶部的变量。 

返回值的名称应当具有一定的意义，它可以作为文档使用。 

没有参数的 return 语句返回已命名的返回值。也就是 直接 返回。 

直接返回语句应当仅用在下面这样的短函数中。在长的函数中它们会影响代码的可读性。 

```
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}

```

结果

7 10

### 变量
var 语句用于声明一个变量列表，跟函数的参数列表一样，类型在最后。 
```
package main

import "fmt"

var c, python, java bool

func main() {
	var i int
	fmt.Println(i, c, python, java)
}
```

结果

0, false, false, false

就像在这个例子中看到的一样，var 语句可以出现在包或函数级别。

### 变量的初始化

变量声明可以包含初始值，每个变量对应一个。

如果初始化值已存在，则可以省略类型；变量会从初始值中获得类型。 

```
package main

import "fmt"

var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}
```

结果

1 2 true false no!

### 短变量声明

在函数中，简洁赋值语句 := 可在类型明确的地方代替 var 声明。 

函数外的每个语句都必须以关键字开始（var, func 等等），因此 := 结构不能在函数外使用。 

```
package main

import "fmt"

func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

结果

1 2 3 true false no!

### 基本类型

Go 的基本类型有 

>bool

>string

>int  int8  int16  int32  int64
>uint uint8 uint16 uint32 uint64 uintptr

>byte // uint8 的别名

>rune // int32 的别名 表示一个 Unicode 码点

>float32 float64

>complex64 complex128

本例展示了几种类型的变量。 同导入语句一样，变量声明也可以“分组”成一个语法块。 

int, uint 和 uintptr 在 32 位系统上通常为 32 位宽，在 64 位系统上则为 64 位宽。 当你需要一个整数值时应使用 int 类型，除非你有特殊的理由使用固定大小或无符号的整数类型。 

```
package main

import (
	"fmt"
	"math/cmplx"
)

var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)

func main() {
	fmt.Printf("Type: %T Value: %v\n", ToBe, ToBe)
	fmt.Printf("Type: %T Value: %v\n", MaxInt, MaxInt)
	fmt.Printf("Type: %T Value: %v\n", z, z)
}
```

结果

Type: bool Value: false

Type: uint64 Value: 18446744073709551615

Type: complex128 Value: (2+3i)




### 零值
没有明确初始值的变量声明会被赋予它们的 零值。 

零值是： 

数值类型为 0，布尔类型为 false，字符串为 ""（空字符串）。

```
package main

import "fmt"

func main() {
	var i int
	var f float64
	var b bool
	var s string
	fmt.Printf("%v %v %v %q\n", i, f, b, s)
}
```

结果

0 0 false ""


### 类型转换

表达式 T(v) 将值 v 转换为类型 T。 

一些关于数值的转换： 
```
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```
或者，更加简单的形式： 
```
i := 42
f := float64(i)
u := uint(f)
```
与 C 不同的是，Go 在不同类型的项之间赋值时需要显式转换。试着移除例子中 float64 或 uint 的转换看看会发生什么。
```
package main

import (
	"fmt"
	"math"
)

func main() {
	var x, y int = 3, 4
	var f float64 = math.Sqrt(float64(x*x + y*y))
	var z uint = uint(f)
	fmt.Println(x, y, z)
}
```
结果

3 4 5

~*.go:10:32: cannot use x * x + y * y (type int) as type float64 in argument to math.Sqrt~

~*.go:11:6: cannot use f (type float64) as type uint in assignment~

### 类型推导
在声明一个变量而不指定其类型时（即使用不带类型的 := 语法或 var = 表达式语法），变量的类型由右值推导得出。
 
当右值声明了类型时，新变量的类型与其相同： 
```
var i int
j := i // j 也是一个 int
```
不过当右边包含未指明类型的数值常量时，新变量的类型就可能是 int, float64 或 complex128 了，这取决于常量的精度： 
```
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```
尝试修改示例代码中 v 的初始值，并观察它是如何影响类型的。 
```
package main

import "fmt"

func main() {
	v := 42 // 修改这里！
	fmt.Printf("v is of type %T\n", v)
}
```

结果

v is of type int

~v is of type float64~

~v is of type complex128~

### 常量
常量的声明与变量类似，只不过是使用 const 关键字。

常量可以是字符、字符串、布尔值或数值。 

常量不能用 := 语法声明。 
```
package main

import "fmt"

const Pi = 3.14

func main() {
	const World = "世界"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}
```

结果

Hello 世界

Happy 3.14 Day

Go rules? true

### 数值常量
数值常量是高精度的值。 

一个未指定类型的常量由上下文来决定其类型。 

再尝试一下输出 needInt(Big) 吧。 

（int 类型最大可以存储一个 64 位的整数，有时会更小。） 

（int 可以存放最大64位的整数，根据平台不同有时会更少。） 
```
package main

import "fmt"

const (
	// 将 1 左移 100 位来创建一个非常大的数字
	// 即这个数的二进制是 1 后面跟着 100 个 0
	Big = 1 << 100
	// 再往右移 99 位，即 Small = 1 << 1，或者说 Small = 2
	Small = Big >> 99
)

func needInt(x int) int { return x*10 + 1 }
func needFloat(x float64) float64 {
	return x * 0.1
}

func main() {
	fmt.Println(needInt(Small))
	fmt.Println(needFloat(Small))
	fmt.Println(needFloat(Big))
}
```
结果

21

0.2

1.2676506002282295e+29

## for
Go 只有一种循环结构：for 循环。 

基本的 for 循环由三部分组成，它们用分号隔开： 

初始化语句：在第一次迭代前执行

条件表达式：在每次迭代前求值

后置语句：在每次迭代的结尾执行

初始化语句通常为一句短变量声明，该变量声明仅在 for 语句的作用域中可见。 

一旦条件表达式的布尔值为 false，循环迭代就会终止。 

注意：和 C、Java、JavaScript 之类的语言不同，Go 的 for 语句后面的三个构成部分外没有小括号， 大括号 { } 则是必须的。 
```
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```

结果

45

其中初始化语句和后置语句是可选的。 
```
package main

import "fmt"

func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}
```
结果

1024

### for 是 Go 中的 “while”
此时你可以去掉分号，因为 C 的 while 在 Go 中叫做 for。 
```
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```

结果

1024

### 无限循环
如果省略循环条件，该循环就不会结束，因此无限循环可以写得很紧凑。 
```
package main

func main() {
	for {
	}
}
```

结果

<font color=#FF0000>process took too long</font>

## if
Go 的 if 语句与 for 循环类似，表达式外无需小括号 ( ) ，而大括号 { } 则是必须的

```
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}
```
结果

1.4142135623730951 2i

### if的简短语句
同 for 一样， if 语句可以在条件表达式前执行一个简单的语句。 

该语句声明的变量作用域仅在 if 之内。 

```
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

结果

9 20

（在最后的 return 语句处使用 v 看看。） 

~*.go:12:9: undefined: v~

### if 和 else

在 if 的简短语句中声明的变量同样可以在任何对应的 else 块中使用。 

（在 main 的 fmt.Println 调用开始前，两次对 pow 的调用均已执行并返回其各自的结果。）
```
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// 这里开始就不能使用 v 了
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

结果

27 >= 20

9 20

## 练习：循环与函数
为了练习函数与循环，我们来实现一个平方根函数：用牛顿法实现平方根函数。 

计算机通常使用循环来计算 x 的平方根。从某个猜测的值 z 开始，我们可以根据 z² 与 x 的近似度来调整 z，产生一个更好的猜测： 

`z -= (z*z - x) / (2*z)`

重复调整的过程，猜测的结果会越来越精确，得到的答案也会尽可能接近实际的平方根。 

在提供的 func Sqrt 中实现它。无论输入是什么，对 z 的一个恰当的猜测为 1。 要开始，请重复计算 10 次并随之打印每次的 z 值。观察对于不同的值 x（1、2、3 ...）， 你得到的答案是如何逼近结果的，猜测提升的速度有多快。 

提示：用类型转换或浮点数语法来声明并初始化一个浮点数值： 
```
z := 1.0
z := float64(1)
```

然后，修改循环条件，使得当值停止改变（或改变非常小）的时候退出循环。观察迭代次数大于还是小于 10。 尝试改变 z 的初始猜测，如 x 或 x/2。你的函数结果与标准库中的 math.Sqrt 接近吗？ 

（*注：* 如果你对该算法的细节感兴趣，上面的 z² − x 是 z² 到它所要到达的值（即 x）的距离， 除以的 2z 为 z² 的导数，我们通过 z² 的变化速度来改变 z 的调整量。 这种通用方法叫做牛顿法。 它对很多函数，特别是平方根而言非常有效。） 

```
package main

import (
	"fmt"
)

func Sqrt(x float64) float64 {
}

func main() {
	fmt.Println(Sqrt(2))
}
```
```package main

import (
	"fmt"
	"math"
)

func Sqrt(x float32) float32 {
	var xhalf float32 = 0.5*x // get bits for floating VALUE 
    	i := math.Float32bits(x) // gives initial guess y0
	i = 0x5f375a86 - (i>>1) // convert bits BACK to float
    	x = math.Float32frombits(i) // Newton step, repeating increases accuracy
    	x = x*(1.5-xhalf*x*x)
    	x = x*(1.5-xhalf*x*x)
   	x = x*(1.5-xhalf*x*x)
 	return 1/x 
}

func main() {
	fmt.Println(Sqrt(2))
}
```

结果

1.4142135

### switch

一个结构体`struct`就是一个字段的集合，除非以`fallthrough`语句结束，否则分支会自动终止。 
```
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.", os)
	}
}
```

结果


Go runs on Linux.


### switch的执行顺序

switch 的条件从上到下的执行，当匹配成功的时候停止，例如：
```
switch i {
	case 0:
	case f():
}
```

当 i==0 时不会调用 `f`。

```
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Println("When's Saturday?")
	today := time.Now().Weekday()
	switch time.Saturday {
	case today + 0:
		fmt.Println("Today.")
	case today + 1:
		fmt.Println("Tomorrow.")
	case today + 2:
		fmt.Println("In two days.")
	default:
		fmt.Println("Too far away.")
	}
}
```

结果

When's Saturday?

Too far away.

### 无条件switch

没有条件的 switch 同 `switch true` 一样。 

这一构造使得可以用更清晰的形式来编写长的 if-then-else 链。 

```
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```

结果

Good afternoon.

### defer

defer 语句会延迟函数的执行直到上层函数返回。 

延迟调用的参数会立刻生成，但是在上层函数返回前函数都不会被调用。 

```
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

结果

hello

world

### defer栈

延迟的函数调用被压入一个栈中。当函数返回时， 会按照后进先出的顺序调用被延迟的函数调用。 

（阅读 [博文](blog.golang.org/defer-panic-and-recover)了解更多关于 defer 语句的信息。 

```
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

结果

counting

done

9

8

7

6

5

4

3

2

1

0

### 指针
Go 具有指针。 指针保存了变量的内存地址。 

类型`*T`是指向类型`T`的值的指针。其零值是`nil`。 

`var p *int`

`& `符号会生成一个指向其作用对象的指针。
```
i := 42
p = &i
```
`* `符号表示指针指向的底层的值。
```
fmt.Println(*p) // 通过指针 p 读取 i
*p = 21         // 通过指针 p 设置 i
```
这也就是通常所说的“间接引用”或“非直接引用”。 

与 C 不同，Go 没有指针运算。 
```package main

import "fmt"

func main() {
	i, j := 42, 2701

	p := &i         // point to i
	fmt.Println(*p) // read i through the pointer
	*p = 21         // set i through the pointer
	fmt.Println(i)  // see the new value of i

	p = &j         // point to j
	*p = *p / 37   // divide j through the pointer
	fmt.Println(j) // see the new value of j
}
```

结果

42

21

73

### 结构体

一个结构体`struct`就是一个字段的集合，而`type`的含义跟其字面意思相符。 

```
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	fmt.Println(Vertex{1, 2})
}
```

结果

{1 2}

### 结构体字段
结构体字段使用点号来访问。 

```
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)
}
```

结果

4

### 结构体指针
结构体字段可以通过结构体指针来访问。 

通过指针间接的访问是透明的。 

```
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
	p.X = 1e9
	fmt.Println(v)
}
```

结果

{1000000000 2}

### 结构体文法
结构体文法表示通过结构体字段的值作为列表来新分配一个结构体。 

使用`Name: `语法可以仅列出部分字段。（字段名的顺序无关。） 

特殊的前缀`&`返回一个指向结构体的指针。 

```
package main

import "fmt"

type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // 类型为 Vertex
	v2 = Vertex{X: 1}  // Y:0 被省略
	v3 = Vertex{}      // X:0 和 Y:0
	p  = &Vertex{1, 2} // 类型为 *Vertex
)

func main() {
	fmt.Println(v1, p, v2, v3)
}
```
结果

{1 2} &{1 2} {1 0} {0 0}

### 数组
类型`[n]T`是一个有 n 个类型为`T`的值的数组，表达式 

`var a [10]int`

定义变量`a`是一个有十个整数的数组。 

数组的长度是其类型的一部分，因此数组不能改变大小。 这看起来是一个制约，但是请不要担心； Go 提供了更加便利的方式来使用数组。

```
package main

import "fmt"

func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)
}
```

结果

Hello World

\[Hello World]

### slice
一个`slice`会指向一个序列的值，并且包含了长度信息。 

`[]T`是一个元素类型为`T`的`slice`。 
```
package main

import "fmt"

func main() {
	p := []int{2, 3, 5, 7, 11, 13}
	fmt.Println("p ==", p)

	for i := 0; i < len(p); i++ {
		fmt.Printf("p[%d] == %d\n", i, p[i])
	}
}
```
结果

p == \[2 3 5 7 11 13]

p\[0] == 2

p\[1] == 3

p\[2] == 5

p\[3] == 7

p\[4] == 11

p\[5] == 13

### 对 slice 切片

`slice`可以重新切片，创建一个新的`slice`值指向相同的数组，表达式

`s[lo:hi]`

表示从 lo 到 hi-1 的`slice`元素，含两端。

因此`s[lo:lo]`是空的，而`s[lo:lo+1]`有一个元素。 

```
package main

import "fmt"

func main() {
	p := []int{2, 3, 5, 7, 11, 13}
	fmt.Println("p ==", p)
	fmt.Println("p[1:4] ==", p[1:4])

	// 省略下标代表从 0 开始
	fmt.Println("p[:3] ==", p[:3])

	// 省略上标代表到 len(s) 结束
	fmt.Println("p[4:] ==", p[4:])
}
```
结果

p == \[2 3 5 7 11 13]

p\[1:4] == \[3 5 7]

p\[:3] == \[2 3 5]

p\[4:] == \[11 13]

### 构造 slice

`slice`由函数`make`创建。这会分配一个零长度的数组并且返回一个`slice`指向这个数组：

`a := make([]int, 5)  // len(a)=5`

为了指定容量，可传递第三个参数到 `make`： 
```
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```
```
package main

import "fmt"

func main() {
	a := make([]int, 5)
	printSlice("a", a)
	b := make([]int, 0, 5)
	printSlice("b", b)
	c := b[:2]
	printSlice("c", c)
	d := c[2:5]
	printSlice("d", d)
}

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}
```

结果

a len=5 cap=5 \[0 0 0 0 0]

b len=0 cap=5 \[]

c len=2 cap=5 \[0 0]

d len=3 cap=3 \[0 0 0]

### nil slice
`slice`的零值是 `nil`，一个`nil`的`slice`的长度和容量是 0。 

```
package main

import "fmt"

func main() {
	var z []int
	fmt.Println(z, len(z), cap(z))
	if z == nil {
		fmt.Println("nil!")
	}
}
```

结果

\[] 0 0

nil!

### 向 slice 添加元素
向`slice`添加元素是一种常见的操作，因此 Go 提供了一个内建函数 `append`。 内建函数的文档对`append`有详细介绍。 

`func append(s []T, vs ...T) []T`

`append`的第一个参数`s`是一个类型为`T`的数组，其余类型为`T`的值将会添加到`slice`。 

`append`的结果是一个包含原`slice`所有元素加上新添加的元素的`slice`。

如果`s`的底层数组太小，而不能容纳所有值时，会分配一个更大的数组。 返回的`slice`会指向这个新分配的数组。 

（了解更多关于`slice`的内容，参阅文章[slice：使用和内幕](https://blog.golang.org/go-slices-usage-and-internals)。） 
```
package main

import "fmt"

func main() {
	var a []int
	printSlice("a", a)

	// append works on nil slices.
	a = append(a, 0)
	printSlice("a", a)

	// the slice grows as needed.
	a = append(a, 1)
	printSlice("a", a)

	// we can add more than one element at a time.
	a = append(a, 2, 3, 4)
	printSlice("a", a)
}

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}
```

结果

a len=0 cap=0 \[]

a len=1 cap=1 \[0]

a len=2 cap=2 \[0 1]

a len=5 cap=6 \[0 1 2 3 4]

### range
`for`循环的`range`格式可以对`slice`或者`map`进行迭代循环。 
```
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```
结果

2\*\*0 = 1

2\*\*1 = 2

2\*\*2 = 4

2\*\*3 = 8

2\*\*4 = 16

2\*\*5 = 32

2\*\*6 = 64

2\*\*7 = 128

可以通过赋值给`_`来忽略序号和值。 

如果只需要索引值，去掉`, value`的部分即可。 
```
package main

import "fmt"

func main() {
	pow := make([]int, 10)
	for i := range pow {
		pow[i] = 1 << uint(i)
	}
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}
```

结果

1

2

4

8

16

32

64

128

256

512

### map
`map`映射键到值。

`map`在使用之前必须用`make`而不是`new`来创建；值为`nil`的`map`是空的，并且不能赋值。 

```
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}
```

结果

{40.68433 -74.39967}

如果顶级的类型只有类型名的话，可以在文法的元素中省略键名。 

```
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}

func main() {
	fmt.Println(m)
}
```
结果

map[Bell Labs:{40.68433 -74.39967} Google:{37.42202 -122.08408}]

### 修改 map
在 map m 中插入或修改一个元素： `m[key] = elem`

获得元素： `elem = m[key]`

删除元素： `delete(m, key)`

通过双赋值检测某个键存在： `elem, ok = m[key]`

如果`key`在`m`中，`ok` 为`true`。否则，`ok`为 `false`，并且`elem`是`map`的元素类型的零值。 

同样的，当从`map`中读取某个不存在的键时，结果是`map`的元素类型的零值。 
```
package main

import "fmt"

func main() {
	m := make(map[string]int)

	m["Answer"] = 42
	fmt.Println("The value:", m["Answer"])

	m["Answer"] = 48
	fmt.Println("The value:", m["Answer"])

	delete(m, "Answer")
	fmt.Println("The value:", m["Answer"])

	v, ok := m["Answer"]
	fmt.Println("The value:", v, "Present?", ok)
}
```

结果

The value: 42

The value: 48

The value: 0

The value: 0 Present? false

### 函数值
函数也是值。 
```
package main

import (
	"fmt"
	"math"
)

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}

	fmt.Println(hypot(3, 4))
}
```

结果

5

### 函数的闭包

Go 函数可以是闭包的。闭包是一个函数值，它来自函数体的外部的变量引用。 函数可以对这个引用值进行访问和赋值；换句话说这个函数被“绑定”在这个变量上。 

例如，函数`adder`返回一个闭包。每个闭包都被绑定到其各自的`sum`变量上。 
```
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
```

结果

0 0

1 -2

3 -6

6 -12

10 -20

15 -30

21 -42

28 -56

36 -72

45 -90

### 练习：斐波纳契闭包
现在来通过函数做些有趣的事情。 

实现一个 fibonacci 函数，返回一个函数（一个闭包）可以返回连续的斐波纳契数。 

```
package main

import "fmt"

// fibonacci 函数会返回一个返回 int 的函数。
func fibonacci() func() int {
	var a int = 0
	var b int = 1
	return func() int {
		c := a
		a = b
		b = a + c
		return c
	}
}
/* another way
func fibonacci() func() int {
	a, b := 0, 1
	return func() int {
		ret := a
		a, b = b, a+b 
		return ret
	}
}
*/
/* another way
func fibonacci() func() int {
	a, b := 0, 1
	return func() int {
		defer func() {
			a, b = b, a+b
		}()
		return a
	}
}
*/
func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
```

结果

0

1

1

2

3

5

8

13

21

34

### 方法
Go 没有类。然而，仍然可以在结构体类型上定义方法。 

方法接收者 出现在`func`关键字和方法名之间的参数中。 
```
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := &Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

结果

5

你可以对包中的 任意 类型定义任意方法，而不仅仅是针对结构体。 

但是，不能对来自其他包的类型或基础类型定义方法。
```
package main

import (
	"fmt"
	"math"
)

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

func main() {
	f := MyFloat(-math.Sqrt2)
	fmt.Println(f.Abs())
}
```
结果

1.4142135623730951

### 接收者为指针的方法

方法可以与命名类型或命名类型的指针关联。 

刚刚看到的两个`Abs`方法。一个是在`*Vertex`指针类型上，而另一个在`MyFloat`值类型上。 

有两个原因需要使用指针接收者。

首先避免在每个方法调用中拷贝值（如果值类型是大的结构体的话会更有效率）。

其次，方法可以修改接收者指向的值。 

尝试修改`Abs`的定义，同时`Scale`方法使用`Vertex`代替`*Vertex`作为接收者。

当`v`是`Vertex`的时候`Scale`方法没有任何作用。`Scale` 修改 `v`。

当`v`是一个值（非指针），方法看到的是`Vertex`的副本，并且无法修改原始值。 

`Abs`的工作方式是一样的。只不过，仅仅读取 `v`。所以读取的是原始值（通过指针）还是那个值的副本并没有关系。 
```
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := &Vertex{3, 4}
	v.Scale(5)
	fmt.Println(v, v.Abs())
}
```
结果

&{15 20} 25

### 接口
接口类型是由一组方法定义的集合。 

接口类型的值可以存放实现这些方法的任何值。 

```
package main

import (
	"fmt"
	"math"
)

type Abser interface {
	Abs() float64
}

func main() {
	var a Abser
	f := MyFloat(-math.Sqrt2)
	v := Vertex{3, 4}

	a = f  // a MyFloat 实现了 Abser
	a = &v // a *Vertex 实现了 Abser

	// 下面一行，v 是一个 Vertex（而不是 *Vertex）
	// 所以没有实现 Abser。
	a = v

	fmt.Println(a.Abs())
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

*注意*： 示例代码的 22 行存在一个错误。 由于`Abs`只定义在`*Vertex`（指针类型） 上， 所以`Vertex`（值类型） 不满足 `Abser`。

结果

.\*.go:287:4: cannot use v (type Vertex) as type Abser in assignment:
	Vertex does not implement Abser (Abs method has pointer receiver)

将`main()`中`a = v`注释掉，结果

5

### 隐式接口
类型通过实现那些方法来实现接口。 

没有显式声明的必要；所以也就没有关键字`implements`。 

隐式接口解藕了实现接口的包和定义接口的包：互不依赖。 

因此，也就无需在每一个实现上增加新的接口名称，这样同时也鼓励了明确的接口定义。 

包`io`定义了`Reader`和 `Writer`；其实不一定要这么做。 
```
package main

import (
	"fmt"
	"os"
)

type Reader interface {
	Read(b []byte) (n int, err error)
}

type Writer interface {
	Write(b []byte) (n int, err error)
}

type ReadWriter interface {
	Reader
	Writer
}

func main() {
	var w Writer

	// os.Stdout 实现了 Writer
	w = os.Stdout

	fmt.Fprintf(w, "hello, writer\n")
}
```

结果

hello, writer

### Stringers
一个普遍存在的接口是`fmt`包中定义的`Stringer`。 
```
type Stringer interface {
    String() string
}
```
`Stringer`是一个可以用字符串描述自己的类型。`fmt`包 （还有许多其他包）使用这个来进行输出。 

```
package main

import "fmt"

type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
	a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}
	fmt.Println(a, z)
}
```
结果

Arthur Dent (42 years) Zaphod Beeblebrox (9001 years)

### 练习：Stringers
让 IPAddr 类型实现 fmt.Stringer 以便用点分格式输出地址。

例如，`IPAddr{1,`2,`3,`4}` 应当输出 `"1.2.3.4"`。 
```
package main

import "fmt"

type IPAddr [4]byte

// TODO: Add a "String() string" method to IPAddr.
func (ip_addr IPAddr) String() string {
	return fmt.Sprintf("%v.%v.%v.%v", ip_addr[0], ip_addr[1], ip_addr[2], ip_addr[3])
}

func main() {
	addrs := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for n, a := range addrs {
		fmt.Printf("%v: %v\n", n, a)
	}
}
```

结果

loopback: 127.0.0.1

googleDNS: 8.8.8.8

### 错误
Go 程序使用`error`值来表示错误状态。 

与`fmt.Stringer`类似，`error` 类型是一个内建接口： 
```
type error interface {
    Error() string
}
```
（与`fmt.Stringer`类似，`fmt` 包在输出时也会试图匹配 `error`。） 
通常函数会返回一个`error`值，调用的它的代码应当判断这个错误是否等于 `nil`， 来进行错误处理。 
```
i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("couldn't convert number: %v\n", err)
}
fmt.Println("Converted integer:", i)
```
`error`为`nil`时表示成功；非`nil`的`error`表示错误。 
```
package main

import (
	"fmt"
	"time"
)

type MyError struct {
	When time.Time
	What string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s",
		e.When, e.What)
}

func run() error {
	return &MyError{
		time.Now(),
		"it didn't work",
	}
}

func main() {
	if err := run(); err != nil {
		fmt.Println(err)
	}
}
```

结果

at 2019-06-24 16:36:13.2975566 +0800 CST m=+0.002014401, it didn't work

### 练习：错误
从之前的练习中复制`Sqrt`函数，并修改使其返回`error`值。 

`Sqrt`接收到一个负数时，应当返回一个非`nil`的错误值。复数同样也不被支持。 

创建一个新类型`type ErrNegativeSqrt float64`为其实现 `func (e ErrNegativeSqrt) Error() string`使其成为一个 `error`

该方法就可以让`ErrNegativeSqrt(-2).Error()`返回 `"cannot Sqrt negative number: -2"`。 

*注意*： 在`Error`方法内调用`fmt.Sprint(e)`将会让程序陷入死循环。可以通过先转换`e`来避免这个问题：`fmt.Sprint(float64(e))`。请思考这是为什么呢？ 
修改`Sqrt`函数，使其接受一个负数时，返回`ErrNegativeSqrt`值。 

```
```

结果


## 恭喜！

你已经完成了本课程！ 
 

