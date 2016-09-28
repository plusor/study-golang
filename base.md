# 编程基础

==========

### 基本数据类型

名称|宽度(字节)|零值|说明
----------|---------|---------|---------
bool|1|false|
byte|1|0|
rune|4|0|
int/uint|-|0|宽度与平台有关
int8/uint8|1|0|
int16/uint16|2|0|
int32/uint32|4|0|
int64/uint64|8|0|
float32|4|0.0|
float64|8|0.0|
complex64|8|0.0|
complex128|16|0.0|
string|-|""|字符串类型的值是不可变的, 创建后不可被改变


> 指针类型的零值是`nil`


#### 常量&变量

和大多数语言一样, 常量只能被赋值一次

```go
// 申明无类型常量
const untypedConstant = 10.0

//申明有类型常量
const typedConstant int64 = 1024

// 常量申明支持并行赋值
const t1, t2, t3 = 1, 2, 3

// 申明多个常量
const (
  t1 = 1
  t2 = 2
)
// 在常量申明的括号内, 如果一个常量没有被赋值, 那么它的值等于上一个常量的值
const (
  t1 = 1
  t2 = 2
  t3		 // 2	隐式申明. 
  t4		 // 2	隐式申明. 
  t5		 // 2	隐式申明. 
)
// 一行内未被赋值的常量数量必须与上一行数量一样
const (
  t1 = 1
  t2, t3 = 2, 3
  t4, t5 		// 2, 3	隐式申明. 
)

// 预定义标示符 iota 在括号内递增
const(
  t1 = iota 	// 0
  t2			// 1	隐式申明. 
  _				// 2	使用 "_" 占位	隐式申明. 
  t3			// 3					隐式申明. 
  t4			// 4					隐式申明. 
  ...
)

const(
  t1 = 1 << iota 	// 1  	1 << 0		隐式申明. 
  t2				// 2	1 << 1		隐式申明. 
  t3				// 4	1 << 2		隐式申明. 
)


// 变量申明
var v int = 1
// 或申明无类型变量
var v = 1
// 并行赋值
var a, b = 1, 2
// 多行申明

var (
  a = 1
  b = 2
)
```

> **变量申明注意不支持隐式申明.**

```go
	
//全局变量&局部变量

package main

import (
  "fmt"
)

var env = "development"			// 全局变量

func main(){
  var env = "test"			// 局部变量
  fmt.Printf("env: %s", env) // "env: test"
}
```



#### 数组

```go
// 表示长度为 num 个元素类型为 type 的数组.
[num]type
// 如: [10]string, 初始化在后面加上大括号:
[10]string{"Go", "Ruby", "Java", "Python", "C", "C++", "PHP", ".NET"}


// 也可以指定字符串在数组内的下标:
[10]string{4: "Go", 0: "Ruby", 1: "Java", 2: "Python", 3: "C", 6: "C++", 5: "PHP", 7: ".NET"}

// 或不显示指定数组索引值:
[...]string{4: "Go", 0: "Ruby", 1: "Java", 2: "Python", 3: "C", 6: "C++", 5: "PHP", 7: ".NET"}
```

#### 切片

> `[]type` // 表示元素类型为`type`的切片.初始化同数组一样.

```go
array := []string{"Ruby", "Java", ".NET", "Python", "PHP"}[:3]
array[:3] => [Ruby, Java, .NET]
array[3:] => [Python, PHP]
array[1:2:3] => [Java] //长度为3
cap(array[1:2:3]) => 2
```
			
#### 字典

> `map[keyType]valueType` 表示键类型为`keyType`,值类型为`valueType`的字典. 键要求为值必须是可比较(=或!=)的

```go
prices := map[string]int{"iphone": 5200, "xiaomi": 1999, "meizu": 2999}
=> [xiaomi:1999 chuizi:2999 iphone:5200]

delete(prices, "meizu") // 删除某个键
```
		
		
#### 函数&方法

```go
// 函数
// Method 函数名称. 注意: 匿名函数不申明函数名称
// (arg1 int, arg2 int) 函数的参数, 如果参数类型相同的话可以简写为 arg1, arg2 int
// (result int) 函数返回值, 如果只有一个值可以去掉括号. 也可以去掉变量名
func Method(arg1 int, arg2 int) (result int) {
  result = arg1 * arg2
  return
  // 等同于
  // return arg1 * arg2
}

// 可变长参数表示法, 
func Method(args ...int) int {
  args // 切片类型
}

// 方法
type Products []string

// (self Products) 方法的接受者, `self` 为方法接收者的变量
// length() 方法名称, 括号内为方法参数
// int 方法返回值类型
func (self Products) length() int {
  return len(self)
}

products := Products{"Mac", "iPhone", "iPad", "Watch", "Music"}
products.length() => 5

注意参数不支持默认值
```

		
> 方法的接收者可以是一个自定义的数据类型,也可以是一个与某个自定义数据类型对应的指针类型.


#### 接口

```go

// 定义了一个 FileInterface 的接口, 该接口包括两个方法  Write() ,  Read() . 如果一个方法的接收者实现了这两个方法, 那么就说明这个接收者实现了这个接口.
type FileInterface interface {
  Write() bool
  Read(str string) string
}

// 在这里定义了一个 LogFile 的结构体, 并且实现了两个方法  Write() ,  Read() .
type LogFile struct {
}

func (file *LogFile) Write() bool {
  return true
}

func (file *LogFile) Read(str string) string {
  return str
}

// 断言 LogFile 实例是否实现了 FileInterface 接口
interface{}(&LogFile{}).(FileInterface) => true
// 没有实现该接口, 因为方法的接收者是指针类型,而不是值类型
interface{}(LogFile{}).(FileInterface) => false
```

#### 结构体

```go

type Cat struct { // 申明一个名为 Cat 的结构体, 包含大括号内几个属性
  age  	int
  name 	string
}

func (self Cat) Kind() string {
  return self.name
}

// 属性名称首字母如果是小写的则只能在当前代码包中可以访问属性值

// "继承"匿名字段方法
type Garfield struct {
  Cat
  color string
}

o := Garfield{Cat{1, "Garfield"}, "gray"} //或 Garfield{Cat: Cat{1, "Garfield"}, color: "gray"}
o.Kind()
// => "Garfield"


// 匿名结构体并初始化
box := struct {
  position int
  color string
}{1, 'blue'}

import "reflect"
// 字段附加属性		
type Person struct {
  Name 	string 	`json:"name"` // 多个标签用空格隔开,如: `json:"name" bson:"name"`
  Age	 	uint8	`json:"age"`
  Address string `json:"addr"`
}

person := Person{"Foo", 66, "China"}
reflect.TypeOf(person).Field(0).Tag
// => "json: \"name\""
reflect.TypeOf(person).Field(0).Tag.Get("json")
// => "name"
```

#### 指针

```go
// 转换成 unsafe.Pointer 类型
var v float32 = 1.0
pointer := unsafe.Pointer(&v)
// 转换成指向 int 类型的指针
inptr := (*int)(pointer)
// unsafe.Pointer 类型转换成 uintptr 类型的值
uptr := uintptr(pointer)
// uintptr 类型的值转换成 unsafe.Pointer 类型的值
pointer2 := unsafe.Pointer(uptr)


// 恒等性公式
uintptr(unsafe.Pointer(&s)) + unsafe.Offsetof(s.f) == uintptr(unsafe.Pointer(&s.f))
```	

#### 流程控制

```go

// 代码块作用域
package main

import (
  "fmt"
)

var v string = "1, 2, 3"

func main() {
  v := []int{1, 2, 3}
  if v != nil {
    var v int = 123
    fmt.Println(v)		// 123
  }
  fmt.Println(v)			// [1, 2, 3]
}
```

```go

// if 语句

if num < 100 {
  num++
} else {
  num--
}

if num == 1 {
  // ...
} else if num == 2 {
  // ...
} else if num == 3 {
  // ...
} else {
	// ...
}

```

```go

// switch 语句
switch day := "Monday"; day {
case "Monday", "Tuesday", "Wednesday", "Thursday", "Friday":
  fmt.Print("Go to work")
case "Saturday", "Sunday":
  fmt.Print("Go to happy")
default:
  fmt.Print("Funny")
}

// => Go to Work

// fallthrough. PS: 下面一行必须是 case 或 default 语句, 不能是其他表达式
switch day := "Monday"; day {
case "Monday", "Tuesday", "Wednesday", "Thursday", "Friday":
  fmt.Print("Go to work ")
  fallthrough
case "Saturday", "Sunday":
  fmt.Print("Go to happy ")
default:
  fmt.Print("Funny ")
}

// => Go To work Go to happy

// break 终止执行当前 switch, for, select 语句
switch day := "Monday"; day {
case "Monday", "Tuesday", "Wednesday", "Thursday", "Friday":
  break
case "Saturday", "Sunday":
  fmt.Print("Go to happy ")
default:
  fmt.Print("Funny ")
}

// =>


score := 80
switch {
case score == 100:
  fmt.Print("A")
case score >= 90:
  fmt.Print("B")
case score >= 80:
  fmt.Print("C")
case score >= 60:
  fmt.Print("D")
case score < 60:
  fmt.Print("F")
default:
  fmt.Print("UNKNOWN")
}

// => C

```

```go

// for 语句
num := 0
for num < 100 {	// 当 num 小于 100 时不断执行代码块
  num++
}
fmt.Print(num)
// => 100

for i:=0;i<100;num++ {
  fmt.Print(num)
}
// => 100

// 一直执行代码块
for {
	
}

```

```go
// rage
nums := []int{1,2,3,4,5]
for index, num := range ints {
  fmt.Printf("%d: %d\n", index, num)
}
// =>   0: 1
//     1: 2
//     2: 3
//     3: 4
//     4: 5
	
products := map[string]float64{"iPhone": 5200, "XiaoMi": 1999, "Chuizi": 2500}
for product, price := range products {
  fmt.Printf("%s: %f\n", product, price)
}
// =>   iPhone: 5200.000000
//     XiaoMi: 1999.000000
//     Chuizi: 2500.000000
	
products := map[string]float64{"iPhone": 5200, "XiaoMi": 1999, "Chuizi": 2500}
for name := range products {
  fmt.Printf("%s\n", name)
}
// =>   iPhone
//     XiaoMi
//     Chuizi
	
products := map[string]float64{"iPhone": 5200, "XiaoMi": 1999, "Chuizi": 2500}
for _, price := range products {
	fmt.Printf("%f\n", price)			
}
// =>   5200.000000
//     1999.000000
//     2500.000000
```

```go
// defer
func increment() (num int){
  defer func(){
    num++
  }()
  return 0  // 分解为 num = 0; 调用 defer 函数; 调用空的 return
}
// => 1

func ping() (num int){
  t := 1
  defer func(){
    t += 1
  }()
  return t  // 分解为 num = t; 调用 defer 函数 t=2(注意此时修改的是 t 的值); 调用空的 return
}
// => 1

func pong() (num int) {
	defer func(num int) {
		num += 1
	}(0)
	return num  // 分解为 返回num 的空值 0; 调用 defer num++ (注意两个变量名称 num 在内存中的地址不一样); 调用空的 return
}
// => 0

```

```go
// goto 语句 跳出当前循环
func main() {
	for i := 0; i < 10; i++ {
		fmt.Println(i)
		if i == 5 {
			goto L
		}
	}
L:
	fmt.Print("hello world\n")
}
// =>  0
//     1
//     2
//     3
//     4
//     5
//     hello world
```

```go
// 异常处理 error, panic&recover
func Exception() {
	defer func() {  // 捕获 panic 异常处理必须调用栈反方向的函数内的 defer 内
		if err := recover(); err != nil {
			fmt.Printf("Capture error: %s", err)
		}
	}()

	panic(errors.New("broken"))
}
// => Capture error: broken

```

#### 测试

```go

// 功能测试

// 基准测试

// 样本测试

// 测试运行记录

// 测试覆盖率
```
