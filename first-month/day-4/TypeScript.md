## 类型拓宽

上一节讲 `类型推导` 的时候有写到，如果 `原始类型` 的变量通过 `let` 定义，又没有指明类型，则会默认推导为对应的类型。

这种推导的方式，也叫做 `类型拓宽`。

但上面的说法并不完全正确，因为在 JavaScript 中，`null` 和 `undefined` 也属于原始数据类型，但 TypeScript 在对这两个值做类型拓宽时，会被推导为 `any`。

如果变量值是字符串等原始类型，则只能重新赋值为相同类型的值：

```typescript
let str = 'hello'
str = 'world'
str = 123 // ts(2322) 赋值为其它类型的值会报错

let sym = Symbol()
sym = Symbol(1)
sym = false  // ts(2322) 赋值为其它类型的值会报错
```

如果是 `null` 和 `undefined`，则会拓宽为 any：

```typescript
let nl = null // let nl: any
nl = 'string'
nl = 123

let udf = undefined // let udf: any
udf = false
udf = [1, 2, 3]
```

但假如把一个值为 `null` 或者 `undefined` 的变量赋值给另一变量时，则该变量就是 `null` 或者 `undefined` 类型的值，而不是 `any`：

```typescript
let nl = null
let x = nl // let x: null
x = 'hello' // ts(2322) 不能将类型“"hello"”分配给类型“null”。
```

## 类型缩小

与类型拓宽相对应的是类型缩小，不过类型缩小需要通过一些方式手动操作，将较为宽泛的类型缩小为准确的类型，降低代码运行时的错误概率。

比如下面代码，`padLeft` 函数接收一个填充值，可以是字符串，也可以是数字。如果是数字，就用空格填充指定的次数：

```typescript
function padLeft(padding: string | number, input: string) {
    // ts(2345) 类型“string | number”的参数不能赋给类型“number”的参数。
    return " ".repeat(padding) + input
}
padLeft(3, 'oh!')
```

对于以上代码，TypeScript 会在开发阶段就报错提示，因为 `repeat` 方法接收字符串无效。

我们可以通过 `typeof` 类型守卫的方式，进行类型缩小，改成下面的代码便能正常运行：

```typescript
function padLeft(padding: number | string, input: string) {
    if (typeof padding === "number") {
        return " ".repeat(padding) + input;
    }
    return padding + input;
}
padLeft(3, 'oh!')
```

类型缩小的方式还有很多，比如通过 `等式`、`in 操作符`、`条件判断`、`断言` 等方式：

```typescript
type Animal = 'cat' | 'dog'
const catchMouse = (cat: 'cat') => `${cat} eat mouse`
const guardDoor = (dog: 'dog') => `${dog} is a guarder`

function move(animal: Animal) {
    if (animal === 'cat') {
        catchMouse(animal)
    } else {
        guardDoor(animal)
    }
}
```

我们并不需要记住所有类型缩小的方式，因为当代码存在潜在风险时，TypeScript 会在开发环境就给我们提示，我们只需要在发现错误时及时做出对应修改就行。
