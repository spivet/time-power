## 静态类型

### 原始类型

```typescript
// 字符串类型
const str1: string = 'string'
const str2: string = String('string')
const str3: string = new String('string') // ts(2322) 不能将类型“String”分配给类型“string”。

// 数字类型
const num1: number = 1
const num2: number = Number(2)
const num3: number = 0xf00d
const num4: number = new Number(3) // ts(2322) 不能将类型“Number”分配给类型“number”。

// bigInt 类型
const bigInt1: bigint = 3n // ts(2737) 目标低于 ES2020 时，bigInt 文本不可用。

// 布尔类型
const boole1: boolean = true
const boole2: boolean = false

// symbol 类型
const sym1: symbol = Symbol()

// undefined 类型
// 原始数据类型中，`undefined` 几乎没用
let undf1: undefined = undefined
undf1 = 'hello' // ts(2322) 不能将类型“"hello"”分配给类型“undefined”。

// null
// 在定义一些前后端交互的数据上可能会用到
const nl: null = null
```

### Array 类型

```typescript
// 子元素为 string 类型的数组
const arr1: string[] = ['hello', 'typescript']
const arr2: string[] = ['今天是', 3, '月'] // ts(2322) 不能将类型“number”分配给类型“string”。

// 子元素为 number 类型的数组
const arr3: number[] = [1, 2, 3]

// 也可以使用 Array 泛型的方式定义
const arr4: Array<boolean> = [true, false]
```

### Tuple 类型（元组）

简单理解，元组就是指定元素个数与类型，并且排好序的数组。

```typescript
const tuple1: [number, string] = [520, 'typescript', 323]
const tuple2: [number, string] = ['520', 'typescript'] // ts(2322) 不能将类型“string”分配给类型“number”。
const tuple3: [number, string] = [520, 'typescript', 666] // ts(2322) 源具有 3 个元素，但目标仅允许 2 个。
```

元组通常用于定义函数参数的类型，比如：

```typescript
function fn(param: [number, string]) {
    return `${param[0]} and ${param[1]}`
}
fn([1, '0']) // 10
fn(['1', 0]) // ts(2322)
```

### any 类型

`any` 指的是一个任意类型，我们可以把任何类型的值赋值给 `any` 类型的变量，也可以把 `any` 类型的值赋值给任意类型（除 `never` 以外）的变量

```typescript
let value: any = 'hello'
value = 666
console.log(value) // 666

let str: string = 'string'
str = value
console.log(str) // 666
```

从长远来看，使用 any 绝对是一个坏习惯。如果一个 TypeScript 应用中充满了 any，此时静态类型检测基本起不到任何作用，也就是说与直接使用 JavaScript 没有任何区别

### unknown 类型

`unknown` 与 `any` 类似，但 `unknown` 类型的值只能赋值给 `unknown` 或 `any`。

```typescript
let result: unknown
let str: string = 'string'
str = result // ts(2322) 不能将类型“unknown”分配给类型“string”。
result = 'hello'
```

### void 类型

void 类型仅适用于表示没有返回值的函数。即如果该函数没有返回值，那它的类型就是 void。

```typescript
// function fn(): void
function fn() {
    return;
}
```

### never 类型

never 表示永远不会发生值的类型

```typescript
function InfiniteLoop(): never {
  while (true) {}
}
```

### object 类型

`JavaScript` 中的所有引用类型都是对象，所以在 `TypeScript` 中的 `object` 类型适用于所有引用类型。

虽然在 `JavaScript` 中，使用 `typeof null`，会返回 `'object'`，但这属于设计失误，在 TypeScript 中弥补了这一点。

```typescript
const obj1: object = {}
const obj2: object = new Number(1)
const obj3: object = new Date()
const obj4: object = Object.create(null)
// 别忘了数组和函数也是对象
const obj5: object = [1, 2, 3]
const obj6: object = function fn() {}
// ts(2322) 不能将类型“null”分配给类型“object”。
const obj7: object = null
```
