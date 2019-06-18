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



## 恭喜！

你已经完成了本课程！ 
 

