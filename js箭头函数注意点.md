## call,apply,箭头函数指向问题
这两天在家一直在复习以前的知识,今天回顾到了call,apply
下面引用一个小的demo

```javascript
    var Pet = {
            words : '...',
            speak : function (say) {
                console.log(say + ''+ this.words)
            }
        }
    Pet.speak('Speak'); // 结果：Speak...

    var Dog = {
            words:'Wang'
    }
    //将this的指向改变成了Dog
    Pet.speak.call(Dog, 'Speak') //结果： SpeakWang
```
我对demo进行了无意修改,Pet对象中的speak改用箭头函数

```javascript
    var Pet = {
            words : '...',
            speak : say => {
                console.log(say + ''+ this.words)
            }
        }
    Pet.speak('Speak'); // 结果：Speakundefined

    var Dog = {
            words:'Wang'
    }
    Pet.speak.call(Dog, 'Speak') // 结果：Speakundefined
```
当用箭头函数的时候,并没有获得this.words,于是上网搜了下,进行下总结:

1.函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

// 我觉得这个解释不太容易理解,箭头函数里面本身没有this指针,它内部的this指针实际是外层代码块的this
就拿demo为例子,由于箭头函数没有this指针,所以箭头函数的this是对象的代码块的this,也就是全局。当箭头函数执行到this.words
的时候,向外层代码块找this,如果还没有,接着向外层找。

2.不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

3.不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

4.除了this，以下三个变量在箭头函数之中也是不存在的，指向外层函数的对应变量：arguments、super、new.target。

5.由于箭头函数没有自己的this，所以当然也就不能用call()、apply()、bind()这些方法去改变this的指向。

6.由于箭头函数使得this从“动态”变成“静态”，下面两个场合不应该使用箭头函数。
// 引用阮一峰的[ECMAScript 6 入门](http://es6.ruanyifeng.com/#docs/function#%E7%AE%AD%E5%A4%B4%E5%87%BD%E6%95%B0)

第一个场合是定义函数的方法，且该方法内部包括this。

```javascript
const cat = {
  lives: 9,
  jumps: () => {
    this.lives--;
  }
}
```

上面代码中，cat.jumps()方法是一个箭头函数，这是错误的。调用cat.jumps()时，如果是普通函数，该方法内部的this指向cat；如果写成上面那样的箭头函数，使得this指向全局对象，因此不会得到预期结果。

第二个场合是需要动态this的时候，也不应使用箭头函数。

```javascript
var button = document.getElementById('press');
button.addEventListener('click', () => {
  this.classList.toggle('on');
});
```
上面代码运行时，点击按钮会报错，因为button的监听函数是一个箭头函数，导致里面的this就是全局对象。如果改成普通函数，this就会动态指向被点击的按钮对象。

另外，如果函数体很复杂，有许多行，或者函数内部有大量的读写操作，不单纯是为了计算值，这时也不应该使用箭头函数，而是要使用普通函数，这样可以提高代码可读性。



