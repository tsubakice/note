![image-20260208011656241](https://s2.loli.net/2026/02/08/2IXikvMBz6qjWUY.png)

# JavaScript进阶部分

前面我们为大家介绍了JS的函数和面向对象，我们知道在JS中，万物皆对象，随便什么类型都可以当做对象使用，本章我们将承接上文，继续介绍JavaScript的高级特性部分。

## 函数进阶

在讲解完对象之后，我们接着回顾之前的函数部分，介绍更多有关函数的高级特性。

### 函数的注释

函数的注释往往可以为函数增加更多辅助信息，我们先来看一个没有注释的函数：

```js
function fn(a, b) {
    return a * b / 2
}
```

你能看出来它是干什么的吗？也许是求三角形面积？也可能是别的逻辑。

此时我们可以考虑为函数添加注释，来介绍这个函数是做什么的。在 JavaScript 中，最常见、也最推荐的函数注释写法叫做 **JSDoc 风格**，特点是：

- 使用 `/** ... */`
- 每一行以 `*` 开头
- 可以被编辑器自动识别（VS Code/WebStorm 会有提示）

我们来尝试添加一个看看：

```js
/**
 * 计算三角形的面积
 */
function fn(a, b) {
    return a * b / 2
}
```

当我们将鼠标移动到函数名称上时，就会自动显示它的注释信息，这样就很直观了：

![image-20260208142122819](https://s2.loli.net/2026/02/08/ChRztiGexf8Lv5D.png)

除此之外，我们还可以针对所有的参数进行描述，比如这里的`a`和`b`是做什么的，需要什么类型的数据：

```js
/**
 * 计算三角形的面积
 * @param {number} a 底边长度
 * @param {number} b 高度
 */
function fn(a, b) {
    return a * b / 2
}
```

![image-20260208154603815](https://s2.loli.net/2026/02/08/jQBLumEwT8doN5O.png)

使用`@param`来为函数参数进行细致化描述，后续可以在花括号中添加函数参数的类型。我们为函数的参数增加了特殊注释之后，用户在使用时，得到函数参数的信息会更加详细。

同样的，针对函数的返回值，我们也可以进行细致化描述：

```js
/**
 * 计算三角形的面积
 * @param {string} a 底边长度
 * @param {number} b 高度
 * @returns {number} 三角形面积
 */
function fn(a, b) {
    return a * b / 2
}
```

使用`@returns`来为返回值添加描述，通过这种方式也可以明确返回值的类型。

我们还可以为用户添加一个示例用法，使用`@example`即可：

```js
/**
 * 计算三角形的面积
 * @param {string} a 底边长度
 * @param {number} b 高度
 * @returns {number} 三角形面积
 * @example
 * fn(4, 2) // 4
 */
```

### 即时调用函数

在前面的学习中，我们已经掌握了**函数的定义和调用**，但有时候我们会遇到这样一种需求：

>  我只想执行一次代码，执行完就“消失”，不污染外部环境

这时，就轮到我们这一节的主角登场了：**即时调用函数（Immediately Invoked Function Expression，简称 IIFE）**，它可以实现函数在定义完成的“同一时间”就立刻执行。

要定义这样的函数也非常简单，首先我们还是正常编写一个函数：

```js
function hello() {
    console.log("Hello World!")
}
```

接着我们在这个函数的外部套上括号，并在后面添加括号和实际参数：

```js
(function hello(text) {
    console.log(`Hello World! ${text}`)
})("You")  //这里已经在调用了，就像急急国王一样
```

这样，一个即时调用函数就创建好了，在函数创建的之后就立即调用。一般情况下，这种函数不会再去其他地方调用了，所以我们可以直接把它写成匿名函数形式：

```js
(function (text) {
    console.log(`Hello World! ${text}`)
})("You")
```

不过，这种调用方式一般用于解决ES6之前的`var`作用域问题，在有了`let`和`const`之后，这种用法实际上很少了，这里仅做了解即可。

### 剩余参数

在之前的章节中，我们学习了如何给函数传递固定数量的参数。但在实际开发中，我们经常会遇到这样的情况：**我不确定用户到底会传多少个参数进来**。

最典型的例子就是我们之前用过的`console.log`，我们发现可以传入任意个数的参数进去：

```js
console.log("A", "B", "C")
```

但是通过前面的学习我们知道，函数的参数数量是定义的时候确定，那么这种效果要怎么才能实现呢？比如，我们要写一个求和函数 `sum`，它可能需要计算 2 个数的和，也可能需要计算 10 个数的和。如果用之前的方法，我们只能死板地定义 `sum(a, b, c...)`，这显然不够灵活。

实际上，在调用函数时，即使形参列表里面没有任何参数，也可以传递实参

```js
test("A", "B", "C")  //在调用函数时，即使形参列表里面没有任何参数，也可以传递实参
```

那么这些实际参数怎么获取到呢？在函数对象上，还有一个属性`arguments`，这个属性包含了在实际调用时所有传入的参数，并且我们可以在函数内部使用它：

```js
function test() {
    console.log(arguments);  //arguments就是实际参数列表
}
```

这个属性类似于我们前面讲解的数组的结构（但不是数组，因为它没有数组具有的很多方法）我们可以通过下标来访问里面的元素：

```js
function test() {
    console.log(arguments[0])
}
```

我们也可以使用`arguments.length`来获取传入了多少个参数：

```js
console.log(arguments.length)
```

这样，我们无论传入多少个参数，都可以通过它来快速获取了。虽然这种方式非常简单，但是它并不直观，因为我们并没有定义任何的形参，为了让函数的定义更加明确，在 ES6 之后引入了**剩余参数**语法：

```js
function test(...args) {  //剩余参数通过在参数名前面加上三个点，表示这里可以接受多个参数
    console.log(args)
}

test(1, 2, 4, 5)  //这样就可以选择传入0-N个参数了
```

同时，这种参数得到的结果是一个数组类型的值：

![image-20260128205533041](https://s2.loli.net/2026/01/28/cBWniaeUml8SqwJ.png)

由于是数组类型，我们可以对这些实际参数进行各种操作：

![image-20260208171719284](https://s2.loli.net/2026/02/08/fXSJ6t1YHZWpaIv.png)

注意，当函数存在其他参数时，剩余参数只能位于最后一位且只能出现一次，否则会出现歧义：

```js
function test(text, ...args) {  //剩余参数前面可以出现任意多的单参数
}
```

在ES6 之后，建议各位小伙伴**优先使用剩余参数，而不是 arguments**，它使用起来更加简单快捷，定义更明确。

### 箭头函数

在 ES6 中，引入了一种全新的函数写法：**箭头函数（Arrow Function）**它的语法更简洁，你可以将他看做是函数的一种替代写法。普通函数我们已经很熟悉了：

```js
function add(a, b) {
    return a + b
}
```

而现在，我们有了箭头函数，可以像这样写：

```js
//使用一个变量来代表箭头函数，就像之前的匿名函数一样
const add = (a, b) => {
    return a + b
}
```

箭头函数去掉了 `function`关键字，在参数和函数体之间，使用 `=>`进行连接，看起来更加简洁大气。当然，箭头函数本质上和函数是一样的，都可以正常调用：

```js
const add = (a, b) => {
    return a + b
}
console.log(add(3, 8))
```

特别的，当函数有且只有一个参数时，可以省略参数列表的小括号：

```js
const square = x => {  //x周围的小括号被省略了
    return x * x
}
```

特别的，当函数体只有一行 `return` 时，还可以省略大括号和 `return`：

```js
const square = x => x * x  //省略大括号时，箭头后面的表达式结果会自动作为返回值
```

是不是感觉写起来很舒服？把我们之前臃肿的函数定义大大简化了。所以，大部分情况下，我们更建议各位小伙伴采用这种新的函数语法，它可以让我们写起来更加简洁优雅。

当然，这里也有很多箭头函数需要注意的点，首先就是如果我们返回一个对象：

```js
const fn = () => { name: "Tom" }  //错误的
```

虽然我们前面说了如果只有一行代码可以简化，但是在这种情况下返回一个对象会存在歧义，因为对象也是使用花括号作为其对象结构的声明，所以 `{}` 会被当成函数体，而不是对象。正确写法是：

```js
const fn = () => ({ name: "Tom" })
```

然后，箭头函数中也没有`arguments`，它不支持使用，我们只能选择使用剩余参数：

![image-20260209000321035](https://s2.loli.net/2026/02/09/vBd6eiG1bNfsFtw.png)

接着，也是箭头函数最最重要的一点，**箭头函数没有自己的 this**，我们先来看普通函数中 `this` 的表现：

```js
const obj = {
    name: "小明",
    say() {
        console.log(this.name)
    }
}
obj.say()  // 小明
```

这里的 `this` 指向调用它的对象 `obj`，没问题。但是如果我们把方法改成箭头函数：

```js
const obj = {
    name: "小明",
    say: () => {
        console.log(this.name)
    }
}
obj.say()  //undefined
```

你会发现，这个函数的`this`作用域并不是对象本身，即使我们箭头函数是写在对象内的。箭头函数的 `this` **不会指向调用它的对象**，而是等于它“外层最近的非箭头函数”的 `this`（也可以说，箭头函数 **没有自己的 this**，它只是“借用”外层作用域的 this）所以这里会向外层查找 this ，在大多数情况下，外层就是全局作用域（浏览器中是 `window`）这跟我们之前直接在最外层的函数中使用`this`是一样的效果，实际上：

* 普通函数的 `this` 是 **调用时决定的**，如果调用时作用域不在对应位置，则会出现问题
* 箭头函数的 `this` 是 **定义时决定的**，定义时的作用域是什么，后续即使传递也不会更改，甚至使用前面介绍的`bind`、`apply`和`call`也无法改变

不管怎么调用，箭头函数的 this 都不会变，它只会指向在定义时确定的全局作用域中的对象：

```js
const fn = () => {
    console.log(this)
}

fn()          // window
obj.fn = fn
obj.fn()      // 仍然是 window，无论在哪里都不会被修改
```

当然，这并不代表箭头函数就没有作用，在很多情况下，如果我们不需要使用`this`，那么它还是非常好用的，比如：

```js
const arr = [1, 2, 3]
const result = arr.map(x => x * 2)  //使用前面讲解的的转换操作
console.log(result)
```

```js
const arr = [1, 2, 3]
arr.forEach(x => console.log(x))  //依次打印元素
```

这种写法在现代 JS 中几乎是标配，我们在后续的课程中遇到回调函数，我们一律使用箭头函数进行讲解。

### 解构语法

在前面的学习中，我们已经非常熟悉 **数组** 和 **对象** 的使用了，但在使用它们的时候，你可能经常会写出这样的代码：

```js
const arr = [10, 20, 30]

const a = arr[0]
const b = arr[1]
const c = arr[2]
```

虽然代码本身没有任何问题，但你会发现写法是不是有点啰嗦，如果元素很多，就会很麻烦。但其实，这种操作本质上只是“拆包取值”的行为。包括对象也是类似的情况：

```js
const person = {
    name: "小明",
    age: 18
}

const name = person.name
const age = person.age
```

有没有一种方式，可以**一次性把结构拆开来用**？这正是我们这节课介绍的 **解构语法（Destructuring）** 要解决的问题。它不是新类型，也不是函数，而是一种**语法糖**，用来让代码写得更简洁、更清晰，它也是ES6新增的语法。解构语法主要分为两种：

- **数组解构**
- **对象解构**

数组解构是**按位置**来匹配的，在定义变量时，可以将多个变量使用`[]`囊括，后面直接使用要被解构的数组进行赋值：

```js
const arr = [10, 20, 30, 40]
const [a, b, c] = arr   //在定义变量时，可以将多个变量使用[]囊括
console.log(a, b, c) // 10 20 30
```

接着，数组中的值会按照变量在`[]`中的顺序进行依次赋值，这里就得到了数组中前三个元素的值。可能会有小伙伴说，那我只想要中间的某些值怎么办：

```js
const arr = [10, 20, 30, 40]
const [, b, c] = arr   //需要跳过的变量，直接不写就行了，直接加逗号
console.log(b, c) // 20 30
```

需要跳过的变量，不写就行了，直接加逗号即可，这里逗号的作用是：**占位但不取值**。这里需要注意的是，如果数组长度不够，解构出来的值会是 `undefined`：

```js
const arr = [10]
const [a, b] = arr
console.log(b) // undefined
```

当然，为了能够在这种情况进行补救， 我们可以为解构语法中的变量设置默认值：

```js
const [a, b = 100] = arr
console.log(b) // 100
```

当对应位置没有值时，才会使用默认值。解构也可以和剩余参数一起使用：

```js
const arr = [1, 2, 3, 4]
const [a, ...rest] = arr  //...rest表示剩余的元素，和剩余参数一样，数组形式
console.log(a)    // 1
console.log(rest) // [2, 3, 4]
```

接下来我们来看看对象的结构如何使用，我们可以使用`{}`来进行解构，注意对象解构是按“属性名”，而不是按顺序，我们在解构列表中写的变量名字必须与对象中的保持一致：

```js
const person = {
    name: "小明",
    age: 18
}

const { name, age } = person  //使用{}来进行对象解构，顺序无所谓，可以不按顺序来
console.log(name, age)
```

如果对象中不存在该属性，那么会得到一个`undefined`作为解构变量的值。

如果你不想用原来的属性名，或是对象的某些属性与外部的某个变量名称发生了冲突，也可以在解构时重命名：

```js
const person = {
    name: "小明",
    age: 18
}

const { name: userName, age } = person  //使用冒号来重命名结构的属性
console.log(userName, age)
```

这里的含义是：`name` 属性，赋值给变量 `userName`（重新起的新名字），同样的，对象解构同样支持默认值：

```js
const person = {
    name: "小明"
}

const { name, age = 18 } = person
console.log(age) // 18
```

注意，当对象中不存在该属性时，默认值才会生效，注意，对象中存在这个属性是`undefined`，也会被视为不存在。

解构和剩余参数一起使用，在对象中同样适用：

```js
const obj = {
    name: "小明",
    age: 18,
    city: "北京"
}

const { name, ...others } = obj
console.log(others) // { age: 18, city: "北京" }
```

学习了数组和对象的结构语法之后，我们最后再来看看函数中如何使用解构语法，这是解构语法**最常用、最实用**的地方之一，这是一个很普通的函数：

```js
function printUser(user) {
    console.log(user.name)
    console.log(user.age)
}
```

在使用对象解构后：

```js
function printUser({ name, age }) {  //直接将形参写成结构后的样子
    console.log(name)
    console.log(age)
}
printUser(person)  //这里省略对象内的属性
```

我们可以直接将形式参数写为解构后的样子，这样，参数结构一目了然，函数内部更简洁，最主要的是能少写很多 `.`运算符，注意，如果存在多个形式参数，也可以一起使用：

```js
function printUser({ name, age }, text, { type }) {  //多种写法混合也可以的
    console.log(name)
    console.log(age)
}
```

```js
printUser(person, "666", anything)
```

函数参数的结构也可以使用默认值，因为之前已经讲解过，这里就不做演示了。

### 展开运算符

在上一节我们学习了解构语法，其中有一个符号你一定已经见过了：`...`，在解构中它表示 **“剩余”**，而在这一节，它有一个新的名字和用途：**展开运算符（Spread Operator）**虽然写法一样，但**使用位置不同，含义也不同**。

我们先来看一个不用展开运算符的写法，假设我们现在想要合并两个数组，我们可以使用`concat`方法来实现：

```js
const arr1 = [1, 2, 3]
const arr2 = [4, 5]   //将第一个数组直接装到第二个数组中
const arr3 = arr1.concat(arr2)
console.log(arr3)  // [1, 2, 3, 4, 5]
```

可能各位小伙伴会觉得这样写没毛病啊，那再比如，我们现在需要合并四个数组：

```js
const arr1 = [1, 2, 3]
const arr2 = [4, 5]
const arr3 = [6, 7]
const arr4 = [8, 9]
const arr5 = arr1.concat(arr2).concat(arr3).concat(arr4)
console.log(arr5)  // [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

大家有没有发现，再这样下去，`concat`都快变成贪吃蛇了。ES6之后，我们可以使用展开运算符来快速实现这样的多合一操作，展开运算符只需要对着数组使用`...`即可：

```js
const arr5 = [...arr1, ...arr2, ...arr3, ...arr4]
```

此时，`...arr1` 会把数组拆成 `1, 2, 3`，再按顺序放入新数组中，得益于这种特性，展开运算符可以非常自然地合并多个数组，相比以前使用 `concat`，这种写法更直观、更灵活、顺序一眼就能看明白。除此之外，很多新手会踩这样一个坑：

```js
const arr1 = [1, 2, 3]
const arr2 = arr1

arr2.push(4)
console.log(arr1) // [1, 2, 3, 4]
```

这是因为数组是引用类型，它本质上也是一个对象，简单的赋值使得`arr2` 和 `arr1` 指向同一个地址。如果我们想要**创建一个新数组**，可以使用展开运算符：

```js
const arr1 = [1, 2, 3]
const arr2 = [...arr1]   //展开运算符会展开原数组所有内容

arr2.push(4)
console.log(arr1) // [1, 2, 3]
```

展开运算符会展开原数组所有内容，并直接放到新声明的数组中。注意这也是一种**浅拷贝**，因为结构出来的值还是原来数组中的，但在日常开发中非常常用。

在ES2018之后，展开运算符不仅可以用于数组，也可以用于对象：

```js
const obj1 = {
    name: "小明",
    age: 18
}

const obj2 = {
    ...obj1,   //直接获得对象1中定义的全部属性
    city: "北京"
}

console.log(obj2)
```

对对象使用`...`的效果是，把对象的属性一个个展开并放进新对象中，这和数组比较类似。不过需要注意的是，当属性名重复时，**后面的会覆盖前面的**：

```js
const base = { a: 1, b: 2 }
const extra = { b: 100, c: 3 }

const result = { ...base, ...extra }
console.log(result)
// { a: 1, b: 100, c: 3 }
```

当我们需要合并多个对象时，使用展开运算符就是一种非常推荐的做法。和数组一样，对象展开也可以用来拷贝对象：

```js
const obj1 = { name: "小明" }
const obj2 = { ...obj1 }

obj2.name = "小红"
console.log(obj1.name) // 小明
```

还是那个问题，只拷贝属性的值本身，它是浅拷贝。

最后需要提及的是，展开运算符还可以用在**函数调用时**，比如某个函数需要两个参数，我们可以直接让数组解构：

```js
const arr = [5, 8, 16]
function sum(a, b) {
    return a + b
}

console.log(sum(...arr))   //数组展开后，会自动使用其中的元素作为实际参数传入
```

数组展开后，会自动使用其中的元素作为实际参数传入，注意，超出形参长度的部分也会被一并作为后续参数传入，我们可以使用`arguments`来验证：

![image-20260209000433384](https://s2.loli.net/2026/02/09/7dFReTtwmUy2HnX.png)

需要注意的是，这种用法仅限于数组，对象即使可以使用展开运算符也是不能作为参数使用的。

### 标签模版

标签模板 (Tagged Templates)这是一种更高级的用法，你可以通过一个函数来“解析”模板字符串。常用于防止 XSS 攻击或国际化处理。

在前面的章节中，我们已经学习过 **模板字符串**，也就是使用反引号包裹的字符串，比如：

```js
const name = "小明"
const age = 18

const str = `我叫${name}，今年${age}岁`
console.log(str)
```

其中字符串部分就是正常编写的字符串内容，而`${}`就是进行拼接的插值表达式。

模板字符串最大的好处是：**可以在字符串中直接嵌入变量或表达式**，不用再拼接 `+`，写起来非常直观。所谓“标签模板”，本质上就是，用一个函数，来“接管”模板字符串的解析过程，它的写法看起来有点特别，我们先直接看一个最简单的例子：

```js
function tag(strs, value) {
    console.log(strs)
    console.log(value)
}
```

这里，我们为函数定义一个变量`strs`来接收字符串，下一个变量`value`来接收插值表达式的结果。需要调用也非常简单，我们只需要直接使用函数名称拼接模版字符串即可：

```js
const name = "小明"
tag`你好，${name}`
```

接着，我们就可以在函数中得到模版字符串拆分的结果：

![image-20260210012337373](https://files.seeusercontent.com/2026/02/11/0eNj/image-20260210012337373.png)

字符串会按照插值表达式的位置进行自动分割，得到一个字符串数组的结果，而所有的插值表达式将作为后续变量传入。由于存在字符串分割，所以`strs`的长度永远比插值表达式多`1`。

实际上，我们在上一章已经接触过标签模版相关的函数，String提供的`raw`函数可以实现对转译字符的忽略效果：

```js
console.log(String.raw`你好，${name} 我爱你 \n 你牛逼`);
```

我们可以自行编写处理，比如我们可以对插值表达式的结果进行国际化展示：

```js
function format(strs, ...values) {
    return strs.reduce((res, str, i) => {
        const value = values[i]
        return res + str + (typeof value === "number" ? value.toFixed(2) : value)
    }, "")
}

const price = 12.3456
const msg = format`商品价格：${price} 元`
console.log(msg)
```

实际上，很多框架也是使用的标签模版实现的自定义DSL，当然本质上，它就是通过标签模板，**把字符串当成结构化数据来解析**。

### 生成器

在前面的学习中，我们已经掌握了箭头函数、解构、展开等特性，但你可能会发现一个问题：**普通函数，要么不执行，要么一次性执行完**，这里来看一个最简单的函数：

```js
function fn() {
    console.log(1)
    console.log(2)
    console.log(3)
}

fn()
```

函数一旦调用，里面的代码会**从上到下全部执行完毕**，中间没有任何“暂停”的机会。那有没有一种函数，可以实现在执行过程暂停呢？下次我们可以从暂停的位置继续执行，这正是ES6中新增的 **生成器（Generator）**的作用。

生成器的写法和普通函数非常像，我们需要在`function`后面添加一个`*`表示这是一个生成器：

```js
function* gen() {
  
}
```

接着，我们可以在生成器中设定不同的阶段：

```js
function* gen() {
    console.log("我是第一阶段")
    console.log("我是第二阶段")
    console.log("我是第三阶段")
}
gen()
```

我们可以来尝试调用一下这个函数，你会发现，这个函数并没有执行。因为调用这个函数返回的其实是一个 **生成器对象**，默认情况下是暂停状态，我们需要对它进行推进才可以正常执行，必须调用 `next()`：

```js
const generator = gen()
generator.next()
```

可以看到，当我们调用`next`之后，函数才真正执行了，不过，这里三个阶段一起执行完成了，如果我们希望一个一个阶段执行，那么可以使用`yield`关键字：

```js
function* gen() {
    console.log("我是第一阶段")
    yield   //当遇到yield时，表示函数已经执行完一个阶段
    console.log("我是第二阶段")
    yield
    console.log("我是第三阶段")
}

const generator = gen()
generator.next()
```

当遇到yield时，表示函数已经执行完一个阶段，此时函数会再次进入暂停状态，等待我们下一次的`next`调用。当我们连续进行三次`next`调用时，函数才能正确执行完所有内容：

```js
const generator = gen()
generator.next()
generator.next()
generator.next()
```

这就是生成器的作用，它可以将任务分段，从而实现阶段性推进执行函数。实际上`next`的执行是有返回值的，我们可以来观察一下：

```js
const generator = gen()
console.log(generator.next());
```

![image-20260209001707705](https://s2.loli.net/2026/02/09/gwrFaBqnCYm5iAo.png)

每完成一个阶段，都会得到当前阶段的执行结果，其中包含这个阶段的返回值，以及是否完成。其中，阶段的返回值由`yield`来指定：

```js
function* gen() {
    console.log("我是第一阶段")
    yield 233
    console.log("我是第二阶段")
    yield 666
    console.log("我是第三阶段")
}

const generator = gen()
console.log(generator.next());  //{value: 233, done: false}
console.log(generator.next());  //{value: 666, done: false}
console.log(generator.next());  //{value: undefined, done: true}
```

注意如果最后没有返回值，那么得到的的`value`就是`undefined`，如果有的话，则返回值作为`value`。正是得益于这种性质，生成器非常适合生成“无限或大型序列”，例如生成一个递增数字：

```js
function* counter() {
    let i = 0
    while (true) {
        yield i++
    }
}

const c = counter()
c.next().value // 0
c.next().value // 1
c.next().value // 2
```

需要注意的是，生成器**并没有一次性生成所有数字，而是用一个给一个**。

除了我们手动调用`next`来获取下一个之外，我们也可以使用`for`循环来一次性执行所有的阶段：

```js
const generator = gen()
for (let value of generator) {  //这里的value就是每一个阶段的执行结果
    console.log(value)
}
```

就像对数组进行遍历一样，我们可以使用`for`来遍历执行每一个阶段，并且这里的`value`实际上就是每一个阶段执行完成之后得到的结果，循环会在所有阶段执行完成之后自动结束。

## 对象进阶

在上一章，我们为大家介绍了对象的相关特性，包括对象的使用、引用类型以及原型链的概念，这一章，我们将继续上一章的内容，为大家介绍更多关于对象的进阶内容。

### 属性的继承*

在现实世界中，**继承**是一个非常常见的概念，比如：

- 学生 **是** 人
- 老师 **是** 人
- 狗 **是** 动物

它们都有一些**共同特征**，但又各自拥有不同的能力，比如：

```js
function Student(name, age) {   //普通学生
    this.name = name
    this.age = age
}

function ArtStudent(name, age, level) {   //美术生
    this.name = name
    this.age = age
    this.level = level
}
```

这里我们定义了两种不同的学生，一种是普通学生，还有一种是美术生，其中，普通学生具有名字和年龄属性，而美术生不仅具有普通学生的名字年龄属性，而且还额外多了一个等级属性。

你会发现，两种构造函数存在大量重复代码，这显然不优雅，也不利于维护，此时我们就需要考虑继承关系了。在 JavaScript 中，由于并不存在像 Java、C++ 那样“真正的类型继承”，**JS 的继承本质上是：对象之间通过原型链共享属性和方法**。

我们可以让 **美术生的对象，通过原型链，去访问普通学生对象中的属性**。在 JavaScript 中，实现这一点的核心方式，就是让子构造函数的 `prototype` 指向父构造函数创建的对象。我们先来看最基础的写法：

```js
function Student(name, age) {
    this.name = name
    this.age = age
}

function ArtStudent(name, age, level) {
    this.level = level
}
```

接着，我们需要建立 **原型继承关系**，首先我们让 `ArtStudent` 的原型指向 `Student` 的一个实例对象：

```js
ArtStudent.prototype = new Student()
```

这样一来，通过 `ArtStudent` 创建出来的对象，在自身找不到属性时，就会沿着原型链，去 `Student` 的实例对象中查找。此时再次测试：

```js
const a = new ArtStudent("小明", 18, "高级")
console.log(a.name) // undefined
```

你会发现，结果不对。这是因为，虽然我们建立了原型链关系，但 `Student` 构造函数并没有被执行，`name` 和 `age` 并没有真正初始化到当前对象中。

为了解决这个问题，我们需要在子构造函数中，**借用父构造函数来初始化属性**。可以通过 `call` 来完成：

```js
function ArtStudent(name, age, level) {
    Student.call(this, name, age)
    this.level = level
}
```

这样，在创建 `ArtStudent` 对象时，由于我们通过`call`手动指定`this`指代的值，所以`Student` 构造函数会在当前对象的作用域中执行，从而为对象添加 `name` 和 `age` 属性。

此时我们再次创建对象：

```js
const a = new ArtStudent("小明", 18, "高级")

console.log(a.name)   // 小明
console.log(a.age)    // 18
console.log(a.level)  // 高级
```

可以看到，美术生对象已经成功继承了普通学生的属性。不过，这里还存在一个小问题。我们来看一下 `constructor` 的指向：

```js
a.constructor === ArtStudent // false
a.constructor === Student    // true
```

这是因为我们直接重写了 `ArtStudent.prototype`，导致其 `constructor` 指向发生了变化。为了解决这个问题，需要手动修正 `constructor`：

```js
ArtStudent.prototype.constructor = ArtStudent
```

到这里，我们已经完成了**属性继承的基本实现**。

### 类的声明与使用*

在前面的内容中，我们已经通过 **构造函数 + 原型链** 的方式，实现了对象的创建以及属性的继承。这种方式在 JavaScript 中是完全合法、也是长期以来的主流写法，但你可能已经发现了一个问题：

> 这种写法偏复杂，不直观，也不太符合大多数人对“类”和“继承”的直觉理解。

例如，我们前面实现一个继承关系时，需要写出类似这样的代码：

```js
function Student(name, age) {
    this.name = name
    this.age = age
}

function ArtStudent(name, age, level) {
    Student.call(this, name, age)
    this.level = level
}

ArtStudent.prototype = new Student()
ArtStudent.prototype.constructor = ArtStudent
```

这段代码虽然能正常工作，但对新手来说理解成本较高，也容易写错。为了解决这个问题，在 **ES6（ES2015）**中，JavaScript 引入了 **class 语法**，用来更直观地描述“类”和“继承”。不过，需要注意的是：**class 仅仅只是语法糖，本质仍然是上面的原型链机制实现的**。

那么，什么是类呢？

>人类、鸟类、鱼类... 所谓类，就是对一类事物的描述，是抽象的、概念上的定义，比如鸟类，就泛指所有具有鸟类特征的动物。比如人类，不同的人，有着不同的性格、不同的爱好、不同的样貌等等，但是他们根本上都是人，所以说可以将他们抽象描述为人类。
>
>而对象是某一类事物实际存在的每个个体，因而也被称为实例（instance）我们每个人都是人类的一个实际存在的个体。
>
>![image-20220919203119479](https://s2.loli.net/2022/09/19/U2P7qWOtRz5bhFY.png)
>
>所以说，类就是抽象概念的人，而对象，就是具体的某一个人。
>
>- A：是谁拿走了我的手机？
>- B：是个人。（某一个类）
>- A：我还知道是个人呢，具体是谁呢？
>- B：是XXX。（具体某个对象）
>
>而我们在JavaScript中，也可以像这样进行编程，我们可以定义一个类，然后进一步创建许多这个类的实例对象。像这种编程方式，我们称为**面向对象编程**。

使用 `class` 关键字可以声明一个类，就像我们之前创建构造函数那样，最基本的写法如下：

```js
class Student {  //创建一个Student类型，这里其实就是之前构造函数的名字
}
```

这个类目前还什么属性都没有，但它已经可以用来创建对象了：

```js
const s = new Student()
```

和构造函数一样，类也需要通过 `new` 关键字来创建实例对象，当然实际上，它本来就是构造函数的语法糖。

在类中，如果我们需要在创建对象时初始化属性，就需要使用一个特殊的方法：`constructor`：

```js
class Student {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
}
```

这里的`constructor`和我们之前介绍的构造函数定义，其实本质就是一个东西，这里定义的参数就是构造函数的参数，我们同样可以在里面使用`this`表示新创建的对象本身，参数列表就是构造函数的参数。需要注意的是，`constructor`只能存在一个，默认情况下，即使我们不写，也会存在一个无参的`constructor`，如果写了就会自动覆盖掉无参的那一个。

因此，使用方式和构造函数完全一致：

```js
const stu = new Student("小明", 18)
const stu = Student("小明", 18)  //class自带检测机制，无new调用会直接报错
```

我们也可以访问它的属性：

```js
console.log(s.name) // 小明
console.log(s.age)  // 18
```

除此之外，我们也可以直接在类中声明属性：

```js
class Student {
    gender   //直接写就行了，然后换行
  	name
    age
}
```

但是注意，这种写法得到的属性初始值都是`undefined`，如果我们需要为属性设置初始值，可以直接使用赋值运算符：

```js
class Student {
    gender   //直接写就行了，然后换行
    name = '名字'
    age = 18
}
```

除了属性之外，类中定义的方法本质上就是对象具有的方法，在类中定义方法时，不需要使用 `function` 关键字，直接写方法名即可：

```js
class Student {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
    sayHi() {
        console.log(`你好，我是${this.name}`)
    }
}
```

调用方式也是和之前完全一样，没有区别：

```js
const s = new Student("小明", 18)
s.sayHi()
```

需要注意的是，中定义的方法，本质上仍然是挂载在 `prototype` 上的，不会为每一个对象单独创建方法，这和我们之前说的优化方案（手动写 `Student.prototype.sayHi = ...`）是一样的，`class`自动帮我们实现好了。

这样，我们就成功创建了一个类型，虽然本质上依然就是之前的构造函数，但是它简化了很多语法，我们可以这个类型进行类型计算：

```js
typeof Student // "function"
```

可以看到，这里得到的依然是一个函数类型的值，说明它本质就是构造函数。`class` 并不是一种全新的类型，它只是让我们用更接近传统面向对象语言的方式来书写代码。

同样的，它对于继承的实现也进行很大程度的简化，我们可以使用 `extends` 关键字，让一个类继承另一个类：

```js
class ArtStudent extends Student {
}
```

这行代码的含义是：**ArtStudent 是 Student 的子类**，当子类需要定义自己的构造器时，必须先调用 `super()`：

```js
class ArtStudent extends Student {
    constructor(name, age, level) {
        super(name, age)   //调用父类的 constructor
        this.level = level
    }
}
```

这里需要注意的是，`super()` 用来调用父类的构造方法，在子类中，必须先调用 `super()`，才能使用 `this`。这里的`super(name, age)` 本质上等价于之前的 `Student.call(this, name, age)`，也就是对构成继承关系的父对象进行初始化。接着我们就可以直接使用了：

```js
const a = new ArtStudent("小明", 18, "高级")

console.log(a.name)   // 小明
console.log(a.age)    // 18
console.log(a.level)  // 高级
```

可以看到，我们什么都没有做直接就可以使用父类的属性，这是因为`extends` 已经自动帮我们建立好了原型链关系。

同样的，由于原型链继承关系，父类中定义的方法，子类可以直接使用：

```js
class Student {
    constructor(name, age) {
        this.name = name
        this.age = age
    }
    sayHi() {
        console.log("你好，我是学生")
    }
}

class ArtStudent extends Student {
    constructor(name, age, level) {
        super(name, age)
        this.level = level
    }
}

const a = new ArtStudent("小明", 18, "高级")
a.sayHi()
```

到这里，我们已经完成了 **类的声明、构造方法、方法定义以及继承的基本使用**。在下一节中，我们将基于 `class`语法，进一步介绍 **私有属性** 的实现方式。

### 私有属性

在前面的内容中，我们已经学习了对象的属性和方法，你会发现一个问题：**对象里的属性，默认都是“公开的”**。也就是说，只要你能拿到这个对象，就可以随意访问、修改它的属性：

```js
const person = {
    name: "小明",
    age: 18
}

console.log(person.age)   // 得到18
person.age = 100
console.log(person.age)   // 得到100
```

从语法角度看，这完全没问题，但从**设计角度**来看，却存在隐患，我们先来看一个生活中的例子：

* 你的手机里有「电量」
* 你可以**查看电量**
* 但你不能直接改成 `100%`，对吧？

如果手机允许你随便写`battery = 100`，那系统早就乱套了，电池的实际容量并不会因为我们修改了变量的值就真的发生改变，我们应该坚持唯物主义。为了防止这种情况，我们需要对对象的属性进行限制，使得：

* 有些数据 **不希望被外部随意访问**
* 有些数据 **只能通过指定方式修改**

在 ES6 之前，JavaScript 并没有真正意义上的私有属性，开发者通常会通过一些“约定”来模拟私有属性，例如使用下划线开头：

```js
const phone = {
    _battery: 100
}
```

这种方式**只是约定俗成**，并不能真正限制访问：

```js
phone._battery = 0
```

从语法层面来说，这个属性依然是完全开放的。在 **ES2022** 中，JavaScript 为类引入了**真正的私有属性**。私有属性通过 `#` 号来声明，我们可以在`class`中直接添加：

```js
class Phone {
    #battery = 100
}
```

这里的 `#battery` 就是一个私有属性，它具有以下特点：

- 只能在类的内部访问
- 在类的外部无法读取、无法修改
- 不会出现在对象的属性列表

我们来尝试在外部访问这个私有属性：

```js
const p = new Phone()
console.log(p.#battery)
console.log(p["#battery"])  //括号访问也不行
```

代码会直接报错，程序无法运行，这种错误是在**语法分析阶段**就会被拦截，而不是运行时报错。这说明：**私有属性并不是通过调整属性配置实现的，而是在语言层面提供的访问控制机制**。

既然私有属性无法直接访问，那么我们通常会提供**公共方法**来进行间接操作：

```js
class Phone {
    #battery = 100
    getBattery() {  //比如当电量超出100时，只返回100
        return this.#battery > 100 ? 100 : this.#battery
    }
    charge(value) {
        if (value > 0) {
            if(this.#battery + value > 105) {
                this.#battery = 105
            } else {
                this.#battery += value
            }
        }
    }
}
```

这样一来，外部只能读取电量，并且我们可以自由控制电量的展示，而不是直接返回原始值。如果我们希望在特定规则下修改私有属性，也可以提供一个修改方法，此时，外部只能通过 `charge` 方法来改变电量：

```js
const p = new Phone()
p.charge(10)  //冲到实际电量105
console.log(p.getBattery()) // 100
```

注意，私有属性是无法通过 `Object.keys`、`for...in` 获取的，也不会被实例枚举。那么，既然我们前面说，`class`本质上就是语法糖，它其实就是简化了我们的构造函数定义，这里的私有属性，本质上是怎么实现的呢？我们可以直接打印这个对象：

![image-20260209115257723](https://s2.loli.net/2026/02/09/UZdbEsfWicVryAM.png)

可以看到，我们在类中定义的方法位于构造函数原型上，而这个私有属性，实际上是存在于对象上，并且名字就是以`#`开头的。

```js
const t = {
    //无法直接 #battery: 10 因为#是特殊字符
    '#battery': 10
}
console.log(t.#battery)   //同样报错，语法层面不允许
```

但是需要注意的是，像这样通过字符串名称定义的属性，它并不是私有的：

```js
console.log(t['#battery'])  //正确得到

const p = new Phone()
console.log(p['#battery'])   //得不到
```

因为字符串形式的属性名称和直接编写的属性名称本质上并不是同一种类型，虽然名字一样，前者仅仅只是一个字符形式的名称，而后者更像是创建的一个Symbol，我们无法通过其他任何方法去访问一个我们不知道的Symbol的属性，所以它可以实现真正意义上的私有。

在实际开发中，**并不是所有属性都需要私有化**，但对于核心状态数据，使用私有属性可以显著提高代码的安全性和可维护性。

### 构造函数安全检查

经过前面的学习我们知道，使用构造函数时，必须使用`new`关键字进行对象创建，否则会导致构造函数内的`this`出现问题，这其实是因为`new`关键字本身就是语法糖，我们调用函数：

```js
function Person(name, age) {
    this.name = name;
    this.age = age;
}

const  p = new Person("小明", 18)
```

实际上在使用`new`时，等价于执行了下面的操作：

```js
 // 1. 创建空对象并继承构造函数的原型
  const obj = Object.create(Person.prototype);
  // 2. 绑定this并执行构造函数
  const result = Person.call(obj, ...args);
  // 3. 返回创建的对象
  return object;
```

> 当然，这里还并不准确，如果构造函数有返回值且返回值是一个对象的话，最后会优先返回构造函数的返回值。

而如果我们不加`new`则相当于函数被直接调用：

```js
function Person(name, age) {
    this.name = name;   //直接对全局作用域对象上的name赋值
    this.age = age;   //直接对全局作用域对象上的age赋值
}

const  p = Person("小明", 18)
console.log(window)  //这种全局作用域上的对象，一般也可以称作globalThis
```

![image-20260209133236157](https://s2.loli.net/2026/02/09/hTJHIXdbeuatqQl.png)

可以看到，如果不添加`new`相当于我们正常把它当做函数进行调用，而普通函数的`this`指向的是当前的作用域对象。

在上一章，我们为大家介绍了多种不同的包装对象，我们发现，包装对象并没有出现这种情况，比如`String`构造函数，虽然我们也可以不使用`new`进行调用，但是它并没有得到一个错误结果或者对作用域对象赋值：

```js
const p = String("无房贷无车贷无后代")
console.log(p.length)
console.log(window.length)  //并没有覆盖
```

实际上，这是因为在String构造函数的内部进行了引用判断。这里我们需要介绍`new.target` ，它是 ECMAScript 2015 (ES6) 引入的一个元属性，用于检测函数或构造方法是否通过 `new` 关键字被调用。它允许开发者在函数内部判断调用方式，从而执行不同的逻辑：

```js
function Person(name) {
    // 判断是否存在目标
    if (!new.target) {
        console.log("没有使用new调用")
    }
    this.name = name;
}
```

* 通过构造函数使用 `new` 关键字调用时，`new.target` 返回该函数的引用（即构造函数本身）
* 如果是在类的构造器中，`new.target` 返回当前正在实例化的类（可能是子类，本质也是构造函数）
* 当函数直接调用（未使用 `new`）时，`new.target` 返回 `undefined`

我们可以直接判断这个属性是否存在真值，如果存在一定是使用了`new`来进行调用。利用这种特性，我们可以将普通调用形式也进行处理，直接返回一个正确的对象：

```js
function Person(name) {
    // 确保必须通过 new 调用
    if (!new.target) {
        return {
            name: name
        }
    }
    this.name = name;
}

const p = Person("小明")  //即使不使用new也可以正确得到了
console.log(p)
```

所以，这就不难理解为什么之前遇到的`Number`、`String`等构造函数正常用也没毛病了。

### Proxy（选学）

`Proxy` 是 ES6 引入的一个新特性，它本身也是一个对象，但是他会包裹一个对象进行代理，它的作用可以用一句话概括：**Proxy 可以拦截并自定义对象的各种操作行为**。创建一个 Proxy 对象非常简单，只需要两个参数：

```js
const proxy = new Proxy(target, handler)
```

* `target`：被代理的原始对象
* `handler`：拦截规则对象（里面定义各种拦截逻辑）

我们先来看一个最最简单的例子：

```js
const obj = {
    name: "小明",
    age: 18
}

const proxy = new Proxy(obj, {})
```

此时这个 `proxy` 看起来和原对象几乎一模一样：

```js
console.log(proxy.name) // 小明
proxy.age = 20
console.log(obj.age)    // 20
```

如果我们没有在 `handler` 中定义任何拦截规则，所有操作都会**原封不动地透传给原对象**，无论是获取属性还是调用对象中的方法。

Proxy 中最常用的一个拦截点就是 `get`，它会在**读取属性时触发**：

```js
const proxy = new Proxy(obj, {
    get(target, key) {
        console.log("正在读取属性:", key)
        return target[key]
    }
})
```

![image-20260209143218021](https://s2.loli.net/2026/02/09/5z8LHMrCbVu9dxD.png)

有了`get`拦截点，我们就可以做很多事情，比如我们可以给不存在的属性一个默认值：

```js
const proxy = new Proxy(obj, {
    get(target, key) {
        if (key in target) {
            return target[key]
        }
        return "暂无该属性"  //这样，即使对象没有这个属性，也可以手动返回一个自定义的
    }
})
```

这样一来，所有不存在的属性访问都会被统一处理，而不用在每个地方写判断。实际上在Vue3中，很多高级功能、数据绑定等，都是利用Proxy来实现的。

除了读取属性，我们更常见的需求是：**拦截属性的修改行为**，这就要用到 `set`。

```js
const proxy = new Proxy(obj, {
    set(target, key, value) {
        console.log(`正在修改 ${key}，新值是 ${value}`)
        target[key] = value
        return true
    }
})
```

这里有一个非常重要的点，set 必须返回 true，否则在严格模式下会报错。

比较常见的场景就是可以利用 `set` 来限制属性的合法性：

```js
const proxy = new Proxy(user, {
    set(target, key, value) {
        if (key === "age") {
            if (typeof value !== "number" || value < 0) {
                console.log("年龄必须是非负数字")
              	return false
            }
        }
        target[key] = value
        return true
    }
})
```

这样，**所有对 `age` 的非法修改都会被集中拦截**，而不是分散在各个业务逻辑中。

当使用 `delete` 删除属性时，可以使用 `deleteProperty` 进行拦截：

```js
const proxy = new Proxy(obj, {
    deleteProperty(target, key) {
        console.log(`属性 ${key} 即将被删除`)
        delete target[key]
        return true
    }
})
```

当我们使用 `in` 判断属性是否存在时：

```js
const proxy = new Proxy(obj, {
    has(target, key) {
        if (key === "age") {
            return false
        }
        return key in target
    }
})
```

Proxy就像囚笼，而对象就是笼中鸟，它没有自由，它的人生只能被Proxy决定，一切的行动都要经过Proxy的严格审核。不过，虽然对象是笼中鸟，但是它没有放弃对自由的幻想，依然坚持自己的原则，所以，Proxy 不会修改原对象的行为规则，它只是包了一层“访问入口”。

### Reflect（选学）

在上一节中，我们学习了 **Proxy**，知道它可以拦截对象的各种操作，比如 `get`、`set`、`delete` 等。但如果你细心一点，可能已经注意到一个问题：

> 在 Proxy 的拦截函数里，我们经常会手动去操作原对象
> 比如：`target[key] = value`、`delete target[key]`

这样写当然没问题，但其实 **并不规范，也不够安全**，为了解决这个问题，ES6 引入了一个新的内置对象：**Reflect**，它提供了一组“标准化的对象操作方法”，用来替代直接操作对象的写法。

我们先来看一个 Proxy 中常见的写法：

```js
const proxy = new Proxy(obj, {
    get(target, key) {
        return target[key]
    }
})
```

Reflect 的设计，几乎就是为了 **Proxy** 准备的，在 Proxy 的拦截器中，**官方推荐的默认行为实现方式，就是调用 Reflect**，而不是去使用正常的对象调用方式，比如一些场景下，Reflect 会**明确告诉你操作是否成功**，不会默默失败。

读取对象属性我们可以使用`Reflect.get()`来完成：

```js
const obj = {
    name: "小明"
}

console.log(Reflect.get(obj, "name")) // 小明
```

等价于：

```js
obj.name
```

前面我们写 Proxy 时，经常这样：

```js
const proxy = new Proxy(obj, {
    get(target, key) {
        return target[key]
    }
})
```

而官方推荐写法是：

```js
const proxy = new Proxy(obj, {
    get(target, key) {
        return Reflect.get(target, key)
    }
})
```

同样的，设置对象属性我们可以使用`Reflect.set()`来完成：

```js
const obj = {}
const success = Reflect.set(obj, "age", 18)  // 如果属性的配置为不可写，返回false
console.log(success) // true
console.log(obj.age) // 18
```

判断属性是否存在我们可以使用`Reflect.has()`来完成：

```js
const obj = { a: 1 }
Reflect.has(obj, "a") // true
Reflect.has(obj, "b") // false
```

等价于：

```js
"a" in obj
```

删除属性我们可以使用`Reflect.deleteProperty()`来完成：

```js
const obj = { a: 1 }
Reflect.deleteProperty(obj, "a") // true
console.log(obj) // {}
```

等价于：

```js
delete obj.a
```

获取对象自身所有属性键我们可以使用`Reflect.ownKeys()`来完成，它比 `Object.keys` 更完整，会包含普通字符串属性、Symbol 属性、不可枚举属性：

```js
const obj = {
    a: 1,
    [Symbol("x")]: 2
}

console.log(Reflect.ownKeys(obj))
```

有关Reflect其他的方法，可以参考：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

### 垃圾回收机制（选学）

在前面的学习中，我们已经接触了大量的**对象、函数、Proxy、Reflect** 相关的内容，你可能已经隐约意识到一个问题：这些对象创建出来之后，**什么时候会被销毁？**内存会不会越用越多？会不会泄漏？

在 JavaScript 中，这些问题由一个非常重要的机制来负责 —— **垃圾回收机制（Garbage Collection，简称 GC）**

在一些底层语言（如 C / C++）中，程序员需要**手动管理内存**：

```js
malloc(...)
free(...)
```

一旦忘记 `free`，就会造成内存泄漏，一旦多次 `free`，程序直接崩溃。而 JavaScript 的设计目标之一就是，让开发者专注于业务，而不是内存管理，所以 JS 采用了 **自动垃圾回收机制**，你只管创建对象，JS 引擎负责判断哪些对象“没用了”并自动回收它们占用的内存。

那么，什么东西会被认为是“垃圾”呢？在 JavaScript 中，无法再被访问到的对象，就是垃圾。

我们来看一个最简单的例子：

```js
let obj = {
    name: "小明"
}

obj = null
```

当我们执行 `obj = null` 后，原来那个 `{ name: "小明" }` 对象已经**没有任何变量再指向它**，并且，程序中再也访问不到它，那么这个对象就变成了**垃圾**，可以被回收。

我们再来看下一个例子：

```js
function test() {
    const tmp = {}  //在函数中创建了一个新的的对象
    console.log(tmp)
}

test()
console.log("Hello World")
```

可以看到，当函数执行完成后，实际上函数内创建的对象已经无法再外部使用了，此时同样会认为这个对象已经没有作用了，那么就会回收这个对象占据的内存空间。

当然，有些时候即使变量被设定为`null`，也有可能出现无法回收内存的情况：

```js
let a = { name: "小明" }
let b = a

a = null
```

此时，虽然`a`已经为`null`了，但是我们仍然可以通过`b`访问到对象，那么这里就不算是无法使用，不会被视为垃圾，除非这里的`b`也变成`null`。

同样的，函数执行完也并非说一定会回收对象：

```js
function outer() {
    const data = { count: 0 }

    return function inner() {
        console.log(data.count)
    }
}

let fn = outer()
```

可以看到，这里虽然函数已经执行结束，但是由于返回的结果是一个函数，并且在这个函数中用到了外层函数中定义的对象，此时这个对象就被带到了它原本作用域之外的区域，这种情况我们也称之为**闭包**（闭包是指**函数能够访问并操作其词法作用域之外的变量**）此时由于对象仍然可被访问，所以不会进行垃圾回收，当然，如果要释放我们只需要把存储闭包的变量给置空即可。

>  不过，虽然垃圾回收机制会自动进行，但是实际上到底什么时候被回收的，我们无从得知。咋ES2021之后，新增了FinalizationRegistry类，它可以实现在对象被垃圾回收之后，注册一个回调通知：
>
>  ```js
>  const registry = new FinalizationRegistry((heldValue) => {
>   console.log("对象被回收了：", heldValue)
>  })
>  
>  let obj = { name: "小明" }
>  registry.register(obj, "这是小明对象")
>  obj = null
>  ```
>
>  在浏览器开发者工具的内存板块，点击清理即可立即进行一次垃圾回收。

至此，有关面面向对象的高级内容部分就到此结束了。

## 常用对象和函数

前面的章节我们为大家介绍了多种不同的包装对象，在这些包装对象中，有很多的实用方法，这一节我们将继续为大家介绍更多不同的常用对象和函数。

### 时间日期对象

在 JavaScript 中，时间和日期并不是基础类型，而是通过一个内置对象来处理的，这个对象就是 **`Date`**。它主要用于获取当前时间、表示某一个具体时间点、进行时间计算（加减、比较）、格式化时间显示等。在实际开发中，**Date 几乎是必用对象之一** 。

和前面讲过的其他内置对象一样，`Date` 也是一个构造函数，需要通过 `new` 来创建实例对象。如果不传任何参数，`Date` 默认表示**当前系统时间**：

```js
const now = new Date()
console.log(now)
```

日期对象打印出来会自动转换为字符串形式，它的结果类似于：

```sh
Tue Feb 10 2026 14:19:32 GMT+0800 (中国标准时间)
```

这个对象中，已经包含了完整的年月日时分秒，甚至毫秒以及时区信息。注意，和之前一样，直接调用`Date()` 和 `new Date()` 是**完全不同的**，前者得到的不是对象，而是字符串形式的日期。

除了当前时间，我们也可以创建一个**指定时间点**：

```js
const now = new Date('2027-02-08')  //传入标准时间格式字符串
console.log(now)
```

```js
const now = new Date('2027-02-08 18:09:50')   //也可以带上具体时间
console.log(now)
```

这种写法直观、好记，新手阶段**推荐优先使用**，我们也可以使用多个参数来描述年月日：

```js
const d = new Date(2025, 0, 1)
```

这里需要注意的是，月份是从 0 开始的，也是很多人容易踩坑的地方，所以不推荐这种写法。

我们可以使用使用时间戳来表示一个时间，时间戳表示的是从 1970-01-01 00:00:00（UTC）开始，到当前时间经过的毫秒数：

```js
const d = new Date(0)   //直接传入一个数字表示时间戳
console.log(d)
```

得到的就是：

```js
1970-01-01 08:00:00  //不同时区得到的时间信息不同
```

因为中国位于东八区，有 8 小时的时区偏移，这里先介绍一下什么是时区：

> 时区（Time Zone）是地球上划分的区域，用于标准化时间。由于地球自转，全球不同地区的日出日落时间不同，为了方便统一时间的管理和交流，地球被划分为多个时区。
>
> ![image-20250708173054858](https://s2.loli.net/2025/07/08/vCtf6e7iMR3caHd.png)
>
> 原则上，全球共分为24个时区，每隔经度15°划分一个时区，时区从英国的本初子午线开始，向东方每增加一个时区则+1，+1则称为东一区，+8就是东八区。向西方每增加一个时区则-1，-1就是西一区。中国相当于是横跨了5个时区，从东五区（西藏）到东九区（吉林），但实际上中国全国统一使用一个标准时间，就是位于东八区的北京时间，当本初子午线的时间为凌晨0点时，中国时间则需要按照规则+8偏移，也就是早上的8点钟，相当于中国已经看到了日出，而英国还在夜晚，这与现实是一致的，其他地区也是一样的计算规则。
>
> 其中位于本初子午线的时间被称为格林威治标准时间GMT，就像周杰伦歌词里写的那样，它是时间的标准起点。
>
> ![image-20250708174926873](https://s2.loli.net/2025/07/08/7KHinqC5ON8X3Mo.png)
>
> UTC（Coordinated Universal Time，协调世界时）是一种世界标准时间，用于同步全球的时间系统。也是按照标准时区进行计算，其中格林威治时间就是UTC+0，而位于东八区的北京时间，则是UTC+8，称为中国标准时间（CTT），位于西六区的北美中部地区则是UTC-6，是中部标准时间（CST）。
>
> 当然，时区也有一些别名，比如中国标准时区CST也可以使用别名`Asia/ShangHai`，一般别名格式为`州名称/城市名称`，比如美国纽约就是`America/New_York`。

时间戳在前端开发中非常重要，它的优点是：不受时区影响、方便存储、便于比较和计算。想要获取当前时间或是某一时间的时间戳，我们可以使用Date的一些方法：

```js
const ts = Date.now()  //调用静态方法
console.log(ts)

const ts = new Date().getTime()  //或是用通过对象获取
```

当我们需要比较两个时间的前后关系时，就可以考虑使用时间戳：

```js
const data1 = new Date('2027-02-08 18:09:50')
const data2 = new Date('2026-02-08 18:09:50')
console.log(data1.getTime() > data2.getTime())   //获取时间戳数字，大的一定是越靠后的时间
```

当然，为了简便，我们也可以直接让两个日期对象进行比较：

```js
console.log(data1 > data2)
```

JS会自动使用时间对象的时间戳进行比较。

Date 对象提供了一系列 `get` 方法，用于获取时间的不同组成部分。假设我们有一个时间对象：

```js
const d = new Date()
d.getFullYear()  // 年份，例如 2026
d.getMonth()    // 月份（0-11）
d.getDate()     // 日期（1-31）
d.getHours()        // 小时 0-23
d.getMinutes()     // 分钟数 0-59
d.getSeconds()     // 秒数 0-59
d.getMilliseconds() // 毫秒数 0-999
d.getDay()    //星期数0代表星期天，6代表星期六
```

我们还可以针对星期数，简单做一个映射：

```js
const weekMap = ["日", "一", "二", "三", "四", "五", "六"]
console.log("星期" + weekMap[d.getDay()])
```

Date 对象不仅可以读，还可以改：

```js
const d = new Date()

d.setFullYear(2030)
d.setMonth(5)   // 6 月
d.setDate(10)

console.log(d)
```

比较好用的是，当日期超出时，会自动进行退位和进位：

```js
const d = new Date("2025-01-31")
d.setDate(d.getDate() + 1)

console.log(d) // 2025-02-01
```

这在做时间计算时非常方便。

时间还有很多不同的字符串转换操作：

```js
console.log(data.toDateString());  //只保留日期部分
console.log(data.toTimeString());  //只保留时间部分
console.log(data.toLocaleDateString());  //以本地化形式，只保留日期部分
console.log(data.toLocaleTimeString());  //以本地化形式，只保留时间部分
```

这些操作可以实现时间字符串的格式快速转换，比如我们只需要时间或是日期部分。

### 数学对象

在 JavaScript 中，虽然我们已经学习过很多运算符了，但是数学相关的高级计算并不是靠我们自己手写公式完成的，而是由一个内置对象统一提供，这个对象就是 **`Math`**。

和前面讲过的 `Date` 不同，**`Math` 不是构造函数**，就是一个携带多种方法的普通对象，你可以把 `Math` 理解成一个**装满数学工具函数的工具箱**。

基本上，数学中场常见的一些常量都是有的：

```js
Math.E   //自然对数的底数（欧拉数，约等于 2.718）
Math.PI   //圆周率 π（约等于 3.14159）
Math.LN2   //2 的自然对数（约等于 0.693）
Math.LN10   //10 的自然对数（约等于 2.303）
Math.LOG2E   //以 2 为底 E 的对数（约等于 1.443）
Math.LOG10E   //以 10 为底 E 的对数（约等于 0.434）
Math.SQRT2   //2 的平方根（约等于 1.414）
Math.SQRT1_2   //1/2 的平方根（约等于 0.707）
```

包括各种数学计算：

```js
Math.abs(x)   //返回 x 的绝对值
Math.ceil(x)   //向上取整（返回大于等于 x 的最小整数）
Math.floor(x)   //向下取整（返回小于等于 x 的最大整数）
Math.round(x)   //四舍五入取整
Math.trunc(x)   //去除小数部分，返回整数（ES6 新增）
Math.sign(x)   //返回 x 的符号（正数返回 1，负数返回 -1，0 返回 0）
Math.pow(x, y)   //返回 x 的 y 次幂（等价于 x ** y）
Math.sqrt(x)   //返回 x 的平方根
Math.cbrt(x)   //返回 x 的立方根（ES6 新增）
Math.hypot(...values)   //返回所有参数的平方和的平方根（ES6 新增）
/* 对数运算 */
Math.exp(x)   //返回 E 的 x 次幂（E^x）
Math.log(x)   //返回 x 的自然对数（ln(x)）
Math.log10(x)   //返回以 10 为底的 x 的对数（ES6 新增）
Math.log2(x)   //返回以 2 为底的 x 的对数（ES6 新增）
Math.log1p(x)   //返回 1 + x 的自然对数（ln(1+x)，适用于 x 接近 0 时的高精度计算）
/* 三角函数 */
Math.sin(x)   //正弦值
Math.cos(x)   //余弦值
Math.tan(x)   //正切值
Math.asin(x)   //反正弦值（返回值范围：-π/2 到 π/2）
Math.acos(x)   //反余弦值（返回值范围：0 到 π）
Math.atan(x)   //反正切值（返回值范围：-π/2 到 π/2）
Math.atan2(y, x)   //返回点 (x, y) 与 x 轴的夹角（返回值范围：-π 到 π）
/* 双曲函数 */
Math.sinh(x)   //双曲正弦
Math.cosh(x)   //双曲余弦
Math.tanh(x)   //双曲正切
Math.asinh(x)   //反双曲正弦
Math.acosh(x)   //反双曲余弦
Math.atanh(x)   //反双曲正切
```

ES6之后，为了进一步强化数学工具，新增了如下方法：

```js
Math.clz32(x)   //返回 32 位整数 x 的前导零位数
Math.imul(x, y)   //返回两个 32 位整数相乘的结果（避免溢出）
Math.fround(x)   //返回 x 的单精度浮点数表示
Math.expm1(x)   //返回 Math.exp(x) - 1（适用于 x 接近 0 时的高精度计算）
```

各位小伙伴根据自己的需求，合理选择即可。

### 正则表达式

在前面的学习中，我们已经掌握了字符串的各种操作方法，但你可能已经发现一个问题，字符串提供的方法很多，但遇到“规则匹配”时就开始力不从心了。比如下面这些需求：

- 判断手机号是否合法
- 判断邮箱格式是否正确
- 从一大段字符串中找出所有数字
- 替换掉所有不符合规则的字符

如果只靠 `indexOf`、`includes`、`slice` 这些方法，代码会变得非常复杂。这时，我们就需要一个**专门用于“规则匹配”的工具**，**正则表达式（Regular Expression，简称 RegExp）**。

正则表达式的创建方式非常简单，只需要使用`//`进行囊括即可：

```js
const reg = /abc/
```

这种写法最直观、最常用，它表示一个匹配规则，判断字符串中是否包含 `"abc"`。除此之外，我们也可以使用构造函数：

```js
const reg = new RegExp("abc")  //构造函数中传入的是字符串，某些特殊字符需要额外转义
```

这种写法适合**规则需要动态拼接**的场景，但新手阶段不太常用，我们还是更推荐上面的简单写法。

当然，正则表达式本身只是规则，**必须配合方法使用才有意义**，我们可以使用`test`方法来对字符串进行正则表达式检测：

```js
const reg = /abc/
console.log(reg.test("hello abc world")) // 包含abc，结果为true
console.log(reg.test("hello world"))     // 不包含abc，结果为false
```

我们也可以反过来，使用字符串提供的`match`方法：

```js
const str = "hello abc world"
console.log(str.match(/abc/))
```

如果匹配成功，返回一个数组，否则返回`null`。同样的，包括字符串的`replace`、`search`方法，都支持正则表达式的匹配方式：

```js
const str = "hello abc world"
const result = str.replace(/abc/, "JS")
console.log(result)
```

了解完正则表达式的用法之后，我们来看看如何编写正则表达式，最简单的就是普通字符匹配：

```js
/abc/   //直接写上要匹配的单个字符或子串就行了
```

只要字符串中出现 `"abc"` 就能匹配，此外，我们也可以使用`.`表示任意字符（换行符除外）：

```js
"a1c".match(/a.c/)  // 匹配
"abc".match(/a.c/)  // 匹配
"a_c".match(/a.c/)  // 匹配
```

当我们想匹配“多个可能字符中的一个”，就需要用到字符类（字符集合）使用方括号进行表示：

```js
/[abc]/
```

只要出现方括号中的任意一个字符就匹配：

```js
"axc".match(/[abc]/)  // 匹配
"a_x".match(/[abc]/)  // 匹配
"666".match(/[abc]/)  // 不匹配
```

除了正着匹配外，我们也可以反着匹配，使用`^`代表否定：

```js
/[^0-9]/
```

表示除了0-9这几个字符之外的其他都算匹配成功：

```js
"axc".match(/[^0-9]/)  // 匹配
"a_x".match(/[^0-9]/)  // 匹配
"666".match(/[^0-9]/)  // 不匹配
```

除此之外，正则中有一些非常常用的“简写规则”：

| 规则 | 含义                       |
| ---- | -------------------------- |
| `\d` | 任意数字（等价于 `[0-9]`） |
| `\D` | 非数字                     |
| `\w` | 字母、数字、下划线         |
| `\W` | 非字母、数字、下划线       |
| `\s` | 空白字符（空格、换行等）   |
| `\S` | 非空白字符                 |

有关更多正则表达式的内容，请前往：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_expressions

### 序列化工具

在 JavaScript 中，**序列化（Serialization）** 指的是：把数据结构转换成“可传输、可存储”的字符串形式。而**反序列化（Deserialization）**，则是相反的过程：把字符串还原成原本的数据结构。

在前端开发中，序列化非常常见，例如：

- 向后端发送数据
- 接收后端返回的数据
- 本地存储（localStorage）
- 数据持久化
- 深拷贝的简化方案

JavaScript 中，最核心、最常用的序列化工具就是：JSON对象。

`JSON`（JavaScript Object Notation）是一种**轻量级的数据交换格式**，它看起来和 JS 对象很像，但**并不是 JS 对象**，而是一种**字符串格式的规范**。比如下面的对象：

```js
const obj = {
  name: "小明",
  age: 18
}
```

转换为JSON格式就是：

```js
{
  "name": "小明",
  "age": 18
}
```

在JSON 中，属性名必须用双引号表示，字符串值必须用双引号、也不能写函数、不能写 `undefined`（这也是为什么前面建议大家一律使用`null`的原因）不能写注释更不能写 Symbol。例如下面是 **非法 JSON**：

```js
{
  name: "小明",        // ❌ 属性名没加双引号
  age: undefined,     // ❌ undefined 不允许
  say() {}            // ❌ 函数不允许
}
```

要将对象转换为JSON格式的字符串，我们可以使用JSON对象提供的方法：

```js
const obj = {
  name: "小明",
  age: 18
}

const jsonStr = JSON.stringify(obj)
console.log(jsonStr)   //{"name":"小明","age":18}
```

注意，序列化的结果是字符串，同样的，数组也可以进行序列化：

```js
const arr = [1, 2, 3]
JSON.stringify(arr)  //[1,2,3] 数组序列化之后看着和原本差别不是很大
```

JSON 支持**对象嵌套对象、对象嵌套数组**：

```js
const obj = {
  name: "小明",
  hobby: ["JS", "HTML"],
  info: {
    city: "台北"
  }
}

JSON.stringify(obj)   //{"name":"小明","hobby":["JS","HTML"],"info":{"city":"台北"}}
```

如果遇到不能序列化的属性，则最终结果中不会包含：

```js
const obj = {
  a: 1,
  b: undefined,
  c: function () {},
  d: Symbol()
}
//{"a":1}
```

这里需要注意的是，如果存在循环引用，会出错：

```js
const obj = {}
obj.self = obj   //obj的一个属性指向自己
```

JSON 不支持循环结构，会直接得到一个错误，这是 JSON 序列化的一个硬限制。此外，我们还可以手动指定格式化哪些属性：

```js
JSON.stringify(obj, ["name", "age"])  //只序列化name和age
```

最后一个参数用于控制格式化输出，它会让生成的字符串自动换行和缩进：

```js
JSON.stringify(obj, null, 2)  //2就是缩进的空格数
```

既然可以序列化成字符串，那么我们也可以将字符串反序列化回普通的对象或是数组：

```js
const jsonStr = '{"name":"小明","age":18}'
const obj = JSON.parse(jsonStr)

console.log(obj.name) // 小明
```

这里需要注意的是，如果字符串不是一个合法的JSON字符串，解析失败会直接报错。

此外，这种反序列化实际上是存在安全隐患的，因为它会将所有属性都还原为对象的属性，即使是一些特殊的属性也可以，比如`__proto__`：

```js
// 恶意JSON输入
const maliciousJson = '{"__proto__": {"isAdmin": true}}';

// 不安全的反序列化
const user = {};
Object.assign(user, JSON.parse(maliciousJson));

console.log(user)

// 原型被污染，所有对象继承isAdmin属性
console.log(user.isAdmin); // 输出: true
```

![image-20260210171446734](https://files.seeusercontent.com/2026/02/11/laO1/image-20260210171446734.png)

配合序列化和反序列化，我们可以实现对象的深拷贝：

```js
const obj = {
    name: "小明",
    age: 18,
    meta: { key: 666 }
}

const parse = JSON.parse(JSON.stringify(obj))

console.log(parse === obj)
console.log(parse.meta === obj.meta)  //深拷贝，所以也不相等
```

可以看到，通过一次序列化和反序列化，对象不仅自己被重新创建，包括里面的内容也被一起重新创建。

### 本地化工具

在前面的内容中，我们已经学习了字符串、数字、日期等基础类型的处理方式。但当程序开始面向**真实用户**时，就会遇到一个非常现实的问题：

> 同样的数据，在不同国家 / 地区，展示方式是不一样的。

举几个最直观的例子：

- 数字
  - 中国：`1,234,567.89`
  - 德国：`1.234.567,89`
- 日期
  - 中国：`2025年2月10日`
  - 美国：`02/10/2025`
- 货币
  - 中国：`￥99.00`
  - 美国：`$99.00`
  - 日本：`￥99`

如果我们靠**手写字符串拼接**来适配这些差异，不但麻烦，而且极其容易出错。为了解决这个问题，JavaScript 提供了一套内置的国际化解决方案：**`Intl`**，`Intl` 并不是一个函数，而是一个**命名空间对象**，里面包含了多个构造函数，例如：

- `Intl.NumberFormat`
- `Intl.DateTimeFormat`
- `Intl.Collator`
- `Intl.RelativeTimeFormat`

它们的使用套路非常统一，基本都是三步：

1. 指定语言 / 地区
2. 指定格式化规则
3. 调用 `format` 方法

我们先从**最常用、最容易理解的数字格式化**开始，假设我们现在有一个普通数字，如果直接输出：

```js
const num = 12345670000
console.log(num)
// 12345670000
```

这对程序来说没问题，但对用户来说并不友好，因为用户一眼看过去根本不知道一共有多少个`0`。我们可以使用 `Intl.NumberFormat` 来进行本地化处理：

```js
const num = 12345670000
const formatter = new Intl.NumberFormat("zh-CN")   //这里的 "zh-CN" 表示 中文（中国）
console.log(formatter.format(num))   //12,345,670,000
```

我们只需要修改地区，就能得到完全不同的格式：

```js
new Intl.NumberFormat("en-US").format(num) // 1,234,567.89
new Intl.NumberFormat("de-DE").format(num) // 1.234.567,89
new Intl.NumberFormat("ja-JP").format(num) // 1,234,567.89
```

默认情况下，Intl 会**尽量保留原始数字结构**，如果我们希望强制控制小数位数，可以传入第二个参数（配置对象）：

```js
const formatter = new Intl.NumberFormat("zh-CN", {
    minimumFractionDigits: 2,   //最小保留2位小数
    maximumFractionDigits: 2    //最大保留2位小数
})

formatter.format(12)        // "12.00"
formatter.format(12.3456)  // "12.35"
```

这在**金额、统计数据展示**中非常常见，此外，`Intl`在格式化货币时尤其强大，因为货币符号、位置、格式，全都因地区不同而不同：

```js
const formatter = new Intl.NumberFormat("zh-CN", {
    style: "currency",
    currency: "CNY"
})
formatter.format(99)   //￥99.00
```

```js
const formatter = new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD"
})
formatter.format(99)   //$99.00
```

除了对数字进行修改外，我们可以通过配置项，精细控制日期展示内容：

```js
const formatter = new Intl.DateTimeFormat("zh-CN", {
    year: "numeric",  //"numeric" | "2-digit"
    month: "long",   //"numeric" | "2-digit" | "long" | "short"
    day: "numeric",
    weekday: "long"
})
formatter.format(new Date())   //2026年2月10日星期二
```

此外，我们也可以以相对时间进行表示：

```js
const rtf = new Intl.RelativeTimeFormat("zh-CN")
rtf.format(-1, "day") // "1天前"
rtf.format(3, "day")  // "3天后"
```

```js
new Intl.RelativeTimeFormat("en-US").format(-1, "day")  // "1 day ago"
new Intl.RelativeTimeFormat("en-US").format(-3, "day")  // "3 days ago" 自动变复数形式
```

这里仅列出数字和日期的格式化操作，有关Intl的更多内容请参阅：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Intl

### 表达式评估

`eval` 是 **JavaScript 内置的全局函数**，作用只有一个：把一段“字符串形式的 JS 代码”，当成真正的 JS 代码执行。比如：

```js
eval("console.log('Hello')");
```

等价于你直接在JS文件中写：

```js
console.log('Hello');
```

所以，`eval`就是JS“字符串 到 代码执行”的转换器。你甚至还可以用它来定义变量和函数：

```js
eval("function foo() { return 123 }");
foo(); // 123
```

虽然用起来感觉很强大，但实际上，这种玩法有非常大的安全隐患，很容易被代码注入 / 存在 XSS 漏洞。同时，JS 引擎没法提前优化 `eval` 里的代码，性能也不如直接写的代码。

在严格模式下，`eval` 有自己的作用域，无法随意污染外层作用域。

### 定时器

在实际开发中，我们经常会遇到这样的需求：

- 过一段时间再执行某段代码
- 每隔一段时间重复执行任务
- 延迟提示、轮询接口、倒计时、动画更新……

这些需求都离不开 **定时器**，最常见的两个定时器函数是：

* `setTimeout`：**延迟执行一次**
* `setInterval`：**按固定间隔重复执行**

`setTimeout` 用于在**指定时间后执行一次代码**，最简单的例子：

```js
setTimeout(() => {
    console.log("3秒后执行")
}, 3000)
```

其中第一个参数`callback`表示要执行的函数，而`delay`则是延迟时间（单位：毫秒）接着，回调函数会在指定时间结束之后执行。

> 这里需要注意一个细节：`setTimeout` **并不是“精确计时”**，而是“**至少等待这么久之后再执行**”，这一点我们会在后面的事件循环中详细解释。

有些时候，我们可能需要在定时器结束之前，提前取消这个任务，`setTimeout` 会返回一个 **定时器 ID**，我们可以用它来取消执行，使用`clearTimeout`对指定ID的定时器取消：

```js
const timerId = setTimeout(() => {
    console.log("这句不会执行")
}, 3000)

clearTimeout(timerId)  //立即清除定时器
```

只要在定时器规定时间执行前调用 `clearTimeout`，回调函数就不会再执行。

如果我们希望某段代码**每隔一段时间就执行一次**，可以使用 `setInterval`，它可以实现周期性执行：

```js
setInterval(() => {
    console.log("每秒执行一次")
}, 1000)
```

这里表示，**每隔 1 秒执行一次回调函数**，和 `setTimeout` 一样，`setInterval` 也会返回一个 ID，我们可以使用`clearInterval`来取消周期任务：

```js
const timerId = setInterval(() => {
    console.log("持续执行")
}, 1000)

clearInterval(timerId)
```

如果在某个时间不需要使用定时器了，一定记得清除定时器，否则永远不会停止。

不过，`setInterval` 会尝试每隔指定时间**固定触发**回调，即使上一次回调还未执行完毕（比如回调执行时间超过了间隔时间）这会导致回调被**堆积**，在主线程空闲时连续执行，造成性能问题。要解决这种问题，我们也可以考虑使用`setTimeout`递归，每次回调执行完成后，才会安排下一次执行。这确保了回调之间的间隔是**相对稳定**的（实际间隔 = 指定延迟 + 回调执行时间），不会出现任务堆积。

```js
// setInterval 版本：可能出现回调堆积
setInterval(() => {
  // 假设这个操作偶尔会耗时超过 100ms
  heavyOperation();
}, 100);
// setTimeout 递归版本：确保回调完成后再执行下一次
function scheduleTask() {
  heavyOperation();
  setTimeout(scheduleTask, 100);
}
scheduleTask();
```

## 错误

在前面的学习中，我们写的代码几乎都是“理想情况”：参数正确、逻辑正确、执行顺利。但在真实开发中，**错误是一定会发生的**，比如：

* 用户输入了非法数据
* 访问了不存在的变量
* 调用了不存在的方法
* 后端返回了异常数据

如果程序在遇到错误时**直接崩溃**，那用户体验会非常差，因此，JavaScript 提供了一套完整的 **错误机制（Error Handling）**，用来控制程序在出错时的行为，理解错误机制，是从“会写代码”走向“能写稳定代码”的重要一步。

```js
/**
 *                             _ooOoo_
 *                            o8888888o
 *                            88" . "88
 *                            (| -_- |)
 *                            O\  =  /O
 *                         ____/`---'\____
 *                       .'  \\|     |//  `.
 *                      /  \\|||  :  |||//  \
 *                     /  _||||| -:- |||||-  \
 *                     |   | \\\  -  /// |   |
 *                     | \_|  ''\---/''  |   |
 *                     \  .-\__  `-`  ___/-. /
 *                   ___`. .'  /--.--\  `. . __
 *                ."" '<  `.___\_<|>_/___.'  >'"".
 *               | | :  `- \`.;`\ _ /`;.`/ - ` : | |
 *               \  \ `-.   \_ __\ /__ _/   .-` /  /
 *          ======`-.____`-.___\_____/___.-`____.-'======
 *                             `=---='
 *          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
 *                     佛祖保佑        永无BUG
 *            佛曰:
 *                   写字楼里写字间，写字间里程序员；
 *                   程序人员写程序，又拿程序换酒钱。
 *                   酒醒只在网上坐，酒醉还来网下眠；
 *                   酒醉酒醒日复日，网上网下年复年。
 *                   但愿老死电脑间，不愿鞠躬老板前；
 *                   奔驰宝马贵者趣，公交自行程序员。
 *                   别人笑我忒疯癫，我笑自己命太贱；
 *                   不见满街漂亮妹，哪个归得程序员？
*/
```

### 什么是错误

从本质上来说，**错误是 JavaScript 在执行过程中遇到的异常情况**，我们先看一个最简单的例子：

```js
console.log(a)
```

运行后，控制台会直接报错：

```js
ReferenceError: a is not defined
```

此时你会发现一个非常重要的现象：**错误一旦出现，后面的代码将不会再执行**：

```js
console.log("开始")
console.log(a)
console.log("结束")  // 永远不会执行
```

这说明，在默认情况下，错误是**致命的**。

在 JavaScript 中，错误并不是随便写出来的一段字符串，而是一个**对象**，所有错误都继承自一个统一的基类：`Error`。系统内置了一些常见的错误类型，例如：

- `ReferenceError`：引用错误（变量不存在）
- `TypeError`：类型错误（对值进行了不合法操作）
- `SyntaxError`：语法错误（代码本身写错）
- `RangeError`：范围错误（值超出合法范围）

```js
const n = 123
n()    // 把数字当函数用

//TypeError: n is not a function
```

这些错误，都会在程序运行到对应位置时被自动创建并**抛出**。

### 抛出错误

除了 JavaScript 自动抛出的错误之外，我们也可以**主动抛出错误**，比如在有些情况下，虽然语法没问题，但**从业务逻辑上来说是错误的**，这也是很致命的问题，所以需要主动抛出。比如：

```js
function sum(a, b) {
    return a + b
}
```

如果这样调用：

```js
sum(1, "2")   //求和的结果变成求字符串的拼接了
```

语法上没有任何问题，但结果显然不是我们想要的，这种情况下，我们就可以在代码中**主动抛出错误**。

JavaScript 中通过 `throw` 关键字来抛出错误：

```js
function sum(a, b) {
    if (typeof a !== "number" || typeof b !== "number") {
        throw new Error("参数必须是数字")  //抛出错误其实就是抛出这个错误对象
    }
    return a + b
}
```

默认情况下，当程序抛出错误时，将被强制终止。不过，`throw` 后面不仅可以抛出 `Error` 对象，理论上也可以抛出任何值：

```js
throw "出错了"
throw 123
```

但在实际开发中，**强烈不推荐这样做**，因为这会破坏错误的统一结构，增加调试成本。规范做法是：**只抛出 Error 或其子类对象**。

### 捕获错误

前面我们看到，错误一旦抛出，程序就会直接中断执行，但在很多场景下，我们并不希望程序“直接崩掉”，而是希望给用户一个提示，不影响程序继续运行（类似于警告）这就需要用到 **错误捕获机制**。

JavaScript 提供了 `try...catch` 语法，用来捕获运行时错误，基本结构如下：

```js
try {
  	// 可能出错的代码
    console.log(a)
} catch (err) {   //也可以不带err参数
    // 出错后的处理逻辑
    console.log("代码出错了", err)
}
```

如果程序可以正常运行，那么会正常执行完`try`中的代码，但是如果`try`中的代码出现了问题，那么此时程序不会直接崩溃，而是进入 `catch` 分支执行，并且这个 `err` 参数就是前面抛出的 `Error` 对象，我们可以通过它获取错误信息，用于调试或提示。这里更建议大家使用不同类型的打印：

```js
console.error("代码出错了", err)   //错误信息
console.warn("代码出错了", err)   //警告信息
```

这样，我们可以根据不同类型的错误，分别对用户发出错误提示或是警告。

不过，各位小伙伴可以思考一下，那要是我们在`catch`块中也出现错误了怎么办？

```js
try {
    // 可能出错的代码
    console.log(a)
} catch (err) {
    // 出错后的处理逻辑
    console.warn("代码出错了", err)
    try {   //我们可以继续嵌套使用try-catch来处理
        console.log(a)
    } catch {

    }
}

```

不过，虽然这种方式可以实现，但是它看起来并不直观。

接着，在 `try...catch` 结构中，我们还可以额外使用 `finally`语句，它可以实现**无论是否出错都会执行**里面的代码：

```js
try {
    // 可能出错的代码
} catch (err) {
    // 错误处理
} finally {
    // 一定会执行的代码
}
```

无论`try`有没有出现错误，`finally`最终都会执行，它会在`try`执行完成后或是`catch`执行完成后执行，这种特性非常适合清理资源、重置状态等。

需要特别强调的一点是：**`try...catch` 只能捕获“运行时错误”**，例如，语法错误是捕获不到的：

```js
try {
    if (true {   //乱写
        console.log("hello")
    }
} catch (e) {
    console.log("捕获不到")
}
```

```js
try {
    obj.#battery  //包括之前的取私有属性
} catch (e) {
    console.log("捕获不到")
}
```

这种错误在代码解析阶段就已经失败，程序压根不会运行，自然也就不会从`try`往下走了。

## 常用数据结构

在 JavaScript 中，**大部分常见数据结构，官方库本身已经帮我们封装好了**，不过，理解这些结构的**特性、适用场景和差异**也是很重要的，推荐各位小伙伴有时间可以学习一下《数据结构与算法》这

本节我们重点介绍 JS 中**最常用、最实用**的几种数据结构。

### 迭代器

在 JavaScript 中，**迭代器（Iterator）** 并不是一种“存数据的结构”，而是一种 **访问数据的统一方式**，只要一个对象实现了这套接口，我们就可以用统一的方式去遍历它。

标记一个对象如果是“可迭代的”，必须满足两点：

1. 对象上存在 `Symbol.iterator` 方法
2. 该方法返回一个 **迭代器对象**

这个迭代器对象必须有一个 `next()` 方法，每次调用返回一个对象：

```js
{
    value: 当前值,
    done: 是否遍历结束
}
```

不知道大家是否发现，这跟我们之前介绍的生成器得到的阶段性结果如出一辙，别着急，真相很快就会浮出水面了。

我们先来研究一下之前遇到过的一些对象，比如数组，数组本身就是可迭代对象：

```js
const arr = [1, 2, 3]
const iterator = arr[Symbol.iterator]()  //取出迭代器

//推动迭代器向下执行
iterator.next() // { value: 1, done: false }
iterator.next() // { value: 2, done: false }
iterator.next() // { value: 3, done: false }
iterator.next() // { value: undefined, done: true }
```

这里可以清楚看到，每次 `next()` 都“取一下个值”，当`done: true` 表示结束，这其实就是迭代器的作用，包括我们之前介绍的生成器也是：

```js
function* gen() {
    console.log("我是第一阶段")
    yield
    console.log("我是第二阶段")
    yield
    console.log("我是第三阶段")
}

const generator = gen()
generator.next()   //实际上生成器本身就是个迭代器
```

我们前面为大家介绍的`for...of` 本质上 **就是在不停调用迭代器的 next 方法**：

```js
for (const item of arr) {
    console.log(item)
}
```

等价于：

```js
const iterator = arr[Symbol.iterator]()
let result = iterator.next()

while (!result.done) {
    console.log(result.value)
    result = iterator.next()
}
```

这下，所有的事情都说得通了。除此之外，针对于可迭代对象，我们还可以使用之前介绍的`...`展开运算符来将所有结果展开：

```js
const numbers = gen();
console.log([...numbers])
```

到目前为止，我们已经学习过的可迭代对象有：Array、String、arguments、Generator，而普通对象**不是可迭代的**，必须符合我们上面所说的条件。

### Map字典

`Map` 是 ES6 新增的一种 **键值对结构**，可以理解为：专门为“数据映射”而生的结构。

要使用Map，我们只需要使用`new`创建一个新对象即可：

```js
const map = new Map()
```

`Map`和对象非常类似，它可以存储键值对，也就是对象的属性和属性值。我们可以使用`set`方法来向其中插入一个新的键值对：

```js
map.set("name", "小明")  //第一个参数就是键，第二个是值
map.set("age", 18)
```

![image-20260210220658384](https://files.seeusercontent.com/2026/02/11/l3bO/image-20260210220658384.png)

注意，键并不要求必须使用字符串，它可以是任意类型的，这与对象的属性有很大不同：

```js
map.set(666, "申请")
map.set(null, "起飞")  //哪怕是null也可以作为键
```

不过，如果添加同样的键到`Map`中，依然是会覆盖之前的键值对：

```js
map.set(null, 18)
map.set(null, "起飞")   //以最后一次设置为准
```

我们可以使用`size`属性来查看`Map`当前存储了多少个键值对：

```js
console.log(map.size)  //不能修改，只能获取
```

当然，如果我们需要获取某个键值对的值，也可以直接使用`get`方法来获取，传入指定的键就行，就像之前访问对象属性那样：

```js
console.log(map.get(null)) 
console.log(map.get("666"))  //注意这里也是判断的严格相等
```

如果不存在这个键值对，那么返回一个`undefined`作为结果。此外，我们还可以使用`has`来判断某个属性是否存在，或者使用`delete`来删除一个键值对，都只需提供键即可：

```js
map.has("age")    // true
map.delete("age")   //删除成功返回true反之false
map.clear()   //清除全部键值对
```

最后，`Map`还提供了三个用于遍历的方法：`keys()`、`values()` 和 `entries()`，它们都返回一个**迭代器对象**（Iterator），可以通过 `for...of` 或 `next()` 方法进行遍历：

```js
const keys = map.keys();
 
// 方式1: 使用 for...of 遍历
for (const key of keys) {
  console.log(key); // 'name', 25, true
}

// 方式2: 使用 next() 手动迭代
console.log(keys.next().value); // 'name'
console.log(keys.next().value); // 25
console.log(keys.next().value); // true
console.log(keys.next().done);  // true（迭代结束）
```

像是一些单纯做“数据映射”，而不是描述一个实体的场景，就很适合使用`Map`作为数据结构。

### Set去重集合

`Set` 表示一种 **不允许重复值的集合结构**，它类似于数组，但是其中所有的值都只能存在一个，不能重复出现。要创建一个`Set`对象我们可以创建对象：

```js
const set = new Set()
```

我们可以使用`add`方法来为`Set`集合添加新的元素：

```js
const set = new Set()
set.add(222)
set.add(333)
console.log(set)
```

注意，在插入重复元素时，会直接去除：

```js
const set = new Set()
set.add(222)
set.add(222)   //依然是使用严格等于进行判断的
console.log(set)   //Set(1) {222}
```

和`Map`一样，`Set`也可以使用`size`来获取当前的元素数量：

```js
console.log(set.size)
```

此外，我们还可以使用`has`来判断某个元素是否存在，或者使用`delete`来删除一个属性：

```js
set.has(2)     // true
set.delete(1)   //删除成功返回true反之false
set.clear()   //清除全部元素
```

由于Set的底层实现是我们数据结构中介绍的哈希表，所以在查询是否存在某个元素的时候性能非常好，比数组的`includes`快很多。

此外，利用`Set`的特性，我们就可以很轻松地实现数组的去重：

```js
const arr = [1, 2, 2, 3]
const result = [...new Set(arr)]
```

需要注意的是，Set不支持像数组一样能够随机访问，虽然它保持了元素的顺序，但是不允许指定下标位置访问。

最后，`Set`也是可迭代对象，用法和数组大差不差：

```js
for (const value of set) {
    console.log(value)
}
```

`Set`非常适合状态标记、唯一值收集等场景。

### 弱引用结构（选学）

弱引用结构主要包括：`WeakMap`和`WeakSet`，这一部分主要和 **垃圾回收** 有关，属于选学内容。

首先介绍一下`WeakMap`，它和 `Map` 类似，但有两个关键限制：

1. key **只能是对象**
2. key 是 **弱引用**

我们来尝试使用一个普通的Map进行属性存储：

```js
const wm = new Map()
let obj = {}

const registry = new FinalizationRegistry(heldValue => {
    console.log(`${heldValue}对象被垃圾回收了`)
})
registry.register(obj, "测试")

wm.set(obj, "data")
obj = null   //取消对这个对象的引用
```

我们可以测试一下垃圾回收，接着会发现垃圾这个对象并没有被回收，因为即使外面的变量取消了对这个对象的引用，但是`Map`还存有这个对象的引用，我们依然可以通过这个`Map`获取到这个对象，所以不会被垃圾回收。

我们也可以正常创建一个新的`WeakMap`对象：

```js
const wm = new WeakMap()
```

此时再来测试：

![image-20260210223603038](https://files.seeusercontent.com/2026/02/11/6Yug/image-20260210223603038.png)

我们发现，当使用`WeakMap`时，测试对象一旦没有其他地方的引用，那么将会被视为垃圾回收。这正是`WeakMap`的效果，它对于对象的引用实际上是一个弱引用，当对象在其他地方不再被需要时，它随时可以被回收。并且在垃圾回收之后，这个键值对将消失。

`WeakSet`和上面是同理的，这里不做详细介绍了。

## Promise专题

在 JavaScript 中，**异步几乎无处不在**。

> 同步：任务按顺序执行，必须等待前一个任务完成后，下一个任务才能开始。
>
> 异步：任务无需等待前序任务完成，多个任务可以同时进行。

我们前面学习的定时器 `setTimeout`，实际上就是一种异步行为，它会在一段时间之后再执行，而不影响我们正常后续的代码。早期 JS 使用 **回调函数** 来处理异步，但这种方式很快就会变得失控，这也是 Promise 出现的原因。

### 回调函数的问题

我们先来看一个最原始的异步写法，这里我要分阶段执行三个定时任务，上一个任务结束之后才能继续下一个，那么我们可能会像这样写：

```js
setTimeout(() => {
    console.log("第一步")
    setTimeout(() => {
        console.log("第二步")
        setTimeout(() => {
            console.log("第三步")
        }, 1000)
    }, 1000)
}, 1000)
```

虽然说确实这样写着没毛病，但是这种代码非常臃肿，随着我们嵌套的层数越来越多，代码 **向右无限缩进**，变得难以阅读，这就是经典的 **回调地狱（Callback Hell）**，因此，Promise 的目标只有一个：把“未来才会完成的事情”，包装成一个对象，让代码写得像同步一样清晰。

### 走进Promise

**Promise 是一个对象**，用于表示一个**尚未完成，但将来一定会有结果的操作**。它有三个状态：

| 状态      | 含义   |
| --------- | ------ |
| pending   | 进行中 |
| fulfilled | 已成功 |
| rejected  | 已失败 |

注意：Promise **状态一旦改变，就不可逆**，初始状态都是`pending`，而状态流转只有两条路：pending → fulfilled 或是 pending → rejected。

Promise 通过 `new Promise()` 创建，构造函数接收一个函数参数：

```js
const p = new Promise((resolve, reject) => {
    // 执行异步操作
})
```

这里的`resolve`需要再我们执行完成之后进行调用，来告诉`Promise`这个任务已经完成了。`reject`用于告诉`Promise`这个任务执行失败了，类似于之前正常执行过程中抛出错误的感觉。

这样，我们就可以把一些异步任务放到`Promise`里面：

```js
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
      try {
        resolve("成功结果")  //在resolve传入执行完成之后的结果，如果没有就填undefined
      } catch(err) {
        reject(err)   //在reject中传入失败原因，一般是错误对象本身
      }
    }, 1000)
})
```

这里的`setTimeout`也会直接执行，但是当执行完成之后，会通过Promise提供的两个回调函数来进行通知。接着，我们可以使用Promise 提供的 `then` 方法，来接收成功的结果：

```js
p.then(data => {   //对刚刚创建的Promise对象使用then
    console.log(data)
})
```

我们可以使用`then`提前编写好这个任务完成之后的处理逻辑，当这个任务在1秒钟之后完成执行时，`resolve`会通知这边任务完成，接着就会执行`then`中的回调函数，并且这里的参数实际上就是刚刚传入的成功结果。

当然，`then`的最大特点是它会返回一个新的 Promise 对象，我们在上一个`then`阶段中执行的返回值，会被自动包装成新的Promise对象的`resolve`值，我们可以继续在这个对象中编写上一个`then`处理完成之后的结果：

```js
p.then(data => {
    console.log(`第一次then得到: ${data}`)
    return 6666
}).then(data => {
    console.log(`第二次then得到: ${data}`)
})
```

![image-20260210231039492](https://files.seeusercontent.com/2026/02/11/G1nr/image-20260210231039492.png)

利用这种机制，我们就可以连续进行多段处理了，此外，当返回值是一个新的Promise对象时，这里会直接将这个新的Promise对象作为结果返回。

```js
p.then(data => {
    console.log(`第一次then得到: ${data}`)
    return Promise.resolve(666)   //直接使用静态方法返回一个已完成的Promise
}).then(data => {
    console.log(`第二次then得到: ${data}`)
})
```

所以，上面的回调地狱，就被我们成功解决了，Promise 能“拉直”回调地狱：

```js
new Promise(resolve => {
    setTimeout(() => resolve(), 1000)
}).then(() => {
    return new Promise(resolve => setTimeout(() => resolve(), 1000))
}).then(() => {
    return new Promise(resolve => setTimeout(() => resolve(), 1000))
})
```

总结一下，`then()` 方法会返回一个**新的 Promise 对象**，这是实现链式调用的关键。每次调用 `then` 时：

- 它会等待前一个 Promise 状态变为 `fulfilled`（已完成）
- 执行传入的回调函数
- 根据回调函数的返回值，决定新 Promise 的状态：
  - 如果返回一个值，新 Promise 会以该值 `fulfilled`
  - 如果返回另一个 Promise，新 Promise 会跟随这个 Promise 的状态
  - 如果抛出异常，新 Promise 会以该异常 `rejected`

下一节，我们接着来为大家介绍出现错误的情况。

### 错误处理

当我们的任务执行失败时，就可以使用`reject`来告诉`Promise`任务失败了，并返回一个错误的结果。使用`reject`或是直接在回调函数中抛出错误都会使得Promise失败：

```js
new Promise((_, reject) => {
    reject(new Error("发生了疯狂星期四vivo50的错误"))  //直接失败吧
}).then(() => {
    console.log("我是正常完结")
})
```

此时我们会发现，`then`并没有执行，因为Promise执行失败了，它的状态变成了`rejected`，如果我们不做任何处理，那么会在控制台出现一个错误信息：

![image-20260210234708624](https://files.seeusercontent.com/2026/02/11/7zIj/image-20260210234708624.png)

要处理这些失败的情况，我们可以使用`catch`方法：

```js
new Promise((_, reject) => {
    reject(new Error("发生了疯狂星期四vivo50的错误"))
}).then(() => {
    console.log("我是正常完结")
}).catch((err) => {
    console.log(`我是出问题了: ${err}`)
})
```

除了在异步操作中出现问题之外，只要链路中某一步出错，依然会进入到`catch`里面：

```js
new Promise(resolve => {
    resolve()
}).then(() => {
    throw new Error("送你个大报错")
    //return Promise.reject("送你个大报错") 或是使用静态方法直接返回一个已失败的Promise
}).catch((err) => {
    console.log(`我是出问题了: ${err}`)
})
```

这种情况也会进入到`catch`中，即使最初的那个异步任务是正确`fulfilled`结束的，后续过程中如果出现问题依然会进入到`catch`里面，同时，只要链路中某一步出错，后续 `then` 会被跳过，直到遇到 `catch`：

```js
new Promise(resolve => {
    resolve()
}).then(() => {
    throw new Error("送你个大报错")
}).then(() => {
    console.log("我是正常继续")   //不会执行，被跳过
}).catch((err) => {
    console.log(`我是出问题了: ${err}`)
})
```

只不过，`catch`和`then`一样，也会返回一个新的Promise对象，所以，如果是`catch`之后的`then`，那么是不会受到影响的：

```js
new Promise(resolve => {
    resolve()
}).then(() => {
    throw new Error("送你个大报错")
}).catch((err) => {
    console.log(`我是出问题了: ${err}`)
}).then(() => {
    console.log("我是正常继续")   //正常执行
})
```

和前面介绍的`try-catch`一样，我们还可以在最后添加一个`finally`方法：

```js
new Promise(resolve => {
    resolve()
}).then(() => {
    throw new Error("送你个大报错")
}).finally(() => {
    console.log("一定会执行")
})
```

无论上面是否出现问题，这里的`finally`一定会执行，但是由于没有处理失败的情况，所以这里控制台依然会有错误信息。需要注意的是，`finally`仍然返回的是一个Promise对象，所以仍然有可能继续向后拼接，但是不推荐大家使用这种方式，正常情况下一般都是一个`catch`一个`finally`收尾即可。

最后需要提到的是，从 ES2024 开始，`Promise.withResolvers()` 正式成为标准API，它可以实现一种更加快速的异步回调和Promise的转换操作：

```js
//使用解构语法直接拿关键东西
const { promise, resolve, reject } = Promise.withResolvers()
setTimeout(() => resolve("任务执行完成"), 1000)
promise.then(console.log)
```

它可以直接得到一个Promise对象，以及这个对象所属的`resolve`和`reject`方法，这样，我们就可以在任何位置对这个Promise结束或是失败，使用灵活性大大提高。但是注意，如果不去手动执行`resolve`或是`reject`，那么这个Promise将永远不能结束，永远处于`pending`状态，永远占用资源。

### 并发控制

这一节我们来看看然后进行更加高级的并发控制操作。在实际开发中，我们经常需要**同时发起多个异步任务**：

```js
const p1 = new Promise(resolve => {
    setTimeout(() => {
        console.log("我是一号结束执行")
        resolve("一号结束")
    }, 2000)
})
const p2 = new Promise(resolve => {
    setTimeout(() => {
        console.log("我是二号任务执行")
        resolve("二号结束")
    }, 3000)
})
```

Promise为我们提供了大量静态方法用于在多个异步任务下的并发控制，我们首先来介绍一下最简单的一个，`Promise.all()`可以实现等待所有Promise完成之后再执行：

```js
//将多个Promise以对象形式传入
Promise.all([p1, p2]).then(res => {
    console.log(res) // ['一号结束', '二号结束']
})
```

此时，`all`会对数组中的所有Promise 进行等待，直到它们完成。这里返回的也是一个Promise对象，当所有的Promise都完成之后，这个总的Promise才会变成`fulfilled`状态，继续执行它所属的一些`then`和`catch`。注意，如果其中一个任务失败了，那么这个总的Promise也会立即宣告失败：

```js
Promise.all([p1, p2]).then(res => {
    console.log(res) // ['一号结束', '二号结束']
}).catch(err => {
    console.log(`失败了${err}`)
})
```

当其中一个Promise失败之后，`Promise.all`得到的总Promise会立即失败，不会继续等待其他未完成的Promise了（但依然会执行）我们可以在后续的`catch`中处理问题（这里其实有一个问题，后续任务即使出现问题，也没办法处理了）

接下来是`Promise.any`，如果说`all`是与运算，那么它类似于或运算，也就是说只要其中一个Promise完成了，`any`生成的总Promise将立即变为`fulfilled`，直接开始后续的`then`和`catch`：

```js
Promise.any([p1, p2]).then(data => {
    console.log(data)
})
```

当然，其他未完成的任务依然会继续完成，但是结果会被忽略。不过，如果其中一个Promise出现了错误执行失败，那么这里并不会立即变成`rejected`状态，而是会继续等待其他Promise完成，只要有其中一个正确完成，那么这里就可以正常变为`fulfilled`。所以：

* `Promise.all` 要求所有Promise都正确完成。
* `Promise.any` 只需要有至少一个Promise能正确完成即可。

此外，还有一种类似于`any`的静态方法，`Promise.race`也可以实现先完成任务的Promise立即得到结果：

```js
Promise.race([p1, p2]).then(data => {
    console.log(data)
})
```

但是注意，无论第一个完成的Promise是成功还是失败，`race`都会立即更新状态，如果成功就是`fulfilled`，如果失败就是`rejected`，这也是它和`any`不同的地方。

接着是`Promise.allSettled`，它类似于上面的`all`，同样是需要等待所有Promise完成：

```js
Promise.allSettled([p1, p2]).then(data => {
    console.log(data)
}).catch(err => {   //实际上不会执行
    console.log(`失败了${err}`)
})
```

但是需要注意的是，它不会在遇到某一个Promise失败时立即得到`rejected`，而是依然进行等待，直到所有都结束。然后再进行综合，然后直接更新为`fulfilled`状态（无论有没有Promise失败）并在`then`中返回一个数组：

![image-20260211003653923](https://files.seeusercontent.com/2026/02/11/2rHa/image-20260211003653923.png)

这个数组中记录了所有Promise的执行结果，我们可以通过结果判断每个Promise是否执行成功。

### async与await

在前面的章节中，我们已经系统学习了 **Promise**，相信大家应该已经能写出结构正确的异步代码了，我们通过Promise简化了之前的回调地狱，但是新的问题来了：

```js
.then(user => {
  return getOrders(user.id)
})
.then(orders => {
  return getDetail(orders[0].id)
})
.then(detail => {
  console.log(detail)
})
.catch(err => {
  console.log(err)
})
```

虽然我们解决了回调嵌套导致的混乱代码问题，但是并没有真正意义上消除回调函数带来的臃肿写法，你会发现使用Promise之后仅仅只是从横向嵌套变成了纵向嵌套。

这正是 **async / await 要解决的问题**，它们是一种更加简便的写法。我们可以将一个函数声明为`async`：

```js
async function fn() {
    return 100
}
```

你可能会觉得：这不就是个普通函数吗？但实际这个函数得到的是一个Promise对象：

![image-20260211010108476](https://files.seeusercontent.com/2026/02/11/3iiJ/image-20260211010108476.png)

实际上，当一个函数被声明为`async`时，它的返回值会被自动包装成一个`fulfilled`状态的Promise对象，就像我们在`then`中返回一样。此外，我们也可以直接返回一个Promise对象作为结果：

```js
async function fn() {
    return Promise.resolve("任务完成了")
}

fn().then(data => {   //可以直接对这个函数的结果使用then
    console.log(data)
})
```

同样的，当这个函数内部出现错误或是手动调用了`reject`，那么这里也会得到一个`rejected`状态的返回值。所以，直接调用一个`async`函数，会直接将返回值作为结果包装，如果没有返回值，那么也会得到一个结果为`undefined`的Promise对象。

但是，光有`async`还不行，它只是简单得到一个Promise对象作为结果，我们需要配合`await`来实现等待结果：

```js
async function fn() {
  	//使用await来等待一个Promise完成，此时会卡住等待Promise状态更新，完成后Promise的结果直接作为返回值
  	//如果await后面的不是Promise，那么会直接包装成已完成状态的Promise对象
    const result = await new Promise(resolve => {
        setTimeout(() => resolve("任务完成了"), 1000)
    })
    return result
}
```

`await`只能放到`async`函数的内部，无法在其他非`async`函数中使用。它的效果是等待后面的Promise完成，当完成之后，会直接得到Promise的结果作为值，然后再继续执行后续代码，类似于暂停的效果。如果Promise执行失败，这里直接抛出错误：

```js
async function fn() {
    //使用await来等待一个Promise完成，此时会卡住等待Promise状态更新，完成后Promise的结果直接作为返回值
    const result = await new Promise((resolve, reject) => {
        setTimeout(() => reject("任务失败了"), 1000)
    })
    return result
}
```

![image-20260211011532951](https://files.seeusercontent.com/2026/02/11/5rRi/image-20260211011532951.png)

相当于这个`saync`函数执行失败，返回的Promise状态也会更新为`rejected`。有了`await`之后，你会发现，**代码从“回调结构”，变成了“顺序结构”**。我们还是以之前的回调地狱为例，此时代码就可以写成这样：

```js
async function fn() {
    //使用await对每个任务依次等待
    await new Promise(resolve => setTimeout(() => resolve("任务一完成了"), 1000))
    await new Promise(resolve => setTimeout(() => resolve("任务二完成了"), 1000))
    await new Promise(resolve => setTimeout(() => resolve("任务三完成了"), 1000))
}

fn().then(() => {
    console.log("任务全部完成啦")
})
```

可以看到，我们前面的一连串回调函数被全部做成顺序调用的形式了，我们通过`async/await`消除了大量的回调函数，它是`then`链的一种替代写法，能够让代码更加简洁，所以很多人说`await`就是`Promise.then`的语法糖，本质上还是不断的`then`调用。

当然，很多新手有一个误区，就是执行`async`函数时的同步异步问题：

```js
console.log(3)
fn()   //fn会直接得到一个pending状态的Promise，而不是阻塞
console.log(4)  //所以继续执行，而不是等fn执行完
```

实际上，`await` **只会阻塞当前 async 函数**，不会阻塞主线程。同样的，直接在外面`await`用也是不行的：

```js
await fn()
console.log("fu执行完了")
```

![image-20260211012359332](https://files.seeusercontent.com/2026/02/11/9mrH/image-20260211012359332.png)

只不过，它可以在ES模块的顶级作用域使用，有关模块化的概念我们会在后面进行介绍。需要注意的是，如果多个Promise需要并发执行而不是顺序执行，我们可以考虑使用`all`来进行整合：

```js
async function fn() {
    //使用await对总Promise进行等待，这样就能实现并发执行
    await Promise.all([
        new Promise(resolve => setTimeout(() => resolve("任务一完成了"), 1000)),
        new Promise(resolve => setTimeout(() => resolve("任务二完成了"), 2000)),
        new Promise(resolve => setTimeout(() => resolve("任务三完成了"), 3000))
    ])
}

fn().then(() => {
    console.log("fu执行完了")
})
```

很多人说，`async/await`是 **Promise** 的语法糖，这种说法不完全正确，实际上它的实现原理是我们之前介绍的生成器，我们可以用 Generator 手动模拟其行为：

```js
function* generator() {
    yield new Promise(resolve => {
        setTimeout(() => resolve("任务一完成了"), 1000)
    })
    yield new Promise(resolve => {
        setTimeout(() => resolve("任务二完成了"), 1000)
    })
}

function fn() {
    const iterator = generator();

    function handle(result) {
        if (result.done) return Promise.resolve(result.value);
        return Promise.resolve(result.value)
            .then(res => handle(iterator.next(res)))
            .catch(err => handle(iterator.throw(err)));
    }

    return handle(iterator.next());
}

fn().then(() => {
    console.log("fu执行完了")
})
```

所以，`await`本质上是 **Generator 函数 + Promise** 的语法糖，核心是依靠生成器来实现的暂停效果，当然，我们无需在关心它最终会等价于什么样的代码，直接爽用就完了。

### 异步迭代

既然普通函数可以变成`async`，那么生成器也可以是异步生成器 `async function*`

```js
async function* gen() {
  yield 1
  yield await Promise.resolve(2)
  yield 3
}
```

可以看到，虽然这也是一个生成器，但是它的每个阶段上有可能会出现`await`进行等待，并且最终的返回结果也是一个Promise：

```js
const generator = gen();
console.log(generator.next());   //得到一个Promise对象
```

这样，我们就可以实现：

```js
function delay(val, ms) {
  return new Promise(resolve => {
    setTimeout(() => resolve(val), ms)
  })
}

async function* gen() {
  yield await delay(1, 1000)
  yield await delay(2, 1000)
  yield await delay(3, 1000)
}
```

可以看到，这里的`yield`的结果就是等到Promise结束的值。所以每次`next`被调用时，都会由于`await`进行等待，直到出现结果后，才能从`next`得到的Promise进行通知，并暂停生成器。

```js
async function test() {
    const generator = gen();   //得到异步迭代器
    const data = await generator.next()  //等待Promise结束
    console.log(data)  //得到此阶段结果
}
```

我们也可以尝试遍历这个生成器：

```js
for (const x of generator) {
    const data = await x
    console.log(data)
}
```

但是程序会得到错误的结果，因为这种方式看似可以，实际上并不支持，因为`for..of`不支持异步迭代器。

对于这种情况，我们可以使用`for await of`语句，效果等价：

```js
for await (const value of gen()) {  //专用于异步迭代器的for..of
    console.log(value)
}
```

至此，有关Promise的内容，就全部为大家介绍完毕了。

### 事件循环（选学）

实际上，JS的执行是单线程的，所谓单线程，就像是同一时间只有一个人来做事情，他手上如果已经在做事情了，那么其他的事情需要等待他干完手上的活才能继续。这个时候，很多新手都会产生一个巨大的疑问：

> JS 不是单线程吗？那为什么 setTimeout、Promise、网络请求看起来像是“同时在跑”？

答案就藏在 JavaScript 非常核心的一套机制中：**事件循环（Event Loop）**理解事件循环，你就理解了整个JS程序的执行底层原理和流程，这对你未来的发展非常关键，这几乎是每一个JS开发者深入底层前必须掌握的知识点。

我们先来看一个非常让人费解的现象：

```js
console.log("A")
setTimeout(() => {
    console.log("B")
}, 0)   //这里延迟0，那应该是立即执行才对
console.log("C")
```

实际上，这个代码并没有按照顺序进行，哪怕`setTimeout`的延迟为`0`。

我们来逐步剖析这个问题，实际上在 JS 中，有一个非常重要的结构，叫做 **执行栈（调用栈 / Call Stack）**，为了维护函数的调用顺序，它存储着当前正在执行的函数队列（后进先出）比如：

```js
function a() {
    b()
}

function b() {
    console.log("B")
}

a()
```

执行过程是：

1. `a()` 入栈
2. `b()` 入栈
3. `console.log` 执行
4. `b()` 出栈
5. `a()` 出栈

只要**执行栈不为空**，JS 就不会去处理任何异步任务。当 JS 遇到异步 API（比如 `setTimeout`）时，并不是 JS 自己在计时：

```js
setTimeout(fn, 1000)
```

真实过程是：JS 把 `setTimeout` 交给 **浏览器** -> 浏览器负责计时 -> 时间到了，把回调函数交还给 JS，也就是说，异步任务本身，不在 JS 的执行栈里。

当异步任务完成后（比如时间到了），回调函数并不会立刻执行，而是会被放进到**任务队列（Task Queue）**中。而事件循环，其实就是不断检查函数的执行栈是否为空，当JS的执行栈是空时，就会从任务队列取出一个任务执行，就像是两波人错峰执行任务一样。这种错峰执行的机制，就是 JS 能在单线程下处理“并发”的根本原因。

所以，这里我们来总结一下上面的ABC为什么顺序不正确，还是按照执行顺序分析：

1. 首先`console.log("A")`这是同步任务，直接在主线程中执行，控制台输出。
2. 遇到 `setTimeout(() => { console.log("B") }, 0)` 它是一个**异步任务**，被交给浏览器并进行计时。
3. 由于时间本来就是`0`，会立即把回调函数 `() => { console.log("B") }` 放入**任务队列**。
4. 此时主线程不会等待，而是继续执行后面的同步代码 `console.log("C")`。
5. 当主线程中的所有同步任务执行完后，会检查任务队列，此时任务队列中有一个任务：`console.log("B")`
6. 主线程会把这个任务取出，放入执行栈中执行，控制台输出：`B`。

所以代码的最终输出顺序是：ACB，这下，你应该理解了JS的异步原理和具体执行流程了，所以本质上它依然是同步的。通过这种机制，我们也可以推出，`setTimeout`其实是不准的，如果主线程很忙，那么实际执行时间可能远远超过 1 秒。

我们接着来详细介绍一下任务队列，在任务队列中，任务并不是只有一种。第一类任务，叫做 **宏任务（Macro Task）**，常见的有：

- `setTimeout`
- `setInterval`
- `setImmediate`（Node）
- DOM 事件回调
- 网络请求回调

除了宏任务，还有一类**优先级更高**的任务，叫做 **微任务（Micro Task）**常见的微任务有：

- `Promise.then / catch / finally`
- `await`之后的代码（因为后续代码本质就是Promise.then，也就是创建了新的微任务）
- `queueMicrotask`
- `MutationObserver`

事件循环的执行顺序是：执行当前宏任务（通常是整段同步代码） ->  执行过程中产生的 **所有微任务 ** -> 微任务清空后 -> 才会执行下一个宏任务，所以**微任务永远比宏任务先执行**，实际上会有两个不同优先级的任务队列，一个是宏任务队列，一个是微任务队列。我们来看这个例子：

```js
console.log("A")
setTimeout(() => console.log("B"), 0)
Promise.resolve().then(() => console.log("C"))
console.log("D")
```

我们来拆解一下它的执行过程：

1. 同步代码执行，也就是说这里会先得到 A
2. `setTimeout` 是宏任务，所以进入宏任务队列
3. `Promise.then` 是微任务，所以直接放进微任务队列
4. 同步代码执行，也就是说这里会得到 D，此时同步代码执行完毕
5. 按照规则，此时微任务队列中存在微任务，优先执行微任务，得到 C
6. 最后再从宏任务队列中执行宏任务，得到 B

最终输出顺序是，A、D、C、B，需要注意的是，如果微任务执行过程中，如果又产生新的微任务，会继续执行，直到清空，例如：

```js
setTimeout(() => console.log(3), 0)
Promise.resolve().then(() => {
    console.log(1)
    Promise.resolve().then(() => {  //内部调用的then等于下崽
        console.log(2)
    })
}).then(() => {  //连续调用的then也等于下崽
    console.log(4)
})
```

这里的结果是：1、2、4、3，因为微任务下崽了，此时有新的微任务进入队列，那么还得继续处理完再去管宏任务。当然，这里介绍的事件循环机制是基于Web环境，在Node环境中可能会稍有不同，但主要思路是大体相同的。

## 模块化

在 JavaScript 中，**模块就是一个“独立作用域的代码单元”**，它通常具备几个特征：有自己独立的作用域、对外只暴露指定的接口、内部实现细节对外不可见、可以被其他模块复用。

在没有模块化之前，常见写法是这样的：

```js
//script.js
const a = 100
```

```js
//test.js
const a = 200
```

此时在同一个HTML文件中引入这两个JS文件：

```html
<script src="js/script.js"></script>
<script src="js/test.js"></script>
```

这种情况下，浏览器会直接报错。我们知道，很多时候我们都需要用到别人提供的JS库，如果别人的库和我们的变量存在冲突，那么岂不是就乱套了？所以，模块化就是为了解决这个问题而生的。

### 模块化的发展

在 JavaScript 诞生之初，它的定位其实非常简单：**给网页加点交互效果**，当时的 JavaScript 代码规模通常只有几十行，甚至直接写在 HTML 里：

```html
<script>
  var count = 0
  function add() {
    count++
  }
</script>
```

在这种体量下，**根本谈不上模块化**，因为所有代码都跑在同一个作用域中，看起来也“没什么问题”。随着前端逐渐变复杂，代码开始被拆分成多个文件：

```html
<script src="a.js"></script>
<script src="b.js"></script>
<script src="c.js"></script>
```

但问题也随之而来：

* 所有变量默认挂在全局作用域
* 不同文件之间**极易发生命名冲突**
* 文件加载顺序强依赖 HTML 中的书写顺序
* 无法明确表达“谁依赖谁”

后加载的文件会直接覆盖前面的变量或是与前面的变量发生冲突，这在大型项目中几乎是灾难。为了解决全局污染问题，开发者开始**主动制造“命名空间”**：

```js
var App = {}   //利用一个特殊名称的对象来存储当前命名空间下的变量和函数

App.count = 0
App.add = function () {
  App.count++
}
```

虽然这种方式可以在一定程度上避免大量全局变量导致的冲突，但是它本质还是全局变量，依然有可能出现冲突，也无法真正隐藏内部实现，甚至容易被意外修改。后来，大家开始利用**函数作用域**来“模拟模块”：

```js
var counter = (function () {
  var count = 0   // 私有变量

  function add() {
    count++
    return count
  }

  return {   //返回一个对象，将函数内部的属性通过返回值给出去，这样外面就没办法看到实现细节了
    add
  }
})()
```

这种写法第一次真正实现了私有变量，内部实现不可直接访问，但问题也很明显，这种写法太繁琐了。随着前端工程化需求的爆发，社区开始尝试**标准化模块方案**：

* **CommonJS**（Node.js）
* **AMD**（浏览器异步加载）
* **CMD**（SeaJS）

这些规范第一次提出了统一思想：一个文件就是一个模块，这一步，标志着 JavaScript **真正进入模块化时代**。虽然社区方案解决了很多问题，但它们都存在一个共同点：不是 JavaScript 语言本身的一部分。直到 ES6（ES2015），官方终于给出了答案，引入了`import`和`export`关键字，至此，模块化正式成为 **语言级能力**，不再依赖外部规范或工具。

### ES Module

前面我们已经看到，CommonJS、AMD 等模块方案，本质上都属于**社区规范**。虽然它们解决了实际问题，但始终存在一个核心缺陷：模块化并不是 JavaScript 语言自身的能力。直到 ES6（ES2015），JavaScript 才正式引入官方模块方案 —— **ES Module（简称 ESM）**

在 ES Module 中，有一个非常重要的规则，只要使用了 `import` 或 `export`关键字，那么这个文件就会被当成一个模块

```js
export const a = 200
```

这个文件天然具备独立作用域，不污染全局，所有需要暴露的属性使用`export`来明确。要引入模块化的JS文件，我们需要在引入时添加类型：

```html
<script type="module" src="js/test.js"></script>
```

我们可以使用`export`导出所有我们想要导出给外面的内容：

```js
//导出常量和变量
export const age = 18
export let count = 0

//导出函数
export function sayHi() {
    console.log("Hi")
}

//导出类（本质就是导出构造函数）
export class Student {}
```

你也可以把这些需要导出的东西统一装到一个对象中一起导出：

```js
const a = 1
const b = 2

export { a, b }
```

不过需要注意的是，`export` **只能写在顶层作用域**，不能写在 if / for / 函数这类代码块内部。一旦使用模块化机制，这些导出的属性无法直接被访问，需要导入之后才可以：

![image-20260211120535502](https://files.seeusercontent.com/2026/02/11/9fOo/image-20260211120535502.png)

在其他ES模块使用`import`来进行导入，同样只能在最外层使用，这里建议大家写到文件的最顶部：

```js
//导入其他模块的age变量，属性需要使用花括号囊括，因为导出的内容不止一个，这里类似于解构
import {age} from "./test.js"

console.log(age)
```

注意这里的`from`后面是导入的文件路径，这个路径必须是静态的，不能是变量，并且变量或者函数的名称必须与导出一致，当然，如果导入的属性和当前模块的变量名称冲突，我们也可以使用`as`来起别名：

```js
import { age as testAge } from "./test.js"

console.log(testAge)
```

ES Module 还支持**默认导出**，但是注意一个模块只能有一个：

```js
export default function () {
    console.log("default")
}
```

当属性采用默认导出时，我们导入可以直接使用一个自定义的名字：

```js
//因为默认导出只有一个，所以名字可以随便起，也不需要花括号
import test from "./test.js"

console.log(test)
```

当然同时存在普通导出和默认导出时，我们可以混合使用：

```js
//默认导出依然随便起名字，其他属性继续花括号
import test, { age } from "./test.js"

console.log(test)
```

最后注意的是，在浏览器中，使用 ES Module 会自动具备以下特性：

- 默认使用 `defer`属性，开启异步加载，不会阻塞HTML渲染
- 自动开启严格模式
- 支持顶级 `await`（模块环境）

由于模块本身就是异步加载的，当我们采用模块化时，JS文件将直接支持在最外层作用域中直接使用`await`：

```js
async function test() {
    const { promise, reject, resolve } = Promise.withResolvers()
    setTimeout(() => resolve(), 1000)
    return promise
}

await test()  //会阻塞后续语句执行
console.log("我是后续语句")
```

有关严格模式的内容，我们将在下一节继续介绍。

### 严格模式

**严格模式（Strict Mode）** 是 JavaScript 提供的一种**更严格的语法和运行规则**，它的目标只有一个：让代码更安全、更规范、更容易发现错误。在严格模式下，一些“以前能跑但其实不合理”的代码，会**直接报错**。

在早期 JavaScript 中，为了“尽量不报错”，语言本身允许了大量**不严谨的写法**，例如：

```js
a = 10   // 实际上根本没有声明变量
console.log(a)
```

这段代码由于没有开启严格模式，即使我们不使用关键字，也会自动创建一个变量`a`，这显然是不合理的。而这行代码在严格模式下，将在执行阶段直接报错，我们只需要在JS文件的顶部写上`"use strict"`表示开启严格模式：

```js
"use strict"

a = 10   //直接报错
```

严格模式的作用，就是**把这些潜在问题直接暴露出来**，防止后续出现的一些安全隐患。除了在全局作用域下使用严格模式，我们也可以在局部作用域使用：

```js
function fn() {
  "use strict"
  x = 10   // ❌ 报错
}
```

但是无论处于哪个作用域，`"use strict"`只能写到所属作用域的最顶部。

此外，在严格模式下，最外层作用域的`this`实际上不指向任何对象：

```js
function fn() {
  console.log(this)   //undefined
}

fn()
```

这可以防止`this`意外指向全局对象，隐式污染全局状态，有关严格模式的所有限制大致如下：

- 变量必须声明后再使用
- 函数参数不能有同名属性，否则报错
- 不能使用 `with` 语句
- 不能对只读属性赋值，否则报错
- 不能使用前缀 0 表示八进制数，否则报错
- 不能删除不可删除的属性，否则报错
- 不能删除变量 `delete prop`，会报错，只能删除属性 `delete global[prop]`
- `eval` 不会在它的外层作用域引入变量
- `eval` 和 `arguments` 不能被重新赋值
- `arguments` 不会自动反映函数参数的变化
- 不能使用 `arguments.callee`
- 不能使用 `arguments.caller`
- 禁止 `this` 指向全局对象
- 不能使用 `fn.caller` 和 `fn.arguments` 获取函数调用的堆栈
- 增加了保留字（比如 `protected`、`static` 和 `interface`）

也可以参阅MDN文档：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode，查看更多细节内容。

至此，有关JavaScript的所有基础语法和内置库部分，就暂时到这里为止，如果你的目标是学习Web开发，请继续学习我们第四章的内容，如果你是做后端开发，可以直接开启前端工程化的课程学习。

## 本章练习

### 两数之和（力扣竞赛）

题目地址：https://leetcode.cn/problems/two-sum/description/

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

### 打家劫舍（力扣竞赛）

题目地址：https://leetcode.cn/problems/house-robber/description/

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

### 选择题精选

1. 关于 JSDoc 注释，下列说法**正确**的是？

   A. `@param` 后面的类型只用于文档，对编辑器无任何意义
   B. `@returns` 只能写在函数体内部
   C. 使用 `/** */` 才能被大多数编辑器识别为函数注释
   D. JSDoc 注释会影响函数运行结果

2. 以下关于 IIFE（即时调用函数）的说法，哪一项是**错误**的？

   A. IIFE 可以创建一个独立作用域
   B. IIFE 在 ES6 之前常用于解决 `var` 作用域问题
   C. IIFE 中定义的变量无法被外部访问
   D. IIFE 在 ES6 之后完全被废弃，已经无法使用

3. 关于 `arguments` 和剩余参数，下列说法**正确**的是？

   A. 剩余参数和 `arguments` 都是真数组
   B. 箭头函数可以正常使用 `arguments`
   C. 剩余参数只能写在参数列表的最后
   D. 剩余参数会自动忽略多余的实参

4. 执行以下代码，输出结果是？

   ```js
   function test(a, ...rest) {
     console.log(rest.length)
   }
   test(1, 2, 3, 4)
   ```

   A. 1    B. 2    C. 3     D. 4

5. 关于箭头函数，下列哪一项描述**最准确**？

   A. 箭头函数的 `this` 在调用时动态绑定
   B. 箭头函数的 `this` 由定义时的作用域决定
   C. 箭头函数可以通过 `bind` 修改 `this`
   D. 箭头函数和普通函数的 `this` 行为完全一致

6. 以下哪种箭头函数写法是**合法且返回对象**的？

   A. `() => { name: "Tom" }`
   B. `() => ({ name: "Tom" })`
   C. `() => return { name: "Tom" }`
   D. `() => [ name: "Tom" ]`

7. 以下代码执行后，`this` 指向的是？

   ```js
   const obj = {
       name: '胖猫',
       say() {
           setTimeout(() => {
               console.log(this.name)
           })
       }
   }
   ```

   A. 对象本身
   B. 调用 fn 的对象
   C. 全局对象（浏览器中为 window）
   D. undefined

8. 以下代码执行后，`this` 指向的是？

   ```js
   const obj = {
       name: '胖猫',
       say() {
           setTimeout(function () {
               console.log(this.name)
           }, 1000)
       }
   }
   ```

   A. fn 本身
   B. 调用 fn 的对象
   C. 全局对象（浏览器中为 window）
   D. undefined

9. 关于数组解构，下列说法**错误**的是？

   A. 数组解构是按位置匹配
   B. 可以通过逗号跳过元素
   C. 解构失败时会抛出异常
   D. 可以设置默认值

10. 执行以下代码，`b` 的值是？

    ```js
    const arr = [10]
    const [a, b = 20] = arr
    ```

    A. undefined    B. null    C. 10    D. 20

11. 关于对象解构，下列哪项说法**正确**？

    A. 对象解构按属性顺序匹配
    B. 解构变量名必须与属性名一致，无法重命名
    C. 不存在的属性解构结果为 undefined
    D. 对象解构不支持默认值

12. 以下代码执行结果是？

    ```js
    const obj = { a: 1, b: 2, c: 3 }
    const { a, ...rest } = obj
    console.log(rest)
    ```

    A. `{}`
    B. `{ a: 1 }`
    C. `{ b: 2, c: 3 }`
    D. `[2, 3]`

13. 关于展开运算符，下列哪项描述**错误**？

    A. 展开数组可以实现浅拷贝
    B. 展开对象时属性冲突，后者覆盖前者
    C. 展开运算符可以深拷贝嵌套对象
    D. 展开数组可用于函数参数传递

14. 标签模板函数中，第一个参数 `strs` 的特点是？

    A. 字符串拼接后的最终结果
    B. 插值表达式计算后的值数组
    C. 按插值位置切割后的字符串数组
    D. 原始模板字符串

15. 关于标签模板，下列哪项是**正确用途**？

    A. 提高字符串拼接性能
    B. 防止 XSS 注入
    C. 替代 JSON.parse
    D. 自动国际化所有字符串

16. 调用生成器函数后，返回的是？

    ```js
    function* gen() {}
    const g = gen()
    ```

    A. 普通函数
    B. Promise
    C. 生成器对象
    D. undefined

17. 关于 `yield` 的作用，下列说法**正确**的是?

    A. `yield` 会结束整个函数
    B. `yield` 只能使用一次
    C. `yield` 会暂停函数执行并返回一个值
    D. `yield` 等价于 `return`

18. 关于“属性继承 + call”的组合，下列哪项是其主要目的？

    A. 复制父类方法
    B. 建立原型链
    C. 初始化父类的实例属性
    D. 修改 constructor 指向

19. 以下哪一行代码**必须写在子类构造函数最前面**？

    A. `this.level = level`
    B. `extends Student`
    C. `super(name, age)`
    D. `constructor()`

20. 关于 `class` 的本质，下列哪项描述**最准确**？

    A. class 是一种全新的类型
    B. class 是构造函数的语法糖
    C. class 不使用原型链
    D. class 无法进行继承

21. 关于私有属性 `#`，下列哪项是**错误**的？

    A. 只能在类内部访问
    B. 不会出现在对象属性枚举中
    C. 可以通过 `this["#x"]` 访问
    D. 是 ES2022 引入的特性

### 事件循环专题练习

请自行推断出下列代码的执行结果：

```js
console.log('start');

setTimeout(() => {
  console.log('timeout1');
  
  Promise.resolve().then(() => {
    console.log('promise1');
  });
}, 0);

Promise.resolve().then(() => {
  console.log('promise2');

  setTimeout(() => {
    console.log('timeout2');
  }, 0);
});

queueMicrotask(() => {
  console.log('microtask');
});

console.log('end');
```

请自行推断出下列代码的执行结果：

```js
console.log('script start');

setTimeout(() => {
    console.log('setTimeout');
}, 0);

Promise.resolve()
    .then(() => {
        console.log('promise1');
        setTimeout(() => {
            console.log('promise1 -> setTimeout');
        }, 0);
    })
    .then(() => {
        console.log('promise2');
    });

async function async1() {
    console.log('async1 start');
    await async2();
    console.log('async1 end');
}

async function async2() {
    console.log('async2');
}

async1();

console.log('script end');
```

————————————————
版权声明：本文为柏码知识库版权所有，禁止一切未经授权的转载、发布、出售等行为，违者将被追究法律责任。
原文链接：https://www.itbaima.cn/zh-CN/document/j35cdc1qz8dzq7pn