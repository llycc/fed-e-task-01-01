# fed-e-task-01-01

## 一

### 1 说出执行结果，为什么
 ```javascript
  var a = [];
  for(var i = 0; i < 10; i++) {
    a[i] = function() {
      console.log(i);
    };
  }
  a[6]();
```

执行结果：控制输出 10
原因：这里for循环中var所声明的i变量的作用域是全局，而由于a数组中函数对i的引用，会使得for循环结束后i也不会被回收，
当执行a[6]()时，i已经在循环退出时累加到了10，所以执行a[6]()会打印10。

### 2 说出执行结果 为什么
```javascript
  var tmp = 123;
  if(true) {
    console.log(tmp);
    let tmp;
  }
```
执行结果： 抛出异常
原因： 因为暂时性死区的存在，if内的tmp是使用let声明的，所以if的tmp会绑定上if的块级作用域，从而忽略var声明的全局变量tmp，
而let声明的变量又不存在变量提升，所以导致执行console.log(tmp)时报错。

### 3 用最简单的方法找出数组中的最小值
```javascript
 var arr = [12, 34, 32, 89, 4];
```
答案：
```javascript
  const minValue = Math.min(...arr);
  console.log(minValue);
```

### 4 详细说明var,let,const 三种声明变量的方式之间的具体差别？

答案：
1. var声明的变量作用域是最接近的外层作用域，没有块级作用域的概念，比如：在函数使用var声明变量，其作用域就是整个函数。
let和const声明的变量的作用域是最近的代码块，比如在if或是for代码块内使用let或const声明变量，其作用域就是if或是for代码块。
2. var声明的变量存在变量提升。而let和const声明的变量不存在变量提升，使用变量前必须先声明。
3. var允许重复声明变量，let和const则不允许重复声明变量。
4. let声明的变量可以不用初始化且可以重复赋值。而const声明的变量必须要初始化，且初始化之后不能改变变量的值。

### 5 说出代码执行结果，为什么
```javascript
  var a = 10;
  var obj = {
    a: 20,
    fn() {
      setTimeout(() => {
        console.log(this.a);
      });
    }
  };
  obj.fn();
```
 执行结果： 控制台输出：20
 原因：首先函数内的this对象，是函数定义时所在的对象，而不是使用时所在的对象。setTimeout的回调函数定义生效是
 在obj.fn执行时，所以obj.fn函数内setTimeout函数的回调函数内的this对象实际上就是指向obj对象，所以当
 setTimeout的回调被执行时this.a就是obj.a 即20.
 
 ### 6 简述Symbol类型的用途
 答案：Symbol生成的值都是独一无二的，可以作为对象的属性名，就能防止对象的属性被重写。
 
### 7 什么时浅拷贝，什么是深拷贝

答案：
* 浅拷贝： 是对内存地址的复制，让目标对象指针和源对象指向同一片内存空间。
* 深拷贝： 拷贝对象的具体内容，二者的内存是分开的，源对象和拷贝生成的对象的值一样，但内存地址不一样，二者互不影响。

### 8 js异步编程，EventLoop是什么，什么是宏任务，什么是微任务？
答案： 
* 异步编程：js是单线程执行的，也是事件驱动的，异步编程是在执行过程中不需要等待某些任务的完成，只需要注册这些任务的回调函数，
等到任务完成时，js引擎来执行注册的回调函数，这样可以避免任务耗时过长，阻塞线程。
* EventLoop: js中 所有同步任务都在主线程上执行，也可以理解为存在一个执行栈，主线程外，还有一个任务队列，
任务队列的作用，就在等待异步任务的结果，只要异步任务有了运行结果，就会加入到“任务队列”中，
一旦执行栈中所有同步任务执行完毕，就从任务队列中读取任务加入到执行栈中，主线程不断的循环上面的过程，这就是EventLoop。
* 宏任务： 每次执行栈执行的代码就是一个宏任务，宏任务包括： script(整体代码)、setTimeout、setInterval、I/O、
交互事件、postMessage、MessageChannel、setImmediate(Node.js 环境)
* 微任务： 当前任务执行结束后立即执行的任务，所以微任务的优先级比宏任务高。微任务包括：Promise.then、MutaionObserver、process.nextTick(Node.js 环境)

### 9 将下面的异步代码使用Promise改进
```javascript
  setTimeout(function() {
    var a = 'hello';
    setTimeout(function() {
      var b = 'lagou';
      setTimeout(function() {
        var c = 'I * U';
        console.log(a + b + c);
      }, 10);
    }, 10);
  }, 10);
```
 答案： 
 ```javascript
  function setTimeoutPromise(result, timeoutMs) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(result)
      }, timeoutMs);
    });
  }
  setTimeoutPromise(['hello'], 10).then((values) => {
    return setTimeoutPromise([...values, 'lagou'], 10);
  }).then((values) => {
    return setTimeoutPromise([...values, 'I * U'], 10);
  }).then((values) => {
    console.log(values.join(' '));
  });
```

### 10 请简述typescript和javascript之间的关系

答案： typescript是javascript的超集，所以typescript完全兼容javascript，typescript是静态类型语言，弥补了javascript
的不足。

### 11 typescript 的优缺点

答案： 
* 优点： 增强代码可读性和可维护行。大量错误可以在编译阶段被检查出。开发时可以使用开发工具的提示功能。扩展了javascript的功能和特性。
* 缺点： 增加项目团队学习成本，不如javascript灵活，开发时限制较多，降低开发速度。
