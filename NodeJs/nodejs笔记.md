##### Promise

特点：（1）对象的状态不受外界影响。Promise 对象代表一个异步操作，有三种状态：Pending（进行中）、Resolved（已完成，又称 Fulfilled）和 Rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是 Promise 这个名字的由来，它的英语意思就是「承诺」，表示其他手段无法改变。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise 对象的状态改变，只有两种可能：从 Pending 变为 Resolved 和从 Pending 变为 Rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。就算改变已经发生了，你再对 Promise 对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

###### all

```js
function pro1(){
    return new Promise((function(resolve, reject){
        setTimeout(function(){
            console.log("执行任务1");
            resolve("执行任务1成功");
        }, 2000);
    }));
}

function pro2(){
    return new Promise((function(resolve, reject){
        setTimeout(function(){
            console.log("执行任务2");
            resolve("执行任务2成功");
        }, 2000);
    }));
}
function pro3(){
    return new Promise((function(resolve, reject){
        setTimeout(function(){
            console.log("执行任务3");
            resolve("执行任务3成功");
        }, 2000);
    }));
}

Promise.all([pro1(), pro2(), pro3()]).then(function(data){
    console.log(data);
    console.log({}.toString.call(data));
})
```

Promise.all接收一个数组参数，里面的值最终都算返回Promise对象。这样，三个异步操作是并行执行的，等到它们都执行完之后才会进入then里面。三个异步操作返回的数据都在then中，all会把所有异步操作的结果放进数组传给then，就是上面的results。



