# Something about Redux

我自己的理解

## 动机

[原文](https://redux.js.org/introduction/motivation)

[官方翻译](http://cn.redux.js.org/docs/introduction/Motivation.html)

我的翻译

> As the requirements for JavaScript single-page applications have become increasingly complicated, our code must manage more state than ever before. This state can include server responses and cached data, as well as locally created data that has not yet been persisted to the server. UI state is also increasing in complexity, as we need to manage active routes, selected tabs, spinners, pagination controls, and so on.

> 随着 JavaScript 单页应用开发日趋复杂，**JavaScript 需要管理比任何时候都要多的 state （状态）**。 这些 state 可能包括服务器响应、缓存数据、本地生成尚未持久化到服务器的数据，也包括 UI 状态，如激活的路由，被选中的标签，是否显示加载动效或者分页器等等。

随着 js 单页面应用的需求变得越来越复杂，我们必须管理比以前更多的状态(state)。状态可以包含远程服务器的响应，缓存的数据和本地创建的还未提交到服务器的数据。UI 状态也在变得越发的复杂，因为我们要管理当前的路由，选中的 tab，等待标志，分页控制等等。

> Managing this ever-changing state is hard. If a model can update another model, then a view can update a model, which updates another model, and this, in turn, might cause another view to update. At some point, you no longer understand what happens in your app as you have lost control over the when, why, and how of its state. When a system is opaque and non-deterministic, it's hard to reproduce bugs or add new features.

> 管理不断变化的 state 非常困难。如果一个 model 的变化会引起另一个 model 变化，那么当 view 变化时，就可能引起对应 model 以及另一个 model 的变化，依次地，可能会引起另一个 view 的变化。直至你搞不清楚到底发生了什么。**state 在什么时候，由于什么原因，如何变化已然不受控制。**  当系统变得错综复杂的时候，想重现问题或者添加新功能就会变得举步维艰。

管理这些持续变化的状态很难。如果一个模型(model)可以更新另一个模型，接着一个视图(view)可以更新一个模型，这又会导致另一个模型被更新。像这样，继而可能导致另一个视图被更新。在某些时候，你不再理解应用中发生的变化，因为你已经对应用中的状态何时，为何，又如何变化失去了控制。而当一个系统变得让人难以捉摸，无法预测时，就很难重现问题(bug)，添加新的功能。

> As if this weren't bad enough, consider the new requirements becoming common in front-end product development. As developers, we are expected to handle optimistic updates, server-side rendering, fetching data before performing route transitions, and so on. We find ourselves trying to manage a complexity that we have never had to deal with before, and we inevitably ask the question: [is it time to give up?](http://www.quirksmode.org/blog/archives/2015/07/stop_pushing_th.html) The answer is *no*.

> 如果这还不够糟糕，考虑一些**来自前端开发领域的新需求**，如更新调优、服务端渲染、路由跳转前请求数据等等。前端开发者正在经受前所未有的复杂性，[难道就这么放弃了吗？](http://www.quirksmode.org/blog/archives/2015/07/stop_pushing_th.html)当然不是。

好像这还不够糟糕，考虑到新的需求在前端产品中变得越来越普遍。我们开发人员会被期望能够处理乐观更新(optimistic updates)，服务器端渲染，在路由变化动画之前获取数据等等。我们发现我们正在试图管理的复杂度前所未有，不免自问，要放弃吗？答案当然是不。

> This complexity is difficult to handle as we're mixing two concepts that are very hard for the human mind to reason about: mutation and asynchronicity. I call them [Mentos and Coke](https://en.wikipedia.org/wiki/Diet_Coke_and_Mentos_eruption). Both can be great in separation, but together they create a mess. Libraries like [React](http://facebook.github.io/react) attempt to solve this problem in the view layer by removing both asynchrony and direct DOM manipulation. However, managing the state of your data is left up to you. This is where Redux enters.

> 这里的复杂性很大程度上来自于：**我们总是将两个难以理清的概念混淆在一起：变化和异步**。 我称它们为[曼妥思和可乐](https://en.wikipedia.org/wiki/Diet_Coke_and_Mentos_eruption)。如果把二者分开，能做的很好，但混到一起，就变得一团糟。一些库如  [React](http://facebook.github.io/react)  试图在视图层禁止异步和直接操作 DOM 来解决这个问题。美中不足的是，React 依旧把处理 state 中数据的问题留给了你。Redux 就是为了帮你解决这个问题。

这种复杂度之所以难以处理，是因为我们将两个以人类心智来说很难理解的概念，变化和异步，混合在了一起。我把它们称为曼妥思和可乐。分开还好，但是混在一起却能制造混乱。像 React 这样的库尝试在视图层通过去除异步和直接操作 dom 来解决这个问题。但是，管理数据的状态却留给了你自己。这就是 Redux 出现的原因。

> Following in the steps of [Flux](http://facebook.github.io/flux), [CQRS](http://martinfowler.com/bliki/CQRS.html), and [Event Sourcing](http://martinfowler.com/eaaDev/EventSourcing.html), Redux attempts to make state mutations predictable by imposing certain restrictions on how and when updates can happen. These restrictions are reflected in the [three principles](https://redux.js.org/introduction/three-principles) of Redux.

> 跟随  [Flux](http://facebook.github.io/flux)、[CQRS](http://martinfowler.com/bliki/CQRS.html)  和  [Event Sourcing](http://martinfowler.com/eaaDev/EventSourcing.html)  的脚步，通过限制更新发生的时间和方式，**Redux 试图让 state 的变化变得可预测**。这些限制条件反映在 Redux 的[三大原则](http://cn.redux.js.org/docs/introduction/ThreePrinciples.html)中。

紧随 Flux，CQRS 和 Event Sourcing 之后，Redux 引入对更新如何发生和何时发生的限制，来尝试让状态变化可预测。这些限制反应在 Redux 的三个原则中。

## 三大原则

> Single source of truth

> 单一数据源

单一数据源

> The [global state](https://redux.js.org/glossary#state) of your application is stored in an object tree within a single [store](https://redux.js.org/glossary#store).

> **整个应用的  **[state](http://cn.redux.js.org/docs/Glossary.html#state)**  被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个  **[store](http://cn.redux.js.org/docs/Glossary.html#store)**  中。**

在一个 store 中使用一个对象树来保存你应用的全局状态

> State is read-only

> State 是只读的

状态是只读的

> The only way to change the state is to emit an [action](https://redux.js.org/glossary#action), an object describing what happened.

> **唯一改变 state 的方法就是触发  **[action](http://cn.redux.js.org/docs/Glossary.html#action)**，action 是一个用于描述已发生事件的普通对象。**

唯一改变状态的方式就是发出一个用对象描述发生了什么的的动作(action)

> Changes are made with pure functions

> 使用纯函数来执行修改

用纯函数改变状态

> To specify how the state tree is transformed by actions, you write pure [reducers](https://redux.js.org/glossary#reducer).

> **为了描述 action 如何改变 state tree ，你需要编写  **[reducers](http://cn.redux.js.org/docs/Glossary.html#reducer)**。**

你需要写纯的 reducers 来指明状态树如何受到 action 影响而变化

## 什么是 Action

> Actions are payloads of information that send data from your application to your store. They are the only source of information for the store. You send them to the store using store.dispatch().

> Action 是把数据从应用（译者注：这里之所以不叫 view 是因为这些数据有可能是服务器响应，用户输入或其它非 view 的数据 ）传到 store 的有效载荷。它是 store 数据的唯一来源。一般来说你会通过 store.dispatch() 将 action 传到 store。

Actions 是从你的应用到你的 store，用来传输数据的一种载体。它们是传输数据到 store 的唯一方式。需要调用 store.dispatch()来发起传输。

## 什么是 Reducers

> Reducers specify how the application's state changes in response to actions sent to the store. Remember that actions only describe what happened, but don't describe how the application's state changes.

> Reducers 指定了应用状态的变化如何响应 actions 并发送到 store 的，记住 actions 只是描述了有事情发生了这一事实，并没有描述应用如何更新 state。

Reducers 定义了当 store 接收到 Actions 时如何改变应用的状态。记住 Actions 只是描述什么发生了，而不是描述应用的状态如何变化。

> Reducers are just pure functions that take the previous state and an action, and return the next state.

```js
(previousState, action) => nextState;
```

> Reducer 只是一些纯函数，它接收先前的 state 和 action，并返回新的 state。

## 什么是 Store

> we defined the actions that represent the facts about “what happened” and the reducers that update the state according to those actions.  
> The Store is the object that brings them together.

简言之，store 就是把 actions 和 reducers 联系在一起的对象。

> The store has the following responsibilities:
>
> - Holds application state;
> - Allows access to state via getState();
> - Allows state to be updated via dispatch(action);
> - Registers listeners via subscribe(listener);
> - Handles unregistering of listeners via the function returned by subscribe(listener).

## 数据流

## 该不该用 Redux

或者说什么时候才需要考虑使用 Redux。

> You have reasonable amounts of data changing over time
> You need a single source of truth for your state
> You find that keeping all your state in a top-level component is no longer sufficient

- 在 js app 中有大量的数据频繁变动，这些数据可能包含 app 的状态
- 需要一个关于 app 状态的单一权威数据源
- 如果在组件树顶层维护所有的状态，会越来越低效
