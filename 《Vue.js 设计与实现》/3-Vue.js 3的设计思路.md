## 渲染函数

在 Vue 里可以直接使用模板的方式描述 UI，简单明了，也方便编译器进行优化。

但在某些特殊的情况下，直接编写虚拟 DOM 具有更高的灵活性。比如要实现一个根据 level 值来显示不同的标题标签，使用模板的方式，代码如下：

```html
<h1 v-if="level === 1">
    title 1
</h1>
<h2 v-else-if="level === 2">
    title 2
</h2>
<h3 v-else-if="level === 3"3
    title 1
</h3>
<!-- ... -->
```

上面代码明显缺少灵活性，但如果使用 JavaScript 对象来描述就会简单很多：

```javascript
let level = 1

const dom = {
    tag: `h${level}`,
    children: `title${level}`
}
```

为了让编写虚拟 DOM 更简单，Vue 还提供了一个 `h` 工具函数：

```javascript
import { h } from 'vue'

export default {
    render() {
        return h('h1')
    }
}
```

上面的 `render` 便是渲染函数，template 模板经过编译同样也会生成渲染函数。

## 创建虚拟 DOM



## 组件的本质

## 渲染虚拟 DOM
