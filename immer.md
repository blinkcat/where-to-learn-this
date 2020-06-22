# Immer

> Immer (German for: always) is a tiny package that allows you to work with immutable state in a more convenient way. It is based on the copy-on-write mechanism.

> The basic idea is that you will apply all your changes to a temporary draftState, which is a proxy of the currentState. Once all your mutations are completed, Immer will produce the nextState based on the mutations to the draft state. This means that you can interact with your data by simply modifying it while keeping all the benefits of immutable data.

![immer](https://immerjs.github.io/immer/img/immer.png)

## Benefits

- Immutability with normal JavaScript objects, arrays, Sets and Maps. No new APIs to learn!
- 无需学习新的 API，就能给原有的 js 对象，数组，Set，Map 带来不可变性
- Strongly typed, no string based paths selectors etc.
- [对类型友好](https://immerjs.github.io/immer/docs/typescript)
- Structural sharing out of the box
- 开箱即用的结构共享
- Object freezing [out of the box](https://immerjs.github.io/immer/docs/freezing)
- 默认在开发环境冻结你的 State
- Deep updates are a breeze
- 深层次的更新很容易
- Boilerplate reduction. Less noise, more concise code
- 可以减少原有的模板代码，因此可以精简代码，更容易看出代码的意图
- First class support for [patches](https://immerjs.github.io/immer/docs/patches)
- 支持 [json patch](https://tools.ietf.org/html/rfc6902)
- Small: 3KB gzipped
- 小

## Where to use

任何需要深层次更新的地方

### 用来简化 reducer 的写法

> Note: it might be tempting after using producers for a while, to just place produce in your root reducer and then pass the draft to each reducer and work directly over said draft. Don't do that. It removes the benefit of using Redux as a system where each reducer is testable as a pure function. Immer is best used when applied to small, individual pieces of logic.

```js
// don't do this
const rootReducer = (state = {}, action) =>
  produce(state, (draftState) => {
    // ...
  });
```

Immer 最好用在小块，独立地逻辑中。

### setState

> Deep updates in the state of React components can be greatly simplified as well by using immer.

```js
/**
 * Classic React.setState with a deep merge
 */
onBirthDayClick1 = () => {
  this.setState((prevState) => ({
    user: {
      ...prevState.user,
      age: prevState.user.age + 1,
    },
  }));
};

/**
 * ...But, since setState accepts functions,
 * we can just create a curried producer and further simplify!
 */
onBirthDayClick2 = () => {
  this.setState(
    produce((draft) => {
      draft.user.age += 1;
    })
  );
};
```

## 特殊的数据结构

### Map and Set

produce 之后会产生一个新的 Map 或者 Set，要注意的是

> Maps and Sets that are produced by Immer will be made artificially immutable. This means that they will throw an exception when trying mutative methods like set, clear etc. outside a producer.

> Note: The keys of a map are never drafted! This is done to avoid confusing semantics and keep keys always referentially equal

### Classes

> Plain objects (objects without a prototype), arrays, Maps and Sets are always drafted by Immer. Every other object must use the immerable symbol to mark itself as compatible with Immer. When one of these objects is mutated within a producer, its prototype is preserved between copies.

```js
import { immerable } from "immer";

class Foo {
  [immerable] = true; // Option 1

  constructor() {
    this[immerable] = true; // Option 2
  }
}

Foo[immerable] = true; // Option 3
```

#### Example

```js
import { immerable, produce } from "immer";

class Clock {
  [immerable] = true;

  constructor(hour, minute) {
    this.hour = hour;
    this.minute = minute;
  }

  get time() {
    return `${this.hour}:${minute}`;
  }

  tick() {
    return produce(this, (draft) => {
      draft.minute++;
    });
  }
}

const clock1 = new Clock(12, 10);
const clock2 = clock1.tick();
console.log(clock1.time); // 12:10
console.log(clock2.time); // 12:11
console.log(clock2 instanceof Clock); // true
```

> Immer does not support exotic objects such as DOM Nodes or Buffers.

> So when working for example with Date objects, you should always create a new Date instance instead of mutating an existing Date object.

## Immer performance

> This test takes 50,000 todo items and updates 5,000 of them. Freeze indicates that the state tree has been frozen after producing it. This is a development best practice, as it prevents developers from accidentally modifying the state tree.

> ![performance](https://immerjs.github.io/immer/img/performance.png)

_[Performance tips](https://immerjs.github.io/immer/docs/performance#performance-tips)_

## Pitfalls

[Pitfalls](https://immerjs.github.io/immer/docs/pitfalls)

还是有很多要注意的地方。底层使用了 Proxy，用法上必须按照规定来，否则会造成意想不到的问题。

## How does Immer work?

1. [Copy-on-write](https://en.wikipedia.org/wiki/Copy-on-write)
2. [Proxies](https://developer.mozilla.org/nl/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

![The producer’s source tree and the draft tree shadowing it](https://miro.medium.com/max/1400/1*bZ2J4iIpsm2lMG4ZoXcj3A.png)

绿色的树表示状态树，有蓝色圆环的称为代理。最开始的时候，只有一个代理，就是传给 produce 函数的 draft 对象。当你开始访问（read） 代理中的非原始类型属性值时，这个代理会为那个属性值创建一个代理。最终，得到了一个覆盖了原始状态树的代理树。当然，只有在 produce 函数中被访问过的那些属性才会被覆盖。

![High level overview of the internal decisions in a proxy. Delegating to either the base tree or a cloned node of the base tree.](https://miro.medium.com/max/1088/1*Lyw8BfTTS3AbdLEDtvJvnA.png)

现在，当你改变（write）代理中的属性时，不管是直接还是间接，这个代理会立即创建一个在原始状态树中，与之相关的节点的浅拷贝，并且标志为 "modified"。从此以后，对这个代理的读写不再操作原始节点，而是会代理到这个浅拷贝节点。同样的，没有被改变过的父节点都会被标志为 "modified"。

当 produce 函数执行结束时，遍历这个代理树。如果一个代理被修改过，就返回浅拷贝的节点。否则，返回原始的节点。这个过程会得到一个和原状态树存在结构共享的新状态树。基本上就是这么个原理了。

## 参考

1. [Documentation](https://immerjs.github.io/immer/docs/introduction)
2. [Introducing Immer: Immutability the easy way](https://medium.com/hackernoon/introducing-immer-immutability-the-easy-way-9d73d8f71cb3)
3. [Immer, Immutability and the Wonderful World of Proxies - Michel Weststrate](https://www.youtube.com/watch?v=4Nb9Gwp2L24)
