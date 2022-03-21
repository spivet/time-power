## 函数

### 创建函数

函数声明，指定函数参数和返回值的类型：

```typescript
function add(a: number, b: number): number {
    return a + b
}
add(1,2) // 正确
add(3, '4') // 报错 ts(2345)
```

也可以使用箭头函数：

```typescript
const add = (a: number, b: number): number => {
    return a + b;
}
```

### 参数类型

指定参数类型与变量一样，参数后面接 `:`，再加上数据类型：

```typescript
function fn1(x: string, y: number ) {
    // ...
}
fn1('hello', 77)
```

只要定义了参数，无论什么类型，调用时都要传值：

```typescript
// 必传参数
function fn2(x: undefined) {
    //...
}
fn2() // ts(2554) 应有 1 个参数，但获得 0 个。
fn2(undefined) // 必须显示传入 undefined
```

如果想定义可选参数，只需要在类型标注的 `:` 前添加 `?` 即可：

```typescript
function fn3(x?: string) {
    // ...
}
fn3('hello') // 可以传入字符串参数
fn3() // 也可以不传参数
```

必选参数不能位于可选参数后：

```typescript
function fn4(x?: string, y: number) { // ts(1016) 必选参数不能位于可选参数后。
    // ...
}
```

### 返回值类型

javascript 中声明函数，没有明确 return 一个返回值，则返回值默认为 undefined：
```typescript
function fn() {
    console.log('fn')
}
console.log(fn()) // undefined
```

但是在 TypeScript 中声明函数，想要表示没有返回值的类型，需要使用 `void`：
```typescript
function fn(): void {
    console.log('fn')
}
console.log(fn())
```

如果使用 undefined 作为返回值类型，回得到错误提示：

```typescript
function fn(): undefined { // ts(2355) 其声明类型不为 "void" 或 "any" 的函数必须返回值。
    console.log('fn')
}
```

### 函数类型

定义函数类型的方式有两种：

1. 使用胖箭头 `=>`，箭头左边是参数，右边是返回值类型；
2. 类似对象方法的简写，`:` 号左边是参数，右边是类型。

```typescript
interface Request {
    get: (url: string, options: object) => any
    post(url: string, options: object): any 
}
```
