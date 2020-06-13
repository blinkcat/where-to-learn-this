# konva

v6.0.0

## 什么是 konva

> Konva is an HTML5 Canvas JavaScript framework that extends the 2d context by enabling canvas interactivity for desktop and mobile applications.

> Konva enables high performance animations, transitions, node nesting, layering, filtering, caching, event handling for desktop and mobile applications, and much more.

> You can draw things onto the stage, add event listeners to them, move them, scale them, and rotate them independently from other shapes to support high performance animations, even if your application uses thousands of shapes.

konva 是一个 canvas 框架，在 2d 上下文的基础上让 canvas 具有交互能力，支持桌面端和移动端。  
konva 可以在桌面端和移动端处理高性能的动画，支持节点嵌套，图层，滤镜，缓存，事件等等。  
你可以在画布上监听图形的各种事件，还可以移动，拉伸，旋转图形，且独立于其他图形以支持高性能的动画表现，即使你的应用有上千个图形。

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
7. 鼠标指针样式(mouse cursor) 其实是通过外层 dom 的 css 设置
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

> To fire events with Konva, we can use the fire() method.
> This enables us to programmatically fire events like click, mouseover,
> mousemove, etc., and also fire custom events, like foo and bar.

可以手动触发事件，甚至是触发自定义事件

> There are no build-in keyboards events like keydown or keyup in Konva.

> You can easily add them by two ways:  
> Listen global events on window object  
> Or make stage container focusable with tabIndex property and listen events on it.

勉强支持 keyboard 事件

## Drag and Drop

> To detect drag and drop events with Konva, we can use the on() method to
> bind dragstart, dragmove, or dragend events to a node.

支持拖拽，很方便。组的拖拽也可以轻松实现。另外，Stage 也支持拖拽。

> To restrict the movement of shapes being dragged and dropped with Konva,
> we can use the dragBoundsFunc property which is a user defined function that
> overrides the drag and drop position. This function can be used to constrain
> the drag and drop movement in all kinds of ways, such as constraining the motion
> horizontally, vertically, diagonally, or radially, or even constrain the node
> to stay inside of a box, circle, or any other path.

还可以设置拖拽的范围，水平垂直，甚至更为复杂的区域。

> Konva does not support drop events. But you can write your own drop events detections.
> To detect drop target shape you have to move dragging object into another layer.

但是 drop 事件需要模拟

## Transform

> Transformer is a special kind of Konva.Group. It allows you easily resize and rotate any node or set of nodes.

Transformer 是一个特殊的组

> To resize a node into both sides at the same time you can set centeredScaling to true or hold ALT key while moving an anchor (even if centeredScaling is false).

Transformer 可以控制拉伸方向

> By default when you resize with corner anchors (top-left, top-right, bottom-left or bottom-right) Transformer will save ratio of a node.

> You can set keepRatio to false if you don’t need that behavior.

> Event if you set keepRatio to false you can hold SHIFT to still keep ratio.

在拉伸中保持比例也是可以的

> You can adjust styles of Konva.Transformer for your web app. You can change stroke, size and fill of all anchors.
> Also you can change stroke color and size of border.

Transformer 的样式可以部分自定义

> Konva.Transformer object has special transform events that you can use in your app: transformstart, transform and transformend.

Transformer 自身和 children 都可以监听这些事件。但是 children 包含了 Transformer 周围的方形控制点，所以最方便的方法是先将子图形放入一个 Group 中，再将 Group 放入 Transformer 中。这样比较方便找到 Transformer 中所有的子图形。

> To limit or change resize and transform behavior you can use boundBoxFunc property.
> It works a bit similar to dragBoundFunc.

和 Drag 一样，Transformer 也可以控制变化的范围

> In some application, you may want to snap rotation near some values. Snapping makes a shape “sticky” near provided values and works like rounding.

贴心的提供了 snap 功能

> Konva.Transformer automatically track properties of attached nodes.
> So it will adopt its own properties automatically.

> But is some cases Konva.Transformer can’t do this. Currently Konva.Transformer can not track deep changes inside Konva.Group node. In this case you will need to use forceUpdate method to reset transforming tools

如果 Transformer 中有 Group，那么 Group 中图形变化时，需要调用 Transformer 的 forceUpdate

## 裁剪

> To draw things inside of clipping regions with Konva, we can set the clip
> property of a group or a layer.
> Clipping regions are defined by an x, y, width, and height.

简单的方式，裁剪出一个矩形

> To draw things inside of complex clipping regions with Konva, we can set the clipFunc
> property of a group, a layer.

复杂的方式，可以裁剪出一个复杂的图形

## Group

> To group multiple shapes together with Konva, we can instantiate
> a Konva.Group() object and then add shapes to it with the add() method.
> Grouping shapes together is really handy when we want to transform multiple
> shapes together, e.g. if we want to move, rotate, or scale multiple shapes
> at once. Groups can also be added to other groups to create more complex
> Node trees.

可以将多个图形或是组放入另一个组中

## 层级(Layering)

> To layer shapes with Konva, we can use one of the following layering methods:
> moveToTop(), moveToBottom(), moveUp(), moveDown(), or zIndex().
> You can also layer groups and layers.

类似于 css 中的 z-index

## 切换容器(Move Shape to Another Container)

> To move a shape from one container into another with Konva, we can use the
> moveTo() method which requires a container as a parameter.
> A container can be another stage, a layer, or a group. You can also move groups
> into other groups and layers, or shapes from groups directly into other layers.

将图形从一个图层移动到另外一个

## 滤镜

konva 支持

1. 模糊度(blur)
2. 亮度(brightness)
3. 对比度(contrast)
4. Emboss
5. Enhance
6. Grayscale
7. HSL
8. HSV
9. RGB
10. Invert
11. 万花筒(Kaleidoscope)
12. Mask
13. Noise
14. Pixelate

> Filter is a function that have canvas ImageData as input and it should mutate it.

konva 支持自定义 filter

> To apply multiple filters to an Konva.Image, we have to cache it first with cache()
> function. Then apply filters with filter() function.

还可以设置多个 filter

## Tween

> To tween properties with Konva, we can instantiate a Konva.Tween object
> and then start the tween by calling play(). Any numeric property of a Shape,
> Group, Layer, or Stage can be transitioned, such as x, y, rotation,
> width, height, radius, strokeWidth, opacity, scaleX, offsetX, etc.

缓动功能用起来非常简单，设置好起始状态和终止状态，时间。然后调用 play。

> To create a non linear easing tween with Konva, we can set the easing
> property to an easing function. Other than Konva.Easings.Linear,
> the other most common easings are Konva.Easings.EaseIn,
> Konva.Easings.EaseInOut, and Konva.Easings.EaseOut.

还可以添加缓动函数

> To play, pause, reverse, reset, finish, and seek tweens with Konva,
> we can use the play(), pause(), reverse(), reset(), finish(), and seek() methods.

对缓动有细致的控制

> To tween a filter using Konva, we can simply tween the properties associated with the filter.

还可以对滤镜做缓动

> Also you can use GreenSock Konva Plugin for tweens.

甚至配合 GreenSock 创建复杂的缓动

## Animation

> To create custom animations with Konva, we can use the Konva.Animation
> constructor which takes two arguments, the required update function and
> an optional layer, or array of layers, that will be updated with each animation frame.
> The animation function is passed a frame object which contains a time property which is the number
> of milliseconds that the animation has been running, a timeDiff property which
> is the number of milliseconds that have passed since the last frame,
> and a frameRate property which is the current frame rate in frames per second.

> The update function should never redraw the stage or a layer because the animation
> engine will intelligently handle that for us.

要注意的地方是，传入的 layer 参数，Konva 在做动画时，会自动 redraw。

```js
var anim = new Konva.Animation(function (frame) {
  var time = frame.time,
    timeDiff = frame.timeDiff,
    frameRate = frame.frameRate;

  // update stuff
}, layer);

anim.start();
```

> To stop an animation with Konva, we can use the stop() method.
> To restart the animation, we can again call the start().

可以控制开始和停止

## select

> To select a shape by id with Konva, we can use the find() method using the # selector.
> The find() method always returns an array of elements, even if we are expecting it to return one element.
> if you need only one element you can use findOne() method.
> The find() method works for any node, including the stage, layers, groups, and shapes.

```js
// 可以用 id 来查找图形。要注意的是，id 不是唯一的
var shape = stage.find("#myRect")[0];
```

> To select shapes by type with Konva, we can use the find() method with the name of the type or class name.
> The find() method returns an array of nodes that match the selector string.

```js
// 还可以通过 type，来查找图形
var shapes = stage.find("Rect");
```

> To select shapes by name with Konva, we can use the find() method using the . selector.
> The find() method returns an array of nodes that match the selector string.

```js
// 通过name
var shapes = stage.find(".rectangle");
```

## 序列化

> To save the stage as a JSON string with Konva, we can use the toJSON()
> method which serializes the Konva Node tree into text which can be saved
> in web storage or in an offline database. We can also serialize other nodes,
> including layers, groups, and shapes.

```js
// save stage as a json string
var json = stage.toJSON();
```

> To deserialize a JSON string with Konva, we can use the Konva.Node.create()
> method which creates a node from a JSON string. If we want to deserialize
> a stage node, we can also pass in an optional container parameter.

```js
// create node using json string
var stage = Konva.Node.create(json, "container");
```

> To get the data URL of the stage with Konva, we can use the toDataURL()
> method which requires a callback function for Stage (for other nodes callback is not required).
> In addition, we can also pass in a mime type such as image/jpeg and a quality value that ranges between 0 and 1.
> We can also get the data URLs of specific nodes, including layers, groups, and shapes.

```js
var dataURL = stage.toDataURL();
```

> By default in Konva, exported images have the pixelRatio attribute set to 1. This means that if you export a stage with a size of 500x500, then the exported image will have the same size of 500x500.

```js
stage.toDataURL({
  pixelRatio: 2, // or other value you need
});
```

## performance

1. Shape Caching

> One way to drastically improve drawing performance for complex Konva shapes is to cache them as images.
> This can be achieved by using the cache() method to convert a node into an image object.

2. Layer Management

> When creating Konva applications, the most important thing to consider,
> in regards to performance, is layer management. One of the things that makes
> Konva stand out from other canvas libraries is that it enables us to create
> individual layers, each with their own canvas elements. This means that we can
> animate, transition, or update some stage elements, while not redrawing others.
> If we inspect the DOM of a Konva stage, we’ll see that there is actually one
> canvas element per layer.

将经常变化图形和不常变化的图形放到不同的图层中

> Note: Do not create too many layers. Usually 3-5 is max.

3. batchDraw method

> No matter how many times you call batchDraw(), Konva will automatically
> limit the number of redraws per second based on the maximum number of frames
> per second that the browser can handle at any given point in time.

4. Optimize Animation

> If you animation have frames without any updates (no nodes are changed) you may return false from animation function.

> In this case Konva wouldn’t update layers.

5. Shape Redraw

> But in small set of cases it is possible to update Konva.Node without updating whole layer.
> You can call shape.draw(), BUT remember that in this case shape will be drawn OVER existing canvas.
> So it is not possible to use this tip if your node should be placed under other nodes or if it has an opacity.

6. If your shape has only position transformation (x and y, no scale, rotation) set transformsEnabled = 'position'

7. If you don’t need event on layer set layer.hitGraphEnabled(false). Or use Konva.FastLayer.

8. For mobile application set viewport: \<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no"\>

9. If you have bad performance on retina devices set Konva.pixelRatio = 1. Make sure that quality of result is ok for you.

10. While dragging a node you can move it on separate layer. Then move it back to original layer.

11. Optimize Stroke Drawing

12. If a shape has fill, stroke and opacity you may set shape.perfectDrawEnabled(false).

> If you have a shape with fill and very small stroke you can set shape.hitStrokeWidth(0) to remove stroke from hit graph.
> Don’t use this property if your stroke is critical for hit detection (like non closed lines).

13. Try to set shape.listening(false) where possible.

> You can set listening(false) to shape to remove it from hit graph. It will increase performance.
> In some cases it may be very useful and will not touch whole logic of your application.

14. Avoid Memory Leaks

15. Don’t create large stages. Try to make canvases as small, as possible.

16. Do not create too many layers. Usually 3-5 is max.

17. Hide (or remove from layer) invisible objects (or objects that go out of the screen).

## tricks

1. 获取当前鼠标操作的图形

```js
var pos = stage.getPointerPosition();
var shape = layer.getIntersection(pos);
```

2. 判断两个图形(矩形)是否相交

```js
const overlapping = Konva.Util.haveIntersection(
  shape1.getClientRect(),
  shape2.getClientRect()
);
```

## 参考

1. [Konva](https://konvajs.org/docs/index.html)
2. [Snap to grid with KonvaJS](https://medium.com/@pierrebleroux/snap-to-grid-with-konvajs-c41eae97c13f)

## DIY Demo

1. [konva 曲线 可控制](https://stackblitz.com/edit/js-dooao9)
2. [konva save as image](https://js-ceu59p.stackblitz.io)
