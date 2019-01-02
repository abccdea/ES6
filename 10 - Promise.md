Promise 是 es6 新功能
generator async await 都需要promise

koa generator async await

axios redux-saga promise

fetch


Promise 是一种异步流程的控制手段
1. 回调地狱，代码难以维护，第一个的输出是第二个的输入
```
$.ajax(
    success() {
        $.ajax()
    }
)
```
promise 链式调用

2. promise 可以支持多个并发的请求，获取并发请求中的数据
```
ajax() // 1
ajax() // 2
// [1, 2]
```
3. promise 可以解决异步的问题，本身不能说promise是异步的

promise 关键字 resolve reject pending

如果一旦promise成功了就不能失败，相反也是一样

只有pending可以转resolve或reject

每一个promise实例上都有一个then方法， then方法有两个参数，一个参数是成功的函数，一个是失败的函数
promise中发生错误，就会执行失败态

事件环
Promise只有一个参数，叫executor执行器 默认new时调用
Promise可以实现不再传递回调函数

```
let p = new Promise((resolve, reject) => {// 默认promise中的executor是同步执行的
    console.log(1);
    resolve('buy');
    throw new Error();
    setTimeout(() => {
        resolve('buy');
    }, 1000)
});
console.log(2);
// then方法是异步调用的，事件环
p.then((value) => { // value成功的原因
    console.log(value);
}, (err) => { // value失败的原因
    console.log(err);
})
```


回调地狱
```
let fs = require('fs');
fs.readFile('1.txt','utf8', function(err, data) {
    if (err) return console.log(err);
    console.log(data);
    fs.readFile(data, 'utf8', function(err, data) {
        if (err) return console.log(err);
        console.log(data);
    })
});



function read(url) {
    return new Promise((resolve, reject) => {
        fs.readFile(url, 'urf8', function(err,data) {
            if(err) reject(err);
            resolve(data);
        })
    })
}

// 如果一个Promise返回，那么会取这个promise的结果
// promise链式调用返回的不是this 而是一个新的promise
read('1.txt').then((data) => {
    console.log(data);
    return read(data);
}, () => {

}).then(data => {

}, err => {

});
```



```
let p = new Promise((resolve, reject) => {
    resolve('success);
});

// 一个promise实例可以then多次
p.then(data => {
    console.log(data);
});
p.then(data => {
    console.log(data);
});
```
```
// all调用后会返回一个新的promise，然后then
// 并发的
Promise.all([read('1.txt'),read('2.txt')]).then(

)

Promise.race().then();
```


```
let p = new Promise(
    executor(reslove, reject)
);
p.then();



// 手写Promise
class Promise {
    constructor(executor) {
        this.status = 'pending';
        this.value = undefined;
        this.reason = undefined;
        let resolve = value => {
            if (this.status === 'pending') {
                this.status = 'resolved';
                this.value = value;
            }
        }
        let reject = reason => {
            if (this.status === 'pending') {
                this.status = 'rejected'
                this.reason = reason;
            }
        }
        try {
            executor(resolve, reject);
        } catch(e) {
            reject(e);
        }
    }
    then(onFufilled,onRejected) {
        if (this.status === 'resolved') {
            onFufilled(this.value);
        }
        if (this.status === 'rejected') {
            onRejected(this.reason);
        }
    }
}

module.exports = Promise
```