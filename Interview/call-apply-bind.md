## call、apply、bind

```javascript
Function.prototype.myCall = function (ctx, ...args) {
    const fn = Symbol()
    ctx.fn = this
    const result = ctx.fn(...args)
    delete ctx.fn

    return result
}
```