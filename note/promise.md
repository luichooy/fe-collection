# Promise

## unhandledRejection

当 Promise 被 reject 但是没有 reject 处理器的时候，会触发`unhandledRejection`事件

#### 事件属性

**_promise_**
触发 `unhandledRejection` 事件的 Promise，该事件被 reject 但没有添加 reject 处理器

**_reason_**
如果设置了 reject 处理器，则 reason 就是将要传入 reject 处理器的参数，即 catch 回调参数

#### 监听

方式一

```javascript
window.addEventListener('unhandledrejection', event => {
	console.warn(`unhandledrejection: ${event.reason}`)
})
```

方式二

```javascript
window.onunhandledrejection = function(event) {
  console.warn(`unhandledrejection: ${event.reason}`);
});
```

#### 示例

```javascript
new Promise((resolve, reject) => {
	reject('rejected!')
}).then(() => {})

Promise.reject('rejected').then(() => {})
```

上述代码都没有添加 reject 处理器，所以会触发`unhandledRejection`事件

```javascript
new Promise((resolve, reject) => {
	reject('rejected!')
})
	.then(() => {})
	.catch()
```

上述代码虽然添加了 `catch`，但没有传入回调函数，即没有 reject 处理器，也会触发`unhandledRejection`事件

```javascript
new Promise((resolve, reject) => {
	reject('rejected!')
})
	.then(() => {})
	.catch(() => {})
```

上述代码添加了 `catch`并且传入了回调函数，不会触发`unhandledRejection`事件

```javascript
new Promise((resolve, reject) => {
	reject('rejected!')
}).then(
	() => {},
	() => {}
)
```

上述代码在 then 回调中添加了 reject 处理器，不会触发`unhandledRejection`事件

```javascript
window.addEventListener('unhandledrejection', event => {
	console.log('unhandledRejection', event)
	event.preventDefault()
})

const promise = Promise.reject('rejected')
```

上述代码中的 promise 未添加 then 回调，因此不会被 `unhandledrejection` 事件捕获到，而是直接在控制台打印错误 `Uncaught (in promise) rejected`(不知道原因)

## rejectionhandled

当 Promise 被 reject 但在本次事件循环结束时没有发现 rejection 处理器，则会触发`rejectionhandled`事件。这意味着 rejection 处理器是被异步添加的，且是在本次事件循环结束之后添加的。

#### 使用

```javascript
window.addEventListener('rejectionhandled', event => {
	console.log('rejectionhandled', event)
	event.preventDefault()
})
```

#### 示例

```javascript
const promise = Promise.reject('rejected!')

setTimeout(() => {
	promise.catch(() => {})
}, 2000)
```

上面代码中，promise 的 rejection 处理器是异步添加，且是在本次事件循环结束之后添加的，故会触发`rejectionhandled`事件

```javascript
const promise = Promise.reject('rejected!')

new Promise((resolve, reject) => {
	resolve()
}).then(() => {
	promise.catch(() => {})
})
```

上面代码中，promise 的 rejection 处理器是异步添加，但却是在本次事件循环过程中添加的，故不会触发`rejectionhandled`事件

```javascript
const promise = Promise.reject('rejected!')

new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve()
	}, 0)
}).then(() => {
	promise.catch(() => {})
})
```

上面代码中，promise 的 rejection 处理器是异步添加，且是在下个事件循环中被添加，故会触发`rejectionhandled`事件


## 注意

1. `unhandledRejection`、`rejectionhandled`这两个事件在高版本浏览器（只测试了edge109.0.1518.52，不可以同时触发的）中不能同时添加，否则只会触发`unhandledRejection`事件，而`rejectionhandled`事件不会触发。而在低版本浏览器中可以同时触发（只测试了chrome80.0.3987.149，是可以同时触发的）
2. 如果页面url是一个 File 协议路径，则`rejectionhandled`也不会触发，必须是 http 协议路径