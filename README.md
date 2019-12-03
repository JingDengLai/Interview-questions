## Promise、async、setTimeout异步执行顺序问题
```
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

const newPromise6 = new Promise(function(resolve, reject) {
  resolve(console.log('promise1'));
});

newPromise6.then(function() {
  console.log('promise2');
}).then(function() {
  console.log('promise3');
});

console.log('script end');在chrome中输出结果？
结果：
	script start
	promise1
	script end
	promise2
	promise3
	setTimeout
```
	解析：
	异步任务之间，会存在差异，所以它们执行的优先级也会有区别。
	大致分为 微任务（microtask，如：Promise、MutaionObserver等）和
	宏任务（macrotask，如：setTimeout、setInterval、I/O等）。

	1、setTimeout 定时器，属于js任务队列里的宏任务。
	指的是把任务队列塞进执行栈里等待执行线程依次执行。虽然可以把它设置微0，但是仍然需要等待本轮执行环境里面的所有同步和异步任执行完毕之后才会去执行它。

	2、promise es6的异步语法，属于js任务队列里的微任务。
	在执行本轮宏任务的过程中，如果遇到promise，会将其塞入本轮执行过程的微任务，待本轮宏任务执行完毕之后，才会执行它。执行完毕之后，开始执行下一轮任务队列。

## 什么是事件循环
详细步骤如下：

　　1、所有同步任务都在主线程上执行，形成一个执行栈

　　2、主线程之外，还存在一个"消息队列"。只要异步操作执行完成，就到消息队列中排队

　　3、一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取消息队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行

　　4、主线程不断重复上面的第三步

【循环】

　　从代码执行顺序的角度来看，程序最开始是按代码顺序执行代码的，遇到同步任务，立刻执行；遇到异步任务，则只是调用异步函数发起异步请求。此时，异步任务开始执行异步操作，执行完成后到消息队列中排队。程序按照代码顺序执行完毕后，查询消息队列中是否有等待的消息。如果有，则按照次序从消息队列中把消息放到执行栈中执行。执行完毕后，再从消息队列中获取消息，再执行，不断重复。

　　由于主线程不断的重复获得消息、执行消息、再取消息、再执行。所以，这种机制被称为事件循环

