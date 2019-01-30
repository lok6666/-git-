## call,apply
下面引用一个小的demo
### call()
call 方法第一个参数也是作为函数上下文的对象，但是后面传入的是一个参数列表，而不是单个数组。
```javascript
    function f(firstNumber, secondNumber) {
        console.log(firstNumber + this.number + secondNumber);
    }
    var obj = {
            number: 1
    }
    var numArr = [1, 2];
    //将this的指向改变成了obj,从而调用了f()
    //call支持用...解构数组来传递参数
    f.call(obj, ...numArr) //call支持用...解构数组结果： 4
```
### apply()
apply 方法传入两个参数：一个是作为函数上下文的对象，另外一个是作为函数参数所组成的数组。

```javascript
    function f(firstNumber, secondNumber) {
        console.log(firstNumber + this.number + secondNumber);
    }
    var obj = {
            number: 1
    }
    //将this的指向改变成了obj,从而调用了f()
    f.apply(obj, [1, 2]) //结果： 4
```



