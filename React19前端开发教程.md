# React19前端开发教程

## 1. 创建 React 项目

```bash
bun create vite . --template react-ts
```

### 1.1. 配置路径别名

#### 1.1.1. vite.config.ts

```ts
export default defineConfig({
  resolve: { tsconfigPaths: true },
})
```

#### 1.1.2. tsconfig.app.json

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["src/*"],
      "@a/*": ["src/assets/*"],
      "@c/*": ["src/components/*"]
    },
  }
}
```

### 1.2. 重写部分文件

#### 1.2.1. index.html

```html
<!doctype html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <meta name="viewport"
        content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <link rel="icon" href="/favicon.svg" type="image/svg+xml">
  <title>Document</title>
</head>
<body>
<div id="root"></div>
<script type="module" src="/src/main.tsx"></script>
</body>
</html>
```

## 2. React 核心语法

### 2.1. tsx 语法

**tsx 是 ts 的 扩展语法**，能够在代码中编写 **类似 html 的代码**：

```tsx
export default () => (<div>hello ya</div>)
```

原理很简单，由于 **tsx 最终会被编译为 ts**，而 `<div>hello ya</div>` 之所以能在 ts 里不报错，是因为其本质上是 **函数调用**（为了简短描述，下文将统称这种函数调用为 **标签**）。

#### 2.1.1. 只能返回一个根标签

当我们试图在 tsx 中返回 **多个标签** 时会直接报错，因为 ts 不支持函数返回多个值，解决方法则是 **使用根标签**（比如 div 标签）**包裹** 需要返回的多个标签：

```tsx
export default () => (<div>
  <div>hello ya</div>
  <div>hello ya</div>
</div>)
```

根标签除了可以是 div 标签外，还可以是 `Fragment` 标签。相比于 div 标签，`Fragment` 标签不会被渲染到界面上（`Fragment` 标签能简写为 `<></>`）：

```tsx
export default () => (<>
  <div>hello ya</div>
  <div>hello ya</div>
</>)
```

#### 2.1.2. 标签必须正确闭合

tsx 要求 **标签必须正确闭合**，哪怕是单标签：

```tsx
export default () => (<img src="/favicon.svg" alt="cover"/>)
```

#### 2.1.3. 使用小驼峰命名法命名标签属性

由于 **tsx 最终会被编译为 ts**，而 tsx 中标签的属性也会变成 ts 对象中的键值对，所以为了命名规范，标签的 **大部分** 属性都需要使用 **小驼峰命名法** 表示（比如 `onclick` 需要使用 `onClick` 代替）：

```tsx
const handleClick = () => console.log('用户登录成功')

// onClick 涉及到了事件响应相关知识，这里只需要知道 onclick 需要使用 onClick 替代即可
export default () => (<button type="button" onClick={handleClick}>login</button>)
```

由于 `class` 属性和 ts 里的关键字重名，所以需要使用 `className` 代替（一般来说不需要特别记忆，因为编辑器会自动提示对应的替代项）：

```tsx
export default () => (<img src="/favicon.svg" alt="cover" className="cover"/>)
```



> [!Warning]
>
> 出于历史原因，`aria-*` 和 `data-*` 属性是以带 `-` 字符的 html 属性格式书写的，这也是为什么上文说的是 **大部分** 而不是 **所有**。

#### 2.1.4. 通过 {} 使用 ts

**tsx 允许在 {} 中使用 ts**，这代表着我们能在 **标签中引用变量作为标签属性** 或在 **标签内添加 ts 逻辑**（不管是引用变量作为标签属性还是添加 ts 逻辑，`{}` 中的 ts **必须也只能计算出一个值**）：

##### 2.1.4.1. 引用变量作为标签属性

```tsx
export default () => {
  return (<div style={{
    width: '320px',
    height: '320px',
    background: 'skyblue',
  }} className="box">
  </div>)
}
```

需要注意的是，`className` 属性只是一个单纯的字符串，因为没有使用 `{}`。

##### 2.1.4.2. 添加 ts 逻辑

```tsx
export default () => {
  return (<div>{1 + 1}</div>)
}
```

在标签内部的 `{}` 中能添加任何 **能够计算出值的表达式** 或一个单纯的 **值**（这个值不能是一个对象，会触发类型错误），同时这个值也会 **被渲染到界面上进行显示**。

### 2.2. 组件的简单使用

在 React 中，**组件是一段返回标签的函数**，从这个角度看，我们之前写的 tsx 代码其实就是一个组件：

```tsx
export default () => (<div>hello ya</div>)
```

但这种写法并不推荐，因为组件没有名字（或者说组件的名字是 `default`），所以我们更推荐书写这种 **具名组件**：

```tsx
// 函数的名称就是组件的名称
// 组件名称必须以大写字母开头，否则 React 无法区分组件和浏览器原生标签
const Component = () => (<div>hello world</div>)

// 组件定义好后记得导出（导出方式随意，但如果是根组件的话则必须使用默认导出）
export default Component
```

#### 2.2.1. 组件的导入

组件定义好后就能导入使用了：

```tsx
// 导入方式随意，选择与组件导出方式对应的导入方式即可
import Component from '@c/Component.tsx'

// 组件导入后直接当标签用
const App = () => (<Component/>)

// 依旧不要忘记导出
export default App
```

#### 2.2.3. 为组件传递 props

React 通过 `props` 实现 **父子组件间的数据传递**。假设我们现在正在开发一个文章组件，而文章组件的标题和内容需要通过父组件传递：

```tsx
// 由于我们使用的是 tsx，所以需要为 props 定义类型
interface ArticleProps {
  title: string,
  content: string,
}

// 组件的 props 在函数的形参处声明
const Article = ({ title, content }: ArticleProps) => (<div>
  <h2>{title}</h2>
  <p>{content}</p>
</div>)

// 依旧不要忘记导出
export default Article
```

然后导入并使用文章组件：

```tsx
import Article from '@c/Article.tsx'

// 随便整条文章数据
const article = { title: '文章标题', content: '文章内容' }

const App = () => {
  return (<>
    {/* 借助提前声明的 props，我们就可以愉快的向文章组件传递数据了 */}
    <Article title={article.title} content={article.content}/>
  </>)
}

// 依旧不要忘记导出
export default App
```

##### 2.2.3.1. 使用展开语法传递 props

当 props 过多时，一个一个的传递显然不太优雅，所以 tsx 还提供了一种 **展开语法** 用于传递 props：

```tsx
const App = () => (<Article {...article}/>)
```

> [!Warning]
>
> 使用 **展开语法** 传递 props 需要确保 **prop 名和对象字段名一致**。

##### 2.2.3.2. 为 prop 设定默认值

如果父组件 **没有传递** 某个 prop 的话，也可以通过 **设定默认值** 来让该 prop 有值：

```tsx
interface ArticleProps {
  // ...
  date?: string, // 缺省 prop 建议加个 ? 修饰，不然父组件那边会报错（虽然不影响运行）
}

// 缺省 prop 在解构时可以使用 = 给个默认值
const Article = ({ /* ... */ date = '2026-03-24' }: ArticleProps) => (<div>
  {/* ... */}
  <span>{date}</span>
</div>)
```

> [!Warning]
>
> 默认值仅在 **props 未传递** 或 **传递 undefined** 时才生效，哪怕手动传递了 null，默认值都不会被使用。

##### 2.2.3.3. 将组件作为 props 传递

props 不仅 **能传递数据**，甚至还 **能传递标签或组件**，当我们使用 props 传递标签或组件时，必须使用名为 **children** 的 prop，其类型为 **ReactElement**：

```tsx
import Article from '@c/Article.tsx'

const App = () => {
  return (<>
    <Article>
      <div>1 号文章标题</div>
    </Article>
    <Article>
      {/* 当有多个标签时，需要用一个根标签包裹内部标签 */}
      <div>
        <div>2 号文章标题</div>
        <div>2 号文章内容</div>
      </div>
    </Article>
  </>)
}

// 依旧不要忘记导出
export default App
```

```tsx
import type { ReactElement } from 'react'

interface ArticleProps {
  children: ReactElement,
}

const Article = ({ children }: ArticleProps) => (children)

// 依旧不要忘记导出
export default Article
```

### 2.3. 条件渲染

根据 **不同的情况显示不同的内容**，这就是条件渲染：

```tsx
const App = () => {
  return (<>
    {Date.now() % 2 ? <div>奇数</div> : <div>偶数</div>}
  </>)
}

// 依旧不要忘记导出
export default App
```

条件渲染的本质就是 **根据不同的条件返回不同的标签**，所以只要根据需要，任意选择 if、switch、&& 或者三元运算符等分支语句即可。

### 2.4. 循环渲染

将 **一组数据转化为一组标签**，这就是循环渲染：

```tsx
// 这种静态数据建议放在组件外面
const list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

const App = () => {
  return (<>
    <ul>
      {/* 数据到标签的转换一般都是通过 map 方法实现的 */}
      {list.map(item => <li key={item}>{item}</li>)}
    </ul>
  </>)
}

// 依旧不要忘记导出
export default App
```

需要注意的是，我们必须给 **循环渲染的标签或组件** 指定一个 **唯一的 key**，这些 key 会告诉 React 每个标签或组件对应着这组数据中的哪一个数据。当这组中的数据在进行移动（如排序）、插入或删除等操作时，key 可以帮助 React **正确的更新 DOM 树**。

### 2.5. 事件响应











































