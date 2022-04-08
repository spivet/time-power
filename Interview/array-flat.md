## 数组扁平化

### 原生 flat()

原生 `flat` 方法可以拍平指定层数的数组，并返回一个新数组。

```javascript
const arr = [1,[2,3,[4,5],6],[7,8]]
const flatedArr = arr.flat(Infinity)
```

### 递归

利用 `reduce` 和 `concat` 递归实现

```javascript
function flatArray(arr) {
    return arr.reduce((flated, next) => {
        return flated.concat(Array.isArray(next) ? flatArray(next) : next)
    }, [])
}
```
