## call、apply、bind

### call 实现
```javascript
Function.prototype.myCall = function (ctx, ...args) {
    const fn = Symbol()
    ctx.fn = this
    const result = ctx.fn(...args)
    delete ctx.fn

    return result
}
```

### bind 实现

```javascript
Function.prototype.myBind = function (ctx, ...initArgs) {
    const fn = this
    return function (...args) {
        const allArgs = [...initArgs, ...args]
        fn.call(ctx, ...allArgs)
    }
}
```