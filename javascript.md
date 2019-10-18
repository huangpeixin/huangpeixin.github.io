###准备知识
####Javascript单线程
>单线程负责用户代码执行和网页渲染，缓慢的代码阻塞网页渲染

```Javascript
$('body').html('')
alert()//alert结束后才回渲染
```

####同步任务/异步任务
>同步任务：线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；
异步任务：不进入主线程、而进入"任务队列"（task queue）的任务，当主线程上任务执行完后才可能执行。
![KZEu6J.png](https://s2.ax1x.com/2019/10/18/KZEu6J.png)



#### 事件与回调函数
> EventTarget.addEventListener() 方法将指定的监听器注册到 EventTarget 上，当该对象触发指定的事件时，指定的回调函数就会被执行。
事件目标可以是一个文档上的元素 Element,Document和Window或者任何其他支持事件的对象 (比如 XMLHttpRequest)。

```Javascript
document.onclick = function(){}
setTimeout(function(){},1000)

```

#### 调用栈
> 调用栈是记录当前程序所在位置的数据结构。
如果当前进入了某个函数，这个函数就会被放在栈里面。
如果当前离开了某个函数，这个函数就会被弹出栈外


#### 事件循环
>![KVnfER.jpg](https://s2.ax1x.com/2019/10/18/KVnfER.jpg)
```
console.info(1)
$.post(url,callback)
console.info(2)
```


#### Tasks、MircoTask
> ![KZ3Ucq.png](https://s2.ax1x.com/2019/10/18/KZ3Ucq.png)
tasks: setTimeout   setInterval   setImmediate   I/O   UI渲染
microtasks: Promise   process.nextTick   Object.observe   MutationObserver



------------
[========]


# 异步操作
## AJAX
```javascript
function ajax(url,callback){
    var xhr = new XMLHttpRequest()
    xhr.open('GET',url)
    //绑定事件
    xhr.onload = function() {
       //回调
       callback(xhr.response)
    }
    xhr.send()   
}
```

## setTimeout()/setInterval()
```
//绑定回调
setTimeout(function(){
  console.info('hi')
},1000)
```
> 当你向 setTimeout() (或者其他函数)传递一个函数时,该函数中的this指向跟你的期望可能不同
由setTimeout()调用的代码运行在与所在函数完全分离的执行环境上。
这会导致，这些代码中包含的 this 关键字在非严格模式会指向 window (或全局)对象，严格模式下为 undefined

## Promise
```javascript
const p1 = new Promise(function(resolve, reject) {
    // ...
    resolve(val)
})

p1.then(callback1).then(callback2)
```
> Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果
Promise{value,[then]}


##async/await (Generator+Promise)
```
$$function sleep(duration){
	return new Promise(function(resolve){
		setTimeout(resolve, duration);
	})
}

async function foo(){
	await sleep(3000)
	console.info(1)
}

foo()$$$$
```


## Web workers
```
//worker.js
onmessage = function(e) {
	console.info(e.data)
	postMessage(data);
}

//main
const myWorker = new Worker("worker.js");
myWorker.postMessage(data);
myWorker.onmessage = function(e) {
	console.log(e.data);
}


```
worker并没有执行I/O操作的权限，只能为主线程分担一些诸如计算等任务

## window.postMessage()

## 其他HTML5
```javascript
<script async="async"></script> 

```





