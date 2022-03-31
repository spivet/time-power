## 实现 new

要实现 new，首先要知道 new 都做了哪些事：

1. 创建一个新对象
2. 将新对象的 `__proto__` 指向构造函数的原型
3. 调用构造函数，将 this 属性赋值给新对象
4. 如果构造函数没有返回对象，则返回新对象

代码如下：

```javascript
const myNew = function (fn, ...arg) {
    const obj = {}
    obj.__proto__ = fn.prototype
    const result = fn.call(obj, ...arg)
    return itypeof result === 'object'
        ? result
        : obj
}
```
