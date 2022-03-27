## 柯里化

### 定义

根据[wikipedia](https://en.wikipedia.org/wiki/Currying)描述，**柯里化是指将接收多个参数的函数，转化为接收单个参数的函数序列**。

比如，柯里化一个带有 3 个参数的函数 `f`，会创建3个函数：

```javascript
// 原函数
var x = f(a, b, c)

// 转化为
var h = g(a)
var i = h(b)
var x = i(c)

// 或者序列化调用
var x = g(a)(b)(c)
```

### 作用

#### 1. 参数复用

```javascript
// 柯里化前
function phoneNum(areaCode, phone) {
    return `+${areaCode} ${phone}`
}
var alice = phoneNum('01', '555-0100')
var xiaoHong = phoneNum('86', '15000000000')

// 柯里化后
function curryPhone(areaCode) {
    return function phoneNum(phone) {
        return `+${areaCode} ${phone}`
    }
}
var usaNum = curryPhone('01')
var alice = usaNum('555-0100')
var chNum = curryPhone('01')
var xiaoHong = chNum('15000000000')
```

#### 2. 延迟执行

```javascript
function add(...args) {
    return args.reduce((prev, next) => prev + next, 0)
}
console.log(add(1,2,3)) // 6，调用 add 函数，就一定会计算出结果

function curry(fn) {
    const allArgs = []
    return function result(...args) {
        if (args.length === 0) {
            return fn(...allArgs)
        }
        allArgs.push(...args)
        return result
    }
}

const sum = curry(add)
sum(1)
sum(2,3) // 传入参数，但并未计算结果
sum(4)
console.log(sum()) // 10，需要时才计算结果
```

与 `bind` 类似，通过柯里化，我们可以延迟函数执行，需要时再进行求值。

### 实现

从上面两段代码可以看出，实现柯里化函数的核心有两点：

1. **获取被柯里化函数的形参个数**
2. **利用闭包存储已接收的参数**

接下来实现定义里的柯里化函数，只需要在上面代码的基础上稍作修改：

```javascript
function curry(fn) {
    // 获取被柯里化函数的形参个数
    const fnLength = fn.length
    const allArgs = []

    return function storeArg(...args) {
        // 利用闭包存储已传入的参数
        allArgs.push(...args)
        if (allArgs.length < fnLength) {
            return storeArg
        }
        // 已有参数大于或等于所需的形参个数，才会执行
        return fn(...allArgs)
    }
}

function getVolume(length, width, height) {
    return length * width * height
}

const calcStep = curry(getVolume)
const area = calcStep(2, 3) // 保存实参，未计算结果
const volume = area(4) // 计算值
console.log(volume) // 24
```

如果想在柯里化时就能传入参数，如 `bind` 方法一样，改动也很简单：

```javascript
function curry(fn, ...initArgs) {
    const fnLength = fn.length
    const allArgs = [...initArgs]

    return function storeArg(...args) {
        allArgs.push(...args)
        const result = allArgs.length < fnLength ? storeArg : fn(...allArgs)
        return result
    }
}
function getVolume(length, width, height) {
    return length * width * height
}
const calcStep = curry(getVolume, 5)
const volume = calcStep(2, 3)
console.log(volume) // 30
```

### 思考题

实现一个 `add` 方法，实现如下预期：

```javascript
// 可以实现值为 6 的预期
add(1)(2)(3)
// 可以实现值为 10 的预期
add(1, 2, 3)(4)
```

`add` 可以不断传入值，因此返回值肯定是一个函数，需要调用其它方法返回值。

```javascript
function add(...initArgs) {
    // 利用闭包保存传入的参数
    const allArgs = [...initArgs];
    const _add = function (...args) {
        allArgs.push(...args)
        return _add
    }

    // 实现一个 value 方法，返回计算值
    _add.value = function() {
        return allArgs.reduce((prev, next) => prev + next, 0)
    }

    // 返回 _add 函数，可以不断传入参数
    return _add
}

console.log(add(1,2,3).value())
console.log(add(1,2,3)(4).value())
```
