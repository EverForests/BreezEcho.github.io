# Go语言实现贪吃蛇


> 摘要：贪吃蛇是我们从小玩到大的小游戏，本次实训以此为载体，将Go语言的基本语法与使用规范进行展示。同时在报告中也会对Go语言的独有特性进行介绍。



**一.项目分析**

(一)贪吃蛇玩法：

在一定区域内通过上下左右方向键控制蛇的方向，寻找食物，吃到食物后获得积分，并且蛇的身子变长，蛇不能碰墙与自己的身体。

 

(二)项目模块分析：

1.蛇模块

Ø 初始化蛇（出生）

Ø 蛇移动

Ø 碰撞食物，长度改变

Ø 碰撞墙或自己，蛇死亡

 

2.食物模块

Ø 食物产生

Ø 食物消失

 

3.游戏控制

Ø 控制键盘输入

Ø 游戏流程

 

4.游戏显示

Ø 游戏地图显示

 

**（三）流程初定**

基于以上的项目模块分析，我们可以初步确定项目实现的基本流程，在之后的每一步再不断优化下一步的处理。具体的流程如下：构建地图、食物类、蛇类 -> 图形显示器 -> 初始化地图信息 -> 初始化蛇信息 -> 设计食物随机产生器-> 设计游戏逻辑 -> 设计主程序。

 

**二.项目的实现**

文件结构：

$GOPATH: src\she.go

$GOROOT: src\Clib\Clib.go

 

**（一）地图、蛇类、食物类的构造**

1.地图设置

  作为一个合理的贪吃蛇游戏，同时考虑我们的贪吃蛇小游戏是在命令行中实现的。因此我们考虑全局声明地图的长和高，且将它们初始化为20。这样我们就可以在main包中的任意位置合理地调用它们。

```go
Package main
Import … 
const WIDE int = 20  // 设置布局
const HIGH int = 20
```

2.蛇类与食物类设置

  作为一款贪吃蛇小游戏，主角当然是我们的小蛇。小蛇有它的长度size，有运动方向dir（小蛇是有头有尾的）。那么，要将小蛇的长度与方向这两个外部显示属性进行修改并显示，就需要引入一个能够对小蛇位置进行控制的数据结构——数组。这个数组中存放着小蛇每一节的位置坐标，考虑到小蛇的运动自由性，我们可以将数组的容量设置为地图的大小即20*20 = 400。

作为贪吃蛇小游戏的次角——食物就没有那么多的信息，不过在后期可以对它的ui进行一个美化。

```go
type Snake struct{
    size int
    dir byte
    pos [WIDE*HIGH]Position
}

type Food struct{
    Position
}

```

**（二）设计图形显示器**

  定义好以上静态信息，如果不对它们进行外部显示，便失去了意义。考虑到我们最终设计出来的小蛇与食物的位置是在不断变化的，但每一次变化的位置可以用光标来控制，因此我们考虑用cgo即c与go语言进行混合编程来实现这一功能，因为c语言标准库中有简单的函数来实现命令行光标的移动。我们要做的仅仅是做一个接口，使得在Go中也能调用的这些c函数。

  首先，我们需要自行构建一个包，在这里我们命名为Clib。在GoRoot目录的src文件下新建一个文件夹并命名为Clib，然后在它的内部创建一个Go文件，写入以下信息：

```go
package Clib

/*
#include <windows.h>
#include <conio.h>

// 使用了WinAPI来移动控制台的光标
void gotoxy(int x, int y)
{
    COORD c;
    c.X = x, c.Y = y;
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), c);
}

// 从键盘获取一次按键，但不显示到控制台
int direct()
{
    return _getch();
}
//去掉控制台光标
void hideCursor()
{
    CONSOLE_CURSOR_INFO  cci;
    cci.bVisible = FALSE;
    cci.dwSize = sizeof(cci);
    SetConsoleCursorInfo(GetStdHandle(STD_OUTPUT_HANDLE), &cci);
}
*/
import "C"
//设置控制台光标位置
func GotoPosition(X int, Y int) {
    //调用C语言函数
    C.gotoxy(C.int(X), C.int(Y))  // go类型转换
}
//无显获取键盘输入的字符
func Direction() (key int) {
    key = int(C.direct())
    return
}
//设置控制台光标隐藏
func HideCursor() {
    C.hideCursor()
}

```



注意包头要注明package Clib，之后在Clib文件夹中新建的Go文件也是如此，这类似于一个归属标识，不可缺少。

/*与*/包裹的部分是C语言，也是Go中可以调用的函数的一个本源映射， 标准的头文件引入与函数定义都是需要的，与C中语法相同。一个要注意的点是import “C”与上面的C语言引用说明间是不能有空行的，否则在之后的主文件包调用中会报错；还有Go函数的名称一定要大写，这与后期调用有密切关系。通过以上两点我们可以看到Go语言对格式的高规范、高要求。

设计好我们需要的包后，就需要在主文件中调用采用使用其中的函数。

```go
import (
    "Clib"
    "fmt"
    "math/rand"
    "os"
    "time"
)

```

  回到正题，我们需要将命令行光标移动到蛇与食物的每个位置来设置它们的ui，因此我们在这里用的是用C设计的gotoxy函数与Go设计的GotoPosition函数。在主文件中，我们定义函数showUI，接口设置为目标（蛇或食物）的位置坐标与需要设置的ui图形。

```go
func ShowUI(X int, Y int, ui byte){
    // 找到对应坐标点光标位置
    Clib.GotoPosition(X*2+2,Y+2)  // 避免蛇移动影响棋盘边界
    // 绘制图形
    fmt.Fprintf(os.Stderr, "%c", ui)
}

```

(三)初始化地图信息

  地图的初始化是简单的，我们仅需将一个方框打印出来，由于篇幅有限这里只做出来它的简化图：

```go
func MapInit(){
    fmt.Fprintln(os.Stderr,`
    #------#
    |       |
    |       |
    #------#
    `)
}

```

可以想象一下，实际上水平方向有40个“-”，竖直方向有20个“|”。

这里采用了一个特殊函数Fprintln，对比ShowUI中的Fprintf，我们可以看到其中的相似与差别:

| Fprintf  | Fprintf 根据 format 参数生成格式化的字符串并写入 w 。返回写入的字节数和遇到的任何错误。 |
| -------- | ------------------------------------------------------------ |
| Fprintln | Fprintln 采用默认格式将其参数格式化并写入 w 。总是会在相邻参数的输出之间添加空格并在输出结束后添加换行符。返回写入的字节数和遇到的任何错误。 |

 

**(****四)初始化蛇信息**

1.初始化蛇的长度、方向与位置，并绘制初始形态

  通常，贪吃蛇的开端是地图的中心，长度为2（包括一头一尾），并且头朝右，尾与头在一条水平面上，我们的游戏也是如此。

 ```go
 s.size = 2  // 一头一尾
     s.dir = 'R'  //初始化方向 用UDLR做上下左右
     s.pos[0].X = WIDE/2
     s.pos[0].Y = HIGH/2
     s.pos[1].X = WIDE/2 - 1
 s.pos[1].Y = HIGH/2
 
 for i := 0; i < s.size; i++{
         var ui byte
         if i == 0{
             ui = '@'
         } else{
             ui = '*'
         }
         ShowUI(s.pos[i].X, s.pos[i].Y, ui)
     }
 
 ```

2.设置蛇方向的调整方式

  考虑到玩家在游戏中唯一的操作就是不断改变蛇的移动方向，那么我们就需要建立按键与方向之间的联系。在本模块的设计中需要查找ASCII码与字符间的对应关系。“WSAD”是我们玩游戏时常用的按键，因此我们将它们与方向“上下左右”建立关系，但有时候用户可能会忽视它们的键盘大写设置，或者因外界因素导致某些键位损坏，因此我们也将”wsad”与”udrl”加入了设计。

```go
go func() {
        for {
            switch Clib.Direction() {
            //方向上  W|w|↑
            case 83, 115, 80:
                s.dir = 'U'
                //方向左
            case 65, 97, 75:
                s.dir = 'L'
                //方向右
            case 100, 68, 77:
                s.dir = 'R'
                //方向下
            case 72, 87, 119:
                s.dir = 'D'
                //暂停  空格键
            case 32:
                s.dir = 'P'
            }
        }
    }()  // 特殊的独立函数

```



这是一个特殊的函数，它独立于main，在程序运行过程中始终保持生效状态。

 

**（五）设计食物随机产生器**

   由于贪吃蛇中的食物是随机产生的，我们需要赋予食物的位置一个随机性。但Go的rand包在边界范围内产生的随机种子不是随机的，因此我们需要在main中加入混淆种子来实现随机性。

 ```go
 func RandomFood(){
     food.X = rand.Intn(WIDE)+1  // 直接写起不到随机的效果,这是go的一个缺点
     food.Y = rand.Intn(HIGH)+1
     ShowUI(food.X, food.Y, 's')
 }
 
 // 设置一个随机种子，用作混淆
     rand.Seed(time.Now().UnixNano())
 
 ```

 

注意函数定义在main外，混淆种子定义在main内。

 

**（六）设置游戏逻辑**

  在游戏开始时，我们就可以对贪吃蛇的方向进行调整，我们在之前已经对键位与贪吃蛇移动方向进行了联系，接下来我们来实现它的动态效果：

 ```go
 var nx, ny int = 0, 0
 
 switch s.dir {
         case 'U':
             nx = 0
             ny = 1
         case 'D':
             nx = 0
             ny = -1
         case 'L':
             nx = -1
             ny = 0
         case 'R':
             nx = 1
             ny = 0
         }
 
 ```



这一部分包含在一个永真循环中，每一次循环，蛇的方向可能都不一样，不同的方向会对应蛇头的位移，紧接着链式反应到蛇身。

众所周知，贪吃蛇有两种死亡方式，那就是头撞墙或撞到自己的身体，在这里我们可以用使用位置信息来进行判断：

```go
// 蛇头和墙体碰撞判断
        if s.pos[0].X < 1 || s.pos[0].Y < 1 || s.pos[0].X >= WIDE+1 || s.pos[0].Y >= HIGH+1{
            return
        }
        // 蛇头和身体判断
        for i := 1; i < s.size; i++{
            if s.pos[0].X == s.pos[i].X && s.pos[0].Y == s.pos[i].Y{
                return
            }
        }
        // 蛇跟食物判断
        if s.pos[0].X == food.X && s.pos[0].Y == food.Y{
            // 身体增长
            s.size++
            // 刷新食物位置
            RandomFood()
            // 分数变量
        }

```



在贪吃蛇世界中，有且仅会发生以上三种碰撞。当贪吃蛇吃到食物，它的各项属性会发生相应的变化：

```go
//获取蛇尾坐标,在之后隐去蛇尾
        wx := s.pos[s.size-1].X
        wy := s.pos[s.size-1].Y

        // 从尾部开始更新蛇的身体坐标
        for i := s.size-1; i > 0; i-- {
            s.pos[i].X = s.pos[i-1].X
            s.pos[i].Y = s.pos[i-1].Y
        }

        // 更新蛇头坐标
        s.pos[0].X += nx
        s.pos[0].Y += ny

        //绘制蛇
        for i := 0; i < s.size; i++{
            var ui byte
            if i == 0{
                ui = '@'
            } else{
                ui = '*'
            }
            ShowUI(s.pos[i].X, s.pos[i].Y, ui)
        }
// 重新绘制图形
        ShowUI(wx, wy, ' ')

```



这里要先更新蛇尾再更新蛇头，否则会导致异形蛇的产生。再更新完蛇的位置坐标后，会重新绘制蛇形。

 

**（七）设计主程序**

  待所有的工具模块设计好后，我们就可以设计主程序的流程了。首先，我们初始化地图，然后有了地图，就利用随机种子生成食物，这一点我们在设计食物随机生成器提到过，还需要加入混淆种子来赋予随机性。之后，我们需要隐藏命令行光标，减小对游戏体验的影响，具体的，需要利用Clib包内的HideCursor函数。最后，就是对蛇的设计，首先我们初始化蛇，然后再将蛇置入游戏中。具体代码如下：

```go
func main() {
    // 设置一个随机种子，用作混淆
    rand.Seed(time.Now().UnixNano())
    // 隐藏光标
    Clib.HideCursor()
    // 初始化地图
    MapInit()
    // 生成随机食物
    RandomFood()
    // 绘制食物
    //ShowUI(food.X, food.Y, 's')

    var s Snake
    s.SnakeInit()

    s.PlayGame()
}

```



**三.项目总结**

  本项目已上传至Github，欢迎体验与交流。

地址是：https://github.com/huansong-dev/GluttonousSnake-Go

以上就是本项目的全部后台设计。在未来，我们还可以为游戏加入更多模式，或增加难度，例如增加障碍、不断加快速度等等。

面向业务、面向需求，不断打磨、不断优化是每一个设计者都需要具有的能力。作为一名优秀的coder，需要匠心独运。

 

**四.参考文献**

[1] 【六星教育】G0语言综合实战贪吃蛇项目开发教学[EB/OL].[2021-04-27].https://www.bilibili.com/video/BV1tA411V7aH?p=4

[2]Golang实现贪吃蛇[EB/OL].[2018-08-12]. http://t.csdn.cn/XLYO3

[3] Go语言文档[EB/OL].[2022-02-26]. https://go.dev/doc/

[TOC]


