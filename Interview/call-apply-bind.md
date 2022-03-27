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

### apply 实现

```javascript
Function.prototype.myApply = function (ctx, arr) {
    if (!Array.isArray(arr)) {
        throw new TypeError('CreateListFromArrayLike called on non-object')
    }

    const fn = Symbol()
    ctx.fn = this
    const result = ctx.fn(...arr)
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