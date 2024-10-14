

- Axios
- Fetch
- Promise



Promise是抽象异步处理对象以及对其进行各种操作的组件。

JS异步处理的方式：

- 回调函数，Node.js等则规定在JavaScript的回调函数的第一个参数为 Error 对象

  ```js
  getAsync("fileName.md", function(error, result) {
    if(error) {
      throw error;
    }
  });
  ```

  像上面这样基于回调函数的异步处理如果统一参数使用规则的话，写法也会很明了。但是，这也仅是编码规约而已，即使采用不同的写法也不会出错。而Promise则是把类似的异步处理对象和处理规则进行规范化， 并按照采用统一的接口 来编写，而采取规定方法之外的写法都会出错。

  ```js
  let p = getAsyncPromise("fileName.md");
  p.then(function(result){}).catch(function(error){});
  ```

  这和回调函数方式相比有哪些不同之处呢? 在使用promise进行一步处理的时候，我们 必须按照接口规定的方法编写处理代码。

  也就是说，除promise对象规定的方法(这里的 then 或 catch )以外的方法都是不可以使 用的， 而不会像回调函数方式那样可以自己自由的定义回调函数的参数，而必须严格 遵守固定、统一的编程方式来编写代码。

  这样，基于Promise的统一接口的做法， 就可以形成基于接口的各种各样的异步处理模 式。

  所以，promise的功能是可以将复杂的异步处理轻松地进行模式化， 这也可以说得上是 使用promise的理由之一。

Promise的标准API

- 构造器

```js
let p = new Promise(function(resolve, reject){

});
```

- 实例方法

对通过new生成的promise对象为了设置其值在 resolve(成功) / reject(失败)时调用的回调，函数 可以使用 promise.then() 实例方法。

```js
p.then(onFulfilled, onRejected); // 参数都是可选的
```

resolve时，onFulfilled会被调用；reject时，onRejected会被调用。

```js
p.catch(onRejected); // p.then(null, onRejected);
```

- 静态方法

```js
Promise.all()
Promise.resolve()
```



new Promise实例化的Promise对象有三个状态

- “has resolution”, Fulfilled，resolve（成功时，会调用onFulfilled
- “has rejection”，Rejected，reject（失败时，会调用onRejected
- “unresolved”，Pending，Promise对象刚被创建后的初始化状态

Pending ->Value -> Fulfilled

Pending -> Error -> Rejected

都是在内部定义的状态。 由于没有公 开的访问[[PromiseStatus]]的用户API，所以暂时还没有查询其内部 状态的方法。

promise对象的状态，从Pending转换为Fulfilled或Rejected之后， 这个promise对象的状 态就不会再发生任何变化。

`.then()`中执行的函数最多只会被执行一次。

Fulfilled和Rejected这两个中的任一状态都可以表示为Settled(不变的)。

Settled  resolve(成功) 或 reject(失败)。

https://speakerdeck.com/kerrick/javascript-promises-thinking-sync-in-an-async-world?slide=31



如何创建Promise

1. new Promise(fn) 返回一个Promise对象
2. 在fn中指定异步处理逻辑
   1. 结果正常，resolve(处理结果值)
   2. 结果异常，reject(Error对象)







Promise是值的“临时占位符”，这个值将在以后某个时间点，作为某个异步操作的结果来提供给当前程序。你可以使用Promise来表示操作的结果，而无需使用事件处理器或回调函数。

Promise有三种状态：Pending、Fulfilled和Rejected，一个Promise从Pending状态开始，在成功执行时进入Fulfilled状态，在失败时进入Rejected状态，无论是哪种情况，都可以通过添加处理器来表明该Promise的状态已经确定，then()方法可用于分配onFulfilled处理器和onRejected处理器；catch()方法用于分配onRejected处理器；finally()方法用于分配解决处理器，无论操作成功或失败，解决处理器都会执行。因为所有的Promise处理器都是作为微任务来执行的，因此它们在当前脚本工作完成之前不会执行。

可以使用构造函数来创建处于Pending状态的Promise对象，该构造函数接受一个执行器函数作为其唯一的参数，执行器函数被传给resolve()和reject()，以表明Promise是成功或失败，执行器在创建Promise时立即执行，这与被作为微任务运行的处理器不同。执行器抛出的任何错误都会被自动捕获并传递给reject()。

可以使用Promise.resolve()创建处于Fulfilled状态的Promise，或者使用Promise.reject()创建处于Rejected状态的Promise，这两个方法都会吧传入的参数包裹在Promise中（如果该参数不是一个Promise对象，也不是一个非Promise的thenable对象），并创建一个新的Promise对象，或者将原来的Promise对象原样传递。当你不确定某对象是Promise，但又希望他表现得想Promise时，这些方法很有帮助

虽然创建单一的Promise是在JS中处理异步操作的一种有用且有效的方法，但是你也可以以多种有趣的组合方式将多个Promise链接在一起来处理不同的问题。







根据确定的值来创建已经解决的Promise