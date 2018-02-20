---
title: Canvas 学习笔记
date: 2018-02-20 17:42:29 //指定博客的日期
tags: 
  - 学习
  - 随笔
  - 心情
categories: 
  - Canvas
---

# Canvas概念

## 为什么要学习canvas

+ 游戏：canvas 在基于Web的图像显示方面比 Flash 更加立体、更加精巧，canvas游戏在流畅度和跨平台方面更牛

  + [8款令人惊叹的HTML5 Canvas动画特效](http://www.html5tricks.com/8-html5-canvas-animation.html)
+ **可视化数据**（数据图表化），比如: [百度的ECharts](http://echarts.baidu.com/)、[d3.js](https://d3js.org/)、[three.js](http://threejs.org/) [highcharts]()


## 什么是Canvas
1. canvas 是一个矩形区域的画布，本身不具备绘图功能，作用只是用来展示内容的。
2. 使用JavaScript对应的API可以在画布上绘制内容。
3. canvas的标准：https://www.w3.org/TR/2dcontext/
4. canvas文档：https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial

【使用画图进行演示】


## 课程目标
- 学会使用基本的 canvas API, 使用 canvas 可以完成简单的绘图
- 实现数据的可视化


# canvas入门

## canvas初体验 

1. 准备画布
2. 使用js进行绘画
   + 拿到canvas画布
   + 获取渲染上下文对象（该对象提供了一系列的API集合）
   + 使用API绘制需要的图形

```html
<!-- 准备画布：canvas本身不具备画图功能，仅仅是用来展示内容的 -->
<canvas id="cvs"></canvas>
<script>
  //通过js提供的api，可以在canvas画布上进行绘画。
  //绘画的基本步骤：
  //1. 获取到画布
  var cvs = document.getElementById("cvs");

  //2. 获取到渲染上下文,在画布上可以画2d的和3d的图形
  // 说明我要在画布上画2d的图形， 返回的对象中提供了很多画2d图形的api
  var ctx = cvs.getContext("2d");

  //3. 使用2d上下文进行会话。
  //3.1 移动画笔的位置,到100,100的坐标点
  ctx.moveTo(100, 100);
  //3.2 连线
  ctx.lineTo(200, 100);
  //3.3 渲染（描边或者填充） 
  // 注意：moveTo和lineTo仅仅是绘制了一个路径（打草稿）,还需要把这条线给渲染出来
  ctx.stroke();
</script>
```

## canvas中的坐标系

原点：画布的左上角（0，0），所有元素都是相对与原点定位。

![](imgs/canvas-x-y.png)

## canvas API学习

### getContext方法

功能：getContext方法用于获取canvas的上下文对象

语法：`HTMLCanvasElement.getContext(contextType);`

参数：contextType是一个字符串

+ `"2d"`会返回一个[`CanvasRenderingContext2D`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D)对象，代表2D的上下文，提供了绘制2D图形的api
+ `"webgl"`会返回一个 [`WebGLRenderingContext`](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGLRenderingContext) 对象，代表3D的上下文，提供了绘制3D图形的api
+ 我们只考虑2D图形的绘制

使用demo:

```javascript
//1. 获取到画布
var cvs = document.getElementById("cvs");
//2. 获取到2d上下文对象
var ctx = cvs.getContext("2d");
console.log(ctx);//CanvasRenderingContext2D对象，进行2d图形的绘制
```

### moveTo方法

功能：设置上下文绘制路径的起点。相当于移动画笔到某个位置

语法：`ctx.moveTo(x, y);` 

参数：x=横坐标  y=纵坐标

注意：**绘制线段前必须先设置起点，不然绘制无效。**

### lineTo方法

功能：从x,y的位置绘制一条直线到起点或者上一个线头点。

语法：`ctx.lineTo(x, y);`

参数：x,y 线头点坐标。

### stroke方法

功能：根据路径绘制线。路径只是草稿，真正绘制线必须执行stroke

语法：`ctx.stroke();`

设置描边颜色：`ctx.strokeStyle = "red";`


## canvas标签

### 基本使用

1. canvas标签和img标签类似，但是没有src和alt属性，只有width和height属性。

2. 如果不设置宽度和高度，默认为width:300px和height:150px

```html
<canvas id="cvs" width="150" height="150"></canvas>
```

### 兼容性处理

```html
<canvas width="600" height="400">
    IE9及其以上版本的浏览器，才支持canvas标签
    提示：您的浏览器不支持canvas，请升级浏览器
</canvas>
```

### 设置宽高的注意点

- 1 使用 html属性 `width` 和 `height`来设置，设置的是画布像素点的个数。
- 2 使用CSS样式来设置宽高，不会改变像素点的格式，而是将每隔像素点进行缩放了。

**如果你绘制出来的图像是扭曲的, 尝试用width和height属性为`<canvas>`明确规定宽高，而不是使用CSS。**

【演示canvas宽高的问题】

## canvasAPI练习

绘制正方形，三角形，梯形

<img src="imgs/beautiful.png" width="600" height="400" />

### closePath方法

功能：闭合路径，它尝试从当前点到起始点画一条直线来闭合路径，但如果当前路径已经闭合，此方法不会做任何的操作。

语法：`ctx.closePath();`

注意：closePath方法并不是必须的，但是如果是绘制矩形或者三角形等需要闭合的路径时，可以使用。

### fill方法

解释：填充，是将闭合的路径的内容填充具体的颜色, 默认黑色。

语法：`ctx.fill();` 

注意：如果当前路径没有闭合，使用`fill`方法时会自动调用`closePath`让路径闭合。

设置填充颜色：`ctx.fillStyle = "red";`

思考：

![](imgs/beginPath.png)

### beginPath方法 

功能：开启新路径，清空之前所有的路径

语法：`ctx.beginPath();`

注意：`stroke`和`fill`方法会对当前画布中所有的路径进行绘制，如果不希望是这种效果，需要在适当的地方使用`beginPath`将路径给清空掉。

【练习：冰火两重天】

## 非零环绕原则（了解）

使用fill方法对路径进行填充时，采用的是非零环绕原则（了解即可，通过我们很少会填充一个非常复杂的路径）

1. 对于路径中的任意给定区域，从该区域内部画一条足够长的线段，使此线段的终点完全落在路径范围之外。
2. 将计数器初始化为0，
3. 每当这条线段与路径相交时，如果是路径是顺时针，计数器+1，如果是逆时针，计数器-1。
4. 如果计数器的值最终是0，那么此区域不填充，如果技术的值最终不是0，那么此区域填充。

![非零环绕](imgs/no-zero.png)

【练习：绘制镂空矩形】

![](imgs/no_zero_1.png)

## 综合练习

练习1：绘制坐标网格

练习2：折线图-绘制坐标轴

练习3：折线图-绘制坐标点

练习4：折线图-连线

## 设置canvas绘制属性

+ `fillStyle`:设置填充颜色

```javascript
ctx.fillStyle = "red";
ctx.fillStyle = "rgb(255,0,0)";
ctx.fillStyle = "#f00";
```

+ `strokeStyle`:设置描边颜色

```javascript
ctx.strokeStyle = "red";
ctx.strokeStyle = "rgb(255,0,0)";
ctx.strokeStyle = "#f00";
```

+ `lineWidth`:设置线宽

```javascript
//设置线宽为20
ctx.lineWidth = 20;
```

+ `setLineDash`设置虚线

```javascript
//参数：数组
// 10 --> 实线的长度
// 5  --> 虚线的长度
ctx.setLineDash([10, 5]);
```

+ `lineCap`设置线帽

```javascript
//butt,默认值，没帽子
ctx.lineCap = "butt";
//round：圆帽子
ctx.lineCap = "round";
//square:方帽子
ctx.lineCap = "square";
```

<img src="imgs/line-cap.png" height="303" width="480" >

+ `lineJoin`设置拐角类型

```javascript
//miter: 默认， 尖角
ctx.lineJoin = "miter";
//round: 拐角是圆的
ctx.lineJoin = "round";
//bevel: 斜角
ctx.lineJoin = "bevel";
```

<img src="imgs/line-join.png" height="387" width="453">

## 1px线宽问题

1. canvas绘制线段时，以当前坐标为中心，向两边扩展绘制。
2. 在canvas中绘制内容的最小单位是1px，如果是0.5px，会自动拉伸成1px(颜色会变淡)


![](imgs/lineWidth.png)

实际的2px的效果：

![](imgs/lineWidth2.png)

思考：如何绘制真正的1px的线？？？

坐标应该是.5的地方

# 绘制矩形

## 绘制矩形路径rect

功能：快速绘制一个矩形的路径

语法：`ctx.rect(x, y, w, h);`

+ x:左顶点的x坐标
+ y:左顶点的y坐标
+ w:矩形的宽度
+ h:矩形的高度

注意：rect方法只是规划了矩形的路径，并没有填充和描边, 所以最后还是要调用 fill 或者 stroke 方法绘制

## 绘制描边矩形strokeRect

功能：快速绘制一个描边矩形

语法：`ctx.strokeRect(x, y, w, h);`

注意：此方法会直接stroke绘制一个矩形，不会产生路径。

## 绘制填充矩形fillRect

功能：快速绘制一个填充矩形

语法：`ctx.fillRect(x, y, w, h);`

注意：此方法会直接stroke绘制一个矩形，不会产生路径。

## 清除矩形clearRect-橡皮擦

功能：清除某个矩形内的绘制内容，相当于画图中的橡皮擦

语法：`ctx.clearRect(x, y, w, h);`

清除整个画布：`ctx.clearRect(0, 0, cvs.width, cvs.height);`

## 练习

绘制一个运动的矩形

# 绘制圆弧

> 圆弧：圆上任意两点间的部分叫做圆弧

## arc方法

 功能：创建一个圆弧的路径

语法：`ctx.arc(x, y, r, startRadian, endRadian, counterclockwise)`

+ x：圆心的x坐标
+ y：圆心的y坐标
+ r：圆的半径
+ startRadian：起始弧度
+ endRadian：结束弧度
+ counterclockwise：是否逆时针（默认false，即顺时针）

注意：绘制的仅仅是路径，需要使用stroke或者fill进行绘制。

```javascript
ctx.arc(200, 200, 150, 0, 1.5 * Math.PI, false);
```



<img src="imgs/arc-angle&radian.gif" />

## 弧度与角度

一个圆的角度是360deg， 

360deg = 2π

一弧度 约等于 57.3度

```javascript
//角度/180 = 弧度/π

//角度转弧度
function toRadian(angle) {
  return angle / 180 * Math.PI;
}

//弧度转角度
function toAngle (radian) {
  return radian / Math.PI * 180;
}
```



## 绘制扇形

- 步骤：先moveTo到圆心，再绘制圆弧，最后closePath
- 如果是 fill 填充的扇形图，那么不需要 closePath 就会自动填充

```javascript
//需求：在画布正中心，绘制一个扇形，圆心150，开始角度 90deg, 结束角度180deg, 顺时针
var x = cvs.width/2;
var y = cvs.height/2;
var r = 150;
//1. 将画笔移动到圆心
ctx.moveTo(x, y);
//2. 绘制圆弧(绘制圆弧会自动的从画笔处连一条线到圆弧开始处)
ctx.arc(x, y, r, toRadian(90), toRadian(180));
//3. 填充扇形
ctx.fill();
```

## 练习

1. 在画布中心，动画绘制一个实心圆
2. 根据数据绘制饼图

 ```javascript
 // 后台给的数据
  var data = [
    {
      'percent': 0.1, // 所占比重
      'color': 'orange', // 绘制的颜色
      'title': '社会招生' // 绘制的文字
    }, {
      'percent': 0.1,
      'color': 'pink',
      'title': '公务员'
    }, {
      'percent': 0.1,
      'color': 'gray',
      'title': '公开课'
    }, {
      'percent': 0.1,
      'color': 'green',
      'title': '前端'
    }, {
      'percent': 0.2,
      'color': 'red',
      'title': '应届生'
    }, {
      'percent': 0.3,
      'color': 'blue',
      'title': '程序员'
    }, {
      'percent': 0.1,
      'color': '#abc',
      'title': '老司机'
    }
  ];
 ```

# 绘制文字

## strokeText

功能：绘制描边文字

用法：`ctx.strokeText(text, x, y);`

+ text：文本内容
+ x：文本的x坐标
+ y：文本的y坐标

## fillText

功能：绘制填充文字

用法：`ctx.fillText(text, x, y);`

- text：文本内容
- x：文本的x坐标
- y：文本的y坐标

## 设置文字的样式

+ `font` 设置文本的样式（与css的font属性相同）
+ `textAlign`  设置本内容的水平对齐方式（以参考点为基准）
  + start :    默认。文本在指定的位置开始。
  + end   :    文本在指定的位置结束。
  + center:    文本的中心被放置在指定的位置。
  + left  :    文本左对齐。
  + right :    文本右对齐。

```js
ctx.textAlign = 'left';
```

![对齐图片](imgs/textAlign-demo.png)

+ `textBaseline`      设置绘制文本时使用的当前文本基线
  + alphabetic ：   默认。文本基线是普通的字母基线。
  + top        ：   文本基线是 em 方框的顶端。
  + hanging    ：   文本基线是悬挂基线。
  + middle     ：   文本基线是 em 方框的正中。
  + ideographic：   文本基线是em基线。
  + bottom     ：   文本基线是 em 方框的底端。

```
例如： ctx.textBaseline = 'top';
```
- [textBaseline的介绍](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/textBaseline)

![设置文字为主](imgs/textBaseline-demo.png)
<img src="imgs/textBaseline.gif" height="268" width="300" alt="">

## 练习

绘制带文字的饼图

# 绘制图片

## drawImage的使用

### 用法1 -基本使用
- `context.drawImage(img, dx, dy);`
- 参数：
    + img : 图片dom对象
    + x,y 绘制图片到画布中坐标
- 注意：使用drawImage时必须保证图片已经加载了

### 用法2-设置宽高

- `context.drawImage(img, dx, dy, dWidth, dHeight);`
- width：绘制到canvas中展示的宽度
- 注意：设置width和height时，为了图片不被拉伸，需要按比例设置

```javascript
如果指定宽高，最好成比例，不然图片会被拉伸
等比公式：  height = imgHeight / imgWidth * width;
           设置高 = 原高度 / 原宽度 * 设置宽;
```

### 用法3-图片裁切

- `context.drawImage(img, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);`
- 参数：
  - img：图片对象
  - sx, sy  : 图片中的裁切位置
  - sWidth, sHeight ：图片上的裁切范围
  - dx, dy : 图片在画布中的位置
  - dWidth, dHeight: 图片在画布中的大小。


## 创建img对象

+ `var img = new Image();`
+ `var img = document.createElement("img")`;

```javascript
var img = new Image(); //这个就是 img标签的dom对象
img.src = "imgs/arc.gif";
img.alt = "文本信息";
img.onload = function() {
    //图片加载完成后，执行此方法
}
```

## 练习

绘制序列帧动画

# 变换 

> canvas中所有的变化都是相对于坐标系的变换

## 平移

- ctx.translate(x,y) 
- 参数说明：
  - x：   整个坐标轴位移到 原来水平坐标（x）上的值
  - y：   整个坐标轴位移到 原来垂直坐标（y）上的值
- 发生位移后，相当于把画布的0,0坐标 更换到新的x,y的位置，所有绘制的新元素都被影响。
- 位移画布一般配合缩放和旋转等。

### 缩放

- scale() 方法缩放当前绘图，更大或更小

- 语法：context.scale(scalewidth,scaleheight)

  - scalewidth  :  缩放当前绘图的宽度 (1=100%, 0.5=50%, 2=200%)
  - scaleheight :  缩放当前绘图的高度 (1=100%, 0.5=50%, 2=200%)

- 注意：缩放的是整个画布，缩放后，继续绘制的图形会被放大或缩小。

  ​

### 旋转

- context.rotate(radian); 旋转当前的坐标轴
- 注意参数是弧度（PI）

### 练习

- 在画布左右两侧分别绘制两个圆

- 绘制两个正方形（宽度：100 和 50）

- 绘制旋转的矩形

  ​

## 状态保存与恢复

每次绘制，都会读取绘图上下文对象中设置的属性。我们把上下文对象的这些属性值就可以理解为是状态。

```
Canvas 中引入了状态的保持机制. 
使用 CanvasRenderingContext2D.save() 方法可以保存当前状态. 
如果需要恢复到已经保存的状态, 只需要调用 CanvasRenderingContext2D.restore() 方法即可.

状态保持的机制是基于状态栈实现的. 也就是说 save 一次就存储一个状态. 
restore 一次就将刚刚存入的恢复. 如果 save 两次, 就需要 restore 两次, 才可以恢复到最先的状态.

一般在封装绘图的时候都会采用开始绘制之前, save 一次, 然后 开启一个新路径 beginPath, 
然后绘制结束后 restore, 这样保持当前状态不会对其他绘图代码构成影响.
```

### 绘制环境保存和还原

- ctx.save()        保存当前环境的状态

- ctx.restore()     返回之前保存过的路径状态和属性

  ​