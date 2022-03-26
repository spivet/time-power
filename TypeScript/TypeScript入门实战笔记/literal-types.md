## 字面量类型

TypeScript 中有三种字面量类型，分别是 `字符串字面量类型`，`数字字面量类型`， `布尔字面量类型`。

```typescript
// 字符串字面量类型
let name: 'tom' = 'tom'
name = 'tony' // ts(2322) 不能将类型"tony"分配给类型"tom"。

// 数字字面量类型
let num: 520 = 521 // ts(2322) 不能将类型“521”分配给类型“520”。

// 布尔字面量类型
let bool: true = true
```

字面量类型与普通类型是充分不必要关系，比如 `字符串字面量类型` 一定是 `字符串类型`，但 `字符串类型` 不一定是某个特定的 `字符串字面量类型`：

```typescript
let name: string = 'tony'
let tom: 'tom' = 'tom'
name = tom // 将字符串字面量类型的值复制给字符串类型是可以的
tom = name // ts(2322) 不能将类型“string”分配给类型"tom"。
```

定义单个的 `字面量类型` 值并没有太大意义，通常是把多个字面量类型组合成一个 `联合类型`，用来描述拥有明确成员的实用的集合。

```typescript
type UserType = 'student' | 'teacher'
function getDataList(type: UserType) {
    // ...
}
getDataList('student')
getDataList('master') // ts(2345) 类型“"master"”的参数不能赋给类型“UserType”的参数。
```

### 类型推导

在编写 TypeScript 代码时，有些时候我们并不需要给变量指明类型，主要是原始类型的数据，TypeScript 会自动进行推导。

在 TypeScript 中，`具有初始化值的变量` 的类型，`有默认值的函数参数` 的类型，`函数返回的值` 的类型，都可以根据上下文推断出来。

定义原始类型数据是，**用 let 定义原始类型数据，TypeScript 会自动推导成对应的数据类型**：

```typescript
let name = 'tom' // 推导为 let name: string
name = 'tony'
let num = 1 // 推导为 let num: number
num = 2
let bool = true // 推导为 let bool: boolean
bool = false
```

**用 const 定义原始类型数据，TypeScript 会自动推导成对应的字面量类型**：
```typescript
const str = 'hello' // const str: "hello"
const num2 = 2 // const num2: 2
const bool2 = false // const bool2: false
```

定义引用类型数据时，如果对象内有值，TypeScript 会连同对象内的属性一起推导：

```typescript
let arr = [3, 2, 1] // let arr: number[]
arr = [1, 2, 3]
arr = [1, 2, '3'] // ts(2322) 不能将类型“string”分配给类型“number”。

const obj = {
    age: 18,
}
// const obj: {
//     age: number;
// }
obj.age = 19
obj.age = '20' // ts(2322) 不能将类型“string”分配给类型“number”。
```

如果对象内没有值，则情况又不一样：

```typescript
let arr = [] // let arr: any[]
arr = [1, 2, 3] // arr 可以赋值为包含任意元素的数组

let obj = {} // let obj: {}
obj = 'string' // obj 可以赋值为除 null、undefined 外的任意值
obj = ['apple']
obj = null // ts(2322) 不能将类型“null”分配给类型“{}”。
```

所以**在定义引用类型的数据时，最佳实践还是指定数据类型**。
