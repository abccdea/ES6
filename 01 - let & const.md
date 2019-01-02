es6 提供了新的声明方式替代了以前的var
let const
1. var 不支持封闭作用域 会声明到全局作用域上

函数作用域 全局作用域

```
(function() {
    for (var i = 0; i< 5; i++) {
        console.log(i)
    }
})();
console.log(i)
```

2. 同一个作用域下重复声明变量
```
var a = 1;
function b() {
    let a = 3;
    let a = 4;
}
b();
```
3. 域解释问题 变量提升
```
a();
function a () {};
```
```
console.log(a);
let a = 1;
```
暂存死区，如果作用域内 有这样一个变量 那么这个作用域内就会绑定这个变量，不会继续向上查找了
```
let a = 1;
{
    console.log(a);
    let a = 2;
}
```

4. 通过const声明的变量不能被修改 基本和let一致，不能被修改引用空间

```
const a = {name: 'xxxx'};
a.age = 9;
console.log(a)
```