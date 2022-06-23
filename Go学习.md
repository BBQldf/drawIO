# Go学习

> 本blog只记录Go的使用过程。背景、运行环境、编辑器、IDE等工具直接跳过。对于数据结构、命名规则等部分也比较简略，只比较和Java不一样的一些地方，针对有语言基础的朋友参考。
>
> mainly supported by [Go 入门指南](https://learnku.com/docs/the-way-to-go)
>
> [PatrickChenSe](https://github.com/PatrickChenSe)/**[learnGO](https://github.com/PatrickChenSe/learnGO)**

## 一、基本结构和基本数据类型

### 1、文件名、关键字与标识符

> Go 语言也是区分大小写的

- Go 的源文件以 .go 为后缀名存储在计算机中，这些文件名均由小写字母组成，如 scanner.go 。如果文件名由多个部分组成，则使用下划线 _ 对它们进行分隔，如 scanner_test.go 。文件名不包含空格或其他特殊字符。(`_` 本身就是一个特殊的标识符，被称为空白标识符。)

- Go 代码中会使用到的 25 个关键字或保留字：


| break    | default     | func   | interface | select |
| -------- | ----------- | ------ | --------- | ------ |
| case     | defer       | go     | map       | struct |
| chan     | else        | goto   | package   | switch |
| const    | fallthrough | if     | range     | type   |
| continue | for         | import | return    | var    |

- Go 语言还有 36 个预定义标识符

| append | bool    | byte    | cap     | close  | complex | complex64 | complex128 | uint16  |
| ------ | ------- | ------- | ------- | ------ | ------- | --------- | ---------- | ------- |
| copy   | false   | float32 | float64 | imag   | int     | int8      | int16      | uint32  |
| int32  | int64   | iota    | len     | make   | new     | nil       | panic      | uint64  |
| print  | println | real    | recover | string | true    | uint      | uint8      | uintptr |

- 注意：程序的代码通过语句来实现结构化。每个语句不需要像 C 家族中的其它语言一样以分号 `;` 结尾

### 2、包的概念、导入与可见性

1. 每个 Go 文件都属于且仅属于一个包。一个包可以由许多以 `.go` 为扩展名的源文件组成，因此**文件名和包名一般来说都是不相同的。**
2. 所有的包名都应该使用小写字母
3. 在源文件中非注释的第一行指明这个文件属于哪个包，如：`package main`。`package main` 表示一个可独立执行的程序，每个 Go 应用程序（注意这里是应用程序，相当于Java的module）都包含一个名为 `main` 的包。
   - 只使用 main 包也不必把所有的代码都写在一个巨大的文件里：你可以用一些较小的文件，并且在每个文件非注释的第一行都使用 `package main` 来指明这些文件都属于 main 包。
   - 如果你打算编译包名不是为 main 的源文件，如 `pack1`，编译后产生的对象文件将会是 `pack1.a` 而不是可执行程序。
4. 一个 Go 程序是通过 `import` 关键字将一组包链接在一起。

**和Java的区别：**

1. Java中的package是组织和管理类文件(xx.java)的一个层级,最终实际上会转换为文件系统中的一个目录文件，可以理解为Java中的package就是个目录文件
2. 一个package中可以管理多个".java"源文件
3. Java中的"import"导入的不是包而是某个包中的某个类或所有类

**总之，**Java中的package实际上就可以理解为文件名，"import"导入的是包中(文件中)的某个类（．java文件）；

而go总的package不是指文件，但一个文件中只能有一个package,一个package组织多个".go"源文件（就package的范围是一致的），"import"导入的是文件而不是包，而程序中实际使用的是包（但是package的使用是不一样的）

#### 2.1 可见性规则

- 当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为**导出（**像面向对象语言中的 public）；

- 标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 private ）。
- **包也可以作为命名空间使用，**帮助避免命名冲突（名称冲突）：两个包中的同名变量的区别在于他们的包名，例如 `pack1.Thing` 和 `pack2.Thing`。

#### 2.2 函数

```go
func functionName(parameter_list) (return_value_list) {
   …
}
//parameter_list 的形式为 (param1 type1, param2 type2, …)
//return_value_list 的形式为 (ret1 type1, ret2 type2, …)
```



一些规定：

1. 函数里的代码（函数体）使用大括号 {} 括起来。  左大括号 { 必须与方法的声明放在同一行，这是编译器的强制规定（对于大括号 `{}` 的使用规则在任何时候都是相同的（如：if 语句等）。
2. 右大括号 `}` 需要被放在紧接着函数体的下一行。如果你的函数非常简短，你也可以将它们放在同一行
3. 只有**当某个函数需要被外部包调用的时候才使用大写字母开头**，并遵循 Pascal 命名法；否则就遵循骆驼命名法，即第一个单词的首字母小写，其余单词的首字母大写。

#### 2.3 注释

> 和C语言一致

- 几乎所有全局作用域的类型、常量、变量、函数和被导出的对象都应该有一个合理的注释。如果这种注释（称为文档注释）出现在函数前面，例如函数 Abcd，则要以 `"Abcd..."` 作为开头。

#### 2.4 类型

1. 类型可以是基本类型，如：int、float、bool、string；
2. 结构化的（复合的），如：struct、array、slice、map、channel；
   - 结构化的类型没有真正的值，它使用 nil 作为默认值（在 Objective-C 中是 nil，在 Java 中是 null，在 C 和 C++ 中是 NULL 或 0）
3. 只描述类型的行为的，如：interface。

一个函数可以拥有多返回值，返回类型之间需要使用逗号分割，并使用小括号 `()` 将它们括起来，如：

```go
func Atoi(s string) (i int, err error)
```

#### 2.5 Go 程序的一般结构

```go
package main

import (
   "fmt"
)

const c = "C"

var v int = 5

type T struct{}

func init() { // initialization of package,会在main执行之前运行
}

func main() {
   var a int
   Func1()
   // ...
   fmt.Println(a)
}

func (t T) Method1() {
   //...
}

func Func1() { // exported function Func1
   //...
}
```

1. 在完成包的 import 之后，开始对常量、变量和类型的定义或声明。
2. 如果存在 init 函数的话，则对该函数进行定义（这是一个特殊的函数，每个含有该函数的包都会首先执行这个函数）。
3. 如果当前包是 main 包，则定义 main 函数。
4. 然后定义其余的函数，首先是类型的方法，接着是按照 main 函数中先后调用的顺序来定义相关函数，如果有很多函数，则可以按照字母顺序来进行排序。

**Go 程序的执行（程序启动）顺序如下：**（这里和Java的init()顺序一致）

1. 按顺序导入所有被 main 包引用的其它包，然后在每个包中执行如下流程：

2. 如果该包又导入了其它的包，则从第一步开始递归执行，但是每个包只会被导入一次。
3. 然后以相反的顺序在每个包中初始化常量和变量，如果该包含有 init 函数的话，则调用该函数。
4. 在完成这一切之后，main 也执行同样的过程，最后调用 main 函数开始执行程序。

#### 2.6 类型转换

Go 语言不存在隐式类型转换，因此所有的转换都必须显式说明：`valueOfTypeB = typeB(valueOfTypeA)`：

```go
var v int = 5
a := 5.0
b := int(a)
```

具有相同底层类型的变量之间可以相互转换：

```go
type IZ int

var a IZ = 5
c := int(a)
d := IZ(c)
```

不同的类型只能在定义正确的情况下转换成功，如从一个取值范围较小的类型转换到一个取值范围较大的类型（例如将 int16 转换为 int32）。大转小（int32 转换为 int16）会截断；

### 3、常量&变量

#### 3.1 简介

1、**常量使用关键字 `const` 定义，用于存储不会改变的数据。**

- 存储在常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。
- 常量的定义格式：`const identifier [type] = value`
  - 显式类型定义： `const b string = "abc"`
  - 隐式类型定义： `const b = "abc"`
- 常量也允许使用并行赋值的形式：

```go
const beef, two, c = "eat", 2, "veg"
const Monday, Tuesday, Wednesday, Thursday, Friday, Saturday = 1, 2, 3, 4, 5, 6
const (
    Monday, Tuesday, Wednesday = 1, 2, 3
    Thursday, Friday, Saturday = 4, 5, 6
)
```

- 常量还可以用作枚举：

```go
const (
    Unknown = 0
    Female = 1
    Male = 2
)
```

```go
const (
    a = iota
    b = iota
    c = iota
)
//在这个例子中，iota 可以被用作枚举值：第一个 iota 等于 0，每当 iota 在新的一行被使用时，它的值都会自动加 1；所以 a=0, b=1, c=2 可以简写为如下形式：
const (
    a = iota
    b
    c
)
//每次 const 出现时，都会让 iota 初始化为0.
```

关于常量计数器iota，可以[参考](https://www.jianshu.com/p/08d6a4216e96)



2、Go 和许多编程语言不同，它在声明变量时将变量的类型放在变量的名称之后。**声明变量的一般形式是使用 `var` 关键字：`var identifier type`。**优势：

1. 它是为了避免像 C 语言中那样含糊不清的声明形式，例如：`int* a, b;`。在这个例子中，只有 a 是指针而 b 不是。如果你想要这两个变量都是指针，则需要将它们分开书写
2. 在 Go 中，则可以很轻松地将它们都声明为指针类型：`var a, b *int`

3、变量的命名规则遵循骆驼命名法，即首个单词小写，每个新单词的首字母大写，例如：`numShips` 和 `startDate`。但如果你的全局变量希望能够被外部包所使用，则需要将首个单词的首字母也大写（第 4.2 节：可见性规则）。

4、关于变量的作用域，和基本的语言保持了一致性，即在函数体外声明，则被认为是全局变量；在函数体内的就是局部变量

5、尽管变量的标识符必须是唯一的，但你可以在某个代码块的内层代码块中使用相同名称的变量，则此时外部的同名变量将会暂时隐藏（结束内部代码块的执行后隐藏的外部同名变量又会出现，而内部同名变量则被释放），你任何的操作都只会影响内部代码块的局部变量。**（这个设定还是很好的，既解决了像Java里面，不让定义同样的变量名，又解决了Python的全局变量在整体都会被改变的问题）**

6、 Go 编译器的智商已经高到可以根据变量的值来自动推断其类型，**这有点像 Ruby 和 Python 这类动态语言，只不过它们是在运行时进行推断，而 Go 是在编译时就已经完成推断过程。**

```go
var a = 15
var b = false
var str = "Go says hello to the world!"
```

7、当你在函数体内声明**局部变量**时，应使用简短声明语法 `:=`，例如：

```go
a := 1
```

**注意：**

- 如果在相同的代码块中，我们不可以再次对于相同名称的变量使用初始化声明，例如：a := 20 就是不被允许的，编译器会提示错误 no new variables on left side of :=，但是 a = 20 是可以的，因为这是给相同的变量赋予一个新的值。
- 如果你声明了一个局部变量却没有在相同的代码块中使用它，同样会得到编译错误，例如下面这个例子当中的变量 a：

```go
func main() {
   var a string = "abc"
   fmt.Println("hello, world")
}
```

尝试编译这段代码将得到错误 `a declared and not used`。使用 `fmt.Println("hello, world", a)` 会移除错误。**但是全局变量是允许声明但不使用。**

#### 3.2 值类型和引用类型

> 这里就是比较重要的地方，包括后面的值引用和地址引用

程序中所用到的内存在计算机中使用一堆箱子来表示（这也是人们在讲解它的时候的画法），这些箱子被称为 **“字”**。根据不同的处理器以及操作系统类型，所有的字都具有 32 位（4 字节）或 64 位（8 字节）的相同长度；所有的字都使用相关的内存地址来进行表示（以十六进制数表示）。



1. 所有像 int、float、bool 和 string 这些基本类型都属于**值类型**，使用这些类型的变量直接指向存在内存中的值；像数组和结构（struct）这些复合类型
   - 当使用等号 `=` 将一个变量的值赋值给另一个变量时，如：`j = i`，实际上是在内存中将 i 的值（Python的数组操作就是直接拷贝的地址，如果i变了，j也会跟着变；）但是Go不会变
2. 更复杂的数据通常会需要使用多个字，这些数据一般使用**引用类型**保存。
   - 一个引用类型的变量 r1 存储的是 r1 的值所在的内存地址（数字），或内存地址中第一个字所在的位置。这个内存地址被称之为**指针**。
   - 指针属于引用类型，其它的引用类型还包括 slices，maps和 channel。被引用的变量会存储在堆中，以便进行垃圾回收，且比栈拥有更大的内存空间。




### 4、基本类型和运算符

> 这一节和其他的语言基本一致，包括：布尔类型、数字类型（**整型int和浮点型float**、**复数**、位运算、逻辑运算符、算数运算符、随机数）、**字符别名、字符类型**。
>
> 下面只列出不一样的地方

#### 4.1 整型 int 和浮点型 float

1. Go 语言中只有 float32 和 float64——Go 语言中没有 float 类型，也没有 double 类型。
2. **格式化字符串里**
   1. `%d` 用于格式化整数（`%x` 和 `%X` 用于格式化 16 进制表示的数字）
   2. `%g` 用于格式化浮点型（`%f` 输出浮点数，`%e` 输出科学计数表示法）
   3. `%0d` 用于规定输出定长的整数，其中开头的数字 0 是必须的
   4. `%n.mg` 用于表示数字 n 并精确到小数点后 m 位，除了使用 g 之外，还可以使用 e 或者 f，例如：使用格式化字符串 `%5.2e` 来输出 3.4 的结果为 `3.40e+00`。

#### 4.2 Go 复数类型

```
complex64 (32 位实数和虚数)
complex128 (64 位实数和虚数)
```

复数使用 `re+imI` 来表示，其中 `re` 代表实数部分，`im` 代表虚数部分，`I` 代表根号负 1。

```go
var c1 complex64 = 5 + 10i
fmt.Printf("The value is: %v", c1)
// 输出： 5 + 10i
```

- 在使用格式化说明符时，可以使用 `%v` 来表示复数，但当你希望只表示其中的一个部分的时候需要使用 `%f`。
- 函数 `real(c)` 和 `imag(c)` 可以分别获得相应的实数和虚数部分。

#### 4.3 字符别名

你在使用某个类型时，你可以给它起另一个名字，然后你就可以在你的代码中使用新的名字（用于简化名称或解决名称冲突）。

在 type TZ int 中，TZ 就是 int 类型的新名称（用于表示程序中的时区），然后就可以使用 TZ 来操作 int 类型的数据。

```go
package main
import "fmt"

type TZ int

func main() {
    var a, b TZ = 3, 4
    c := a + b
    fmt.Printf("c has the value: %d", c) // 输出：c has the value: 7
}
```



#### 4.4 字符类型

`byte` 类型是 `uint8` 的别名，对于只占用 1 个字节的传统 ASCII 编码的字符来说，完全没有问题。例如：`var ch byte = 'A'`；字符使用单引号括起来。

在 ASCII 码表中，A 的值是 65，而使用 16 进制表示则为 41，所以下面的写法是等效的：

```go
var ch byte = 65 或 var ch byte = '\x41'
```

Go 同样支持 Unicode（UTF-8），因此字符同样称为 Unicode 代码点或者 runes，并在内存中使用 int 来表示。

- 在文档中，一般使用格式 U+hhhh 来表示，其中 h 表示一个 16 进制数。其实 `rune` 也是 Go 当中的一个类型，并且是 `int32` 的别名。

```go
var ch int = '\u0041'
var ch2 int = '\u03B2'
var ch3 int = '\U00101234'
fmt.Printf("%d - %d - %d\n", ch, ch2, ch3) // integer
fmt.Printf("%c - %c - %c\n", ch, ch2, ch3) // character
fmt.Printf("%X - %X - %X\n", ch, ch2, ch3) // UTF-8 bytes
fmt.Printf("%U - %U - %U", ch, ch2, ch3) // UTF-8 code point
/*
输出：
65 - 946 - 1053236
A - β - r
41 - 3B2 - 101234
U+0041 - U+03B2 - U+101234

*/
```

包 unicode 包含了一些针对测试字符的非常有用的函数（其中 ch 代表字符）：

- 判断是否为字母：unicode.IsLetter(ch)

- 判断是否为数字：unicode.IsDigit(ch)
- 判断是否为空白符号：unicode.IsSpace(ch)

### 5、字符串

#### 5.1 简介

1. **字符串是 UTF-8 字符的一个序列**（当字符为 ASCII 码时则占用 1 个字节，其它字符根据需要占用 2-4 个字节）。
2. UTF-8 是被广泛使用的编码格式，是文本文件的标准编码，其它包括 XML 和 JSON 在内，也都使用该编码。
3. 在golang中，**汉字采用utf-8编码，占用三个字节，编码后的值是int类型；若为gbk编码，则占用两个子节。字母采用ASCII编码。**
4. 由于该编码对占用字节长度的不定性，Go 中的字符串也可能根据需要占用 1 至 4 个字节（示例见第 4.6 节），这与其它语言如 C++、Java 或者 Python 不同（Java 始终使用 2 个字节）。
5. Go 这样做的好处是不仅减少了内存和硬盘空间占用，同时也不用像其它语言那样需要对使用 UTF-8 字符集的文本进行编码和解码。

**代码测试：**创建一个用于统计字节和字符（rune）的程序，并对字符串 asSASA ddd dsjkdsjs dk 进行分析，然后再分析 asSASA ddd dsjkdsjsこん dk，最后解释两者不同的原因（提示：使用 unicode/utf8 包）

```go
package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	// count number of characters:
	str1 := "asSASA ddd dsjkdsjs dk"
	fmt.Printf("The number of bytes in string str1 is %d\n", len(str1))
	fmt.Printf("The number of characters in string str1 is %d\n", utf8.RuneCountInString(str1))
	str2 := "asSASA ddd dsjkdsjsこん dk"
	fmt.Printf("The number of bytes in string str2 is %d\n", len(str2))
	fmt.Printf("The number of characters in string str2 is %d", utf8.RuneCountInString(str2))
}

/* Output:
The number of bytes in string str1 is 22
The number of characters in string str1 is 22
The number of bytes in string str2 is 28
The number of characters in string str2 is 24
*/
```

```go
package main

import "fmt"

func main() {
  var str = "hello 你好"
  fmt.Println("len(str):", len(str))
  for i := 0; i < len(str); i++ {
    fmt.Printf("%c",str[i])
  }
    
  var str = "hello 你好"
  fmt.Println("len(str):", len(str))
  for _, v := range str {
    fmt.Printf("%c", v)
  }
    
  
     
  var str = "hello"
  byteStr := []byte(str)
  byteStr[0] = 'H'
  fmt.Println(string(byteStr))

  var s = "你好吗"
  runeS := []rune(s)
  runeS[2] = '啊'
  fmt.Println(string(runeS))
}

/*输出结果
hello ä½ å¥½
hello 你好
Hello
你好啊*/
```

一个结论：

- 有汉字用rune，没汉字随意。
- for采用byte类型循环，for range采用rune类型循环
- 要修改字符串，需要先将其转换为rune或者byte类型，完成后再转换为string。

#### 5.2 strings和strconv包

1. `HasPrefix/HasSuffix` 判断字符串 `s` 是否以 `prefix/suffix` 开头：

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    var str string = "This is an example of a string"
    fmt.Printf("T/F? Does the string \"%s\" have prefix \"%s\"? ", str, "Th")
    fmt.Printf("%t\n", strings.HasPrefix(str, "Th"))
}
/*
T/F? Does the string "This is an example of a string" have prefix "Th"? true
*/
```

2. `Contains` 判断字符串 `s` 是否包含 `substr`：`strings.Contains(s, substr string)`，返回Boolean类型

3. 判断子字符串或字符在父字符串中出现的位置（索引）：

   - `strings.Index(s, str string)`判断str在字符串s中的位置，-1表示不包含字符串str。
   - `strings.LastIndex(s, str string) `，返回str在字符串s最后出现位置的索引
   - 如果 `ch` 是非 ASCII 编码的字符，建议使用以下函数来对字符进行定位：`strings.IndexRune(s string, r rune)`，返回的是int类型下标位置

4. 字符串替换：`strings.Replace(str, old, new, n)`，就是将字符串 `str` 中的前 `n` 个字符串 `old` 替换为字符串 `new`，并返回一个新的字符串；如果n=-1.则替换所有字符（相当于strings.RepleaseAll()）

5. 字符串重复拼接：`strings.Repeat(s, count int)`，生成重复 `count` 次字符串 `s` ，并返回一个新的字符串

6. 修改字符串的大小写：

   - `strings.ToLower(s)`， 将字符串中的 Unicode 字符全部转换为相应的小写字符
   - `strings.ToUpper(s)`，将字符串中的 Unicode 字符全部转换为相应的大写字符

7. 裁剪字符串：

   - `strings.TrimSpace(s)` 来剔除字符串开头和结尾的空白符号
   - `strings.Trim(s, "cut")` 来将开头和结尾的 `cut` 去除掉；
     - 如果你只想剔除开头或者结尾的字符串，则可以使用 `TrimLeft` 或者 `TrimRight` 来实现。

8. 分割字符串

   - `strings.Fields(s)` 利用空白作为分隔符将字符串分割为若干块，并返回一个 slice 。如果字符串只包含空白符号，返回一个长度为 0 的 slice 。
   - `strings.Split(s, sep)` 自定义分割符号对字符串分割，返回 slice 。（Java里面把这两个方法就直接合并为String.split("xxx")了）
   - `strings.Join(sl []string, sep string)`，`Join` 用于将元素类型为 string 的 slice 使用分割符号来拼接组成一个字符串

   ```go
   package main
   
   import (
       "fmt"
       "strings"
   )
   
   func main() {
       str := "The quick brown fox jumps over the lazy dog"
       sl := strings.Fields(str)
       fmt.Printf("Splitted in slice: %v\n", sl)
       for _, val := range sl {
           fmt.Printf("%s - ", val)
       }
       fmt.Println()
       str2 := "GO1|The ABC of Go|25"
       sl2 := strings.Split(str2, "|")
       fmt.Printf("Splitted in slice: %v\n", sl2)
       for _, val := range sl2 {
           fmt.Printf("%s - ", val)
       }
       fmt.Println()
       str3 := strings.Join(sl2,";")
       fmt.Printf("sl2 joined by ; is—— %s\n", str3)
   }
   /*
   Splitted in slice: [The quick brown fox jumps over the lazy dog]
   The - quick - brown - fox - jumps - over - the - lazy - dog -
   Splitted in slice: [GO1 The ABC of Go 25]
   GO1 - The ABC of Go - 25 -
   sl2 joined by ; is—— GO1;The ABC of Go;25
   
   */
   ```

9. 从字符串中读取内容

   - 函数 `strings.NewReader(str)` 用于生成一个 `Reader` 并读取字符串中的内容，然后返回指向该 `Reader` 的指针
   - `Read()` 从 [] byte 中读取内容。
   - `ReadByte()` 和 `ReadRune()` 从字符串中读取下一个 byte 或者 rune。

10.  **字符串与其它类型的转换——`strconv` 包**

    - 针对从数字类型转换到字符串，Go 提供了以下函数：

      - strconv.Itoa(i int) string 返回数字 i 所表示的字符串类型的十进制数。
      - strconv.FormatFloat(f float64, fmt byte, prec int, bitSize int) string 将 64 位浮点型的数字转换为字符串，其中 fmt 表示格式（其值可以是 'b'、'e'、'f' 或 'g'），prec 表示精度，bitSize 则使用 32 表示 float32，用 64 表示 float64。
    - 针对从字符串类型转换为数字类型，Go 提供了以下函数：
      
      - strconv.Atoi(s string) (i int, err error) 将字符串转换为 int 型。
      - strconv.ParseFloat(s string, bitSize int) (f float64, err error) 将字符串转换为 float64 型。
		
      ```go
      var orig string = "666"
          var an int
          var newS string
      
          fmt.Printf("The size of ints is: %d\n", strconv.IntSize)      
      
          an, _ = strconv.Atoi(orig)
          fmt.Printf("The integer is: %d\n", an) 
          an = an + 5
          newS = strconv.Itoa(an)
          fmt.Printf("The new string is: %s\n", newS)
      
      /*
      The size of ints is: 64(说明是64位系统)
      The integer is: 666
      The new string is: 671
      */
      ```

#### 5.3 时间和日期

`time` 包为我们提供了一个数据类型 `time.Time`（作为值使用）以及显示和测量时间和日期的功能函数。

```go
fmt.Printf("当前时间为：%s;具体时间：%02d.%02d.%4d\n", time.Now(), time.Now().Day(), time.Now().Month(), time.Now().Year())
```

Duration 类型表示两个连续时刻所相差的纳秒数，类型为 int64。Location 类型映射某个时区的时间，UTC 表示通用协调世界时间。

```go
now := time.Now().UTC()
		// 显示时间格式： UnixDate = "Mon Jan _2 15:04:05 MST 2006"
		fmt.Printf("%s\n", now.Format(time.UnixDate))
		fmt.Println(now.Format("02 Jan 2006 15:04"))
	// 显示时间戳
	fmt.Printf("%ld\n", now.Unix())
	// 显示时分:Kitchen = "3:04PM"
	fmt.Printf("%s\n", now.Format("3:04PM"))
/*
注意这里是UTC时间，不是中部时间
Thu Jun 16 04:11:08 UTC 2022
16 Jun 2022 04:11           
%!l(int64=1655352668)d      
4:11AM 
*/

```

`time.Sleep（Duration d）` 可以实现对某个进程（实质上是 goroutine）时长为 d 的暂停。

#### 5.4 指针

不像 Java 和 .NET，Go 语言为程序员提供了控制数据结构的指针的能力；但是，你不能进行指针运算。

- 一个指针变量可以指向任何一个值的内存地址。`var intP *int`，然后使用 `intP = &i1` 是合法的，此时 intP 指向 i1。
- 指针的格式化标识符为 `%p`
- 符号 `*` 可以放在一个指针前，**如 `*intP`，那么它将得到这个指针指向地址上所存储的值**；这被称为反引用（或者内容或者间接引用）操作符；另一种说法是指针转移。所以， 如下表达式都是正确的：`var == *(&var)`

## 二、控制结构

> Go 提供了下面这些条件结构和分支结构：
>
> - if-else 结构
> - switch 结构
> - select 结构，用于 channel 的选择（第 14.4 节）
>
> Go 完全省略了 `if`、`switch` 和 `for` 结构中条件语句两侧的括号，相比 Java、C++ 和 C# 中减少了很多视觉混乱的因素

### 1、if-else结构

```go
if condition1 {
    // do something 
} else if condition2 {
    // do something else    
} else {
    // catch-all or default
}
```

**注意点：**

1. 即使当代码块之间只有一条语句时，大括号也不可被省略

2. 关键字 if 和 else 之后的左大括号 `{` 必须和关键字在同一行，如果你使用了 else-if 结构，则前段代码块的右大括号 `}` 必须和 else-if 关键字在同一行。

3. 把尽可能先满足的条件放在前面

4. 当 if 结构内有 break、continue、goto 或者 return 语句时，Go 代码的常见写法是省略 else 部分（但是这个只是建议，其实把else显示地写出来更好）

5. 使用简短方式 := 声明的变量的作用域只存在于 if 结构中（在 if 结构的大括号之间，如果使用 if-else 结构则在 else 代码块中变量也会存在）。如果变量在 if 结构之前就已经存在，那么在 if 结构中，该变量原来的值会被隐藏。最简单的解决方案就是不要在**初始化语句**中声明变量（就还是在if结构之前写出）

   ```go
   val := 10
   if val > max {
       // do something
   }
   //等价于下面：
   if val := 10; val > max {
       // do something
   }
   ```

**几个常见的函数：**

1. 通过常量 `runtime.GOOS` 来判断 操作系统类型
2. 函数 `Abs()` 用于返回一个整型数字的绝对值
3. `func isGreater(x,y int) bool` 用于比较两个整型数字的大小

#### 1.1、多返回值函数的错误

> Go 语言的函数经常使用**两个返回值来表示执行是否成功**：返回某个值以及 true 表示成功；返回零值（或 nil）和 false 表示失败

**习惯用法**:

如果确实存在错误，则会打印相应的错误信息然后通过 return 提前结束函数的执行。我们还可以使用携带返回值的 return 形式，例如 return err。这样一来，函数的调用者就可以检查函数执行过程中是否存在错误了。

```go
value, err := pack1.Function1(param1)
if err != nil {
    fmt.Printf("An error occured in pack1.Function1 with parameter %v", param1)
    return err
}
// 未发生错误，继续执行，会返回err真实的信息
```

如果我们想要在错误发生的同时终止整个程序的运行，我们可以使用 `os` 包的 `Exit` 函数：

```go
if err != nil {
    fmt.Printf("Program stopping with error %v", err)
    os.Exit(1)
}
```

除了这种err的判断方法，还可以将 ok-pattern 的获取放置在 if 语句的初始化部分，然后进行判断：

```go
if value, ok := readData(); ok {
		//…
}
```

当然，你也可以直接屏蔽掉err的显示输出。比如，当您将字符串转换为整数时，且确定转换一定能够成功时，可以将 `Atoi` 函数进行一层忽略错误的封装：

```go
func atoi (s string) (n int) {
    n, _ = strconv.Atoi(s)
    return
}
```

**PS：**实际上，`fmt` 包（第 4.4.3 节）最简单的打印函数也有 2 个返回值

```go
count, err := fmt.Println(x) // number of bytes printed(nil or 0), error
```

**当打印到控制台时**，可以将该函数返回的错误忽略；**但当输出到文件流、网络流等具有不确定因素的输出对象时，**应该始终检查是否有错误发生

### 2、switch结构

> 相比较 C 和 Java 等其它语言而言，Go 语言中的 switch 结构使用上更加灵活。
>
> 语法上也有一些不一样的地方！

```go
switch var1 {
    case val1:
        //...
    case val2:
        //...
    case var3:
    	fallthrough
    default:
        //...
}
```

注意点：

1. 一旦成功地匹配到某个分支，在执行完相应代码后就会退出整个 switch 代码块，也就是说您**不需要特别使用** `break` 语句来表示结束。
2. go提供了fallthrough语句，表示在执行完每个分支的代码后，继续执行后续分支的代码。（原本的是，执行完就退出，现在是继续执行）

switch 语句的第二种形式是不提供任何被判断的值（实际上默认为判断是否为 true），然后在每个 case 分支中进行测试不同的条件

```go
package main

import "fmt"

func main() {
    var num1 int = 7

    switch {
        case num1 < 0:
            fmt.Println("Number is negative")
        case num1 > 0 && num1 < 10:
            fmt.Println("Number is between 0 and 10")
        default:
            fmt.Println("Number is 10 or greater")
    }
}
```

switch语句也可以包含初始化语句：

```go
switch result := calculate(); {
    case result < 0:
        ...
    case result > 0:
        ...
    default:
        // 0
}
```

### 3、for循环结构

> go中的for结构，要比Java中具有更多的功能。

#### 1、最简单的形式，**基于计数器**——不需要括号 `()` 将它们括起来

```go
for i := 0; i < 5; i++ {
        fmt.Printf("This is the %d iteration\n", i)
}

//在循环中同时使用多个计数器		//注意，平行条件一条不满足，for循环就终止
for i, j := 0,2; i < j; i, j = i+1, j-1 {
		//xxx
	}
//将两个 for 循环嵌套
for i:=0; i<5; i++ {
    for j:=0; j<10; j++ {
        println(j)
    }
}
```

特别注意

1. 永远不要在循环体内修改计数器，这在任何语言中都是非常差的实践！
2. 计数器类型的for循环，打印的一个 Unicode 编码的字符串会有问题：

```go
package main

import "fmt"

func main() {
    str := "Go is a beautiful language!"
    fmt.Printf("The length of str is: %d\n", len(str))
    for ix :=0; ix < len(str); ix++ {
        fmt.Printf("Character on position %d is: %c \n", ix, str[ix])
    }
    str2 := "日本語"
    fmt.Printf("The length of str2 is: %d\n", len(str2))
    for ix :=0; ix < len(str2); ix++ {
        fmt.Printf("Character on position %d is: %c \n", ix, str2[ix])
    }
}
/*
str := "Go is a beautiful language!"是正常打印，长度是27
 str2 := "日本語"打印乱码，长度是9
 Character on position 0 is: æ 
Character on position 1 is:  
Character on position 2 is: ¥ 
Character on position 3 is: æ 
Character on position 4 is:  
Character on position 5 is: ¬ 
Character on position 6 is: è 
Character on position 7 is: ª 
Character on position 8 is:  


*/
```

可以发现，ASCII 编码的字符占用 1 个字节，既每个索引都指向不同的字符，而非 ASCII 编码的字符（占有 2 到 4 个字节）不能单纯地使用索引来判断是否为同一个字符。（要用`for-range`结构）

练习：

1. 使用按位补码从 0 到 10，使用位表达式 `%b` 来格式化输出。

```go
for i := 0; i <= 10; i++ {
		fmt.Printf("the complement of %b is: %b\n", i, ^i)
	}
/*
说明：^作为一元运算符，若x为无符号数： 表示按位取反，即1变0，0变1,即^x = m^x，m的二进制位全为1, 可用计算机位最大数-n。例3：
如对于unint8数5，^5 = 255^5 = 1111 1111^0000 0101（补码运算） = 1111 1010（结果补码） = 1111 1010 （结果反码）= 1111 1010 （结果原码） = 255 - 5 = 250；


若x为有符号正数： 表示二进制加1后符号位取反，即^n = -1 ^ n； 例4： 如对于int8数5，求^5  方法1： ^5 = -1^5 = 1111 1111^0000 0101（补码运算） = 1111 1010（结果补码） = 1000 0101（结果反码）= 1000 0110 （结果原码） = -6；
*/


/* Output:
the complement of 0 is: -1
the complement of 1 is: -10
the complement of 10 is: -11
the complement of 11 is: -100
the complement of 100 is: -101
the complement of 101 is: -110
the complement of 110 is: -111
the complement of 111 is: -1000
the complement of 1000 is: -1001
the complement of 1001 is: -1010
the complement of 1010 is: -1011
*/
```

2. 写一个从 1 打印到 100 的程序，但是每当遇到 3 的倍数时，不打印相应的数字，但打印一次 "Fizz"。遇到 5 的倍数时，打印 Buzz 而不是相应的数字。对于同时为 3 和 5 的倍数的数，打印 FizzBuzz（提示：**使用 switch 语句**）。

```go
package main

import "fmt"

const (
	FIZZ     = 3
	BUZZ     = 5
	FIZZBUZZ = 15
)

func main() {
	for i := 0; i <= 100; i++ {
		switch {
		case i%FIZZBUZZ == 0:
			fmt.Println("FizzBuzz")
		case i%FIZZ == 0:
			fmt.Println("Fizz")
		case i%BUZZ == 0:
			fmt.Println("Buzz")
		default:
			fmt.Println(i)
		}
	}
}
```

#### 2、 基于条件判断的迭代

> 没有头部的条件判断迭代（类似其它语言中的 while 循环）;也可以认为这是没有初始化语句和修饰语句的 for 结构，因此 `;;` 便是多余的了。

```go
var i int = 5

for i >= 0 {
	i = i - 1
	fmt.Printf("The variable i is now: %d\n", i)
}

```

#### 3、无限循环

- 条件语句是可以被省略的，如` i:=0; ; i++` 或` for { } `或` for ;; { }`（;; 会在使用 gofmt 时被移除）：这些循环的本质就是无限循环。最后一个形式也可以被改写为 `for true { }`，但一般情况下都会直接写` for { }`


- 因此循环体内必须有相关的条件判断以确保会在某个时刻退出循环。

#### 4、 for-range 结构

> 这是 Go 特有的一种的迭代结构，它可以迭代任何一个集合（包括数组和 map），类似于Python的enumerate()函数。

要注意的是，`val` 始终为集合中对应索引的**值拷贝**，因此它一般只具有只读性质，对它所做的任何修改都不会影响到集合中原有的值（**译者注：**如果 `val` 为指针，则会产生指针的拷贝，依旧可以修改集合中的原值）

一个字符串是 Unicode 编码的字符（或称之为 `rune`）集合，因此您也可以用它迭代字符串：（**解决上面计数器模式打印字符的问题**）

```go
package main

import "fmt"

func main() {
    str := "Go is a beautiful language!"
    fmt.Printf("The length of str is: %d\n", len(str))
    for pos, char := range str {
        fmt.Printf("Character on position %d is: %c \n", pos, char)
    }
    fmt.Println()
    str2 := "Chinese: 日本語"
    fmt.Printf("The length of str2 is: %d\n", len(str2))
    for pos, char := range str2 {
        fmt.Printf("character %c starts at byte position %d\n", char, pos)
    }
    fmt.Println()
    fmt.Println("index int(rune) rune    char bytes")
    for index, rune := range str2 {
        fmt.Printf("%-2d      %d      %U '%c' % X\n", index, rune, rune, rune, []byte(string(rune)))
    }
}
/*
第一个 str := "Go is a beautiful language!"正常打印，长度为27

第二个 str2 := "Chinese: 日本語"，长度为18
The length of str2 is: 18
character C starts at byte position 0
character h starts at byte position 1
character i starts at byte position 2
character n starts at byte position 3
character e starts at byte position 4
character s starts at byte position 5
character e starts at byte position 6
character : starts at byte position 7
character   starts at byte position 8
character 日 starts at byte position 9
character 本 starts at byte position 12
character 語 starts at byte position 15

第三个打印：
index int(rune) rune    char bytes
0       67      U+0043 'C' 43
1       104      U+0068 'h' 68
2       105      U+0069 'i' 69
3       110      U+006E 'n' 6E
4       101      U+0065 'e' 65
5       115      U+0073 's' 73
6       101      U+0065 'e' 65
7       58      U+003A ':' 3A
8       32      U+0020 ' ' 20
9       26085      U+65E5 '日' E6 97 A5
12      26412      U+672C '本' E6 9C AC
15      35486      U+8A9E '語' E8 AA 9E

可以看到，常用英文字符使用 1 个字节表示，而汉字使用 3 个字符表示
*/
```

两个测试：

```go
for i := 0; i < 3; {
		fmt.Println("Value of i:", i)
	}
//会一直输出Value of i:0
```

```go
for i := 0; ; i++ {
    fmt.Println("Value of i is now:", i)
}
//输出一直上升
```

```go
for i, j, s := 0, 5, "a"; i < 3 && j < 100 && s != "aaaaa"; i, j,
		s = i+1, j+1, s+"a" {
		fmt.Println("Value of i, j, s:", i, j, s)
}
/*
Value of i, j, s: 0 5 a     
Value of i, j, s: 1 6 aa    
Value of i, j, s: 2 7 aaa   
*/
```

#### 5、Break 与 continue

> 与其他语言一致！

#### 6、标签与goto

> **特别注意** 使用标签和 goto 语句是不被鼓励的：它们会很快导致非常**糟糕**的程序设计，而且总有更加可读的替代方案来实现相同的需求。

## 三、函数（function）

> Go 里面有三种类型的函数：
>
> - 普通的带有名字的函数
> - 匿名函数或者 lambda 函数
> - 方法（Methods）——这个和Java中的方法完全不一样

注意点：

1. 函数重载（function overloading）指的是可以编写多个同名函数，只要它们拥有不同的形参 / 或者不同的返回值，**在 Go 里面函数重载是不被允许的**。（Go 语言不支持这项特性的主要原因是函数重载需要进行多余的类型匹配影响性能）
2. 如果需要申明一个**在外部定义的函数**，你只需要给出函数名与函数签名，不需要给出函数体——类似于Java接口中的方法

```go
func flushICache(begin, end uintptr) // implemented externally
```

3. 函数也可以以申明的方式被使用，作为一个函数类型

```go
type binOp func(int, int) int
//函数作为第一类值（first-class value）：可以赋值给变量，就像 add := binOp 一样
```

4. **函数值（functions value）之间可以相互比较**：如果它们引用相同的函数或者都是 nil 的话，则认为它们是相同的函数。
5. 函数不能在其它函数里面声明**（不能嵌套）**，不过我们可以通过使用匿名函数来破除这个限制。
6. 目前 Go 没有泛型（generic）的概念，也就是说它不支持那种支持多种类型的函数。(所以go的函数更加具体，一定会指定好类型)——有一些方式（接口和反射）可以实现泛型的功能，但是这会让代码很复杂，最好是为每一个类型单独创建一个函数，而且代码可读性更强。

### 1、函数参数与返回值

> 相比于 C、C++、Java 和 C#，多值返回是 Go 的一大特性，为我们判断一个函数是否正常执行（参考 [第 5.2 节](https://learnku.com/docs/the-way-to-go/function-parameters-and-return-values/05.2.md)）提供了方便
>
> 没有参数的函数通常被称为 **niladic** 函数（niladic function），就像 `main.main()`

#### 1.1 按值传递（call by value） 按引用传递（call by reference）

- Go 默认使用按值传递来传递参数，也就是传递参数的副本。函数接收参数副本之后，在使用变量的过程中可能对副本的值进行更改，但不会影响到原来的变量，比如 `Function(arg1)`。
- 如果你希望函数可以直接修改参数的值，而不是对参数的副本进行操作，你需要将参数的地址（变量名前面添加 & 符号，比如 &variable）传递给函数，这就是按引用传递，比如 Function(&arg1)，此时传递给函数的是一个指针。（指针也是变量类型，有自己的地址和值，通常指针的值指向一个变量的地址。所以，**按引用传递也是按值传递。**——这就是为什么有部分人认为所有语言中实际上只有值传递）


**注意点：**

1. 几乎在任何情况下，传递指针（一个 32 位或者 64 位的值）的消耗都比传递副本来得少
2. 在函数调用时，像切片（slice）、字典（map）、接口（interface）、通道（channel）这样的引用类型都是默认使用引用传递（即使没有显式的指出指针）。

#### 1.2 返回值——命名返回，和非命名返回

```go
func getX2AndX3(input int) (int, int) {
    return 2 * input, 3 * input
}

func getX2AndX3_2(input int) (x2 int, x3 int) {
    x2 = 2 * input
    x3 = 3 * input
    // return x2, x3
    return
}
```

注意点：

1. 当需要返回多个非命名返回值时，需要使用 `()` 把它们括起来，比如 `(int, int)`。
2. 任何一个非命名返回值（**使用非命名返回值是很糟的编程习惯**）在 `return` 语句里面都要明确指出包含返回值的变量或是一个可计算的值
3. **命名返回值作为结果形参（result parameters）被初始化为相应类型的零值**（也就是说，如果func里面不做调用，就会返回空值），当需要返回的时候，我们只需要一条简单的不带参数的 return 语句。需要注意的是，即使只有一个命名返回值，也需要使用 () 括起来
4. **尽量使用命名返回值：会使代码更清晰、更简短，同时更加容易读懂**

#### 1.3 空白符（blank identifier）

> 空白符用来匹配一些不需要的值，然后丢弃掉。

`i1, _, f1 = ThreeValues()`，将第一个与第三个返回值赋给了 `i1` 与 `f1`。第二个返回值赋给了空白符 `_`，然后自动丢弃掉。

#### 1.4 改变外部变量

传递指针给函数不但可以节省内存（因为没有复制变量的值），而且赋予了函数直接修改外部变量的能力，所以被修改的变量不再需要使用 `return` 返回。

但是这种操作方法很危险，因为没有返回，直接传递一个指针很容易引发一些不确定的事。

```go
// this function changes reply:
func Multiply(a, b int, reply *int) {
    *reply = a * b
}
func main() {
    n := 0
    reply := &n
    Multiply(10, 5, reply)
    fmt.Println("Multiply:", *reply) // Multiply: 50
}
```

### 2、传递变长参数

1. 如果函数的最后一个参数是采用 `...type` 的形式，那么这个函数就可以处理一个变长的参数，这个长度可以为 0。`func myFunc(a, b, arg ...int) {}`

```go
func Greeting(prefix string, who ...string)
Greeting("hello:", "Joe", "Anna", "Eileen")
```

在 Greeting 函数中，变量 `who` 的值为 `[]string{"Joe", "Anna", "Eileen"}`。

2. 如果参数被存储在一个 slice 类型的变量 `slice` 中，则可以通过 `slice...` 的形式来传递参数调用变参函数。

```go
slice := []int{7,9,3,5,1}
x = min(slice...)

func min(s ...int) int {
    if len(s)==0 {
        return 0
    }
    min := s[0]
    for _, v := range s {
        if v < min {
            min = v
        }
    }
    return min
}
```

假如这里只是需要直接传slice，那可以改为：

```go
slice := []int{7,9,3,5,1}
x = min(slice)

func min(s []int) int {
    if len(s)==0 {
        return 0
    }
    min := s[0]
    for _, v := range s {
        if v < min {
            min = v
        }
    }
    return min
}
```

3. 一个接受变长参数的函数可以将这个参数作为其它函数的参数进行传递：

```go
func F1(s ...string) {
    F2(s...)
    F3(s)
}

func F2(s ...string) { }
func F3(s []string) { }
```

**但是如果变长参数的类型并不是都相同的呢？**使用 5 个参数来进行传递并不是很明智的选择，有 2 种方案可以解决这个问题：

- 定义一个**结构类型**。假设它叫 `Options`，用以存储所有可能的参数：

  ```go
  type Options struct {
      par1 type1,
      par2 type2,
      ...
  }
  ```

  函数 F1 可以使用正常的参数 a 和 b，以及一个没有任何初始化的 Options 结构： `F1(a, b, Options {})`。如果需要对选项进行初始化，则可以使用 `F1(a, b, Options {par1:val1, par2:val2})`。

- 使用空接口。默认的空接口 `interface{}`，这样就可以接受任何类型的参数。该方案不仅可以用于长度未知的参数，还可以用于任何不确定类型的参数。**一般而言（其实应该说是规范操作）**，我们会使用一个 for-range 循环以及 switch 结构对每个参数的类型进行判断：

```
func typecheck(..,..,values … interface{}) {
    for _, value := range values {
        switch v := value.(type) {
            case int: …
            case float: …
            case string: …
            case bool: …
            default: …
        }
    }
}
```

### 3、defer和追踪

#### 3.1defer的基本操作

> 关键字 defer 允许我们推迟到函数返回之前（或任意位置执行 `return` 语句之后）一刻才执行某个语句或函数。
>
> 为什么要在返回之后才执行这些语句？因为 `return` 语句同样可以包含一些操作，而不是单纯地返回某个值。
>
> 关键字 defer 的用法类似于面向对象编程语言 Java 和 C# 的 `finally` 语句块，**它一般用于释放某些已分配的资源**。（这个就特别容易理解了）

```go
package main
import "fmt"

func main() {
    function1()
}

func function1() {
    fmt.Printf("In function1 at the top\n")
    defer function2()
    fmt.Printf("In function1 at the bottom!\n")
}

func function2() {
    fmt.Printf("function2: Deferred until the end of the calling function!")
}
/*
基础用法：
In Function1 at the top
In Function1 at the bottom!
Function2: Deferred until the end of the calling function!
*/
```

使用 defer 的语句同样可以接受参数，下面这个例子就会在执行 defer 语句时打印 `0`（所以这个defer是相当于已经执行后挂起了的线程，等待return唤醒）

```go
func a() {
    i := 0
    defer fmt.Println(i)
    i++
    return
}
```

```go
func main(){
	fmt.Println(defer_test())
}
func defer_test() int {

	a := 1
	defer func() {
		fmt.Println("a", a)
		a = 4
		fmt.Println("2", a)
	}()
	return a			//a还是1

}
/*
输出：
a 1
2 4
1
*/
```

当有多个 defer 行为被注册时，它们会以**逆序执行**（类似栈，即后进先出）——所以写的时候，如果对顺序有要求，就一定要注意：

```go
func f() {
    for i := 0; i < 5; i++ {
        defer fmt.Printf("%d ", i)
    }
}
/*
4 3 2 1 0
*/
```

这种其实没有太大意义，我们在项目中一般是这么用：

1. **关闭文件流**

```go
// open a file  
defer file.Close()
```

2. **解锁一个加锁的资源**（一般紧跟着加锁语句）

```go
mu.Lock()  
defer mu.Unlock() 
```

3. **打印最终报告**

```go
printHeader()  
defer printFooter()
```

4. **关闭数据库链接**

```go
// open a database connection  
defer disconnectFromDB()
```

#### 3.2 使用 defer 语句实现代码追踪

一个基础但十分实用的实现代码执行追踪的方案就是在进入和离开某个函数打印相关的消息:

```go
package main

import "fmt"

func trace(s string)   { fmt.Println("entering:", s) }
func untrace(s string) { fmt.Println("leaving:", s) }

func a() {
    trace("a")
    defer untrace("a")
    fmt.Println("in a")
}

func b() {
    trace("b")
    defer untrace("b")
    fmt.Println("in b")
    a()
}

func main() {
    b()
}
/*
entering: b
in b
entering: a
in a
leaving: a
leaving: b
*/
```

在调试时使用 defer 语句的手法(常用？？)：

```go
package main

import (
    "io"
    "log"
)

func func1(s string) (n int, err error) {
    defer func() {
        log.Printf("func1(%q) = %d, %v", s, n, err)
    }()
    return 7, io.EOF
}

func main() {
    func1("Go")
}

/*
只会打印log信息（带时间）：2022/06/22 10:44:27 func1("xxx") = 7, EOF
*/
```

### 4、内置函数

> 都是一些常规操作，唯一不同的就是Go有个**分配内存**、和**复数操作**。

| 名称                | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| close               | 用于管道通信                                                 |
| len、cap            | len 用于返回某个类型的长度或数量（字符串、数组、切片、map 和管道）；cap 是**容量**的意思，用于返回某个类型的最大容量（**只能**用于切片和 map，好像也只有这两个结构带有容量的属性） |
| new、make           | new 和 make **均是用于分配内存**（一个自定义，一个用于内置）：new 用于值类型和用户定义的类型，如自定义结构，make 用于内置引用类型（切片、map 和管道）：new (type)、make (type)<br />new (T) 分配类型 T 的零值并返回其地址，也就是指向类型 T 的指针。它也可以被用于基本类型：`v := new(int)`。<br />make (T) 返回类型 T 的初始化之后的值（不是指针），因此它比 new 进行更多的工作 |
| copy、append        | 用于复制和连接切片                                           |
| panic、recover      | 两者均用于错误处理机制                                       |
| print、println      | 底层打印函数，**在部署环境中建议使用 fmt 包**                |
| complex、real、imag | 用于创建和操作复数                                           |

### 5、讲函数作为参数

> 这个是第一次看到，很新奇。函数的调用顺序发生了变化

函数可以作为其它函数的参数进行传递，然后在其它函数内调用执行，一般称之为**回调**。这一个特性很有可能是go对函数参数要求不严格导致的。

- 要理解这里的函数调用顺序，其实我们就是要理解回调函数callback
  - 当程序跑起来时，一般情况下，应用程序（application program）会时常通过API调用库里所预先备好的函数。但是有些库函数（library function）却要求应用先传给它一个函数，好在合适的时候调用，以完成目标任务。这个被传入的、后又被调用的函数就称为**回调函数**（callback function）。——在下面这个例子中，回调函数就是Add()函数
  - 回调的作用：
  - 打个比方，有一家旅馆提供叫醒服务，但是要求旅客自己决定叫醒的方法。可以是打客房电话，也可以是派服务员去敲门，睡得死怕耽误事的，还可以要求往自己头上浇盆水。这里，“叫醒”这个行为是旅馆提供的，相当于库函数（即，这里的callback函数），但是叫醒的方式是由旅客决定并告诉旅馆的，也就是回调函数（add函数）。
  - 所以，回调机制提供了非常大的灵活性。回调函数可以是add，可以是multi，也可以是mod。

```go
package main

import (
    "fmt"
)

func main() {
    callback(1, Add)
}

func Add(a, b int) {
    fmt.Printf("The sum of %d and %d is: %d\n", a, b, a+b)
}

func callback(y int, f func(int, int)) {
    f(y, 2) // this becomes Add(1, 2)
}
/*
main() ——> callback() ——> Add()
The sum of 1 and 2 is: 3。
*/
```

### 6、闭包

> 其实可以理解成匿名函数，可以不加函数名直接调用，并且这个匿名函数还有自己的地址（话说没有分配地址又怎么能运行呢？肯定是有地址的）

- 这样的一个函数**不能够独立存在**（编译器会返回错误：`non-declaration statement outside function body`）；
  - 但可以被赋值于某个变量，即保存函数的地址到变量中：`fplus := func(x, y int) int { return x + y }`，然后通过变量名对函数进行调用：`fplus(3,4)`；
  - 也可以直接对匿名函数进行调用：`func(x, y int) int { return x + y } (3, 4)`。参数列表的第一对括号必须紧挨着关键字 `func`，因为匿名函数没有名称。花括号 `{}` 涵盖着函数体，最后的一对括号表示对该匿名函数的调用。

将匿名函数赋值给变量并对其进行调用：

```go
package main

import "fmt"

func main() {
    f()
}
func f() {
    for i := 0; i < 4; i++ {
        g := func(i int) { fmt.Printf("%d ", i) } //此例子中只是为了演示匿名函数可分配不同的内存地址，在现实开发中，不应该把该部分信息放置到循环中。
        g(i)
        fmt.Printf(" - g is of type %T and has value %v\n", g, g)
    }
}
/*
可以看到变量 g 代表的是 func(int)，变量的值是一个内存地址:
0 - g is of type func(int) and has value 0x681a80
1 - g is of type func(int) and has value 0x681b00
2 - g is of type func(int) and has value 0x681ac0
3 - g is of type func(int) and has value 0x681400
*/
```

匿名函数可以像所有函数一样可以接受或不接受参数：

```go
package main

import "fmt"

func f() (ret int) {
    defer func() {
        ret++
    }()
    return 1
}
func main() {
    fmt.Println(f())
}
/*
输出：
1

变量 ret 的值为 2，因为 ret++ 是在执行 return 1 语句后发生的。
*/
```

闭包的真实作用：

1. 闭包可使得某个函数捕捉到一些外部状态，例如：函数被创建时的状态。
2. 一个闭包继承了函数所声明时的作用域。这种状态（作用域内的变量）都被共享到闭包的环境中，因此这些变量可以在闭包中被操作，直到被销毁。（把闭包作为一个内部func来用，突破go不能嵌套定义函数的限制）

#### 6.1 应用闭包：将函数作为返回值

将 Adder 返回的函数存到变量中：

```go
package main

import "fmt"

func main() {
    // make an Add2 function, give it a name p2, and call it:
    p2 := Add2()
    fmt.Printf("Call Add2 for 3 gives: %v\n", p2(3))
    // make a special Adder function, a gets value 2:
    TwoAdder := Adder(2)
    fmt.Printf("The result is: %v\n", TwoAdder(3))
}

func Add2() func(b int) int {
    return func(b int) int {
        return b + 2
    }
}

func Adder(a int) func(b int) int {
    return func(b int) int {
        return a + b
    }
}
/*
Call Add2 for 3 gives: 5
The result is: 5
*/
```

上面这个很好理解，也很直接。

下面再看一个例子，函数 Adder () 现在直接就被赋值到变量 f 中：

```go
package main

import "fmt"

func main() {
    var f = Adder()
    fmt.Print(f(1), " - ")
    fmt.Print(f(20), " - ")
    fmt.Print(f(300))
}

func Adder() func(int) int {
    var x int
    return func(delta int) int {
        x += delta
        return x
    }
}
/*
1 - 21 - 321
调试发现，在f(20)进入之后，var x int，初始化的时候 x = 1；
*/
```

我们可以看到，在多次调用中，变量 x 的值是被保留的，即 0 + 1 = 1，然后 1 + 20 = 21，最后 21 + 300 = 321：**闭包函数保存并积累其中的变量的值**，不管外部函数退出与否，它都能够继续操作外部函数中的局部变量。——其实就相当于f()这个函数还没有退出。

- **一个理解：**没有闭包的时候，函数就是一次性买卖，函数执行完毕后就无法再更改函数中变量的值（应该是内存释放了）；有了闭包后函数就成为了一个变量的值，只要变量没被释放，函数就会一直处于存活并独享的状态，因此可以后期更改函数中变量的值（因为这样就不会被go给回收内存了，会一直缓存在那里）。

另外，闭包函数也能像普通函数一样，使用外部声明的变量：

```go
var xx int
	func(i int) {
		s := 0
		for j := 0; j < i; j++ {
			s += j
		}
		xx = s
	}(10) // Passes argument 1000 to the function literal.
	fmt.Println(xx)		// 输出：45
//这样闭包函数就能够被应用到整个集合的元素上，并修改它们的值。

```

- **但是这种操作是不建议的**！因为相当于在函数内部修改全局的值，这会让程序变得混乱。
- 

**结论：**函数外的变量只能通过参数传递进去，不要通过全局变量的方式的渠道传递进去，当函数内能读取到的变量越多，出错概率(误操作)也就越高。

#### 6.2 使用闭包调试

> 在分析和调试复杂的程序时，无数个函数在不同的代码文件中相互调用，如果这时候能够准确地知道哪个文件中的具体哪个函数正在执行，对于调试是十分有帮助的。

- 可以使用 `runtime` 或 `log` 包中的特殊函数来实现这样的功能。

```go
log.SetFlags(log.Llongfile)
log.Print("")
```

- 包 `runtime` 中的函数 `Caller()` 提供了相应的信息，因此可以在需要的时候实现一个 `where()` 闭包函数来打印函数执行的位置：

```go
where := func() {
    _, file, line, _ := runtime.Caller(1)
    log.Printf("%s:%d", file, line)
}
where()
// some code
where()
// some more code
where()

```

或者更简单，直接用where()指向log信息打印函数（相当于闭包函数的返回，一个意思了）：

```
var where = log.Print
func func1() {
where()
... some code
where()
... some code
where()
}
```



## 四、数组与切片

> 这一点上go和Python有很多类似的地方。数组在go中有一些特定的用处，但是却有一些呆板，所以在 Go 语言的代码里并不是特别常见。相对的，切片却是随处可见的，它们构建在数组之上并且提供更强大的能力和便捷

### 1、声明和初始化

- 数组是具有相同 **唯一类型** 的一组以编号且长度固定的数据项序列（这是一种同构的数据结构）
- 数组长度也是数组类型的一部分，所以 [5] int 和 [10] int 是属于不同类型的。
- 如果我们想让数组元素类型为任意类型的话可以使用空接口作为类型，然后再使用具体值的时候先做一下类型判断。
- 基本的操作，和C/Java类似

```go
//声明
var arr1 [5]int
```

Go 语言中的数组是一种 **值类型**（不像 C/C++ 中是指向首元素的指针），所以可以通过 `new()` 来创建： `var arr1 = new([5]int)`。——类比于Java（`int[] a = new int[5]`）

- **但是这种方式和`var arr2 [5]int`有区别，**arr1 的类型是 `*[5]int`，而 arr2 的类型是 `[5]int`。具体而言：
  - 当把一个数组赋值给另一个时，需要再做一次数组内存的**拷贝**操作。`arr2 := *arr1; arr2[2] = 100`；两个数组就有了不同的值，在赋值后修改 arr2 不会对 arr1 生效。

- 所以在函数中数组作为参数传入（值传递）时，如 `func1(arr2)`，会产生一次数组拷贝，func1 方法不会修改原始的数组 arr2。——**所以这就是传参的值传递的底层原理！**

- 如果你想修改原数组，那么 arr2 必须通过 & 操作符以引用方式传过来，例如 func1 (&arr2）

#### 1.1 数组常量初始化

```go
package main
import "fmt"

func main() {
    // var arrAge = [5]int{18, 20, 15, 22, 16}
    // var arrLazy = [...]int{5, 6, 7, 8, 22}
    // var arrLazy = []int{5, 6, 7, 8, 22}
    var arrKeyValue = [5]string{3: "Chris", 4: "Ron"}
    // var arrKeyValue = []string{3: "Chris", 4: "Ron"}

    for i:=0; i < len(arrKeyValue); i++ {
        fmt.Printf("Person at %d is %s\n", i, arrKeyValue[i])
    }
}
/*
Person at 0 is
Person at 1 is
Person at 2 is
Person at 3 is Chris
Person at 4 is Ron
*/
```

#### 1.2 多维数组

> 数组通常是一维的，但是可以用来组装成多维数组，例如：`[3][5]int`，`[2][2][2]float64`。
>
> 和大多数的语言一致

#### 1.3 将数组传递给函数

> 把一个大数组传递给函数会消耗很多内存。有两种方法可以避免这种现象：
>
> - 传递数组的指针
> - **使用数组的切片**

指针都很好理解，就是把数组的首地址传进去，然后在通过这个地址转换成具体的值：

```go
package main
import "fmt"

func main() {
    array := [3]float64{7.0, 8.5, 9.1}
    x := Sum(&array) // Note the explicit address-of operator
    // to pass a pointer to the array
    fmt.Printf("The sum of the array is: %f", x)
}

func Sum(a *[3]float64) (sum float64) {
    for _, v := range *a { // derefencing *a to get back to the array is not necessary!
        sum += v
    }
    return
}
/*
The sum of the array is: 24.600000
*/
```

但这在 Go 中并不常用，通常使用切片↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

### 2、切片

> 切片（slice）是对数组一个连续片段的引用（该数组我们称之为相关数组，通常是匿名的），所以切片是一个引用类型（因此更类似于 C/C++ 中的数组类型，或者 Python 中的 list 类型）。

切片拥有**长度**和**容量**。
切片的长度**是它所包含的元素个数**。（实际的元素个数）
切片的容量**是从它的第一个元素开始数，到其底层数组元素末尾的个数**。(注意，就是从当前第一个，到最后一个；所以cap一定是大于等于len的)

```go
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// 截取切片使其长度为 0
	s = s[:0]
	printSlice(s)

	// 拓展其长度
	s = s[:4]
	printSlice(s)

	// 舍弃前两个值
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
/*
len=6 cap=6 [2 3 5 7 11 13]
len=0 cap=6 []
len=4 cap=6 [2 3 5 7]
len=2 cap=4 [5 7]
*/
```





- 切片提供了一个相关数组的动态窗口，和数组不同的是，切片的长度可以在运行时修改，最小为 0 最大为相关数组的长度：切片是一个 **长度可变的数组**。

- 切片提供了计算容量的函数 `cap()` 可以测量切片最长可以达到多少：它等于切片从第一个元素开始，到相关数组末尾的元素个数。如果 s 是一个切片，`cap(s)` 就是从 `s[0]` 到数组末尾的数组长度。切片的长度永远不会超过它的容量，所以对于 切片 s 来说该不等式永远成立：`0 <= len(s) <= cap(s)`。

- **优点** 因为切片是**引用**，所以它们不需要使用额外的内存并且比使用数组更有效率，所以在 Go 代码中 切片比数组更常用。(所以对切片的修改就是对原始数据的同步修改)

- 声明切片的格式是： `var identifier []type`（和数据的神明一样）。一个切片在未初始化之前默认为 nil，长度为 0。

  切片的初始化格式是：`var slice1 []type = arr1[start:end]`。

  切片也可以用类似数组的方式初始化：`var x = []int{2, 3, 5, 7, 11}`。这样就创建了一个长度为 5 的数组并且创建了一个相关切片。（**基本可以看到，数组和切片很容易混用**）





#### 2.1 用 make () 创建一个切片

当相关数组还没有定义时，我们可以使用 make () 函数来创建一个切片 同时创建好相关数组：`var slice1 []type = make([]type, len)`。也可以简写为 `slice1 := make([]type, len)`，这里 `len` 是数组的长度并且也是 `slice` 的初始长度。

- 定义 `s2 := make([]int, 10)`，那么 `cap(s2) == len(s2) == 10`。
- make 的使用方式是：`func make([]T, len, cap)`，其中 cap 是可选参数。
- 也可以先创建数组，然后对其进行切片：

```go
make([]int, 50, 100)
new([100]int)[0:50]
```

#### 2.2 new()和make()的区别

> 看起来二者没有什么区别，都在堆上分配内存，但是它们的行为不同，适用于不同的类型。

- new (T) 为每个新的类型 T 分配一片内存，初始化为 0 并且返回类型为 * T 的内存地址：这种方法 **返回一个指向类型为 T，值为 0 的地址的指针**，它适用于值类型如数组和结构体
- make(T) **返回一个类型为 T 的初始值**，返回值为类型本身，而不是指针。它只适用于 3 种内建的引用类型：切片、map 和 channel

换言之，new 函数分配内存，make 函数初始化

#### 2.3  bytes 包

> bytes 包和字符串包十分类似。而且它还包含一个十分有用的类型 Buffer.

Buffer 可以这样定义：`var buffer bytes.Buffer`。

**通过 buffer 串联字符串**:

类似于 Java 的 StringBuilder 类。

在下面的代码段中，我们创建一个 buffer，通过 buffer.WriteString(s) 方法将字符串 s 追加到后面，最后再通过 buffer.String() 方法转换为 string：

```go
var buffer bytes.Buffer
for {
    if s, ok := getNextString(); ok { //method getNextString() not shown here
        buffer.WriteString(s)
    } else {
        break
    }
}
fmt.Print(buffer.String(), "\n")
```

这种实现方式比使用 `+=` 要更节省内存和 CPU，尤其是要串联的字符串数目特别多的时候。

### 3、for-range结构

```go
for ix, value := range slice1 {
    ...
}
```

如果只需要index，_ 可以用于忽略索引，也可以直接忽略第二个变量：

```go
for ix := range seasons {
    fmt.Printf("%d", ix)
}
// Output: 0 1 2 3
```

**多维切片下的 for-range：**

```go
for row := range screen {
    for column := range screen[row] {
        screen[row][column] = 1
    }
}
```

### 4、切片重组（reslice）

我们已经知道切片创建的时候通常比相关数组小。

- 将切片扩展 1 位可以这么做：

```go
sl = sl[0:len(sl)+1]
```

切片可以反复扩展直到占据整个相关数组：

```go
var ar = [10]int{0,1,2,3,4,5,6,7,8,9}
var a = ar[5:7] // reference to subarray {5,6} - len(a) is 2 and cap(a) is 5
a = a[0:4] // ref of subarray {5,6,7,8} - len(a) is now 4 but cap(a) is still 5：所以，它的扩展是按照a[0]的位置，不是ar[0]的位置
```

#### 4.1 切片的复制和追加

如果想增加切片的容量，我们必须**创建一个新的更大的**切片并把原分片的内容都拷贝过来。下面的代码描述了从拷贝切片的 copy 函数和向切片追加新元素的 append 函数。

```go
sl_from := []int{1, 2, 3}
sl_to := make([]int, 10)

n := copy(sl_to, sl_from)
fmt.Println(sl_to)                    //[1 2 3 0 0 0 0 0 0 0]
fmt.Printf("Copied %d elements\n", n) // n == 3

sl3 := []int{1, 2, 3}
sl3 = append(sl3, 4, 5, 6)
fmt.Println(sl3) //[1 2 3 4 5 6]
```



## 五、Map

> 这种数据封装还是非常常见的。一种元素对（pair）的无序集合，pair 的一个元素是 key，对应的另一个元素是 value，所以这个结构也称为关联数组或字典。map 这种数据结构在其他编程语言中也称为字典（Python）、hash 和 HashTable （Java）等。

### 1、声明、初始化和make

map 是引用类型，可以使用如下声明：

```go
//格式：var map1 map[keytype]valuetype
var map1 map[string]int
```

在声明的时候不需要知道 map 的长度，map 是可以动态增长的。  未初始化的 map 的值是 nil。

- key 可以是任意可以用 == 或者！= 操作符比较的类型，比如 string、int、float。所以切片和结构体不能作为 key (译者注：含有数组切片的结构体不能作为 key，**只包含内建类型的 struct 是可以作为 key 的**（就Node这种定义的结构体还是可以作为key的）），但是指针和接口类型可以。
  - 如果要用结构体作为 key 可以提供 `Key()` 和 `Hash()` 方法，这样可以通过结构体的域计算出唯一的数字或者字符串的 key。
- 如果 key1 是 map1 的 key，那么 `map1[key1]` 就是对应 key1 的值，就如同数组索引符号一样（数组可以视为一种简单形式的 map，key 是从 0 开始的整数）。key1 对应的值可以通过赋值符号来设置为 `val1：map1[key1] = val1`。（可见，go的map比Java中的获取map.get要简单很多）

- map 是 **引用类型** 的： 内存用 make 方法来分配。map 的初始化：`var map1 = make(map[keytype]valuetype)`。
- **不要使用 new，永远用 make 来构造 map** 如果错误的使用 new () 分配了一个引用对象，你会获得一个空引用的指针，相当于声明了一个未初始化的变量并且取了它的地址。



#### 1.1 map 容量

和数组不同，map 可以根据新增的 key-value 对动态的伸缩，因此它不存在固定长度或者最大限制。但是你也可以选择标明 map 的初始容量 `capacity`，就像这样：`map2 := make(map[string]float32, 100)`；当 map 增长到容量上限的时候，如果再增加新的 key-value 对，map 的大小会自动加 1。所以出于性能的考虑，对于大的 map 或者会快速扩张的 map，即使只是大概知道容量，也最好先标明。（在Java中，map的初始化、扩缩容都很讲究，一开始是16，每次容量是倍增，因此，Java中对于map的容量设置就要合理，避免大量扩缩容影响性能。）

#### 1.2 测试键值对是否存在及删除元素

如果 map 中不存在 key1，val1 就是一个值类型的空值。这就会给我们带来困惑了：现在我们没法区分到底是 key1 不存在还是它对应的 value 就是空值。解决方法如下：

1. 用：`val1, isPresent = map1[key1]`；isPresent 返回一个 bool 值：如果 key1 存在于 map1，val1 就是 key1 对应的 value 值，并且 isPresent 为 true；如果 key1 不存在，val1 就是一个空值，并且 isPresent 会返回 false。
2. 如果你只是想判断某个 key 是否存在而不关心它对应的值到底是多少，你可以这么做：`_, ok := map1[key1] // 如果key1存在则ok == true，否则ok为false`
3. 从 map1 中删除 key1：直接 `delete(map1, key1)` 就可以。如果 key1 不存在，该操作不会产生错误。

#### 1.3 for-range 的配套用法

可以使用 for 循环遍历 map：

```go
map1 := make(map[int]float32)
    map1[1] = 1.0
    map1[2] = 2.0
    map1[3] = 3.0
    map1[4] = 4.0
    for key, value := range map1 {
        fmt.Printf("key is: %d - value is: %f\n", key, value)
    }
```

#### 1.4 map 类型的切片

想获取一个 map 类型的切片，我们必须使用**两次 `make()` 函数**，第一次分配切片，第二次分配 切片中每个 map 元素：

```go
package main
import "fmt"

func main() {
    // Version A:
    items := make([]map[int]int, 5)
    for i:= range items {
        items[i] = make(map[int]int, 1)
        items[i][1] = 2
    }
    fmt.Printf("Version A: Value of items: %v\n", items)

    // Version B: NOT GOOD!
    items2 := make([]map[int]int, 5)
    for _, item := range items2 {
        item = make(map[int]int, 1) // item is only a copy of the slice element.
        item[1] = 2 // This 'item' will be lost on the next iteration.
    }
    fmt.Printf("Version B: Value of items: %v\n", items2)
}
/*
Version A: Value of items: [map[1:2] map[1:2] map[1:2] map[1:2] map[1:2]]
Version B: Value of items: [map[] map[] map[] map[] map[]]
*/
```

**应当像 A 版本那样通过索引使用切片的 map 元素。**在 B 版本中获得的项只是 map 值的一个拷贝而已，所以真正的 map 元素没有得到初始化。

#### 1.5 map 的排序

- map 默认是无序的，不管是按照 key 还是按照 value 默认都不排序;
- 如果你想为 map 排序，需要将 **key（或者 value）拷贝**到一个切片，再对切片排序，再用这个有序切片对应的index去取value

```go
// the telephone alphabet:
package main
import (
    "fmt"
    "sort"
)

var (
    barVal = map[string]int{"alpha": 34, "bravo": 56, "charlie": 23,
                            "delta": 87, "echo": 56, "foxtrot": 12,
                            "golf": 34, "hotel": 16, "indio": 87,
                            "juliet": 65, "kili": 43, "lima": 98}
)

func main() {
    fmt.Println("unsorted:")
    for k, v := range barVal {
        fmt.Printf("Key: %v, Value: %v / ", k, v)
    }
    keys := make([]string, len(barVal))
    i := 0
    for k, _ := range barVal {
        keys[i] = k
        i++
    }
    sort.Strings(keys)
    fmt.Println()
    fmt.Println("sorted:")
    for _, k := range keys {
        fmt.Printf("Key: %v, Value: %v / ", k, barVal[k])
    }
}
/*
unsorted:
Key: bravo, Value: 56 / Key: echo, Value: 56 / Key: indio, Value: 87 / Key: juliet, Value: 65 / Key: alpha, Value: 34 / Key: charlie, Value: 23 / Key: delta, Value: 87 / Key: foxtrot, Value: 12 / Key: golf, Value: 34 / Key: hotel, Value: 16 / Key: kili, Value: 43 / Key: lima, Value: 98 /
sorted:
Key: alpha, Value: 34 / Key: bravo, Value: 56 / Key: charlie, Value: 23 / Key: delta, Value: 87 / Key: echo, Value: 56 / Key: foxtrot, Value: 12 / Key: golf, Value: 34 / Key: hotel, Value: 16 / Key: indio, Value: 87 / Key: juliet, Value: 65 / Key: kili, Value: 43 / Key: lima, Value: 98 /
*/
```

#### 1.6 将 map 的键值对调

这里对调是指调换 key 和 value。如果 map 的值类型可以作为 key 且所有的 value 是唯一的，那么通过下面的方法可以简单的做到键值对调。——其实就是用for-range函数，把map的key和value都拿出来，然后对调一下就行了

```
package main
import (
    "fmt"
)

var (
    barVal = map[string]int{"alpha": 34, "bravo": 56, "charlie": 23,
                            "delta": 87, "echo": 56, "foxtrot": 12,
                            "golf": 34, "hotel": 16, "indio": 87,
                            "juliet": 65, "kili": 43, "lima": 98}
)

func main() {
    invMap := make(map[int]string, len(barVal))
    for k, v := range barVal {
        invMap[v] = k
    }
    fmt.Println("inverted:")
    for k, v := range invMap {
        fmt.Printf("Key: %v, Value: %v / ", k, v)
    }
}
```

如果原始 value 值不唯一那么这么做肯定会出错；为了保证不出错，当遇到不唯一的 key 时应当立刻停止，这样可能会导致没有包含原 map 的所有键值对！

- 一种解决方法就是仔细检查唯一性并且使用多值 map，比如使用 `map[int][]string `类型。



## 六、包（package）







## 七、结构与方法

### 1、 结构体定义

结构体定义/赋值的一般方式如下：

```go
type identifier struct {
    field1 type1
    field2 type2
    ...
}

var s identifier
s.field1 = 5
s.field2 = 8
```

会发现这里的结构体很像Java中的class（经常用户表示一个node，LinkedNode这种）

数组可以看作是一种结构体类型，不过它使用下标而不是具名的字段。

#### 1、使用 new给一个新的结构体变量分配内存

使用 **new** 函数给一个新的结构体变量分配内存，它返回指向已分配内存的指针：`var t *T = new(T)`

- 写这条语句的惯用方法是：`t := new(T)`，变量 `t` 是一个指向 `T` 的指针，此时结构体字段的值是它们所属类型的零值。

```go
package main
import "fmt"

type struct1 struct {
    i1  int
    f1  float32
    str string
}

func main() {
	ms := new(struct1)
	mx1 := struct1{}
	fmt.Println("mx1空的初始化:", mx1)
	fmt.Println("ms空的初始化:", ms)
	ms.i1 = 10
	ms.f1 = 15.5
	ms.str = "Chris"

	mx2 := struct1{1, 2, "ss"}
	fmt.Println(mx2)
	fmt.Printf("The int is: %d\n", ms.i1)
	fmt.Printf("The float is: %f\n", ms.f1)
	fmt.Printf("The string is: %s\n", ms.str)
	fmt.Println(ms)		//使用 fmt.Println 打印一个结构体的默认输出可以很好的显示它的内容，类似使用 %v 选项。
}
/*
&{0 0 }                     
The int is: 10              
The float is: 15.500000     
The string is: Chris        
&{10 15.5 Chris}   
*/
```

- 就像在面向对象语言所作的那样，可以使用点号符给字段赋值：`structname.fieldname = value`。


- 同样的，使用点号符可以获取结构体字段的值：`structname.fieldname`;


**初始化一个结构体实例**（一个结构体字面量：struct-literal）的更简短和惯用的方式如下：

```go
ms := &struct1{10, 15.5, "Chris"}
// 此时ms的类型是 *struct1
//或者下面这种方式，更惯用：
var ms struct1
ms = struct1{10, 15.5, "Chris"}
```

```go
package main
import (
    "fmt"
    "strings"
)

type Person struct {
    firstName   string
    lastName    string
}

func upPerson(p *Person) {
    p.firstName = strings.ToUpper(p.firstName)
    p.lastName = strings.ToUpper(p.lastName)
}

func main() {
    // 1-struct as a value type:
    var pers1 Person
    pers1.firstName = "Chris"
    pers1.lastName = "Woodward"
    upPerson(&pers1)
    fmt.Printf("The name of the person is %s %s\n", pers1.firstName, pers1.lastName)

    // 2—struct as a pointer:
    pers2 := new(Person)
    pers2.firstName = "Chris"
    pers2.lastName = "Woodward"		//可以直接通过指针赋值，Go 会自动做这样的转换。
    (*pers2).lastName = "Woodward"  // 这是合法的，通过解指针的方式来设置值
    upPerson(pers2)
    fmt.Printf("The name of the person is %s %s\n", pers2.firstName, pers2.lastName)

    // 3—struct as a literal:
    pers3 := &Person{"Chris","Woodward"}
    upPerson(pers3)
    fmt.Printf("The name of the person is %s %s\n", pers3.firstName, pers3.lastName)
}

/*
The name of the person is CHRIS WOODWARD
The name of the person is CHRIS WOODWARD
The name of the person is CHRIS WOODWARD
*/
```

注意：

1. 混合字面量语法（composite literal syntax）`&struct1{a, b, c}` 是一种简写，底层仍然会调用 `new ()`，这里值的顺序必须按照字段顺序来写。
2. 值必须以字段在结构体定义时的顺序给出，**&** 不是必须的。

#### 1.2、 递归结构体

结构体类型可以通过引用自身来定义。这在定义链表或二叉树的元素（通常叫节点）时特别有用，此时节点包含指向临近节点的链接（地址）。

![image-20220621162951552](https://cdn.learnku.com/uploads/images/201808/27/23/T9sRQUcN1b.jpg?imageView2/2/w/1240/h/0)

```go
type Node struct {
    data    float64
    su      *Node
}
```

#### 1.3、结构体转换

> Go 中的类型转换遵循严格的规则。当为结构体定义了一个 alias 类型时，此结构体类型和它的 alias 类型都有相同的底层类型，才能转换

```go
package main
import "fmt"

type number struct {
    f float32
}

type nr number   // alias type

func main() {
    a := number{5.0}
    b := nr{5.0}
    // var i float32 = b   // compile-error: cannot use b (type nr) as type float32 in assignment
    // var i = float32(b)  // compile-error: cannot convert b (type nr) to type float32
    // var c number = b    // compile-error: cannot use b (type nr) as type number in assignment
    // needs a conversion:
    var c = number(b)
    fmt.Println(a, b, c)
}

//输出：{5} {5} {5}
```

练习：

1. 

```go
package main

import (
	"time"
	"fmt"
)

type Address struct {
	Street           string
	HouseNumber      uint32
	HouseNumberAddOn string
	POBox            string
	ZipCode          string
	City             string
	Country          string
}

type VCard struct {
	FirstName string
	LastName  string
	NickName  string
	BirtDate  time.Time
	Photo     string
	Addresses map[string]*Address
}

func main()  {
	addr1 := &Address{"Elfenstraat", 12, "", "", "2600", "Mechelen", "België"}
	addr2 := &Address{"Heideland", 28, "", "", "2640", "Mortsel", "België"}
	addrs := make(map[string]*Address)

	addrs["youth"] = addr1
	addrs["now"] = addr2

	birthdt := time.Date(1956, 1, 17, 15, 4, 5, 0, time.Local)
	photo := "MyDocuments/MyPhotos/photo1.jpg"
	vcard := &VCard{"Ivo", "Balbaert", "", birthdt, photo, addrs}

	fmt.Printf("Here is the full VCard: %v\n", vcard)
	fmt.Printf("My Addresses are:\n %v\n %v", addr1, addr2)

}

/*
Here is the full VCard: &{Ivo Balbaert  1956-01-17 15:04:05 +0800 CST MyDocument
s/MyPhotos/photo1.jpg map[now:0xc000120070 youth:0xc000120000]}
My Addresses are:
 &{Elfenstraat 12   2600 Mechelen België}
 &{Heideland 28   2640 Mortsel België}

*/

```

2. 使用坐标 X、Y 定义一个二维 Point 结构体。同样地，对一个三维点使用它的极坐标定义一个 Polar 结构体。实现一个 Abs() 方法来计算一个 Point 表示的向量的长度，实现一个 Scale 方法，它将点的坐标乘以一个尺度因子（提示：使用 math 包里的 Sqrt 函数）。


```go
package main

import (
	"math"
	"fmt"
)

type Point struct {
	X, Y float64
}

type Point3 struct {
	X, Y, Z float64
}

type Polar struct {
	R, T float64
}

func Abs(p *Point) float64 {
	return math.Sqrt(float64(p.X*p.X + p.Y*p.Y))
}

func Scale(p *Point, s float64) (q Point) {
	q.X = p.X * s
	q.Y = p.Y * s
	return
}

func main() {
	p1 := new(Point)
	p1.X = 3
	p1.Y = 4
	fmt.Printf("The length of the vector p1 is: %f\n", Abs(p1))

	p2 := &Point{4, 5}
	fmt.Printf("The length of the vector p2 is: %f\n", Abs(p2))

	q := Scale(p1, 5)
    fmt.Printf("The length of the vector q is: %f\n", Abs(&q))		//Abs传入的是指针，q不是指针，那就要先要转换为地址

	fmt.Printf("Point p1 scaled by 5 has the following coordinates: X %f - Y %f", q.X, q.Y)
}
/*
The length of the vector p1 is: 5.000000
The length of the vector p2 is: 6.403124
The length of the vector q is: 25.000000
Point p1 scaled by 5 has the following coordinates: X 15.000000 - Y 20.000000
 */
```

