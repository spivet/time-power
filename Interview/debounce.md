## 防抖（debounce）

函数防抖是指在一定时间内，如果没有继续操作，再执行回调函数。通常用于输入框搜索等。

```JavaScript
function debounce(fn, delay) {
    let timer = null

    return function (...args) {
        if (timer) {
            clearTimeout(timer)
        }
        timer = setTimeout(() => {
            fn.call(this, ...args)
        }, delay)
    }
}
```
