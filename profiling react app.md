# Profiling React App

## metric

可以分为两个层面，React 框架层面和底层。每个层面以不同粒度进行度量。在框架层面，我们以组件为粒度，这时组件每次渲染花费的时间就是一个很好的性能指标。如果有问题超出了框架的范围，就需要从更底层，更细的粒度进行分析。这时，

- FPS(帧数)
- 占用 cpu 资源
- network
- 占用内存

都是不错的指标。

## React Profiler

### The React Profiler API

通过[Profiler API](https://reactjs.org/docs/profiler.html)，我们可以得到组件渲染所花费的时间。

> React calls this function any time a component within the profiled tree “commits” an update. It receives parameters describing what was rendered and how long it took.

但是这样对源码的侵入性较强，更好的方法是使用 [React Devtools](https://reactjs.org/blog/2019/08/15/new-react-devtools.html) 的 Profiler 面板来查看每个组件的渲染耗时。

![profile](https://addyosmani.com/assets/images/876878.jpg)

通过配置还可以看到组件渲染的具体原因。

### The Interaction Tracing API

通过[Tracing Api](https://gist.github.com/bvaughn/8de925562903afd2e7a12554adcdda16)可以记录某个操作造成的这次渲染，花费了多长时间。

> one common piece of feedback was that the performance information would be more useful if it could be associated with the events that caused the application to render (e.g. button click, XHR response). Tracing these events (or "interactions") would enable more powerful tooling to be built around the timing information, capable of answering questions like "What caused this really slow commit?" or "How long does it typically take for this interaction to update the DOM?".

![tracing](https://addyosmani.com/assets/images/4324242.jpg)

必须配合 devtools 使用。**我自己是没能试出来，不知道是什么原因**

有了 Profile 和 Trace 这两个 api，我们可以得到任何一个组件的渲染耗时和交互耗时，几乎可以满足日常所需。

## Chrome Performance Tab

在 chrome 浏览器中的 performance 面板下，可以进行更全面通用的性能分析。

## references

1. [Profiling React.js Performance](https://addyosmani.com/blog/profiling-react-js/)
2. [149. 精读《React 性能调试》.md](https://github.com/dt-fe/weekly/blob/v2/149.%20%E7%B2%BE%E8%AF%BB%E3%80%8AReact%20%E6%80%A7%E8%83%BD%E8%B0%83%E8%AF%95%E3%80%8B.md)
3. [Profiler API](https://reactjs.org/docs/profiler.html)
4. [Interaction tracing with React](https://gist.github.com/bvaughn/8de925562903afd2e7a12554adcdda16)
5. [How to memoize calculations?](https://reactjs.org/docs/hooks-faq.html#how-to-memoize-calculations)
6. [JavaScript component-level CPU costs](https://calendar.perfplanet.com/2019/javascript-component-level-cpu-costs/)
7. [react devTools interactive tutorial](https://react-devtools-tutorial.now.sh/)
8. [Introducing the React Profiler](https://reactjs.org/blog/2018/09/10/introducing-the-react-profiler.html)
9. [React Production Performance Monitoring](https://kentcdodds.com/blog/react-production-performance-monitoring)
10. [Tracing user interactions with React](https://kentcdodds.com/blog/tracing-user-interactions-with-react)
