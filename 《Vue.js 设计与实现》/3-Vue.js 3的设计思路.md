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

Vue 在运行时会调用组件上的 `render` 函数生成虚拟 DOM，那具体是如何创建的呢？

我们先来看看渲染函数的逻辑。

假如我们有如下模板代码：

```html
<template>
    <div>
        <div class="header">
            <h1>Hello Vue</h1>
        </div>
    </div>
</template>
```

通过模板编译生成的 `render` 函数：

```javascript
import { createElementVNode, openBlock, createElementBlock } from "vue"

const _hoisted_1 = /*#__PURE__*/createElementVNode(
    "div", 
    { class: "header" }, 
    [ /*#__PURE__*/ createElementVNode("h1", null, "Hello Vue") ], 
    -1 /* HOISTED */
)
const _hoisted_2 = [
  _hoisted_1
]

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (openBlock(), createElementBlock("div", null, _hoisted_2))
}
```

为了方便阅读，我将生成的代码稍微美化了一下。可以看出 `render` 函数内部主要通过 `createElementBlock` 来创建 VNode，而 `createElementBlock` 内部实现如下：

```javascript
function createElementBlock(type, props, children, patchFlag, dynamicProps, shapeFlag) {
    return setupBlock(createBaseVNode(type, props, children, patchFlag, dynamicProps, shapeFlag, true /* isBlock */));
}
```

再来看看使用 `h` 函数编写渲染函数：

```javascript
import { h } from 'vue'
export default {
    render() {
        return h('div', null, [
            h('div', { class: 'header' }, [
                h('h1', 'Hello Vue')
            ])
        ])
    }
}
```

`h` 函数的内部实现：

```javascript
function h(type, propsOrChildren, children) {
    const l = arguments.length;
    if (l === 2) {
        if (shared.isObject(propsOrChildren) && !shared.isArray(propsOrChildren)) {
            // single vnode without props
            if (isVNode(propsOrChildren)) {
                return createVNode(type, null, [propsOrChildren]);
            }
            // props without children
            return createVNode(type, propsOrChildren);
        }
        else {
            // omit props
            return createVNode(type, null, propsOrChildren);
        }
    }
    else {
        if (l > 3) {
            children = Array.prototype.slice.call(arguments, 2);
        }
        else if (l === 3 && isVNode(children)) {
            children = [children];
        }
        return createVNode(type, propsOrChildren, children);
    }
}
```

可以发现 `h` 函数只是做了一些简便用户使用的工作，最终还是调用的 `createVNode` 函数。

```javascript

function _createVNode(type, props = null, children = null, patchFlag = 0, dynamicProps = null, isBlockNode = false) {
    // ...
    const shapeFlag = shared.isString(type)
        ? 1 /* ELEMENT */
        : isSuspense(type)
            ? 128 /* SUSPENSE */
            : isTeleport(type)
                ? 64 /* TELEPORT */
                : shared.isObject(type)
                    ? 4 /* STATEFUL_COMPONENT */
                    : shared.isFunction(type)
                        ? 2 /* FUNCTIONAL_COMPONENT */
                        : 0;
    return createBaseVNode(type, props, children, patchFlag, dynamicProps, shapeFlag, isBlockNode, true);
}
```

从上面的代码可以看出，无论是 template 模板编译后的 `render` 函数，还是直接手写 `render` 函数，最终都将调用 `createBaseVNode` 方法去创建 VNode。

## 组件的本质

## 渲染虚拟 DOM
