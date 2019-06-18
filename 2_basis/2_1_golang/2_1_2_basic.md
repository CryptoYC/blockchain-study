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

~~当然你也可以编写多个导入语句 ~~
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

