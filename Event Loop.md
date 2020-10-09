# Event Loop

## 浏览器

先看一个问题

```js
console.log("script start");
setTimeout(function () {
  console.log("setTimeout");
}, 0);
new Promise((resolve, reject) => {
  console.log("promise1");
  resolve();
})
  .then(() => {
    console.log("then11");
    new Promise((resolve, reject) => {
      console.log("promise2");
      resolve();
    })
      .then(() => {
        console.log("then2-1");
      })
      .then(() => {
        console.log("then2-2");
      });
  })
  .then(() => {
    console.log("then12");
  });
console.log("script end");
```

## Node

## references

1. [Node.js 事件循环，定时器和 process.nextTick()](https://nodejs.org/zh-cn/docs/guides/event-loop-timers-and-nexttick/)
2. [这一次，Event Loop 一波带走](https://juejin.im/post/6844904106121936903)
3. [Event Loop and the Big Picture — NodeJS Event Loop Part 1](https://blog.insiderattack.net/event-loop-and-the-big-picture-nodejs-event-loop-part-1-1cb67a182810)
4. [Linux 一切皆文件（包含好处和弊端）](http://c.biancheng.net/view/2852.html)
5. [Linux 文件描述符到底是什么？](http://c.biancheng.net/view/3066.html)
6. [程序员应该这样理解 IO](https://www.jianshu.com/p/fa7bdc4f3de7)
7. [select、poll、epoll 之间的区别(搜狗面试)](https://www.cnblogs.com/aspirant/p/9166944.html)
8. [IO 多路复用是什么意思？](https://www.zhihu.com/question/32163005)
9. [高性能 IO 模型分析-IO 模型简介（一）](https://zhuanlan.zhihu.com/p/95550964)
10. [高性能 IO 模型分析-Reactor 模式和 Proactor 模式（二）](https://zhuanlan.zhihu.com/p/95662364)
11. [Does Node.js enforce a minimum delay for setTimeout?](https://stackoverflow.com/questions/7221504/does-node-js-enforce-a-minimum-delay-for-settimeout)
12. [Timers, Immediates and Process.nextTick— NodeJS Event Loop Part 2](https://blog.insiderattack.net/timers-immediates-and-process-nexttick-nodejs-event-loop-part-2-2c53fd511bb3)
13. [Understanding Non-deterministic order of execution of setTimeout vs setImmediate in node.js event-loop](https://codeburst.io/understanding-non-deterministic-order-of-execution-of-settimeout-vs-setimmediate-in-node-js-49e8d5956cab)
14. [Handling IO — NodeJS Event Loop Part 4](https://blog.insiderattack.net/handling-io-nodejs-event-loop-part-4-418062f917d1)
15. [Event Loop Best Practices — NodeJS Event Loop Part 5](https://blog.insiderattack.net/event-loop-best-practices-nodejs-event-loop-part-5-e29b2b50bfe2)
