# canvas

[教程](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial)

## \<canvas\> 元素

```html
<!-- 只有两个可选属性 width和height -->
<canvas id="tutorial" width="150" height="150"></canvas>
```

> 当没有设置宽度和高度的时候，canvas 会初始化宽度为 300 像素和高度为 150 像素。该元素可以使用 CSS 来定义大小，但在绘制时图像会伸缩以适应它的框架尺寸：如果 CSS 的尺寸与初始画布的比例不一致，它会出现扭曲。

> 当开始时没有为 canvas 规定样式规则，其将会完全透明。

### 替换内容

> 这非常简单：我们只是在\<canvas>标签中提供了替换内容。不支持\<canvas>的浏览器将会忽略容器并在其中渲染后备内容。而支持\<canvas>的浏览器将会忽略在容器中包含的内容，并且只是正常渲染 canvas。

```html
<canvas id="stockGraph" width="150" height="150">
  current stock price: $3.15 +0.15
</canvas>

<canvas id="clock" width="150" height="150">
  <img src="images/clock.png" width="150" height="150" alt="" />
</canvas>
```

### \</canvas> 标签不可省

> 与 \<img> 元素不同，\<canvas> 元素需要结束标签(</canvas>)。如果结束标签不存在，则文档的其余部分会被认为是替代内容，将不会显示出来。

## 渲染上下文（The rendering context）

```js
// 在同一个canvas上以相同的 contextType 多次调用此方法只会返回同一个上下文。
var canvas = document.getElementById("tutorial");
var ctx = canvas.getContext("2d");
```

## 检查支持性

```js
var canvas = document.getElementById("tutorial");

if (canvas.getContext) {
  var ctx = canvas.getContext("2d");
  // drawing code here
} else {
  // canvas-unsupported code here
}
```

## 栅格

![栅格](https://mdn.mozillademos.org/files/224/Canvas_default_grid.png)

## 绘制矩形

> 不同于 SVG，\<canvas> 只支持两种形式的图形绘制：矩形和路径（由一系列点连成的线段）。所有其他类型的图形都是通过一条或者多条路径组合而成的。

```js
// 绘制一个填充的矩形
fillRect(x, y, width, height);
// 绘制一个矩形的边框
strokeRect(x, y, width, height);
// 清除指定矩形区域，让清除部分完全透明
clearRect(x, y, width, height);
```

## 绘制路径

```js
// 新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
beginPath();
// 闭合路径之后图形绘制命令又重新指向到上下文中。
closePath();
// 通过线条来绘制图形轮廓。
stroke();
// 通过填充路径的内容区域生成实心的图形。
fill();
```

> closePath(),不是必需的。这个方法会通过绘制一条从当前点到开始点的直线来闭合图形。如果图形是已经闭合了的，即当前点为开始点，该函数什么也不做。

> 当你调用 fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用 closePath()函数。但是调用 stroke()时不会自动闭合。

### 移动笔触

```js
// 将笔触移动到指定的坐标x以及y上
moveTo(x, y);
```

### 线

```js
// 绘制一条从当前位置到指定x以及y位置的直线
lineTo(x, y);
```

### 圆弧

```js
// 画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。
// 参数anticlockwise为一个布尔值。为true时，是逆时针方向，否则顺时针方向
// arc()函数中表示角的单位是弧度，不是角度。角度与弧度的js表达式:
// 弧度=(Math.PI/180)*角度
arc(x, y, radius, startAngle, endAngle, anticlockwise);
```

```js
// 根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点。
arcTo(x1, y1, x2, y2, radius);
```

### 二次贝塞尔曲线及三次贝塞尔曲线

```js
// 绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点
quadraticCurveTo(cp1x, cp1y, x, y);
```

```js
// 绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。
bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y);
```

### 矩形

```js
// 绘制一个左上角坐标为（x,y），宽高为width以及height的矩形。
rect(x, y, width, height);
```

## Path2D 对象

> Path2D()会返回一个新初始化的 Path2D 对象（可能将某一个路径作为变量——创建一个它的副本，或者将一个包含 SVG path 数据的字符串作为变量）。

```js
new Path2D(); // 空的Path对象
new Path2D(path); // 克隆Path对象
new Path2D(d); // 从SVG建立Path对象
```

> 所有的路径方法比如 moveTo, rect, arc 或 quadraticCurveTo 等，如我们前面见过的，都可以在 Path2D 中使用。

> Path2D API 添加了 addPath 作为将 path 结合起来的方法。当你想要从几个元素中来创建对象时，这将会很实用。比如：

```js
// 添加了一条路径到当前路径（可能添加了一个变换矩阵）。
Path2D.addPath(path [, transform])
```

### Path2D 示例

```js
function draw() {
  var canvas = document.getElementById("canvas");
  if (canvas.getContext) {
    var ctx = canvas.getContext("2d");

    var rectangle = new Path2D();
    rectangle.rect(10, 10, 50, 50);

    var circle = new Path2D();
    circle.moveTo(125, 35);
    circle.arc(100, 35, 25, 0, 2 * Math.PI);

    ctx.stroke(rectangle);
    ctx.fill(circle);
  }
}
```

### 使用 SVG paths

> 新的 Path2D API 有另一个强大的特点，就是使用 SVG path data 来初始化 canvas 上的路径。这将使你获取路径时可以以 SVG 或 canvas 的方式来重用它们。

```js
var p = new Path2D("M10 10 h 80 v 80 h -80 Z");
```

## 色彩 Colors

```js
// 设置图形的填充颜色。
fillStyle = color;
// 设置图形轮廓的颜色。
strokeStyle = color;
```

> 注意: 一旦您设置了 strokeStyle 或者 fillStyle 的值，那么这个新值就会成为新绘制的图形的默认值。如果你要给每个图形上不同的颜色，你需要重新设置 fillStyle 或 strokeStyle 的值。

## 透明度 Transparency

```js
// 这个属性影响到 canvas 里所有图形的透明度，有效的值范围是 0.0 （完全透明）到 1.0（完全不透明），默认是 1.0。
globalAlpha = transparencyValue;
// 指定透明颜色，用于描边和填充样式
ctx.strokeStyle = "rgba(255,0,0,0.5)";
ctx.fillStyle = "rgba(255,0,0,0.5)";
```

## 线型 Line styles

```js
// 设置线条宽度。
lineWidth = value;
// 设置线条末端样式。
lineCap = type;
// 设定线条与线条间接合处的样式。
lineJoin = type;
// 限制当两条线相交时交接处最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度。
miterLimit = value;
// 返回一个包含当前虚线样式，长度为非负偶数的数组。
getLineDash();
// 设置当前虚线样式。
setLineDash(segments);
// 设置虚线样式的起始偏移量。
lineDashOffset = value;
```
