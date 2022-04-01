## 实现 new

要实现 new，首先要知道 new 都做了哪些事：

1. 创建一个新对象
2. 将新对象的 `__proto__` 指向构造函数的原型
3. 将 this 赋值给新对象，即 this 指向新对象
4. 调用构造函数，给新对象添加属性
5. 如果构造函数没有返回对象，则返回新对象

代码如下：

```javascript
const myNew = function (fn, ...arg) {
    const obj = {}
    obj.__proto__ = fn.prototype
    const result = fn.call(obj, ...arg)
    return result instanceof Object
        ? result
        : obj
}
```
