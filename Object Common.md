# Object

## for...in

> for...in 语句以任意顺序遍历一个对象的除 Symbol 以外的可枚举属性。

## in

> 如果指定的属性在指定的对象或其原型链中，则 in 运算符返回 true。

## Object.prototype.hasOwnProperty()

> hasOwnProperty() 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键）。

## for...of

> for...of 语句在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句

### for...of 与 for...in 的区别

> 无论是 for...in 还是 for...of 语句都是迭代一些东西。它们之间的主要区别在于它们的迭代方式。

> for...in 语句以任意顺序迭代对象的可枚举属性。

> for...of 语句遍历可迭代对象定义要迭代的数据。

## Object.keys()

> Object.keys() 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 。

## Object.freeze()

> Object.freeze() 方法可以冻结一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。freeze() 返回和传入的参数相同的对象。

## Object.getOwnPropertyNames()

> Object.getOwnPropertyNames()方法返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括 Symbol 值作为名称的属性）组成的数组。

## Object.getOwnPropertySymbols()

> Object.getOwnPropertySymbols() 方法返回一个给定对象自身的所有 Symbol 属性的数组。

## Object.create()

> Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的\_\_proto\_\_。

```js
/**
 * proto 新创建对象的原型对象。
 * propertiesObject
 * 可选。如果没有指定为 undefined，则是要添加到新创建对象的不可枚举（默认）属性（即其自身定义的属性，而不是其原型链上的枚举属性）对象的属性描述符以及相应的属性名称。这些属性对应Object.defineProperties()的第二个参数。
 *
 * returns 一个新对象，带着指定的原型对象和属性。
*/
Object.create(proto[, propertiesObject])
```

## Object.assign()

> Object.assign() 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。

> Object.assign 方法只会拷贝源对象自身的并且可枚举的属性到目标对象。该方法使用源对象的[[Get]]和目标对象的[[Set]]，所以它会调用相关 getter 和 setter。因此，它分配属性，而不仅仅是复制或定义新的属性。如果合并源包含 getter，这可能使其不适合将新属性合并到原型中。为了将属性定义（包括其可枚举性）复制到原型，应使用 Object.getOwnPropertyDescriptor()和 Object.defineProperty() 。

> String 类型和 Symbol 类型的属性都会被拷贝。

> 在出现错误的情况下，例如，如果属性不可写，会引发 TypeError，如果在引发错误之前添加了任何属性，则可以更改 target 对象。

## Object.getOwnPropertyDescriptor()

> Object.getOwnPropertyDescriptor() 方法返回指定对象上一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）
