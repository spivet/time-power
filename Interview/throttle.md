## 节流（throttle）

函数节流是指一段时间内，多次触发操作，也只执行一次会掉函数。

```javascript
function throttle(fn, delay) {
    let timer = null

    return function (...args) {
        if (timer) return
        timer = setTimeout(() => {
            fn.call(this, ...args)
            timer = null
        }, delay)
    }
}
```
