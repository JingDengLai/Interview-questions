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
