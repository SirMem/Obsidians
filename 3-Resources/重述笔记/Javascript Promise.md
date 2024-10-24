---
Project: "[[web前端]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-09-20
Connected:
---
## Promise介绍

### 介绍

`Promise` 是异步编程的一种新的解决方案，它比之前的纯回调函数形式更加合理和强大。它用于处理异步操作，允许你在未来某个时间点获取操作的结果。

#### Promise 的表达

- **从语法上说**：Promise 是一个构造函数。
- **从功能上说**：Promise 实例对象可以用来封装一个异步操作，并在未来某时刻获取到其结果。

由于异步操作的结果具有不确定性，并不一定每次都能成功获取结果。Promise 技术通过状态标识不同的返回结果。

#### Promise 对象的三种状态：

1. **`pending`**：初始状态，表示尚未完成（等待中）。
2. **`resolved/fulfilled`**：成功状态，表示异步操作已成功并获取到了结果。
3. **`rejected`**：失败状态，表示异步操作失败，未能获取到结果。

> Promise 的状态只能从 `pending` 变为 `resolved` 或 `rejected`，一旦状态发生变化就不能再改变。

#### 状态的变化：

1. 从 `pending` 变为 `resolved`，通过调用 `resolve` 函数实现。
2. 从 `pending` 变为 `rejected`，通过调用 `reject` 函数实现。

Promise 只能改变一次状态，状态一旦确定下来，任何时间都可以获取到这个结果（除非发生错误导致结果获取失败）。

---

### Promise 构造函数

使用 `new Promise()` 创建一个 Promise 实例对象。Promise 构造函数接受一个执行器函数（`executor`）作为参数，该函数包含两个参数：`resolve` 和 `reject`，这两个参数都是函数，用来通知 Promise 最终的执行结果。

```js
const promise = new Promise((resolve, reject) => {
    // 执行器函数：可以放置异步任务的代码
    console.log(1);  // 立即执行的同步代码

    // 模拟异步操作
    setTimeout(() => {
        resolve('成功的结果');  // 当异步操作成功时调用 resolve
        // reject('失败的原因');  // 当异步操作失败时调用 reject
    }, 1000);
});

console.log(2);  // 同步代码，优先于异步执行
```

上面的代码输出顺序是：

```
1
2
// 1秒后
成功的结果
```

#### 执行流程解析：

1. 构造 `Promise` 时，执行器函数立即同步执行。
2. 异步任务（如 `setTimeout`）延迟执行，最终通过调用 `resolve` 或 `reject` 来改变 `Promise` 的状态。
3. 状态一旦变为 `resolved` 或 `rejected`，结果会被传递到 `then` 或 `catch` 中的相应回调函数。

---

### Promise 的 `then` 方法

当 Promise 状态从 `pending` 变为 `resolved` 或 `rejected` 后，可以通过 `then` 方法分别指定成功和失败的回调函数。

#### value
- 当 `Promise` 状态变为 `fulfilled`（已成功）时，`then()` 方法的第一个回调函数会接收一个参数，这个参数就是 `value`。
- 这个 `value` 通常是由 `resolve()` 函数传递的值。
- 也可以是链式调用中then的上级传递下来的值(`Promise` 状态变为 `fulfilled`（已成功))
#### reason
- 当 `Promise` 状态变为 `rejected`（已失败）时，`then()` 方法的第二个回调函数会接收一个参数，这个参数就是 `reason`。
- 这个 `reason` 通常是由 `reject()` 函数传递的值，表示 `Promise` 失败的原因。
- 也可以是链式调用中then的上级传递下来的值(`Promise` 状态变为 `rejected`（已失败))

```js
promise.then(
    value => {
        // 状态为 resolved 时执行此函数
        console.log('resolved', value);
    },
    reason => {
        // 状态为 rejected 时执行此函数
        console.log('rejected', reason);
    }
);
```

- 第一个回调函数：当 Promise 状态为 `resolved` 时执行，参数 `value` 是上级then方法或Promise对象异步操作成功时传递的结果。
- 第二个回调函数：当 Promise 状态为 `rejected` 时执行，参数 `reason` 是上级then方法或Promise对象异步操作失败时传递的原因。

#### 例子：

```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('异步操作成功！');
    }, 1000);
});

promise.then(
    value => {
        console.log(value);  // 打印：异步操作成功！
    },
    reason => {
        console.error(reason);
    }
);
```

---

### Promise 状态变化与捕获

`Promise` 的状态是不可逆的，一旦 `resolve` 或 `reject` 被调用，状态便会锁定为 `resolved` 或 `rejected`，无法再次更改。

`Promise` 可以通过 `resolve`、`reject` 或 `throw` 来改变状态。状态变化时会触发相应的回调函数，但如果发生异常，状态会自动变为 `rejected`，并且抛出的错误会被 `.catch()` 捕获。

#### 1. 状态变化：`resolve` 和 `reject`

- 调用 `resolve()` 会将 Promise 的状态从 `pending` 变为 `resolved`，并传递成功的值。
- 调用 `reject()` 会将状态从 `pending` 变为 `rejected`，并传递拒绝的原因。

#### 2. 主动抛出异常 `throw`

当 Promise 内部主动抛出异常时（例如使用 `throw new Error()`），Promise 的状态会自动变为 `rejected`，并且错误信息会传递给 `.catch()` 进行处理。


```JavaScript
const p = new Promise((resolve, reject) => {
    // resolve(1);  // 这会将状态设置为 resolved
    // reject(2);   // 这会将状态设置为 rejected
    throw new Error('出错了');  // 抛出异常，状态会自动变为 rejected
});

p.then(
    value => console.log(value)  // 成功时不会执行此处
)
.catch(
    reason => console.log(reason.message)  // 捕获错误信息，输出：出错了
);

```
#### 捕获失败状态

在实际开发中，推荐使用 `.catch()` 方法来处理 Promise 的失败情况，它相当于 `.then()` 的第二个参数，但更具可读性。

```js
promise
    .then(value => {
        console.log('成功：', value);
    })
    .catch(error => {
        console.error('失败：', error);
    });
```


## 获取Promise结果的时机


`Promise` 是用于处理异步操作的对象，无论异步任务何时完成，`Promise` 的结果都可以通过 `.then()` 方法进行获取。在 `Promise` 对象创建后，内部的异步任务会立即开始执行，但结果只会在任务完成后才能被访问。在这一过程中，`.then()` 方法提供了获取成功或失败结果的方式。

### 1. 异步任务开始执行，Promise 进入 `pending` 状态

一旦创建 `Promise` 对象，内部的异步任务（例如 `setTimeout`）会立即开始执行，但结果尚未传递到 `resolve` 或 `reject` 中，此时 `Promise` 处于 `pending` 状态。

示例代码：

```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const time = Date.now();
        if (time % 2 === 0) {
            resolve('成功了的结果！' + time);
        } else {
            reject('被拒绝的原因！' + time);
        }
    }, 1000);
});
```

此时，异步任务开始执行，但 `resolve` 或 `reject` 还未被调用，`promise` 仍处于 `pending` 状态。

### 2. 异步任务执行完毕，Promise 状态变更为 `resolved` 或 `rejected`

异步任务完成后，Promise 状态会从 `pending` 变为 `resolved`（成功）或 `rejected`（失败）。无论状态如何变化，`.then()` 方法都会响应并获取相应的结果。

```js
promise.then(
    value => {
        console.log('成功的结果:', value);
    },
    reason => {
        console.log('失败的原因:', reason);
    }
);
```

在此例中，`.then()` 在异步任务完成时，获取到 `resolve` 或 `reject` 传递的值。

### 3. `.then()` 可在异步任务完成前调用

`Promise` 的 `.then()` 方法可以在异步任务执行完成之前调用，不影响结果的获取。即使在异步任务还未完成时注册 `.then()`，当 `Promise` 的状态变为 `resolved` 或 `rejected` 时，注册的回调函数会自动执行。

示例代码：

```js
setTimeout(() => {
    promise.then(
        value => {
            console.log('异步任务执行完毕，成功的结果:', value);
        },
        reason => {
            console.log('异步任务执行完毕，失败的原因:', reason);
        }
    );
}, 2000);  // 这会在 promise 内部的异步任务完成后才执行
```

即使 `Promise` 内部的任务已经执行完毕，调用 `.then()` 依然可以获得之前的结果。

### 4. 总结

- `Promise` 对象创建后，异步任务立即开始执行。
- `.then()` 方法可以在异步任务开始执行前调用，也可以在异步任务完成后调用，结果都会被正确获取。
- 当异步任务完成时，`Promise` 的状态从 `pending` 变为 `resolved` 或 `rejected`，并触发相应的回调函数。
- 无论何时调用 `.then()`，只要 `Promise` 的状态已经变更，都会立即返回结果。

## Promise API

### 1. Promise 构造函数：`new Promise(excutor)`

- **参数 excutor**：接收两个回调函数 `resolve` 和 `reject`。
- **执行时机**：executor 会在 Promise 内部立即同步调用，但异步操作会在执行器中执行。

- **`resolve` 方法**：请求成功时调用的回调函数，返回结果：
  
  ```js
  value => {}
  ```

- **`reject` 方法**：请求被拒绝时调用的回调函数，返回结果：

  ```js
  reason => {}
  ```

### 2. Promise.prototype.then 方法 `(onResolved, onRejected) => {}`

- **`onResolved` 回调**：当成功时调用，取到上面成功执行的结果：

  ```js
  (value) => { console.log(value) }
  ```

- **`onRejected` 回调**：当被拒绝时调用，取到上面失败的结果：

  ```js
  (reason) => { console.log(reason) }
  ```

- **说明**：指定用于得到 `value` 的成功回调，和用于得到 `reason` 的失败回调，并返回一个新的 `Promise` 对象，方便链式调用。

### 3. Promise.prototype.catch 方法 `(onRejected) => {}`

- **`onRejected` 回调**：被拒绝时的回调：

  ```js
  (reason) => { console.log(reason) }
  ```

- **说明**：`then()` 的语法糖，等同于 `.then(undefined, onRejected)`。

### 4. `Promise.resolve(value)` 方法

- **功能**：通过参数把成功的数据放到 Promise 对象上。
- **返回**：一个成功的 `Promise` 对象。

### 5. `Promise.reject(reason)` 方法

- **功能**：通过参数把被拒绝的原因放到 Promise 对象上。
- **返回**：一个被拒绝的 `Promise` 对象。

### 6. `Promise.all(promiseArr)` 方法

- **参数**：包含 `Promise` 对象的数组 `promiseArr`。
- **功能**：返回一个新的 `Promise`，当数组中的所有 `Promise` 都成功时，返回成功值；如果有一个被拒绝，则返回该拒绝的 `reason`。
- **返回**：一个新的 `Promise`，返回结果是一个数组，数组顺序与 `promiseArr` 的顺序一致。

### 7. `Promise.race(promiseArr)` 方法

- **参数**：包含 `Promise` 对象的数组 `promiseArr`。
- **功能**：返回一个新的 `Promise`，只要其中任何一个 `Promise` 完成，无论成功或失败，都会立即返回这个 `Promise` 的结果。
- **返回**：一个新的 `Promise`，第一个完成的 `Promise` 结果就是最终的结果。

---

### 示例代码：

```js
// 生成一个成功的返回值为1的promise对象
const p1 = new Promise((resolve, reject) => {
  resolve(1);
});
p1.then(value => console.log(value));

// 生成一个成功的返回值为2的promise对象
const p2 = Promise.resolve(2);
p2.then(value => console.log(value));

// 生成一个被拒绝的返回值为3的promise对象
const p3 = Promise.reject(3);
p3.catch(reason => console.log(reason));

// 使用 Promise.all
//将多个promise实例放到一起，包装成一个新的promise实例
const pAll = Promise.all([p2, p1]);
pAll.then(values => console.log('values', values))
    .catch(reason => console.log('reason', reason));

// 使用 Promise.race
const pRace = Promise.race([p1, p2, p3]);
pRace.then(values => console.log('values', values))
     .catch(reason => console.log('reason', reason));
```

---

### 补充说明：

1. **`Promise.resolve()` 和 `Promise.reject()`** 提供快速生成已成功或已失败状态的 `Promise`，可以用于需要快速模拟或操作 `Promise` 状态的场景。(语法糖)
2. **`Promise.all()`** 适用于需要等待多个异步任务全部完成的场景，若有任一 `Promise` 失败，`Promise.all()` 会立即返回失败。
3. **`Promise.race()`** 则用于竞速场景，返回最先完成的 `Promise` 结果，无论成功或失败。


## Promise窜连多个任务
### then返回的promise的状态
- promise.then返回的新的promise的状态由什么决定？
> 一句话回答：由then()指定的回调函数中自动执行的结果决定(then的上级任务)

#### `.then()` 返回值的三种情况：

1. **如果回调函数中 `return` 一个非 `Promise` 的值**：
   - `Promise` 的状态为 `resolved`，返回值为 `return` 的这个值。
   - 相当于return new Promise.resolve("返回的非Promise值")

 
  ```js
  new Promise((resolve) => resolve('success'))
    .then(value => 'new value')  // 返回非Promise
    .then(value => console.log(value));  // 输出 'new value'
  ```

2. **如果回调函数中 `return` 一个 `Promise` 对象**：
   - `Promise` 的状态由返回的 `Promise` 决定，即外层 `Promise` 的状态和返回的 `Promise` 的状态保持一致。
     - 如果返回的 `Promise` 状态是 `resolved`，则外层 `Promise` 也是 `resolved`，并返回这个 `Promise` 的成功值。
     - 如果返回的 `Promise` 状态是 `rejected`，则外层 `Promise` 也是 `rejected`，并返回这个 `Promise` 的失败原因。

  ```js
  new Promise((resolve) => resolve('success'))
    .then(value => new Promise((resolve) => resolve('new value')))  // 返回Promise
    .then(value => console.log(value));  // 输出 'new value'
  ```

3. **如果回调函数中抛出异常**：
   - `Promise` 的状态为 `rejected`，返回值为抛出的异常。

  ```js
  new Promise((resolve) => resolve('success'))
    .then(value => { throw new Error('出错了！'); })  // 抛出异常
    .catch(reason => console.log(reason.message));  // 输出 '出错了！'
  ```
### 示例代码：

```js
// 创建一个Promise，初始resolve一个成功的值
new Promise((resolve, reject) => {
  resolve('成功的值！');
})
.then(value => {
  // 返回一个非Promise的值
  return 'abc';
  // 或者返回新的Promise
  /*
  return new Promise((resolve, reject) => {
    resolve('新的Promise的value');
    // reject('新的Promise的reason');
  });
  */
  // 抛出异常
  // throw new Error('出错了！');
})
.then(
  value => {
    // 捕获上一个then返回的值或Promise的结果
    console.log('value', value);
  },
  reason => {
    // 捕获上一个then中的异常
    console.log('reason', reason);
  }
);
```

### 用promise串联多个操作任务(链式调用)
- 调用多个 `then()` 方法来处理异步任务即为串联多个操作任务，也为链式调用
- 链式调用的多个then方法里面可以包括不同的任务，可以有同步任务和异步任务

```js
	// 使用 Promise 执行多个异步任务
	new Promise((resolve, reject) => {
		//定时器，异步函数
		setTimeout(() => {
			console.log('执行异步任务1');
			resolve('a'); // 完成任务1，传递结果 'a'
		}, 1000);
	})
	.then(value => {
		// 处理任务1的结果
		console.log('任务1的结果: ' + value);
		console.log('执行同步任务2');
		return 'b'; // 返回任务2的结果
	})

```

## Promise的异常穿透和中断链
### 异常穿透
- 当Promise链中发生错误时，它会沿着链向下传播，也就是说，如果有一个上级的then发生了错误，多个下级的then没有相应的错误处理方法，那么这个错误就会一直传播下去，直到遇到了相应的错误处理方法，这就是异常穿透

```js
.then(
  value => console.log(value),
  // 相当于默认的错误处理逻辑
  // reason => {throw reason}
  // 还相当于写成
  // reason => Promise.reject(reason)
  
  // 如果在这里就处理了异常，最后的catch的回调函数就不会再执行了
  reason => {
    console.log('onRejected1()' + reason);
    return 'b';
  }
)
.then(
  value => {
    console.log('onResolved2()' + value);
    return 'c';
  },
  reason => {throw reason}
)
.then(
  value => console.log('onResolved3()' + value),
  reason => {throw reason}
)
.catch(
  reason => console.log(reason) // 异常传递
)
```

- 每个`.then()`可以有两个回调函数：`onFulfilled`和`onRejected`。
- 如果没有提供`onRejected`，它默认为`reason => { throw reason; }`。
- 如果在`.then()`的`onRejected`回调中处理了错误并返回了一个非拒绝的值，链式调用会正常继续。
- 后续的`.then()`调用将接收到前一个`.then()`或`.catch()`返回的值。
- 如果在`.then()`调用中没有进行错误处理，错误会传播到最终的`.catch()`。

### 中断链
- 如果不希望下面方法的回调函数执行就可以返回一个pending状态的promise, 这样以下的任何then或者catch方法的回调函数都不会调用了，相当于对下级的then进行了中断
- 中断链式调用：`return new Promise(() => {})`
- 这会返回一个永远处于pending状态的Promise，这effectively会阻止后续的`.then()`或`.catch()`回调函数的执行。

```java
```javascript
.catch(
  reason => {
    console.log('最后的catch: ' + reason);
    // throw reason
    // return Promise.reject(reason)
    // 相当于在这里处理了一次错误
    return new Promise(() => {}) //已中断，无法执行下级的then方法
  }
)
```

### 总结

- 错误会沿着Promise链向下传播，直到被捕获。
- 异常穿透和中断链息息相关，通常一起使用
- `.catch()`实际上是`.then(null, errorHandler)`的语法糖。
- 在`.catch()`块中返回一个永远处于pending状态的Promise（`new Promise(() => {})`）将阻止链式调用的进一步执行。

## async函数
### 介绍
async函数是JavaScript中处理异步操作的一种方式，它建立在Promise之上，提供了更简洁的语法来处理异步代码。(语法糖)

### 特性
1. async关键字用于声明异步函数 
2. async函数内部可以使用await关键字 
3. async函数总是返回一个Promise对象

### 基本使用示例
```js
//返回非Promise值
async function fn1() {
    return 1;
}
const f1 = fn1();
console.log(f1); // 输出：Promise {<resolved>: 1}
f1.then(value => console.log(value)); // 输出：1

//返回resolved状态的Promise
async function fn2() {
    return Promise.resolve(2);
}
console.log(fn2()); // 输出：Promise {<resolved>: 2}

//抛出异常
async function fn3() {
    throw 3;
}
console.log(fn3()); // 输出：Promise {<rejected>: 3}

//返回rejected状态的Promise
async function fn4() {
    return Promise.reject(4);
}
console.log(fn4()); // 输出：Promise {<rejected>: 4}
```

### 异步操作
```js
async function fn5() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(5);
        }, 1000);
    });
}

console.log(fn5()); // 输出：Promise {<pending>} 
//fn5返回一个Promise，这个Promise在1秒后resolve。虽然fn5立即返回，但返回的Promise initially处于pending状态，1秒后变为resolved状态。

fn5().then(value => console.log(value)); // 1秒后输出：5

```


### 注意事项
1. async函数可以看作是使用Promise的语法糖，它简化了Promise的使用，使得异步代码更易于编写和理解。
2. 在async函数中，可以使用await关键字等待一个Promise解决，这使得异步代码可以以同步的方式编写。
3. 虽然async函数总是返回一个Promise，但这个Promise的状态和值取决于函数的执行结果。
4. 处理async函数的错误可以使用try-catch语句，或者在调用时使用.catch()方法。


## await函数及其处理异常
### 基本概念

await是一个表达式，用于等待异步操作的结果。它只能在async函数中使用。

### 使用

1. await后面跟一个表达式，表示等待该表达式的结果
2. await命令只能在async函数中使用（async函数中不一定非要使用await命令）

### 示例代码

```javascript
function fn() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(1)
    }, 2000)
  })
}

async function fn1() {
  console.log('abc');
  // await后面如果是promise对象的话，返回的是当前状态下的值，并非promise对象本身
  const result = await fn(); // 代码运行到await命令这一行，如果需要等待就会暂停代码，并继续执行后面的代码。
  // await后面如果是普通值，则返回的就是值本身
  // const result = await 2
  console.log(result);  //输出 `1`,resolve结果的值
  console.log('hello'); //输出'hello'
}

fn1();

//展示了 `await` 如何使异步代码看起来像同步代码，同时保持了非阻塞的特性。
```

- 完整的输出顺序是：
1. `'abc'`（立即输出）
2. （等待 2 秒）
3. `1`
4. `'hello'`

- 当遇到await命令时，如果后面是Promise对象，JavaScript引擎会暂停执行，等待Promise对象resolve，然后将resolve的值作为await表达式的运算结果。
- 如果await后面不是Promise对象，会直接返回对应的值。

### 处理异常
- await命令后面的Promise对象，运行结果可能是rejected，所以最好把await命令放在try...catch代码块中。
```js
async function fn2() {
  try {
    const result = await fn();
    console.log(result);
  } catch (error) {
    console.error('Error occurred:', error);
  }
}
```

#### Promise.all同时触发
- 多个await命令后面的异步操作，如果不存在继发关系，最好让它们同时触发。

```js
// 非优化版本
let foo = await getFoo();
let bar = await getBar();

// 优化版本
let [foo, bar] = await Promise.all([getFoo(), getBar()]);
```

### 实际应用
```js
async function fetchUserData(userId) {
  try {
    const response = await fetch(`https://api.example.com/users/${userId}`);
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const userData = await response.json();
    console.log('User data:', userData);
    return userData;
  } catch (error) {
    console.error('Error fetching user data:', error);
    throw error;  // 重新抛出错误，让调用者知道发生了错误
  }
}

// 使用方法
fetchUserData(123)
  .then(data => {
    // 处理获取到的用户数据
  })
  .catch(error => {
    // 处理可能发生的错误
  });
```

这个例子展示了如何在实际应用中使用async/await，包括错误处理和链式调用。

### 总结
await关键字提供了一种更直观的方式来处理异步操作，使得异步代码看起来更像同步代码，提高了代码的可读性和可维护性。然而，使用await时需要注意错误处理和性能优化，以确保代码的健壮性和效率。


## 扩展：网络请求
在 JavaScript 中，`fetch()` 是一个用于发起网络请求并处理响应的 API，基于 `Promise`，允许你异步地获取资源，例如通过 HTTP 请求获取数据。

`fetch()` 被引入以取代较早的 `XMLHttpRequest`，使得代码更简洁且易于处理异步操作。它支持常见的 HTTP 方法如 `GET`、`POST`、`PUT`、`DELETE` 等。

### 基本语法
```javascript
fetch(url, [options])
```

- `url`：请求的 URL，字符串格式。
- `options`（可选）：一个配置对象，用于设置请求的详细信息，例如请求方法、请求头、请求体等。

### `fetch()` 的特点：
1. **基于 Promise**：`fetch()` 返回一个 `Promise`，该 `Promise` 会在请求完成后解析为 `Response` 对象。
2. **不会自动拒绝**：即使服务器返回 404 或 500 错误，`fetch()` 也不会拒绝（不会进入 `.catch()`），除非请求失败（如网络错误）。
3. **默认是 GET 请求**：如果不提供 `options`，默认会发起一个 `GET` 请求。

### 示例：发起一个 GET 请求

```javascript
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();  // 解析为 JSON
  })
  .then(data => {
    console.log(data);  // 处理数据
  })
  .catch(error => {
    console.error('There has been a problem with your fetch operation:', error);
  });
```

#### 解释：
- 首先调用 `fetch()` 发送网络请求，返回一个 `Promise`，解析为 `Response` 对象。
- `response.ok` 用于检查请求是否成功（即 HTTP 状态码为 200-299），如果不是，则抛出错误。
- `response.json()` 将响应内容解析为 JSON 格式，返回一个 `Promise`。
- 在 `.then()` 中处理解析后的数据。
- `.catch()` 用于捕获网络错误或代码中抛出的异常。

### 示例：发起一个 POST 请求

```javascript
fetch('https://api.example.com/data', {
  method: 'POST',   // 使用 POST 方法
  headers: {
    'Content-Type': 'application/json'  // 设置请求头
  },
  body: JSON.stringify({ key: 'value' })  // 请求体数据，需转换为字符串
})
  .then(response => response.json())
  .then(data => {
    console.log(data);  // 处理返回的数据
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

#### 解释：
- 使用 `method: 'POST'` 指定为 POST 请求。
- 使用 `headers` 设置 `Content-Type` 为 `application/json`，告诉服务器我们发送的是 JSON 数据。
- 使用 `body` 发送请求体数据，需将对象转换为 JSON 字符串。

### `fetch()` 的一些常见使用选项：
- `method`：指定 HTTP 方法，如 `GET`、`POST`、`PUT`、`DELETE`。
- `headers`：设置请求头，如 `Content-Type`、`Authorization`。
- `body`：设置请求体内容（主要用于 `POST`、`PUT` 请求）。
- `mode`：请求的模式，如 `cors`（默认，允许跨域请求）、`no-cors`（不允许跨域请求）、`same-origin`（仅限同源请求）。
- `credentials`：是否在跨域请求中发送 cookie，例如 `same-origin` 或 `include`。
- `cache`：控制缓存模式，如 `default`、`no-store`、`reload`、`no-cache`。

### 处理不同格式的响应
`fetch()` 的 `Response` 对象提供了多种解析方法，来处理不同类型的响应：
- `.json()`：解析响应为 JSON 格式，返回一个 `Promise`。
- `.text()`：解析响应为纯文本，返回一个 `Promise`。
- `.blob()`：解析响应为二进制数据（`Blob`），返回一个 `Promise`。
- `.arrayBuffer()`：解析响应为 `ArrayBuffer`，用于处理低级二进制数据。

### 总结：
- `fetch()` 是现代 JavaScript 中处理网络请求的主要方式，它使得发送 HTTP 请求更加简单和直观。
- 通过 `Promise` 链，可以轻松处理异步网络请求的结果或捕获错误。
- 它比传统的 `XMLHttpRequest` 更易读、简洁，并且有更广泛的支持选项，如 `mode`、`credentials` 等。

