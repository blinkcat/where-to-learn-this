# konva

v6.0.0

## 什么是 konva

> Konva is an HTML5 Canvas JavaScript framework that extends the 2d context by enabling canvas interactivity for desktop and mobile applications.

> Konva enables high performance animations, transitions, node nesting, layering, filtering, caching, event handling for desktop and mobile applications, and much more.

> You can draw things onto the stage, add event listeners to them, move them, scale them, and rotate them independently from other shapes to support high performance animations, even if your application uses thousands of shapes.

konva 是一个 canvas 框架，在 2d 上下文的基础上让 canvas 具有交互能力，支持桌面端和移动端。  
konva 可以在桌面端和移动端处理高性能的动画，节点嵌套，图层，滤镜，缓存，事件等等。  
你可以在画布上监听图形的各种事件，移动，拉伸，旋转图形，且独立于其他图形以支持高性能的动画表现，即使你的应用有上千个图形。

## 如何工作

> Every thing starts from Konva.Stage that contains several user’s layers (Konva.Layer).

> Each layer has two \<canvas\> renderers: a scene renderer and a hit graph renderer.The scene renderer is what you can see, and the hit graph renderer is a special hidden canvas that’s used for high performance event detection.

> Each layer can contain shapes, groups of shapes, or groups of other groups. The stage, layers, groups, and shapes are virtual nodes, similar to DOM nodes in an HTML page.

首先从 Konva.Stage 开始，可以想象成一个画布。画布中包含了多个图层(Konva.Layer)。  
每个图层用两个\<canvas\>来渲染，一个 scene canvas 来渲染你能看到的图形，一个特殊的隐藏 canvas，hit canvas 用来做事件检测。  
每个图层 Layer 可以包含多个图形，多组图形或多个组。  
stage，layers，groups，shapes 都只是虚拟意义上的节点，类似于 html 的 dom 节点。  
下面是节点的层级结构：

```
              Stage
                |
         +------+------+
         |             |
       Layer         Layer
         |             |
   +-----+-----+     Shape
   |           |
 Group       Group
   |           |
   +       +---+---+
   |       |       |
Shape   Group    Shape
           |
           +
           |
         Shape
```

所有的节点都可以添加样式，做变形操作。konva 有一套内置的图层可供使用，如 rectangles, circles, images, sprites, text, lines, polygons, regular polygons, paths, stars 等等。你也可以自定义图形。

## Position vs Offset

> So x and y properties define position of Node on canvas. If you set draggable = true property and start dragging, Konva will change x and y properties of the node. That logic will be applied for all nodes (even Konva.Line).

> Position of a rectangle shape defines its top-left point. Position of circle defines its center.

position(x, y)，就是图形在 canvas 上的坐标。

> When you are changing the offset property it may looks like you are changing position of the node. But actually not. You are changing ORIGIN of the shape.

> What is it origin? You may think of it as “point from where we start drawing of a shape” or “center of the shape” or “the point around which we rotating a shape”.

offset 的概念稍微有点难理解，可以用点的偏移来解释。在绘制图形时，可以分为两种情况。矩形类和圆形类。

1. 矩形类以左上角为起点，开始绘制。旋转时，以左上角为中心点。引入 offset 后，起点和中心点会根据 offset 发生偏移。如 offset={x:5, y:-5}, 起点和中心点会在原来的 x，y 基础上，x-5，y+5。
2. 圆形类绘制时以圆心为起点，旋转时以圆心为中心点。加入 offset 后，和上面的效果一样。

## Tainted canvases may not be exported

> That is a CORS error. For security reasons, a browser can mark a canvas as tainted when you load images from another domain. In that case the browser blocks canvas exporting into dataURL or imageData (that is what we are doing on export or when filters are used).

跨域问题，解决方法

> First you may try to set crossOrigin = Anonymous attribute of the loading image. This approach will work only if requested domain has an Access-Control-Allow-Origin headers that allow shared requests.

## Styling

可以设置

1. 图形的填充色，还可以填充线性渐变(Linear Gradient)，径向渐变(Radial Gradient)，图片
2. 描边(stroke)
3. 透明度
4. 阴影
5. 线条交汇点(line join)
6. show / hide
7. 鼠标指针样式(mouse cursor)
8. 混合模式(blend mode)

## Events

> Konva supports mouseover, mouseout, mouseenter, mouseleave, mousemove, mousedown, mouseup, wheel, click, dblclick, dragstart, dragmove, and dragend desktop events.

> Konva supports touchstart, touchmove, touchend, tap, dbltap, dragstart, dragmove, and dragend mobile events.

> There are two ways to change hit region of the shape: hitFunc and hitStrokeWidth properties.

通过这两个属性来设置 hit region。

> To get the event target with Konva, we can access the target property
> of the Event object. This is particularly useful when using event delegation,
> in which we can bind an event handler to a parent node, and listen to events
> that occur on its children.

事件代理也是支持的。

> There are no build-in keyboards events like keydown or keyup in Konva.

> You can easily add them by two ways:  
> Listen global events on window object  
> Or make stage container focusable with tabIndex property and listen events on it.

勉强支持 keyboard 事件

## 参考

1. [Konva](https://konvajs.org/docs/index.html)
2. [Snap to grid with KonvaJS](https://medium.com/@pierrebleroux/snap-to-grid-with-konvajs-c41eae97c13f)
