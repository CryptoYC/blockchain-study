# Golang指北

## 欢迎

欢迎来到Evolab的Golang指北！首先向大家介绍一下Golang的特性：静态强类型、编译型、并发型编程语言，支持面向对象（~~Golang是面向对象语言~~），自带垃圾回收功能。

像所有语言一样，我们先用代码来欢迎你！

```
package main

import "fmt"

func main() {
	fmt.Println("Hello, 世界")
}
```

## 包

可以看到我们之前欢迎大家的代码第一行:`package main`
它表明我们目前在main包，也是我们程序开始运行的地方，更确切一点，程序从main包的`func main() {}`开始。

接下来是`import "fmt"`，这也是包的一种使用方式，即引用。
一般来说，我们会引用Golang自身提供的一些工具包来实现自己的程序，如"fmt"、"io/ioutil"、"log"、"math"、"os"、"path/filepath"、"strings"、"unicode"、"unsafe"等实用包文件。
（使用Golang提供的包中的函数而未引用时，可以快捷键自动导入！）

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

