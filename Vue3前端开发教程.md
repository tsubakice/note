# Vue3 前端开发教程

## 0. 基础代码模板

### 0.1. 配置路径映射

#### 0.1.1. vite.config.ts

```ts
// ...
import { fileURLToPath } from 'url'

// https://vite.dev/config/
export default defineConfig({
  // ...
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('src', import.meta.url)),
    },
  },
})
```

#### 0.1.2. tsconfig.app.json

```json
{
  // ...
  "compilerOptions": {

    /* 配置路径别名 */
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },
  // ...
  }
}
```

### 0.2. 重写部分文件

#### 0.2.1. index.html

```html
<!doctype html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <meta name="viewport"
        content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="icon" href="/favicon.svg" type="image/svg+xml">
  <title>Title</title>
</head>
<body>
<div id="app"></div>
<script type="module" src="/src/main.ts"></script>
</body>
</html>
```

#### 0.2.2. style.css

```css
@font-face {
  font-family: LXGWWenKai;
  src: url("@/assets/fonts/LXGWWenKai-Regular.ttf");
  font-display: swap;
}

@font-face {
  font-family: CodeNewRoman;
  src: url("@/assets/fonts/CodeNewRomanNerdFont-Regular.otf");
  font-display: swap;
}

:root {
  font-family: LXGWWenKai, CodeNewRoman, system-ui, sans-serif;
  text-rendering: optimizeLegibility;
}

body {
  margin: 0;
}
```

### 0.3. 引入 element 组件库

```bash
bun add element-plus # 引入 element-plus 本体
bun add -D unplugin-vue-components unplugin-auto-import # 引入自动配置插件
```

```ts
// ...
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

// https://vite.dev/config/
export default defineConfig({
  // ...
  plugins: [
    // ...
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
})
```

### 0.4. 引入 tabler 图标库

```bash
bun add @tabler/icons-vue # 引入 tabler 本体的 vue 版本
```

```vue
<script setup>
import { IconHome } from '@tabler/icons-vue'
</script>

<template>
<!-- size：图标尺寸；color：图标颜色 -->
<el-icon size="36" color="gray">
  <IconHome/>
</el-icon>
</template>

<style scoped>

</style>
```

## 1. 创建 Vue3 项目

```bash
# 于当前目录下，使用 vue-ts 模板创建 Vue3 项目
bun create vite . --template vue-ts
```

### 1.1. 简单示例

```vue
<script setup lang="ts">
const number = 0
</script>

<template>
  <div :data-number="number">{{ number }}</div>
</template>

<style scoped lang="scss">

</style>
```

上述代码中的 `:data-number="number"` 和 `{{ number }}` 均为 vue 提供的 [模板语法](https://cn.vuejs.org/guide/essentials/template-syntax.html)，能够声明式地将数据绑定/渲染到指定的 DOM 上。

## 2. Vue3 核心语法

### 2.1. 响应式数据

**响应式数据** 是指，当数据变化时，依赖该数据的所有地方（如视图、计算属性、副作用函数）能够 **自动更新** 的机制。

### 2.2. 使用 `ref()` 创建响应式数据

该函数将传入的值包装为一个响应式的 `ref` 对象，该 `ref` 对象可以通过其 `value` 属性访问或更新传入的值：

```ts
import { ref } from 'vue'

const count = ref(0)

console.log(count) // { ..., value: 0 }
console.log(count.value) // 0

count.value++
console.log(count.value) // 1
```

> 扩展：[为 ref 标注类型](https://cn.vuejs.org/guide/typescript/composition-api#typing-ref)

`ref` 对象在模板中可以不加 `.value` 直接使用（因为 `ref` 会自动解包），不过也有一些 [注意事项](https://cn.vuejs.org/guide/essentials/reactivity-fundamentals#caveat-when-unwrapping-in-templates)：

```html
<button type="button" @click="count.value++">
  {{ count }}
</button>
```

> 扩展：有关更多 `ref()` 的详细信息，请参阅 [`ref()` 的官方文档](https://cn.vuejs.org/api/reactivity-core.html#ref)。

### 2.3. 使用 `reactive()` 创建响应式数据

除 `ref()` 外还可以使用 `reactive()` 创建响应式数据，和 `ref()` 这种将数据包装在对象中的方式不同，`reactive()` 将会使对象本身具有响应式：

```ts
import { reactive } from 'vue'

const state = reactive({ count: 0 })

state.count++ // 由于是对象本身具有了响应式，所以不用像访问 ref 对象那样 .value
console.log(state.count) // 1
```

> 扩展：[为 reactive 标注类型](https://cn.vuejs.org/guide/typescript/composition-api.html#typing-reactive)

`reactive` 对象在模板中的使用：

```html
<button type="button" @click="state.count++">
  {{ state.count }}
</button>
```

> 扩展：有关更多 `reactive()` 的详细信息，请参阅 [`reactive()` 的官方文档](https://cn.vuejs.org/api/reactivity-core.html#reactive)。

由于 `reactive()` 存在着某些 [限制](https://cn.vuejs.org/guide/essentials/reactivity-fundamentals.html#limitations-of-reactive)，官方明确建议使用 `ref()` 作为创建响应式数据的主要方式。

### 2.4. toRef() 和 toRefs()

#### 2.4.1 toRef() 函数

`toRef()` 可以将值、现存的 `ref` 或 `getter` 转化为 ref 对象，也可以基于响应式对象（ref 和 reactive 均可）上的一个属性，创建与之关联的 ref 对象，这样创建的 ref 对象将与其源属性保持同步，改变源属性的值将更新 ref 对象的值，反之亦然：

```ts
import { ref, toRef } from 'vue'

const existingRef = ref(1)

// 等同于 ref(1)
toRef(1)

// 原样返回现有的 ref
console.log(existingRef === toRef(existingRef)) // true

// 创建一个只读的 ref，当访问其 value 属性时会调用此 getter
const readonlyRef = toRef(() => 666)
console.log(readonlyRef.value) // 666
```

基于 `ref` 对象的某个属性：

```ts
import { ref, toRef } from 'vue'

// 准备一个 ref 对象
const state = ref({
  foo: 1,
  bar: 2,
})

// 双向 ref，fooRef 会与源属性 foo 同步
const fooRef = toRef(state.value, 'foo')

// 更改 fooRef 会更新源属性 foo
fooRef.value++
console.log(state.value.foo) // 22

// 更改源属性 foo 也会更新 fooRef
state.value.foo++
console.log(fooRef.value) // 23
```

基于 `reactive` 对象的某个属性：

```ts
import { reactive, toRef } from 'vue'

// 准备一个 reactive 对象
const state = reactive({
  foo: 1,
  bar: 2,
})

// 双向 ref，fooRef 会与源属性 foo 同步
const fooRef = toRef(state, 'foo')

// 更改 fooRef 会更新源属性 foo
fooRef.value++
console.log(state.foo) // 2

// 更改源属性 foo 也会更新 fooRef
state.foo++
console.log(fooRef.value) // 3
```

> 扩展：有关 `toRef(state, 'foo')` 和 `ref(state.foo)` 的区别，请参阅 [`toRef()` 的官方文档](https://cn.vuejs.org/api/reactivity-utilities.html#toref)。

#### 2.4.2 toRefs() 函数

将一个 `reactive` 对象转换为一个普通对象，这个普通对象的每个属性都是指向源对象相应属性的 `ref` 对象，这些 `ref` 对象都是使用 `toRef()` 创建的：

```ts
const state = reactive({
  foo: 1,
  bar: 2,
})

const stateAsRefs = toRefs(state)

// 这个 ref 和源属性已经链接上了
state.foo++
console.log(stateAsRefs.foo.value) // 2

stateAsRefs.foo.value++
console.log(state.foo) // 3

// 如果此时使用解构写法的话，就解决了前文提到的 reactive 的限制之一，直接解构会导致响应式丢失
const { foo, bar } = toRefs(state)
```

> 扩展：有关更多 `toRefs()` 的详细信息，请参阅 [`toRefs()` 的官方文档](https://cn.vuejs.org/api/reactivity-utilities.html#torefs)。

### 2.5. computed() 计算属性

接受一个 `getter`，返回一个只读的 `ref` 对象，该 ref 对象的 `value` 属性值就是 getter 的返回值。又或者接受一个带有 `get` 和 `set` 属性的对象来创建一个可写的 ref 对象。

创建一个只读的计算属性：

```ts
import { computed, ref } from 'vue'

const count = ref(1)
const plusOne = computed(() => count.value + 1)

console.log(plusOne.value) // 2

plusOne.value++ // 报错
```

创建一个可写的计算属性：

```ts
import { computed, ref } from 'vue'

const count = ref(1)
const plusOne = computed({
  get: () => count.value + 1,
  set: (val) => {
    count.value = val - 1
  },
})

plusOne.value = 1
console.log(count.value) // 0
```

> 扩展：
>
> - [为 computed 标注类型](https://cn.vuejs.org/guide/typescript/composition-api.html#typing-computed)
> - 有关更多 `computed()` 的详细信息，请参阅 [`computed()` 的官方文档](https://cn.vuejs.org/api/reactivity-core.html#computed)。

### 2.6. watch() 数据侦听

默认 **懒侦听** 一个或多个响应式数据，并在数据变化时执行传入的回调函数。

第一个参数是侦听的数据源，这个数据源可以是以下几种类型：

- 一个 ref 对象
- 一个 reactive 对象
- 一个 getter
- 由以上类型的值组成的数组

第二个参数则是在数据变化时执行的回调函数。该回调函数接受三个参数：**新值、旧值**，以及用于清理副作用的回调函数（简称 **清理函数**），清理函数会在回调函数下一次重新执行前执行。当侦听多个数据源时，回调函数接收两个数组，分别对应多个数据源的新值和旧值。

第三个可选参数则是一个配置对象，支持如下选项：

- `immediate`，在侦听器创建时立即执行传入的回调函数，其第一次执行时旧值为 undefined。
- `deep`，如果侦听的数据源是对象，则强制深度侦听，以便在数据源深层级数据变更时执行回调。
- `once`，明确声明回调函数只会执行一次，侦听器将在回调函数首次执行后自动停止。
- 更多选项请查阅 [`watch()` 的官方文档](https://cn.vuejs.org/api/reactivity-core.html#watch)。

#### 2.6.1 侦听一个简单类型的 ref 对象

```ts
import { ref, watch } from 'vue'

const state = ref(0)

// 当侦听的是一个 ref 对象时，newValue 和 oldValue 会分别获取 ref 对象 value 属性变化前后的值
watch(state, (newValue, oldValue) => {
  console.log(`state.count newValue: ${newValue}, oldValue: ${oldValue}`)
})
```

#### 2.6.2 侦听一个引用类型的 ref 对象

```ts
import { ref, watch } from 'vue'

const state = ref({ count: 0 })

// 当侦听一个引用数据类型的 ref 对象时，需要开启 deep 选项，否则回调函数无法被执行
// 前文提到过，当侦听的是一个 ref 对象时，newValue 和 oldValue 会分别获取 ref 对象 value 属性变化前后的值
// 由于此时 ref 对象的 value 属性值是一个对象，并且变化的是该对象内部的属性，该对象本身并没有发生改变，所以 newValue 和 oldValue 获取的还是这个对象
// 这解释了为什么此时 newValue === oldValue 的结果为 true，也解释了为什么需要开启 deep 选项才能执行回调函数
watch(state, (newValue, oldValue) => {
  console.log(newValue === oldValue, oldValue.count)
}, { deep: true })
```

#### 2.6.3 侦听一个 reactive 对象

```ts
import { reactive, watch } from 'vue'

const state = reactive({ count: 0 })

// 侦听一个 reactive 对象和侦听一个引用类型的 ref 对象类似，同样的 newValue === oldValue 的结果为 true
// 只不过不需要手动开启 deep 选项了，deep 选项会默认启用，并且是无法关闭的
watch(state, (newValue, oldValue) => {
  console.log(newValue === oldValue, oldValue.count)
}, { deep: false }) // 就算这么写了也没用
```

#### 2.6.4 侦听一个 getter

```ts
import { reactive, watch } from 'vue'

const state = reactive({
  a: {
    b: {
      c: 0, // 由于此时使用了 getter 来侦听 c，即使这是一个 reactive 对象，在修改 d 的时候也不会执行回调
      d: 1, // 有疑惑的话可以尝试修改一下
    },
  },
})

// 当侦听的是一个 getter 时，回调只有在 getter 的返回值变化时才会被执行
// 从另一个角度上看，借助这个特性能够实现属性的精确侦听，从而减小侦听开销
// 如果使用 getter 返回的是一个对象的话，需要开启 deep 选项才能侦听对象内部的属性（和侦听引用类型的 ref 对象类似）
watch(() => state.a.b.c, (newValue, oldValue) => {
  console.log(`state.a.b.c newValue: ${newValue}, oldValue: ${oldValue}`)
})
```

#### 2.6.5 侦听由以上类型的值组成的数组

```ts
import { ref, watch } from 'vue'

const person = ref({
  name: 'tsubaki',
  age: 21,
  gender: 'male',
})

// 回调函数中的 [newName, newAge], [oldName, oldAge] 分别是 newValue, oldValue 的解构写法
watch([() => person.value.name, () => person.value.age], ([newName, newAge], [oldName, oldAge]) => {
  console.log(newName, newAge, oldName, oldAge)
})
```

#### 2.6.6 暂停/恢复/停止侦听

```ts
// ...
const { pause, resume, stop } = watch(source, callback)

// 暂停侦听器
pause()

// 稍后恢复
resume()

// 停止
stop()
```

### 2.7. watchEffect() 数据自动侦听

立即执行传入的回调函数，同时响应式地侦听其依赖的数据，并在数据发生更改时重新执行传入的回调。

其第一个参数就是执行的回调函数，该回调的参数是一个清理副作用的回调函数（简称 **清理函数**）。清理函数会在该回调函数下一次重新执行前执行。

第二个可选参数则是一个配置对象，详情请查阅 [`watchEffect()` 的官方文档](https://cn.vuejs.org/api/reactivity-core.html#watcheffect)。

示例如下：

```ts
import { ref, watchEffect } from 'vue'

const count = ref(0)

watchEffect(() => console.log(count.value))
// -> 输出 0

count.value++
// -> 输出 1
```

停止/暂停/恢复侦听：

```ts
// ...
const { pause, resume, stop } = watchEffect(callback)

// 暂停侦听器
pause()

// 稍后恢复
resume()

// 停止
stop()
```

### 2.8. useTemplateRef() 获取模板引用

`useTemplateRef()` 接收一个 ref 标识，返回一个模板引用的 ref 对象，其值为模板中的具有匹配 ref 标识的元素或组件实例。

```vue
<script setup lang="ts">
import Child from '@/components/Child.vue'
import { useTemplateRef } from 'vue'

const inputRef = useTemplateRef('inputRef')
const childRef = useTemplateRef('childRef')
</script>

<template>
<input type="text" ref="inputRef">
<Child ref="childRef"></Child>
</template>

<style scoped>

</style>
```

> 扩展：
>
> - [为模板引用标注类型](https://cn.vuejs.org/guide/typescript/composition-api.html#typing-template-refs)
> - [为组件模板引用标注类型](https://cn.vuejs.org/guide/typescript/composition-api.html#typing-component-template-refs)

上述代码中，我们通过 `const childRef = useTemplateRef('childRef')` 拿到了 Child 的组件实例，不知道大家有没有尝试过访问这个实例中的属性？如果尝试过就会发现，我们无法访问 childRef 这个实例身上的任何属性。这是因为 Vue 的组件默认都是相互隔离的，不会因为拿到了组件实例就能随意访问其属性，所以 Vue 提供了 `defineExpose()` 宏函数让子组件暴露数据：

```ts
import { ref } from 'vue'

const person = ref({
  id: 0,
  name: 'tsubaki',
  age: 21,
})

// 就像这样
defineExpose({ person })
```

此时如果拿到 Child 的组件实例，就能访问到 person 属性啦。

> 扩展：
>
> - 有关更多 `useTemplateRef()` 的详细信息，请参阅 [`useTemplateRef()` 的官方文档](https://cn.vuejs.org/api/composition-api-helpers.html#usetemplateref)。
> - 有关更多 `defineExpose()` 的详细信息，请参阅 [`defineExpose()` 的官方文档](https://cn.vuejs.org/api/sfc-script-setup.html#defineexpose)。

### 2.9. 组件通信

#### 2.9.1. 父组件向子组件传递数据

假设我们当前正在构建一个博客，那么就需要一个表示博客文章的 `Article` 组件，每篇博客文章的内容肯定是不同的。要实现这样的效果自然而然的就需要向文章 **组件传递数据**，例如文章的标题和内容，这就需要使用到 **props**：

```vue
<script setup lang="ts">
import Article from '@/components/Article.vue'
import { ref } from 'vue'

const article = ref({
  title: '文章标题',
  content: '文章内容',
})
</script>

<template>
<!-- props 实际上是一种特别的属性，可以直接在组件上声明注册 -->
<Article :title="article.title" :content="article.content"></Article>
</template>

<style scoped>

</style>
```

```vue
<script setup lang="ts">
// 然后在组件内部使用 defineProps 宏函数进行接收
const props = defineProps<{
  title: string,
  content: string,
}>()

// 脚本中访问 props 需要依赖于 defineProps 宏函数的返回值
console.log(props.title)
// console.log(title) // 像这样直接访问 title 会报错
</script>

<template>
<!-- 模板则可以直接访问接收的 props -->
<h2>{{ title }}</h2>
<p>{{ content }}</p>
</template>

<style scoped>

</style>
```

> 注意：
>
> - 所有的 props 都遵循着 **单向绑定** 原则，这意味着当 props 因父组件的修改而更新时，子组件会自然而然的接收到最新的 props，所以我们 **不应该在子组件中去更改一个 prop**。
> - props 能传递函数吗？答案是可以的，但不建议。因为传递函数无外乎就是想要子组件在特定时机下能够修改父组件的数据，达成“子传父”的效果，虽然能行，但更推荐使用 **自定义事件** 来完成。
>

> 扩展：有关更多 `defineProps()` 的详细信息，请参阅 [`defineProps()` 的官方文档](https://cn.vuejs.org/api/sfc-script-setup.html#defineprops-defineemits)。

#### 2.9.2. 子组件向父组件传递数据

让我们继续关注 `Article` 组件，我们会发现有时候 `Article` 组件需要和父组件进行交互。比如，告知父组件今日的阅读人数，那么怎么实现这种“子传父”的效果呢？前面我们说过如果想要实现“子传父”，那么使用 **自定义事件** 将会是一个很不错的选择：

```vue
<script setup lang="ts">
import Article from '@/components/Article.vue'
import { ref } from 'vue'

const readers = ref(0)
const addReaders = (value: number) => readers.value += value
</script>

<template>
<div v-show="readers">如下文章的阅读人数：{{ readers }}</div>
<!-- 自定义事件和普通事件（比如点击事件）类似 -->
<Article @add-readers="addReaders"></Article>
</template>

<style scoped>

</style>
```

```vue
<script setup lang="ts">
// 然后在组件内部使用 defineEmits 宏函数进行接收
const emits = defineEmits<{
  addReaders: [value: number],
}>()
</script>

<template>
  <h2>文章标题</h2>
  <p>文章内容</p>
  <!-- 在特定时机触发指定事件即可，这样就实现了“子传父” -->
  <button type="button" @click="emits('addReaders', 1)">阅读完毕</button>
</template>

<style scoped>

</style>
```

> 扩展：有关更多 `defineEmits()` 的详细信息，请参阅 [`defineEmits()` 的官方文档](https://cn.vuejs.org/api/sfc-script-setup.html#defineprops-defineemits)。

#### 2.9.3. 在组件上使用 v-model

在往组件身上写 v-model 之前，我们先来了解一下表单输入元素（如 `input` 标签）是如何利用 v-model 实现双向绑定的呢？一句话总结就是 `v-model = :value + @input`。

> 注意：当 v-model 写在表单输入元素上才是 `v-model = :value + @input`，当 v-model 写在组件上的话就不是这个了。

```vue
<script setup lang="ts">
import { ref } from 'vue'

const uname = ref('tsubaki')
</script>

<template>
<div>{{ uname }}</div>
<input type="text" v-model="uname">
<br> <!-- br 标签上下两行其实是等价的哦 -->
<input type="text" :value="uname" @input="uname = ($event.target as HTMLInputElement).value">
</template>

<style scoped>

</style>
```

> 注意：当 v-model 写在组件上时，相当于 `v-model = :model-value + @update:model-value`，其实和 `v-model = :value + @input` 也挺类似的。

```vue
<script setup lang="ts">
import { ref } from 'vue'
import BeautifulInput from '@/components/BeautifulInput.vue'

const uname = ref('tsubaki')
</script>

<template>
<div>{{ uname }}</div>
<BeautifulInput v-model="uname"></BeautifulInput>
<br> <!-- br 标签上下两行其实是等价的哦 -->
<BeautifulInput :model-value="uname" @update:model-value="value => uname = value"></BeautifulInput>
</template>

<style scoped>

</style>
```

```vue
<script setup lang="ts">
defineProps<{
  modelValue: string,
}>()

const emits = defineEmits<{
  'update:modelValue': [modelValue: string],
}>()
</script>

<template>
<input type="text" :value="modelValue" @input="emits('update:modelValue', ($event.target as HTMLInputElement).value)">
</template>

<style scoped>

</style>
```

但是呢，由于 `:model-value + @update:model-value` 的配合略微有点繁琐，所以 Vue 还提供了 `defineModel()` 函数来简化这些操作（怎么样，够简单嘛）：

```vue
<script setup lang="ts">
const modelValue = defineModel<string>({ required: true })
</script>

<template>
<input type="text" v-model="modelValue">
</template>

<style scoped>

</style>
```

> 有关更多 `defineModel()` 的详细信息，请参阅 [`defineModel()` 的官方文档](https://cn.vuejs.org/api/sfc-script-setup.html#definemodel)。

#### 2.9.4. 后代组件通信（包括子传父）

### 2.10. 插槽

#### 2.10.1. 默认插槽

#### 2.10.2. 命名插槽

#### 2.10.3. 作用域插槽

### 2.11. 生命周期钩子

每个 Vue 组件实例在创建时都需要经历一系列的初始化步骤，比如设置好数据侦听，编译模板，挂载实例到 DOM，以及在数据改变时更新 DOM。在此过程中，它也会运行被称为生命周期钩子的函数，让开发者有机会在特定阶段运行自己的代码。

如下是组件实例的生命周期图表，大概了解一下，以后当作参考即可。

![](./assets/Snipaste_2026-02-25_09-45-26.png)

举例来说，`onMounted` 钩子运行在组件完成初始渲染并创建 DOM 节点后：

```ts
import { onMounted } from 'vue'

onMounted(() => {
  console.log('this App component is now mounted.')
})
```

除 `onMounted` 外，还有其它的钩子会在组件生命周期的不同阶段被调用，有关所有生命周期钩子及其各自用例详情请参阅：[组合式 API：生命周期钩子 | Vue.js](https://cn.vuejs.org/api/composition-api-lifecycle)

## 3. 自定义钩子

自定义钩子，通常指利用 组合式 API 封装的 **可复用逻辑函数**，在 Vue 中更常见的称呼是 **组合式函数**，由于很多开发者受 React 影响，也习惯将其称为 `hook`。其本质上是一种特殊的函数，用于将组件内的状态逻辑提取到独立的函数中，实现 **逻辑的分离和复用**。

下述示例代码使用了 hook 实现了累加功能的逻辑隔离：

```ts
import { ref } from 'vue'

export default () => {
  const num = ref(0)
  const acc = () => num.value++

  return { num, acc }
}
```

```vue
<script setup lang="ts">
import useAcc from '@/hooks/useAcc.ts'

// hook 通常以 use 作为前缀，后接功能的名称
const { num, acc } = useAcc()
</script>

<template>
<button type="button" @click="acc">{{ num }}</button>
</template>

<style scoped>

</style>
```

## 4. 路由

> Vue Router 官网：[Vue Router | Vue.js 的官方路由](https://router.vuejs.org/zh/)

Vue Router 是 Vue 官方的客户端路由解决方案，客户端路由的作用是在单页面应用中将 url 和用户看到的内容绑定起来。当用户在应用中浏览不同页面时，url 会随之更新，但页面不需要从服务器重新加载。

```bash
# 虽然 Vue Router 是 Vue 官方的客户端路由解决方案
# 但却并不是 Vue 的一部分，所以需要额外安装
pnpm add vue-router
```

### 4.1. 路由的简单使用

在使用路由前，我们需要先了解一下 `RouterLink` 和 `RouterView` 组件。

- `RouterLink`：不同于常规的 a 标签，其能够让 Vue Router 在不重新加载页面的情况下改变 url
- `RouterView`：用于告诉 Vue Router 应该在哪里渲染当前 url 对应的 **路由视图组件**

想要使用路由，首先需要创建路由器实例，而路由器实例可以通过 `createRouter()` 函数创建：

```ts
import { createRouter, createWebHistory, type RouteRecordRaw } from 'vue-router'

// routes 定义了一组路由，用于建立 url 和组件的联系
const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    redirect: '/home',
  },
  {
    path: '/home',
    component: () => import('../views/Home.vue'), // 以函数的形式懒加载路由视图
  },
  {
    path: '/about', // 除 path 和 component 外，还有其它可设置的路由选项，后续再详细介绍
    component: () => import('../views/About.vue'),
  },
]

// 创建并导出路由器
export default createRouter({
  history: createWebHistory(), // history 选项用于指定路由器的工作模式
  routes,
})
```

路由器创建完成之后，我们还需要将其注册为 Vue 的插件，这样就能在 Vue 使用路由进行页面的跳转了：

```ts
import { createApp } from 'vue'
import '@/style.css'
import App from '@/App.vue'
import router from '@/router'

// 和大多数的 Vue 插件一样，use() 需要在 mount() 之前调用
createApp(App).use(router).mount('#app')
```

最后再来到根组件，在里面随便写点内容就行了：

```vue
<script setup lang="ts">

</script>

<template>
<RouterLink to="/">Home</RouterLink>
<!-- ps：这种写法其实是为了后续的编程式导航做铺垫 -->
<RouterLink :to="{ path: '/about' }">About</RouterLink>
<br>
<RouterView></RouterView>
</template>

<style scoped>

</style>
```

### 4.2. 命名路由

当定义一个命名路由时，需要设置路由的 `name` 属性：

```ts
// ...
const routes: Array<RouteRecordRaw> = [
  {
    name: 'home', // 顾名思义，给这个路由一个名称
    path: '/home',
    component: () => import('../views/Home.vue'),
  },
]

// ...
```

然后进行路由跳转时就可以使用 name 而不是 path：

```html
<RouterLink :to="{ name: 'home' }">Home</RouterLink>
```

使用 `name` 有很多优点：

- 没有硬编码的 url，能尽可能避免写错 url
- 能够自动解码 `params`（后续会详细讲解）
- 能够绕过路径排序，例如展示一个匹配相同路径但排序较为在后的路由

> 注意：所有的路由 **名称** 都必须是 **唯一** 的，如果为多条路由添加相同的命名，路由器只会保留最后那条路由。

### 4.3. 嵌套路由

当定义一个嵌套路由时，需要设置路由的 `children` 属性：

```ts
// ...
const routes: Array<RouteRecordRaw> = [
  {
    path: '/about',
    component: () => import('../views/About.vue'),
    children: [ // children 也是一个路由数组，就像 routes 本身一样，所以可以根据需要，不断嵌套路由视图
      {
        path: 'other', // 子视图的 path 不能加 /，因为加了 / 的路径将被视为根路径
        component: () => import('../views/Other.vue'),
      },
    ],
  },
]

// ...
```

不要忘了在 `About.vue` 中留一个 `RouterView` 占位：

```vue
<script setup lang="ts">

</script>

<template>
About
<RouterView></RouterView>
</template>

<style scoped>

</style>
```

### 4.4. 路由传参

在讲解路由传参之前，我们还需要了解一下怎么在脚本中获取当前路由和路由器实例。

在组合式 API 中，Vue Router 给我们提供了 `useRoute()` 和 `useRouter()` 这两个 hooks 函数来获取当前路由和路由器实例：

```vue
<script setup lang="ts">
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()
</script>

<template>
<RouterLink to="/">Home</RouterLink>
<RouterLink to="/about">About</RouterLink>
<button type="button" @click="console.log(route)">log route</button>
<button type="button" @click="console.log(router)">log router</button>
<br>
<RouterView></RouterView>
</template>

<style scoped>

</style>
```

#### 4.4.1. query 传参

传递 query 参数：

```html
<RouterLink :to="{
  path: '/about',
  query: {
    id: 0,
    name: 'about',
  },
}">About</RouterLink>
```

接收 query 参数：

```ts
import { useRoute } from 'vue-router'
import { onMounted } from 'vue'

// 接收 query 参数需要依赖于 useRoute() 获取当前路由
const route = useRoute()
onMounted(() => console.log(route.query))
```

#### 4.4.2. params 传参

> 为了更好地了解 params 传参方式，建议有必要阅读一下 [路由的匹配语法 | Vue Router](https://router.vuejs.org/zh/guide/essentials/route-matching-syntax.html)

定义 params 参数：

```ts
// ...
const routes: Array<RouteRecordRaw> = [
  {
    name: 'about', // 使用 params 参数的话必须要配置 name 属性哦，因为无法使用 path 进行跳转
    path: '/about/:id/:name', // 由于 params 参数直接定义在路径中，所以 params 参数的变量名也在这里决定
    component: () => import('../views/About.vue'),
  },
]

// ...
```

传递 params 参数：

```html
<RouterLink :to="{
  name: 'about',
  params: {
    id: 0,
    name: 'about',
  },
}">About</RouterLink>
```

接收 params 参数：

```ts
import { useRoute } from 'vue-router'
import { onMounted } from 'vue'

// 接收 params 参数需要依赖于 useRoute() 获取当前路由
const route = useRoute()
onMounted(() => console.log(route.params))
```

### 4.5. 路由的 props 配置

前文提到过，在组件中不管是获取 query 参数还是 params 参数，都需要使用 `useRoute()` 获取当前路由，这会让组件与路由紧密耦合，限制了组件的灵活性。虽然这不一定是件坏事，但我们可以通过路由的 `props` 配置来解除这种行为。

> props 配置优化的仅仅是 **接收参数** 这一步骤，至于参数该怎么传还是怎么传。

#### 4.5.1. 布尔模式

当 props 为 true 时，路由的 `params` 参数会被设置为组件的 `props`。

```ts
// ...
const routes: Array<RouteRecordRaw> = [
  {
    name: 'about',
    path: '/about/:id/:name',
    component: () => import('../views/About.vue'),
    props: true,
  },
]

// ...
```

```vue
<script setup lang="ts">
defineProps<{
  id: string, // 有点美中不足的就是获取的数据将被转为 string 类型，哪怕传入的是个 number
  name: string,
}>()
</script>

<template>
About
{{ id }} {{ name }}
</template>

<style scoped>

</style>
```

#### 4.5.2. 函数模式

props 也可以是一个函数，其接受 **当前路由** 作为参数，返回值是一个 **对象**，这个对象中的属性将被设置为组件的 `props`。

```ts
// ...
const routes: Array<RouteRecordRaw> = [
  {
    name: 'about',
    path: '/about',
    component: () => import('../views/About.vue'),
    props: route => route.query,
  },
]

// ...
```

```vue
<script setup lang="ts">
defineProps<{
  id: string, // 有点美中不足的就是获取的数据将被转为 string 类型，哪怕传入的是个 number
  name: string,
}>()
</script>

<template>
About
{{ id }} {{ name }}
</template>

<style scoped>

</style>
```

#### 4.5.3. 对象模式

当 props 是一个对象时，和函数模式类似，这个对象中的属性将被设置为组件的 `props`。

```ts
// ...
const routes: Array<RouteRecordRaw> = [
  {
    name: 'about',
    path: '/about',
    component: () => import('../views/About.vue'),
    props: {
      id: 0,
      name: 'about',
    },
  },
]

// ...
```

```vue
<script setup lang="ts">
defineProps<{
  id: string, // 有点美中不足的就是获取的数据其类型默认为 string
  name: string,
}>()
</script>

<template>
About
{{ id }} {{ name }}
</template>

<style scoped>

</style>
```

### 4.6. 编程式导航

上述示例代码中，如果想要跳转路由无一使用的都是 `RouterLink` 组件，而除了使用 `RouterLink` 跳转路由外，我们还可以借助路由器的实例方法，例如 push、replace，通过代码来实现路由跳转。

> 注意：下述示例中的 `router` 均指代路由器实例。在组合式 API 中可以通过调用 `useRouter()` 来获取路由器实例。

在 Vue Router 中，push 和 replace 都是用于导航的核心方法，区别仅在于对 **浏览器历史记录栈** 的控制行为：

- `push` 会向历史栈中 **添加一条新记录**，用户能够通过后退按钮回到之前的页面
- `replace` 会 **直接替换** 历史栈中的 **当前历史条目**，用户无法通过后退按钮回到之前的页面

使用 push 方法跳转路由：

```ts
// 字符串路径
router.push('/users/eduardo')

// 带有路径的对象
router.push({ path: '/users/eduardo' })

// 命名路由，附加 params 参数，让路由建立 url
router.push({ name: 'user', params: { username: 'eduardo' } })

// 附加 query 参数，结果是 /register?plan=private
router.push({ path: '/register', query: { plan: 'private' } })

// 带 hash，结果是 /about#team
router.push({ path: '/about', hash: '#team' })
```

使用 replace 方法跳转路由：

```ts
// 其接收的参数和 push 方法相同
router.replace({ path: '/home' })

// 也可以写成 push 方法
router.push({ path: '/home', replace: true })
```

### 4.7. 重定向

可以配置路由的 `redirect` 属性从而实现重定向：

```ts
// ...
const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    redirect: '/home',
  },
]

// ...
```

## 5. 状态管理

> Pinia 官网：[Pinia | Vue.js 的专属状态管理库](https://pinia.vuejs.org/zh/)

Pinia 是 Vue 的专属状态管理库，它允许我们能够跨组件共享数据。

```bash
# 和 Vue Router 类似，Pinia 同样不是 Vue 的一部分，所以需要额外安装
pnpm add pinia
```

### 5.1. pinia 的简单使用

在使用 pinia 之前，我们需要先了解一下 `store` 是什么？`store` 是一个保存 **共享数据和业务逻辑** 的实体，它并不和组件绑定，也就是说每个组件都可以读取和写入它，了解到这里就够了。

要想在 Vue 中使用 pinia，我们首先需要创建一个 pinia 实例，并将其注册为 Vue 的插件：

```ts
import { createApp } from 'vue'
import '@/style.css'
import App from '@/App.vue'
import { createPinia } from 'pinia'

// 和大多数的 Vue 插件一样，use() 需要在 mount() 之前调用
createApp(App).use(createPinia()).mount('#app')
```

现在我们来定义一个 store 来存储累加功能所需要的数据和方法：

```ts
import { defineStore } from 'pinia'
import { ref } from 'vue'

// 其命名规范和 hooks 类似，都需要以 use 作为前缀
// 但 store 还需要以 store 作为后缀，以便和 hooks 进行区分
export const useCountStore =  defineStore('count', () => {
  let count = ref(0)
  const increment = () => count.value++

  return { count, increment }
})
```

然后在根组件内引入并使用：

```vue
<script setup lang="ts">
import { useCountStore } from '@/stores/count.ts'

// 好像和 hooks 有点类似
const countStore = useCountStore()
</script>

<template>
<button type="button" @click="countStore.increment">{{ countStore.count }}</button>
</template>

<style scoped>

</style>
```

### 5.2. 解构 store

不知道大家发现没有，当我们直接解构 store 时，解构出的数据会丢失响应式，就像最开始解构 reactive 对象一样，那么是否存在一种类似于 `toRefs()` 的函数能解决这个问题呢？答案当然是有的，我们需要使用 `storeToRefs()` 函数为 store 中的每一个响应式数据创建引用，不包括方法，因为方法能解构出来直接用：

```vue
<script setup lang="ts">
import { useCountStore } from '@/stores/count.ts'
import { storeToRefs } from 'pinia'

const countStore = useCountStore()
const { increment } = countStore // 方法可以解构出来直接用
const { count } = storeToRefs(countStore) // 但是数据需要使用 storeToRefs() 函数包裹之后才能解构，不然会丢失响应式
</script>

<template>
<button type="button" @click="count++">{{ count }}</button>
<button type="button" @click="increment">{{ count }}</button>
</template>

<style scoped>

</style>
```
