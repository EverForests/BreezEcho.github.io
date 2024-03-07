# Css


  <!--more-->

[文档手册](https://www.acwing.com/file_system/file/content/whole/index/content/4194723/)

## 1 定义方式、选择器、颜色

- 样式
  - 行内样式 <img src="/images/mountain.jpg" alt="" style="width: 300px; height: 200px;">
  - 内部样式 写在 html-head 下的 style 标签下
  - 外部样式 调用外部某个 css 样式 <link rel="stylesheet" href="/static/css/word.css" type="css">
- 选择器
  - 标签选择器 img {}
  - ID 选择器 #username {}
  - 类选择器 .img-field {}
  - 伪类选择器
    - 链接伪类：在上述选择器后添加 :xxx 如 .hover-div:hover
    - 位置伪类：:nth-child(n)选择是其父标签第 n 个子元素的所有元素。
    - 目标伪类：:target：当 url 指向该元素时生效。
  - 复合选择器
    - ![image-20240308002741908](https://raw.githubusercontent.com/BreezEcho/PicGo/master/image-20240308002741908.png)
  - 通配符选择器
    - ![image-20240308002752672](https://raw.githubusercontent.com/BreezEcho/PicGo/master/image-20240308002752672.png)
  - 伪元素选择器
    - ![image-20240308002824607](https://raw.githubusercontent.com/BreezEcho/PicGo/master/image-20240308002824607.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

	/* 内部样式表 */
    <style>
        div {  
            color: white;
            height: 300px;
            width: 300px;
        }

        div:nth-child(2n+1) {
            background-color: #0000FF;
        }

        div:nth-child(2n) {
            background-color: rgba(255, 0, 0, 0.7);
        }

        .small-div {
            width: 200px;
            height: 200px;
        }

        #big-div {
            width: 400px;
            height: 400px;
        }

        .hover-div:hover {
            background-color: orange;
        }
    </style>
</head>

<body>
    <div class="small-div">1</div>
    <div id="big-div">2</div>
    <div>3</div>
    <div class="small-div">4</div>
    <div class="small-div hover-div">5</div>
    <div>6</div>
    <div>7</div>
    <div class="hover-div">8</div>
    <div>9</div>
    <div>10</div>
</body>

</html>
```

## 2 文本、字体

- 文本调节
  - 文本对齐：text-align (center.eg)
  - 调多行元素空间：line-height (em.eg)
  - 调文本间距：letter-spacing (px)
  - 调首行缩进：text-indent (em.eg)

> ![image-20240308002937006](https://raw.githubusercontent.com/BreezEcho/PicGo/master/image-20240308002937006.png)

- 字体形态调整（文本微调）
  - 调大小：font-size
  - 调粗细：font-weight
  - 换字体家族：font-family
- 字体特效
  - 基本的：font-style (italic or oblique)
  - 综合修饰：text-decoration
  - 加阴影：text-shadow (x y r color)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="/static/css/word.css" type="css">
    <style>
        h2 {
            text-align: center;
        }

        p {
            line-height: 1.5em;
            letter-spacing: 1px;  /*字间距*/
            text-indent: 2em;  /*缩进*/
            /* 2rem? */
        }

        .beautiful-p {
            text-decoration: underline wavy green;
        }

        .shadow-p {
            text-shadow: 5px 5px 5px grey;
        }

        .bold-p {
            font-weight: 800;
        }

        .italic-p {
            font-style: italic;
            /* italic是斜体表示 */
        }

        .fantasy-p {
            font-family: monospace;
        }

        p::first-letter {
            /* 注意一下如何定位块内元素 */
            font-size: 1.2em;
            color: red;
        }
    </style>
</head>

<body>
    <h2>春</h2>
    <p>盼望着，盼望着，东风来了，春天的脚步近了。</p>
    <p class="shadow-p">一切都像刚睡醒的样子，欣欣然张开了眼。山朗润起来了，水涨起来了，太阳的脸红起来了。</p>
    <p class="fantasy-p">小草偷偷地从土里钻出来，嫩嫩的，绿绿的。园子里，田野里，瞧去，一大片一大片满是的。坐着，躺着，打两个滚，踢几脚球，赛几趟跑，捉几回迷藏。风轻悄悄的，草软绵绵的。</p>
    <p>桃树、杏树、梨树，你不让我，我不让你，都开满了花赶趟儿。红的像火，粉的像霞，白的像雪。花里带着甜味儿；闭了眼，树上仿佛已经满是桃儿、杏儿、梨儿。花下成千成百的蜜蜂嗡嗡地闹着，大小的蝴蝶飞来飞去。野花遍地是：杂样儿，有名字的，没名字的，散在草丛里，像眼睛，像星星，还眨呀眨的。
    </p>
    <p class="beautiful-p">
        “吹面不寒杨柳风”，不错的，像母亲的手抚摸着你。风里带来些新翻的泥土的气息，混着青草味儿，还有各种花的香，都在微微润湿的空气里酝酿。鸟儿将窠巢安在繁花嫩叶当中，高兴起来了，呼朋引伴地卖弄清脆的喉咙，唱出宛转的曲子，与轻风流水应和着。牛背上牧童的短笛，这时候也成天在嘹亮地响。
    </p>
    <p class="bold-p italic-p">
        雨是最寻常的，一下就是三两天。可别恼。看，像牛毛，像花针，像细丝，密密地斜织着，人家屋顶上全笼着一层薄烟。树叶子却绿得发亮，小草也青得逼你的眼。傍晚时候，上灯了，一点点黄晕的光，烘托出一片安静而和平的夜。乡下去，小路上，石桥边，有撑起伞慢慢走着的人；还有地里工作的农夫，披着蓑，戴着笠的。他们的草屋，稀稀疏疏的，在雨里静默着。
    </p>
    <p>天上风筝渐渐多了，地上孩子也多了。城里乡下，家家户户，老老小小，他们也赶趟儿似的，一个个都出来了。舒活舒活筋骨，抖擞抖擞精神，各做各的一份事去。“一年之计在于春”，刚起头儿，有的是工夫，有的是希望。</p>
    <p class="beautiful-p">春天像刚落地的娃娃，从头到脚都是新的，他生长着。</p>
    <p>春天像小姑娘，花枝招展的，笑着，走着。</p>
    <p class="shadow-p">春天像健壮的青年，有铁一般的胳膊和腰脚，他领着我们上前去。</p>
</body>

</html>
```

## 3 背景

设置背景大小与背景：

调元素背景颜色：background-color (color)

调背景图像：background-image

调背景图大小：background-size xy 拉伸或等比例收缩

背景图重复覆盖：background-repeat

设置背景图初始位置：background-position

页面滚动时是否与视窗保持相对静止：background-attachment

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div {
            width: 300px;
            height: 300px;
            margin: 10px;
        }

        div:nth-child(1) {
            background-color: lightblue;
        }

        div:nth-child(2) {
            background-image: url('/static/images/mountain.jpg');
            background-size: 100% 100%;
            background-repeat: no-repeat;
        }

        div:nth-child(3) {
            background-image: url('/static/images/mountain.jpg'), url('/static/images/logo.png');
            background-size: 50% 100%, 50% 100%;
            background-repeat: no-repeat, no-repeat;
            background-position: top left, 150px 0;
        }

        div:nth-child(4) {
            background-image: url('/static/images/mountain.jpg');
            background-size: 100% 100%;
            background-repeat: no-repeat;
            background-attachment: fixed;
        }
    </style>
</head>

<body>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
</body>

</html>
```

## 4 边框

设计边框样式：border-style

设置边框宽度：border-width

设置边框四边颜色：border-color

设置外边框圆角：border-radius

设置表格边框分开还是合并：border-collapse

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div {
            width: 300px;
            height: 300px;
        }

        div:nth-child(1) {
            border-width: 10px;
            border-style: solid dotted inset dashed;
            border-color: blue red;
            /*自动按照顺序对上右下左四条边进行渲染，无需写4次*/
            border-radius: 50%;
        }

        div:nth-child(2) {
            border-style: solid 3px black;
            border-radius: 10px;
        }
    </style>
</head>

<body>
    <div></div>
    <div></div>
</body>

</html>
```

## 5 元素展示方式与内边距、外边距

展示方式

主要的：

display（block、inline、inline-block）不同展示方式主要的差别在于元素是否能独占一行，对 width、height、margin、padding 等的控制权限。

> div 属于 block 类型、span 属于 inline 类型、img 属于 inline-block 类型

辅助工具：

填充元素空白：white-space

设置溢出内容的展示方式：text-overflow

设置超量溢出的格式化方式：overflow



外边距 margin

![image-20240308003005953](https://raw.githubusercontent.com/BreezEcho/PicGo/master/image-20240308003005953.png)

内边距 padding

![image-20240308003014777](https://raw.githubusercontent.com/BreezEcho/PicGo/master/image-20240308003014777.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .container {
            display: block;
            width: 500px;
            height: 500px;
            padding: 10px;
            margin: 20px;
            background-color: lightblue;
        }

        .inline-block-div {
            display: inline-block;
            width: 100px;
            height: 100px;
            padding: 10px;
            margin: 10px;
            background-color: blue;
            color: white;
        }

        span {
            display: inline;
            padding-left: 10px;
            padding-right: 10px;
            margin-left: 20px;
            margin-right: 20px;
            background-color: bisque;
            color: green;
        }

        .block-div {
            display: block;
            width: 100px;
            height: 100px;
            padding: 10px;
            margin: 10px;
            background-color: blue;
            color: white;
            overflow: hidden;
            text-overflow: ellipsis;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="inline-block-div">div-1</div>
        <div class="inline-block-div">div-2</div>
        <div class="inline-block-div">div-3</div>
        <br>
        <span>span-1</span>
        <span>span-2</span>
        <span>span-3</span>
        <br>
        <div class="block-div">
            123456789101112
        </div>
    </div>
</body>

</html>
```

## 6 控制标签块的位置

CSS position 属性用于指定一个元素在文档中的定位方式。top，right，bottom 和 left 属性则决定了该元素的最终位置。

position 的值主要包含：static、relative、absolute、fixed 和 sticky

static：该关键字指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时 top, right, bottom, left 和 z-index 属性无效。

relative：该关键字下，元素先放置在未添加定位时的位置，再在不改变页面布局的前提下调整元素位置（因此会在此元素未添加定位时所在位置留下空白）。

absolute：元素会被移出正常文档流，并不为元素预留空间，通过指定元素相对于最近的非 static 定位祖先元素的偏移，来确定元素位置。

fixed：元素会被移出正常文档流，并不为元素预留空间，而是通过指定元素相对于屏幕视口（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变，相当于与最近滚动祖先保持静止。

sticky：元素根据正常文档流进行定位，然后相对它的最近滚动祖先，包括 table-related 元素，基于 top、right、bottom 和 left 的值进行偏移。偏移值不会影响任何其他元素的位置。 该值总是创建一个新的层叠上下文（stacking context）。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div {
            width: 100px;
            height: 100px;
            background-color: lightsalmon;
        }

        div:nth-child(1) {
            margin: 10px 20px 30px 40px;
            padding: 10px 20px 30px 40px;
        }

        div:nth-child(2) {
            /*注意如何让标签块在页面内居中*/
            margin: 0 auto;
            position: absolute;
            left: 50%;
            top: 50
%;
            transform: translate(-50%, -50%);  /*相对自身偏移*/
        }
    </style>
</head>

<body>
    <div>文本1</div>
    <div>文本2</div>
</body>

</html>
```

## 7 盒子模型

盒子模型主要通过 box-sizing 属性进行定义，它包含两个值：content-box 和 border-box，前者是默认值，即对于任何一个 html body 内部的标签，都初始被理解为盒子模型。二者的主要差别在于对它们的 width 和 height 的定义。

![image-20240308003034798](https://raw.githubusercontent.com/BreezEcho/PicGo/master/image-20240308003034798.png)

content-box：width 和 height 都是对内容部分的设置。

border-box：width 和 height 不仅包括内容部分，还有内外边距及边框厚度。

> 一个很好的例子
>
> content-box
>  注意：内边距、边框和外边距都在这个盒子的外部比如说，.box {width: 350px; border: 10px solid black;}在浏览器中的渲染的实际宽度将是 370px。
>  尺寸计算公式：
>  width= 内容的宽度
>  height= 内容的高度
>
> border-box
>  Note that padding and border will be inside of the box. For example,.box {width: 350px; border: 10px solid black;} renders a box that is 350px wide, with the area for content being 330px wide.
>  width = border+ padding + 内容的宽度
>  height =border+padding + 内容的高度

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div {
            width: 300px;
            height: 300px;
            margin: 10px;
            padding: 10px;
            background-color: lightblue;
            border: solid 10px gray;
            /*注意为什么不能写成border-style，border包含三个属性而其一就是border-style*/
        }

        div:nth-child(1) {
            box-sizing: content-box;
        }

        div:nth-child(2) {
            box-sizing: border-box;
        }
    </style>
</head>

<body>
    <div>1</div>
    <div>2</div>
</body>

</html>
```

## 8 位置

[MDN position](https://developer.mozilla.org/en-US/docs/Web/CSS/position)

![image-20240308003049188](https://raw.githubusercontent.com/BreezEcho/PicGo/master/image-20240308003049188.png)

> 还有 fixed，它和 absolute 相当，都是浮动型位置。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            height: 2000px;
            margin: 0;
        }

        div {
            width: 200px;
            height: 200px;
            background-color: lightblue;
            margin: 20px;
        }

        div:nth-child(1) {
            position: static;
        }

        div:nth-child(2) {
            position: relative;
            left: 50px;
            top: 15px;
        }

        div:nth-child(3) {
            position: absolute;
            background-color: rgba(0, 0, 255, 0.5);
            left: 30px;
            top: 30px;
        }

        div:nth-child(4) {
            position: fixed;
            background-color: rgba(0, 255, 0, 0.5);
            left: 100px;
            top: 250px;
        }

        div:nth-child(10) {
            position: sticky;
            background-color: rgba(255, 0, 0, 0.5);
            bottom: 10px;
        }
    </style>
</head>

<body>
    <div>1</div>
    <div>2</div>
    <div>3</div>
    <div>4</div>
    <div>5</div>
    <div>6</div>
    <div>7</div>
    <div>8</div>
    <div>9</div>
    <div>10</div>
</body>

</html>
```

## 9 浮动

float 会强制将元素变为块布局，也就是说，对于 display 为 inline-block 或 inline 的情况，都会变成 block。

float CSS 属性指定一个元素应沿其容器的左侧或右侧放置，允许文本和内联元素环绕它。该元素从网页的正常流动(文档流)中移除，尽管仍然保持部分的流动性（与绝对定位相反）。与之前所学的 position: fixed 相似，设置 float 属性的数值也可以使标签元素浮动在页面上层。

同时，也可以使用 clear 属性消除之前所设置的其它 float。

![image](https://raw.githubusercontent.com/BreezEcho/PicGo/master/image-20231013221604-aiku7gm-20240308002700523.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

    <style>
        .container {
            width: 500px;
            height: 500px;
            background-color: lightblue;
        }

        .item {
            width: 100px;
            height: 100px;
            margin: 10px;
            background-color: lightcoral;
            float: left;
        }

        .next-line {
            width: 150px;
            height: 150px;
            background-color: rgba(0, 0, 255, 0.5);
            clear: left;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="next-line"></div>
    </div>
</body>

</html>
```

## 10 flex，响应式布局

### 10.1 flex

flex 模型，是一种 display，它包含 flex 容器与 flex 项目（两种标签类）。

对于容器，可以调整它内部项目的排列方式与对齐方式等。

- flex-flow 整体简写，包含下述属性
  - 确定主轴：flex-direction
  - 改变 flex 元素显示方式（单行/多行）：flex-wrap

- 调整容器中各主轴向元素的排列/对齐方式：justify-content
- 直接调整容器中主轴向空间在交叉轴（与主轴垂直）的排列方式：align-content
- 保持主轴向空间相对位置不变的情况下，调整主轴向空间的位置：align-items

> 注意如果值为 stretch，那么 flex 项目会动态伸缩填满容器。

对于项目，可以设置当我们对页面进行伸缩时标签块如何变化。

- flex 当 flex 容器大小发生变化时
  - 设置 flex 项目主轴向尺寸的增长系数：flex-grow
  - 设置 flex 项目主轴向尺寸的收缩系数：flex-shrink
  - 设置 flex 项目在主轴方向上的初始大小：flex-basis

### 10.2 响应式布局

响应式布局指的是让页面在不同端获得自适应效果，使所有功能协调、美观。通常使用判断语句 @media (minwidth: xxx) {} 进行判断，对不同分类下的标签进行管理。通常 css 代码比较繁琐，如果是做 web 网页可以使用 bootstrap 框架进行配置，如果是做 web 应用则通常需要自行编写以适配后端逻辑。

### 10.3 应用

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .container {
            display: flex;
            width: 50%;
            height: 500px;
            background-color: lightblue;
            margin: 10px;
        }

        .container>div {
            width: 120px;
            height: 120px;
        }

        .container>div:nth-child(odd) {
            background-color: lightsalmon;
        }

        .container>div:nth-child(even) {
            background-color: lightgreen;
        }

        .container:nth-child(1) {
            flex-wrap: wrap;
            justify-content: space-between;
            align-content: flex-start;
        }

        .container:nth-child(2) {
            flex-direction: row-reverse;
            flex-wrap: nowrap;
        }

        .container:nth-child(2)>div:nth-child(odd) {
            flex-grow: 3;
            flex-shrink: 3;
        }

        .container:nth-child(2)>div:nth-child(even) {
            flex-grow: 1;
            flex-shrink: 1;
        }
    </style>
</head>

<body>
    <div class="container">
        <div>1</div>
        <div>2</div>
        <div>3</div>
        <div>4</div>
        <div>5</div>
        <div>6</div>
    </div>

    <div class="container">
        <div>1</div>
        <div>2</div>
        <div>3</div>
        <div>4</div>
        <div>5</div>
        <div>6</div>
    </div>
</body>

</html>
```


