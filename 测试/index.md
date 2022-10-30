# Go Syntax


## 包、变量和函数

待填

## 流程控制语句（for、if、else、switch和defer）

待填

## 实用的类型

### 切片

+ 相关特性

1. 切片的长度len：切片内包含元素的数量
2. 切片的容量cap：受切片起点影响，由当前对数组引用形成的切片起点，到原数组末的元素数量
3. 当切片的长度与容量皆为0，则切片又被命名为nil（零值）



+ 特点

1. 切片是基于数组但又更灵活的
2. 通过append(s, elem)能够将若干元素加入切片

### Range遍历

- 可以用Range来遍历数组
- Range在遍历各元素时，会返回当前元素的下标和数值，因此在循环遍历中需要用两个变量来承接，如果不需要某个方面如下标，可以用_填充该方面，一般会先返回下标，再返回数值。若只要索引，可以忽略掉第二个变量

### 映射

+ 相关描述

1. 映射将键连接到值
2. 映射的零值是nil，nil既没有键，也没有值
3. make函数会返回给定类型的映射，并将其初始化，通常用make来无参初始化一个映射

```go
m = make(map[int]Vertex)
…
m[“hello”] = 8  // 对映射进行设置
```



* 映射的文法

```go
var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}
```

若顶级类型为一个系统类型名，则可以省略，例如上述的{}中的Vertex



* 对映射的高级处理（在上述基础上）

```go 
m[“a”] = 1
m[“a”] = 2  // 修改映射

elem = m[“a”]  // 获取键对应的值

v, ok = m[“a”]  // 查询键a是否在映射中,	若不在映射，则将v作为键“a“的零值
```



### 函数

- 函数不仅可以做参数，还可以做函数的返回值


```go
package main

import (
	"fmt"
	"math"
)

func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}  // 函数可以做参数

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)  // 函数可以做返回值
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))
	fmt.Println(compute(math.Pow))
}

```



### 函数的闭包

Go 函数可以是一个闭包。闭包是一个函数值，它引用了其函数体之外的变量。该函数可以访问并赋予其引用的变量的值，换句话说，该函数被这些变量“绑定”在一起。

也就是说Go函数可以实现自给自足，它的参数由内部参数决定，这样的结构使得其能够简化为名义上的函数值

- 代码体现：


```go
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

例如，函数 `adder` 返回一个闭包。每个闭包都被绑定在其各自的 `sum` 变量上。

- 一个闭包的简单示例


```go
// 利用函数的闭包实现斐波那契数列
package main

import "fmt"

// 返回一个“返回int的函数”（注意返回值的书写方法
func fibonacci() func() int {
	sum := 1
	m := -1
	return func() int{
		k := sum
		sum += m
		m = k
		return sum
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
```

## 方法和接口

### 方法

#### 1. 用结构体作为方法接收者

Go没有类，但可以为结构体赋予方法，结构体对象可以做方法接收者

```go
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

这样的话就可以调用`v.Abs()`处理v内部数据

**方法只是带接受者参数的函数**，但是方法一般不称为函数，如果令v作为Abs的形式参数，那么依然有Abs(v)，存在同样的效果。同时，也可以为非结构体类型声明方法，比如为基本数据类型取别名，然后为别名类型赋予方法。

但是，要注意接收者的类型定义和方法声明必须在同一包内，不能为内建类型声明方法。



#### 2.用结构体指针作为方法接收者

作为方法的函数可以对接收者内的元素进行直接的操作，若要使得操作生效，就需要将一般方法接收者升级为指向改接受者的指针，否则函数修改的只是接收者的副本，并不会对原结构体类型生效。

```go
func (*v Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```



#### 3.指针重定向

- 通常带指针参数的函数只能接收指针类型实参，值传递型函数只接受值传入。但方法不同，如果接受者为指针类型，那么在调用方法时接收者可以为值也可以为指针类型，因为系统会将值解释为指针类型，即实现定向。

```go
var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
```

+ 同样的事情也发生在相反的方向，当方法的接受者为值类型，那么在调用方法时如果接收者为指针，系统会将其转化为值类型

```go
var v Vertex
fmt.Println(v.Abs()) // OK
p := &v
fmt.Println(p.Abs()) // OK
```



#### 4.选择值或指针做接收者

- 使用指针接收者的原因有二：

1. 首先，方法能够修改其接收者指向的值。
2. 其次，这样可以避免在每次调用方法时复制该值。若值的类型为大型结构体时，这样做会更加高效。
3. 通常来说，所有给定类型的方法都应该有值或指针接收者，但并不应该二者混用



### 接口

#### 1.相关描述

- 接口是一种数据类型，是由一组方法签名定义的集合
- 接口类型的变量可以保存任何实现了这些方法的值
- 外界参量需与接口内部方法的接受者类型保持一致才能够让接口保存参量

代码片段如下：

```go
func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

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
```



#### 2.接口与隐式实现

- 类型通过实现一个接口的所有方法来实现该接口。既然无需专门显式声明，也就没有“implements”关键字。
- 隐式接口从接口的实现中解耦了定义，这样接口的实现可以出现在任何包中，无需提前准备。
- 因此，也就无需在每一个实现上增加新的接口名称，这样同时也鼓励了明确的接口定义。

这个刚开始会有点难理解，不过可以通过代码来简化我们的理解：

```go
type I interface {
	M()
	N()
}

type T struct {
	S string
}

// 此方法表示类型 T 实现了接口 I，但我们无需显式声明此事。
func (t T) M() {
	fmt.Println(t.S)
}

func (t T) N() {
	fmt.Println(t.S, "World")
}

func main() {
	var i I = T{"hello"}
	i.M()
	i.N()
}
```

如果实际接收者与接口内方法接收者类型相同，那么该数据类型表示便可以用接口名替换，该接收者也可以调用接口内对应方法。




