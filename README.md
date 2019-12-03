## Promise、async、setTimeout异步执行顺序问题
(```)
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
(```)