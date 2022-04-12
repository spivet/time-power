## 函数 compose

compose 函数是高阶函数的一种典型使用，它可以将多层函数嵌套调用扁平化。

```javascript
function compose(...funcs) {
    return function (...args) {
        return funcs.reduce((result, fn) => {
            return fn(prev())
        }, args)
    }
}
```