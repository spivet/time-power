## 柯里化

根据[wikipedia](https://en.wikipedia.org/wiki/Currying)描述，**柯里化是指将接收多个参数的函数，转化为接收单个参数的函数序列**。

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