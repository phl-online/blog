
# Event Loop

## 进程和线程
* 进程（资源分配的最小单位）
  - 1、 应用程序的执行实例。
  - 2、每一个进程都是由私有的虚拟地址空间、代码、数据、和和其他系统资源组成。
* 线程（程序分配的最小单位）
  - 1、线程是进程内的一个独立执行单元，在不同的线程之间是可以共享进程资源的。


| 场景 | 进程 | 线程 |
| :------:| :------: | :------: |
| | 拥有独立的堆栈空间和数据段 |拥有独立的堆栈空间，但是共享数据段 |
| | 启动新的进程必须分配独立的空间<br/>还要建立众多的数据表维护代码段、堆栈段、数据段 |彼此之前使用相同的地址空间，<br/>共享大部分数据， |


## 浏览器中的Event Loop
### 执行栈与事件队列

- 当js代码执行的时候会将不同的变量存于内存中的不同位置，堆（heap）/栈（stack）中加以区分，
- 堆存放一些对象，栈里存在一些基础的类变量、对象指针，
- 当所有的同步任务都在主线程上执行时，这些任务被排列在一个单独的地方，形成一个执行栈
- js引擎在解析代码段时，会将同步任务顺序加入执行栈依次执行，

- 当遇到异步并行的时候，并不会一直等待，而是将异步任务挂起，--->先执行同步任务--->

### 宏任务（marco task）、微任务（micro task）

- 异步任务又分为宏任务和微任务，执行方式，microtask->marotask
- 宏任务、微任务分类

  * microtask-> process.nextTick（node独有）, Promises, Object.observe(废弃), MutationObserver
  * marotask->  script(整体代码), setTimeout, setInterval, setImmediate（node独有）, I/O, UI rendering



## Node中的Event Loop

- timers，执行定时器队列中的回调，setTimeout()....
- i/o callbacks，这个阶段执行几乎所有的回调，但是不包括close事件，定时器和setImmediate()的回调
- idle，prepare,这个阶段仅在内部使用，
- poll，等待i/o事件，node在一些特殊情况下阻塞在这里
- cheack，setImmediate（）的回调在这阶段执行
- close callbacks，


```js
举个例子（2）
MicroTask队列与MacroTask队列
setTimeout(function () {
   console.log(1);
});
console.log(2);
process.nextTick(() => {
   console.log(3);
});
new Promise(function (resolve, rejected) {
   console.log(4);
   resolve()
}).then(res=>{
   console.log(5);
})
setImmediate(function () {
   console.log(6)
})
console.log('end');

```
2，4，end,3,5,1,6

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/571544ce046e4989a875911c1e9e8080~tplv-k3u1fbpfcp-zoom-1.image)













## 参考文献
[如何解释Event Loop面试官才满意？](https://mp.weixin.qq.com/s?__biz=MzUxMjkwMjU1MQ==&mid=2247483743&idx=1&sn=6e45d22f4a207004942f90739c4aa8ce&chksm=f95c15a7ce2b9cb1ab8ffb1c7a120b886e8c1110bfd48f02deb8d182bfdc34455f28f4703a4a&scene=21#wechat_redirect)