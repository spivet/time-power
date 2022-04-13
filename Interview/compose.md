## 函数 compose

compose 函数是高阶函数的一种典型使用，它可以将多层函数嵌套调用扁平化。

```javascript
function compose(...funcs) {
    return function (args) {
        return funcs.reduce((result, fn) => {
            return fn(result)
        }, args)
    }
}

function fn1(x) {
    return x+1
}
function fn2(x) {
    return x+2
}
function fn3(x) {
    return x+3
}

// 不使用 compose
fn3(fn2(fn1(1))) // 7

// 使用 compose
var c = compose(fn1, fn2, fn3)
c(1) // 7
```