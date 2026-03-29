![image-20260127113616394](https://s2.loli.net/2026/01/27/TgUXZ5KFQmG8MkY.png)

# JavaScript核心知识

在上一章我们介绍了JS的基础语法，包括变量的创建、数据类型、运算符以及流程控制语句，只不过，仅仅有这些东西还不足以实现高级程序设计，我们还需要学习JavaScript的更多特性，本章我们将为大家介绍函数、面向对象编程以及JS常用对象的使用。

## 函数初步

很多编程语言都有函数的概念，JS也不例外，实际上，在上一章我们一直使用的`﻿console.log`、`alert`就是函数，不过这个函数是标准库中已经实现好了的，这一部分我们就来为大家详细介绍一下如何创建和使用函数。

首先，函数的具体定义是什么呢？

> 函数是完成特定任务的独立程序代码单元。

其实简单来说，函数时为了完成某件任务而生的，可能我们要完成某个任务并不是一行代码就可以搞定的，现在可能会遇到这种情况：

```js
let a = 10;

console.log("H");   //比如下面这三行代码就是我们要做的任务
console.log("A");
a += 10;

if(a > 20) {
    console.log("H");   //这里我们还需要执行这个任务
    console.log("A");
    a += 10;
}

switch (a) {
    case 30:
        console.log("H");   //这里又要执行这个任务
        console.log("A");
        a += 10;
}
```

可以看到，这个任务执行的操作其实是相同的，但是我们每次要执行这个任务时，都要完完整整地将任务的每一行代码重新写一遍，如果我们的程序中多处都需要执行这个任务，每个地方都去完整地写一遍，这实在是太臃肿了，那有没有一种更好的办法能优化我们的代码呢？

这时我们就可以考虑使用函数了，我们可以将我们的程序逻辑代码全部编写到函数中，当我们执行函数时，实际上执行的就是函数中的全部内容，也就是执行对应的任务，每次需要做这个任务时，只需要调用函数即可。

### 创建和使用函数

首先我们来看看如何创建一个函数，其实创建一个函数是很简单的，格式如下：

```c
function 函数名称([函数参数...]) { 函数体 }
```

这里函数名称也是有要求的，并不是所有的字符都可以用作函数名称，它的命名规则与变量的命名规则基本一致，所以这里就不一一列出了，函数里面就是我们要写的函数执行逻辑了，也就是我们我们可以将一系列的代码全部封装到函数里面，我们来尝试创建一个简单的函数：

```js
function test() {
    console.log("我是第一句")
    console.log("我是第二句")
    console.log("我是第三句")
}
```

这个函数包含了我们封装的三句代码，那么怎么才能使用这个函数呢，调用函数非常简单，和之前一样，我们只需要写下函数的名字再加上花括号即可：

```js
test()
```

这样，当执行到这一行代码时，就会自动调用前面声明好的函数了，调用函数时，会自动执行函数体内的代码。

不过需要注意的是，函数声明不一定需要放到调用它之前，我们可以在任何位置调用函数：

```js
sum()
function sum() {
    console.log("66666")
}
```

> 这是因为 JavaScript 引擎在执行代码前，会先扫描整个作用域，将所有**函数声明**提升到作用域的顶部。
>
> 你可以简单地理解为，函数无论写到哪里，在执行之前都会被扫描一遍，然后把函数声明自动排到前面去。

此外，我们还可以给函数一些参数，让函数拿到我们给定的参数进行使用，函数参数可以直接在小括号内声明，多个参数可以使用逗号隔开：

```js
function test(a) {  //定义参数只需要写上参数名即可，后续在函数中就可以直接使用这个参数了
    console.log(`a + 10 = ${a + 10}`)
    console.log(`a - 10 = ${a - 10}`)
    console.log(`a ** 10 = ${a ** 10}`)
}

test(2)  //调用函数时，需要传入实际参数
```

像这样在函数声明中定义的参数我们称为**形式参数**，而在调用函数时传入的实际值，我们称为**实际参数**，形式参数就像是函数中声明的一个变量一样，在调用函数时传入的实际参数会自动赋值给形式参数，所以我们可以在函数中将其当做一个变量来使用：

```js
function test(a) {
    a = a + 20
    console.log(a)
}
```

但是注意，函数的形式参数作用域仅限函数体以内，也就是函数后面紧跟的代码块内部，超出此区域是无法访问的，这和前面的流程控制语句是一样的，函数的形式参数相当于重新创建的一个新的变量，很多小伙伴认为里面用的就是外面传进去的，就容易出现：

```js
function swap(a, b) {
    const tmp = a;
    a = b;
    b = tmp;
}
let a = 6, b = 9
swap(a, b)
```

这是初学者很容易犯的错误，实际上这种操作根本不能交换外面的实际参数变量的值。

注意，即使函数存在形式参数，在调用函数时也可以不传递实际参数：

```js
function test(a) {
    console.log(a)  //undefined
}

test()
```

因为这里的函数调用并未传入一个实际参数，所以相当于函数的形式参数未被赋值，默认情况下就和变量一样，是`undefined`，不过，在ES6之后，我们也可以为函数形参设置一个默认值：

```js
function test(a = 6) {  //使用赋值运算符为其指定默认值
    console.log(a)
}
```

当调用函数时没有传递实际参数，此时就会使用默认值作为形式参数的值。

函数除了接受参数进行操作外，我们也可以让函数返回一个结果，使用`return`关键字来将函数结果进行返回：

```js
function sum(a, b) {
    return a + b  //return表示返回结果，这里返回的是a+b结果
}

const result = sum(10, 20)  //我们可以使用一个变量来接收函数的返回结果
console.log(result)
```

可以看到，当我们从函数中返回结果时，外部可以得到这个结果，你可以使用变量接收它，也可以把他当做另一个函数的实际参数去使用：

```js
const a = sum(sum(10, 20), 30)
console.log(sum(10, 20))  //想怎么玩都可以，但是注意这个是一个右值
```

默认情况下，如果一个函数没有任何`return`语句或者走到其某一个分支下没有`return`语句，那么默认的返回值是`undefined`：

```js
function test() {
    console.log("什么都不返回")
}
console.log(test())
```

到这里，我们就可以解释之前的问题了，为什么在之前我们在控制台使用`console.log`的时候会输出一个`undefined`呢？就是因为这个函数它没有返回值：

![image-20260128175136770](https://s2.loli.net/2026/01/28/fHPhMxmTAgS2GKb.png)

我们在控制台调用函数之后，会自动展示函数的返回值。

需要注意的是，函数一旦返回，后续所有内容都不会再执行了，因为返回代表着这个函数已经结束任务了：

```js
function test(a) {
    if(a > 0) {
        return "6666"
    }
    console.log("HelloWorld")
    return "7777"
}

console.log(test(10))
```

这里由于`if`判断提前执行了`return`语句，所以后续的操作都不会执行了。

**思考：**下面的函数会怎么执行？

```js
function test() {
    for (let i = 0; i < 5; i++) {
        if(i === 3) {
            return
        }
        console.log(i)
    }
}
```

### 递归调用（选学）

在之前的章节中，我们已经学习了函数的定义与调用、作用域以及如何向函数传递参数。这一节，我们将深入探讨一个在编程中非常有意思，同时也极具挑战性的概念：**递归（Recursion）**

什么是递归呢？简单来说，递归就是**函数在内部调用了它自己**，就像下面这个隧道镜一样，随着调用的进行越来越深：

![image-20260128182848462](https://s2.loli.net/2026/01/28/WuTKBCDNfPVhE8x.png)

很多小伙伴第一次听到这个定义可能会觉得：这不就变成死循环了吗？函数 A 调 A，A 内部又调 A……这不就没完没了了吗？其实，递归非常像我们生活中的“套娃”或者是两面相对的镜子产生的无限镜像。但在编程中，我们必须给这个“套娃”设置一个终点，否则程序确实会因为没完没了的调用而崩溃。

我们先来看一个最简单的、错误的递归例子（没有出口）：

```js
function test() {
    console.log("我正在调用自己...");
    test(); // 内部再次调用自己
}

test();
```

![image-20260128182337298](https://s2.loli.net/2026/01/28/CJ9pNtWgsYwGUT4.png)

执行这段代码，你会发现控制台很快就会报错：`Uncaught RangeError: Maximum call stack size exceeded`，这就是所谓的**栈溢出**，因为计算机的内存是有限的，它不能无限制地存储还没执行完的函数。

不过，只要我们加以控制，设置一个合适的出口，就可以达成事半功倍的效果，其中一个比较经典的例子就是：阶乘计算，在数学中，一个正整数 n 的阶乘（记作 n!）定义为：$n!=n×(n−1)×(n−2)×⋯×1$ ，比如5的阶乘就是： $ 5!=5×4×3×2×1=120$

我们可以发现一个规律：$5!=5×4!$，而 $4!=4×3!$。 推广开来，就是：$f(n)=n×f(n−1)$，这就是我们的**递归表达式**，那么终点在哪里呢？当 n=1 时，$1!=1$，这就是我们的**递归出口**，因为已经不用继续向下了：

```js
function factorial(n) {
    // 1. 首先设置出口：如果 n 是 1，直接返回 1，不再递归
    if (n === 1) {
        return 1;
    }
    // 2. 递归表达式：n 乘以 (n-1) 的阶乘
    return n * factorial(n - 1);
}

console.log(factorial(5)); // 输出 120
```

为了让各位小伙伴彻底理解递归，我们拆解一下 `factorial(3)` 的执行过程：

1. **调用 `factorial(3)`**：`n` 不等于 1，进入递归，准备返回 `3 * factorial(2)`。此时函数还没算完，卡在这等 `factorial(2)` 的结果。
2. **进入 `factorial(2)`**：`n` 不等于 1，准备返回 `2 * factorial(1)`。同样卡在这等 `factorial(1)`。
3. **进入 `factorial(1)`**：此时触发了 `if (n === 1)`，直接返回 **1**。
4. **开始回溯**：
   - `factorial(2)` 拿到了 `factorial(1)` 的结果 `1`，计算 `2 * 1`，返回 **2**。
   - `factorial(3)` 拿到了 `factorial(2)` 的结果 `2`，计算 `3 * 2`，返回 **6**。

递归就像是你去问路：你问路人甲，路人甲说“我不知道，你去问路人乙”，乙说“问丙”，丙说“就在这！”，然后丙告诉乙，乙再跑回来告诉你。

## 对象和引用

我们在前面已经学习了面向过程编程，也可以自行编写出简单的程序了。我们接着就需要认识 面向对象程序设计（Object Oriented Programming）它是我们在JavaScript语言中要学习的重要内容，面向对象也是高级语言的一大重要特性。

在 JavaScript 中，几乎所有的东西都可以看作是对象。如果说函数是完成任务的“动作”，那么对象就是拥有数据和行为的“实体”。

### 创建对象

在现实生活中，**对象就是一个具体的“东西”**。比如你的手机、电脑、甚至你自己都是对象。每个对象都有：

- **属性**：描述它的特征（比如手机的品牌、颜色、屏幕尺寸）
- **行为/方法**：它能做的事情（比如手机能打电话、拍照、发消息）

比如你自己，有你自己的名字、年龄、性别，你的手机有它的颜色、尺寸、内存容量等，所有的对象都有各自的属性，甚至还有一些行为，比如你自己有学习的行为，吃饭的行为，你的手机由上网的功能、打电话的功能、拍照的功能。

在 JavaScript 中，对象就是对现实世界物体的抽象，用代码把这些“东西”的特征和功能封装起来，让程序能像操作真实物品一样操作它们。JavaScript 的对象是一种复合数据类型，用于存储多个相关的数据和功能。其中最简单的对象可以像这样声明：

```js
const obj = {}
console.log(typeof obj)  //得到object类型
```

这就是所谓的**对象字面量**语法，使用一对花括号就可以创建一个空对象，在打印其类型名称时，得到的是`object`，注意，对象并不属于基本数据类型，它属于引用类型（我们会在后续介绍引用类型和基本类型的区别）不过，空对象并没有什么实际用途，我们通常需要在对象中存储一些数据。

对象中存储的数据我们称为**属性**，每个属性都有一个名称和对应的值。我们可以在创建对象时直接定义属性，比如我们想要创建一个人的对象，那么这个人肯定有名字、年龄、性别等属性：

```js
const person = {
    name: "张三",
    age: 18,
    gender: "男"
}
```

可以看到，属性的定义格式是 `属性名: 属性值`，多个属性之间用逗号分隔，就像定义一个变量一样。属性值可以是任意类型的数据（包括上一章学习的数字、字符串、布尔值，甚至可以是另一个对象或函数）需要注意的是，属性名可以是任何有效的字符串，它的命名规则不像变量那样有着严格约束：

```js
const person = {
    "2$name": "张三",  //当对象的属性名称存在特殊字符干扰时，可以直接使用字符串形式表示，效果完全一样
    age: 18,
    gender: "男"   //如果是最后一个属性可以不接逗号
}
```

如果字符串是变量存储的，我们也可以直接将一个变量的值作为属性名称使用：

```js
const key = "name"  //如果不是字符串，在作为属性名称时会发生隐式类型转换
const person = {
    [key]: "小明",
  	age: 18,
    gender: "男"
}
```

不过需要注意的是，如果变量存储的不是字符串，而是其他类型，那么会自动转换为字符串类型再作为属性名称使用。下一节我们会详细介绍对象的属性如何使用。

### 对象的属性

前面我们体验了如何创建一个对象，对象可以具有多种属性，我们可以把它当做一个真正的现实世界的对象来看。

那么创建好对象后，我们如何访问其中的属性呢？有两种方式，其中第一种方式是使用`.`运算符来访问对象的属性，比如这里我们要访问`person`的年龄和性别，就可以直接`.age`或者`.gender`访问：

```js
console.log(person.gender)   //输出：男
console.log(person.age)    //输出：18
```

除此之外，我们也可以使用方括号来访问：

```js
console.log(person["2$name"])   //只需在方括号中传入字符串形式的变量名称即可
console.log(person["age"])    //这对于一些特殊名称的属性或是需要动态获取名称访问的属性，就非常好用
```

这种方式将属性名作为字符串放在方括号中。虽然写起来麻烦一些，但它有一个独特的优势：**当属性名包含特殊字符或需要动态确定时，必须使用方括号表示法**。利用这种机制，我们还可以让方括号使用变量来实现动态访问属性：

```js
const key = "2$name"
console.log(person[key])
```

这在很多复杂场景下都非常灵活，不过，在简单情况下我们还是更建议大家选择`.`运算符的方式进行属性访问，它看起来更直观简洁。

对象的属性也可以参与到运算当中，或是给其他变量或对象的属性赋值，就像使用变量那样：

```js
const name = person.name  //作为结果赋值
console.log(person.name + "666")  //也可以参与运算
```

需要注意的是，如果我们访问了一个不存在的属性，并不会出现报错：

```js
console.log(person.title)
```

此时得到的结果是一个`undefined`而不是直接报错。

除了读取对象的属性值之外，也可以修改对象的属性，我们可以直接使用之前的赋值运算符来进行赋值：

```js
person.age = 16
```

可以看到，无论是修改还是添加，语法都是一样的：`对象名.属性名 = 新值`，当然，我们也可以使用方括号的形式：

```js
person["age"] = 16
```

可以看到，当我们直接对已存在的属性赋值时，就会覆盖原来的值。不过需要注意的是，我们也可以对一些对象中不存在的属性赋值，如果对象中没有这个属性，那么就会自动创建：

```js
person.school = "重庆邮电大学移通学院"
console.log(person)
```

![image-20260203152127999](https://s2.loli.net/2026/02/03/9JIdWCjvPFmZrfA.png)

既然可以凭空创建属性，那么也可以凭空删除属性，如果不再需要某个属性，我们可以使用 `delete` 关键字将其删除：

```js
delete person.age
```

![image-20260203153215604](https://s2.loli.net/2026/02/03/j7u1gz2IKoBkA9D.png)

> 如果你学习过Java的Map，你会觉得JS中的对象更像是一个Map，而不是Java中的对象。

此外，`delete`运算也是有结果的，如果结果为`true`表示在删除属性之前对象中确实存在这个属性或是这个属性本来就不存在于对象中，如果结果为`false`，那么表示这个属性是无法被删除的，也就无法进行删除操作（后续我们介绍的一些内置对象的某些属性就是不可删除的）不过，虽然`delete`删除方便，但是它的弊端也有很多：

1. 现代 JS 引擎（V8、SpiderMonkey 等）会给对象做**结构优化**（Hidden Class / Shape）比如一开始的时候对象里面有`a`和`b`两个属性，那么引擎会认为 “这个对象**永远有 a 和 b**” 从而进行各种优化，一旦你进行了属性删除，引擎会发现这个对象结构变了，之前的优化全部作废。
2. 影响属性的查找，暴露原型链（有关原型链的内容我们会在后续课程中介绍）

因此，在正常情况下，我们更建议大家直接为对象上某个不需要的值设置一个`undefined`作为值，表示不存在的同时也能使得属性得以正常保留。

### 对象的方法

对象不仅可以存储数据，还可以存储函数，它就像是我们为对象赋予了一种行为。存储在对象中的函数我们称为**方法**（Method）：

```js
const person = {
    name: "张三",
    age: 18,
    gender: "男",
    //由于存在属性名称，对象中的方法可以不带函数名称，直接编写参数列表和函数体
  	//这种没有函数名的函数，我们称为匿名函数
    say: function () {
        console.log("大家好")
    }
}
```

如果对象中存在函数属性，那么我们也可以通过对象来调用这个函数，这里依然是使用`.`运算符：

```js
person.say()  //函数的名称就是属性的名称
```

调用对象的方法，实际上就像是在让这个对象执行这个行为，就好像是我们真的让一个人去做一件事情一样。既然对象有自己的属性，那么我们能不能联动一下，让对象在执行方法的时候，顺便访问一下自己的属性呢？我们可以使用`this`关键字来代表当前对象本身：

```js
const person = {
    name: "张三",
    age: 18,
    gender: "男",
    say: function () {
        console.log(`大家好，我叫${this.name}`)   //this.name表示访问当前对象的name属性
    }
}
```

这样，在我们调用`say`方法的时候，就会去获取当前对象的`name`属性，并打印到控制台。

在ES6之后，官方简化了对象方法的声明方式，我们可以直接像这样写：

```js
const person = {
    name: "张三",
    age: 18,
    gender: "男",
    say() {   //无需function关键字，直接在属性名称后添加参数列表和函数体即可
        console.log(`大家好，我叫${this.name}`)
    }
}
```

思考：下面的代码执行结果是什么呢？

```js
const person = {
    name: "小明",
    say() {
        console.log(`大家好，我叫${this.name}`)
    }
}

const person2 = {
    name: "小红",
    say() {
        console.log(`大家好，我叫${this.name}`)
    }
}

person.say()
person2.say()
```

由于此时的函数调用是基于对象的，因此，不同的对象调用方法，那么对象属性的作用域也是在各自的空间内，我们让小明执行`say`的动作的时候，相当于是让小明打印自己的名字出来。而我们让小红执行时，就是小红自己的名字。所以，大家在使用对象时，一定要时刻记得自己操作的是一个真正的实体。

### 属性的遍历

前面我们已经学习了如何**访问、修改、添加和删除对象的属性**，但在实际开发中，还有一种非常常见的需求：**我不知道对象里到底有多少个属性，想把它们一个个“拿出来看看”**，比如，我们有这样一个对象：

```js
const person = {
		name: "小明",
    age: 18,
    gender: "男",
    school: "深圳职业技术学院"
}
```

如果我们想把这个对象里的所有属性都打印出来，难道要一个个手写吗？

```js
console.log(person.name)
console.log(person.age)
console.log(person.gender)
console.log(person.school)
```

这样写显然不现实，一旦属性多了或者属性是**动态变化的**，代码就完全没法维护了。这时，我们就需要使用 **对象属性的遍历**。

前面我们为大家介绍了`for`循环语句，它可以实现循环执行某个任务，而在这里我们可以是一种新的`for`语法来实现对对象属性遍历。在 JavaScript 中，最经典、最常见的对象遍历方式，就是使用 `for...in` 循环：

```js
for (let key in 对象) {
    // key 表示属性名
}
```

我们来直接遍历刚才的 `person` 对象试试：

```js
for (let key in person) {
    console.log(key)   //依次打印每一个属性的字符串名称
}
```

可以看到，`for...in` 会 **依次取出对象中所有的属性名**，并赋值给循环变量 `key`，一定要注意 key 是属性名（字符串）不是属性值。既然 `key` 是属性名，那么我们就可以利用之前学过的 **方括号访问方式**，拿到对应的属性值：

```js
for (let key in person) {
    console.log(key, person[key])  //这样就可以同时访问属性名称和值了
}
```

在遍历过程中，我们不仅可以读取属性，还可以**修改属性值**：

```js
for (let key in person) {
    person[key] = "已处理"
}
console.log(person)
```

当然，`for`语句除了能够遍历普通对象之外，还可以遍历我们后续学习的数组类型，有关数组的知识点我们会在后续章节中介绍。

### 符号类型

Symbol 是 ES6 引入的一种新的基本数据类型，它的主要作用是创建唯一的标识符，避免属性名冲突。在 Symbol 出现之前，对象的属性名只能是一个字符形式的名称，比如`name`属性：

```js
const person = { name: "小明" }
// 在使用过程中可能会被不经意地覆盖掉
person.name = "小红"
```

虽然正常情况下没问题，但是如果别人在不知道有这个属性的情况下进行赋值，就会导致原来的 `name` 被覆盖。在大型项目 / 库 / 框架中，可能会出现很多人操作同一个对象，就很容易出现“撞属性名”，而Symbol就是为了解决这个问题的。

创建一个Symbol类型的值非常简单，类似于进行一个函数调用：

```js
const s = Symbol()
```

或是在其中添加参数，来为符号增加备注：

```js
const s1 = Symbol("id")   //可以加字符串描述（注意这里只是备注，没有实际效果）
const s2 = Symbol("id")
```

这里相当于我们创建了两个新的Symbol，它就像是我们创造的计算机中不存在的新符号一样，由于是从未存在的新符号，所以它是独一无二的：

```js
console.log(s1 === s2)   //false，备注一样没用，因为创建的就是两个新符号
```

我们可以直接将其作为属性名称：

```js
const s2 = Symbol()
const s1 = Symbol()
const person = {
    name: "小明",
    [s1]: 666
}
person[s2] = 888
```

![image-20260203182345263](https://s2.loli.net/2026/02/03/iUOAGovwydsKW2Y.png)

此时，撞车现象就消失了，两个符号都作为属性名称存在。不过需要注意的是，由于这个符号是我们自己创造的，那么当我们需要访问这个符号的属性时，也需要使用这个符号去拿：

```js
console.log(person[s1])
```

除此之外，没有其他任何直接访问这个属性的方式，必须拿到符号才可以。

不过有时候，我们可能也 **希望大家用的是同一个 Symbol**，在JS 内部有一个 **全局 Symbol 注册表**，我们可以使用`Symbol.for("xxx")`来访问，传入的字符串作为ID进行辨识：

```js
const s1 = Symbol.for("token")  //没有就新建
const s2 = Symbol.for("token")  //有就直接拿现成的
```

可以看到，这里拿到的两个符号实际上就是同一个。

JS官方库也内置了一些提前创建好的符号，我们可以通过`Symbol.xxx`的形式直接获取：

```js
const test = Symbol.toPrimitive  //获取JS预置符号
```

不过，符号类型使用的频率非常少，只有一些库开发者可能会用到，这里我们只做了解就行。

### 引用类型

在 JavaScript 中，数据类型主要分为两大类：**基本类型（Primitive Types）** 和 **引用类型（Reference Types）**

- **基本数据类型**（number、string、boolean、undefined、null、symbol、bigint）
- **引用类型**（object、array、function 等）

基本类型也被称为“原始值”，占据固定大小的空间，基本类型的变量，保存的是**具体的值本身**，比如：

```js
let a = 10
const b = a   //把a赋值给b实际上进行的是值交换
console.log(a, b)
```

这里的`b`得到的实际上是`a`所代表值的拷贝，比如这里是10，那么`b`也会存储一份10这个数据，在计算机底层就是一堆0和1了。

而引用类型（Object 类型）是保存在**堆（Heap）内存**中的对象，它可以动态地添加、修改或删除属性和方法，引用类型的变量在内存中存储的实际上是一个**引用**，这个引用（指针）指向堆内存中的实际对象。比如：

```js
const p1 = {
    name: "小明",
    age: 18
}
const p2 = p1
```

这里，我们将变量p2赋值为p1的值，那么实际上只是传递了对象的引用，而不是对象本身的复制，这跟我们前面的基本数据类型有些不同，p2和p1都指向的是同一个对象（如果你学习过C语言，它就类似于指针一样的存在）

![image-20220919211443657](https://s2.loli.net/2022/09/19/GBPaNZsr2MSKvCq.png)

那么如何验证呢？我们可以对着`p1`进行属性修改，然后直接打印`p2`看看：

```js
p1.name = "东北雨姐"
console.log(p2)
```

![image-20260203174242546](https://s2.loli.net/2026/02/03/OV3XwYISPlgusc5.png)

可以看到，由于这两个变量指向的都是同一个对象，因此我们无论对着哪一个变量进行对象的属性访问，实际上操作的都是同一个对象，改变的也是同一个对象的属性，所以就出现了上面的效果。引用类型存储的不是对象本身，而是对象的引用，复制时也不是对象的拷贝，而是引用的拷贝。

当然，引用类型的变量虽然表示的是指向某个对象，但是有些时候，我们也可以让其不指向任何对象，如果一个变量应该保存引用类型的数据同时又不指向任何一个对象，我们可以使用`null`这个特殊值来表示：

```js
const p2 = null
```

除了表示没有引用外，我们也可以用它来表示没有值，它的意义和我们之前介绍的`undefined`比较类似，有些时候很多小伙伴不知道该在什么时候用，建议各位小伙伴参考下面的场景进行合理使用：

* `undefined`适合系统默认状态，比如方法无返回值，变量未初始化，也就是还不存在这个东西。
* `null`表示人为、有意识地表示“空”，常用于对象、数据结构、接口返回，也就是这里本该有东西，这个东西是存在的，只是现在没有。

所以这里建议各位小伙伴如果想让某个变量或属性不代表任何值，就尽可能使用`null`而不是`undefined`，还需要注意的是，`null`也是基本数据类型的一种，虽然`typeof`运算的结果是`object`但是它本质是基本类型的一种（历史遗留问题）

此外，需要注意的是，引用类型之间的比较和基本类型不同：

```js
const p1 = { name: "小明" }
const p2 = p1
const p3 = { name: "小明" }

console.log(p2 === p1)
console.log(p3 === p1)
```

引用类型比较的是两个变量是否指向同一个对象，而不是比较对象的内容是否相同，所以这里的`p3`和`p1`虽然内容相同，但是它们不是同一个对象，所以结果是`false`，同时，无论使用双等号还是三等号，都是对引用进行比较。

不过，引用类型在进行其他运算时，也会按照我们上一章介绍的隐式类型转换规则进行转换后再计算：

* 如果是对象，先转换成基本类型。
* 如果是基本类型但是不是需要的类型，根据运算符，将基本类型转为最终需要的格式。

比如，加法在之前既可以进行“数字相加”，也可以进行“字符串拼接”，当遇到对象时，首先会将其转换为基本类型。那么怎么转换成基本类型的呢？实际上，它会按照以下顺序查找并调用对象的方法：

1. **`[Symbol.toPrimitive](hint)`**: ES6新增，如果定义了这个方法，直接由它说了算，推荐这种方式。
2. **`valueOf()`**: 返回对象自身的原始值。
3. **`toString()`**: 返回对象的字符串表示。

我们可以通过方法来自由定义类型转换的过程，方法的返回值就是它发生类型转换之后的结果，当需要类型转换时，JS会自动调用我们这里编写的方法，比如我们这里可以设置对象的`Symbol.toPrimitive`属性：

```js
p2[Symbol.toPrimitive] = function (hint) {
    console.log(hint)  //查看期待类型
    return this.name  //这里就直接返回对象的name属性作为类型转换之后的值吧
}
console.log(p2 + "AAA")
```

这里有一个参数 **hint**，它是一个字符串类型的值，表示转换的期望类型，有三种可能的值：

- `"string"`：表示需要将对象转换为字符串（如调用 `String(obj)` 或在模板字符串中使用）
- `"number"`：表示需要将对象转换为数字（如调用 `Number(obj)` 或使用数学运算符）
- `"default"`：表示没有明确的转换类型期望（如使用 `+` 运算符或 `==` 比较）

![image-20260203190444208](https://s2.loli.net/2026/02/03/eTx3wm1pn8avtL4.png)

注意，如果这里不返回一个基本类型，会导致程序出现错误，因为无法正常进行转换。

默认情况下，如果我们不手动为对象添加以上三种方法的任何一个，那么会根据场景进行调用，比如期待值是一个字符串的情况下，那么就会优先使用`toString`方法，在对象中`toStirng()`有一个默认的实现（即使我们什么都不写，也存在一个默认的行为）对象会自动转换为`[object 类型]`这样的字符串，这里的类型是它的构造函数：

```js
console.log(p2 + "")
```

![image-20260203190959948](https://s2.loli.net/2026/02/03/vqWBU6LV1xzTFoZ.png)

至于为什么默认转换出来长这样，由于JS出的比较早，在 90 年代，遍历一个深层嵌套的大对象会消耗大量内存和计算资源。所以默认只给一个标签，性能开销最小，此设计一直留存至今。

如果期待的值是一个数字类型或是没有明确期望的结果，那么这里会优先调用`valueOf`方法，在对象中`valueOf`也有一个默认实现，就是返回对象本身：

```js
console.log(p2.valueOf())
```

![image-20260203192006836](https://s2.loli.net/2026/02/03/rF6V7dUalQhtcSn.png)

可见，在 ES6 引入 `Symbol.toPrimitive` 之前，`valueOf` 和 `toString` 的互相调用逻辑非常复杂且不够直观（比如加法运算有时表现得很怪异）引入 `[Symbol.toPrimitive]` 后，开发者可以通过**一个方法**统一处理所有转换逻辑，并且根据 `hint` 参数精准控制对象在不同场景下的表现，这大大减少了隐式转换带来的一些潜在问题。

最后，这里还要提及的是，在ES2020推出了一种全新的空处理安全调用机制，专用于对象引用可能为空的情况。我们来看下面这个例子，正常情况下，下面的操作都会执行成功：

```js
let obj = {
    name: "小明",
    say() {
        console.log(`你好, 我叫${this.name}`)
    }
}

obj.say()  //调用对象方法
console.log(obj.name.length)  //获取名字长度
```

那如果我们在后续过程中吧`obj`变量变成`null`呢？

```js
obj = null

obj.say()   //此时由于变量没有指向任何对象，会出现错误
console.log(obj.name.length)
```

![image-20260205110540015](https://s2.loli.net/2026/02/05/pUjAYigNoeFV95L.png)

可以看到，如果变量没有指向任何对象，那么这里将无法正确调用对象的方法。很简单的道理，人都没有确定是谁，我让谁去`say`？不可能对着空气吧。所以，如果引用没有指向任何对象，那么这里方法调用、属性获取都将失败，这也是很多程序里面都会出现的空指针问题。

很多时候，我们可能并不明确某个变量是否不为`null`，所以，对于这种不明确的变量，我们需要在使用之前，对变量进行空判断：

```js
if(obj != null) {   //判断不为空时，才执行后续的代码，也可以直接写为if(obj)，因为null是假值
    console.log(obj.name.length)
}
```

这样，一旦判断到`obj`是`null`，那么这里将无法继续获取属性`name`。在ES2020之后，这种写法得到了简化，我们可以直接在`.`运算符前面添加问号来实现为`null`自动停止，不为`null`正常获取：

```js
console.log(obj?.name.length)  //不会报错
```

当发现`obj`为`null`时，本该获取`name`属性的操作会直接返回一个`undefined`作为结果，而不是继续向后执行。

同样的，针对于这种情况，我们还能连续使用：

```js
let obj = {
    name: null,   //现在对象的属性也有可能是null
    say() {
        console.log(`你好, 我叫${this.name}`)
    }
}

console.log(obj?.name?.length)  //可以连续判断
```

当表达式中出现了一串连续的`?.`运算符时，依然是从左往右进行操作，中途任何一个位置出现`null`，都会直接返回`undefined`。这种操作叫做**可选链**。

同样的，对于方法来说，也可以像这样处理：

```js
obj?.say()  //如果对象为null，同样直接返回undefined
```

但是注意，如果方法也有可能为空，在调用方法时同样需要加上`?.`运算符：

```js
let obj = {
    name: null,
    say: null
}
obj?.say?.()  //此时如果say为null，将不会调用函数，并直接返回undefined
```

有了空安全的特殊处理运算符，我们就可以更加简单地进行可能为空的代码编写了。

### 函数类型

在前面我们一直把函数当作“可以调用的一段代码”来使用，但实际上，在 JavaScript 中，**函数本身也是一种数据**。换句话说： **函数不是特殊存在，它和数字、字符串一样，也是一种“值”**，这一点是 JavaScript 非常重要、也非常“灵魂”的特性。

我们先来做一件很简单的事情，用 `typeof` 看一下函数的类型：

```js
function test() {
    console.log("Hello")
}

console.log(typeof test)
```

![image-20260203170503891](https://s2.loli.net/2026/02/03/6ehojCxAVk3GWqb.png)

也就是说，**函数拥有自己独立的类型：`function`**

不过需要注意的是，在 JavaScript 内部实现中，函数本质上是一种**对象（Object）**类型，只是引擎为它单独区分出了 `function` 这个类型，方便我们识别和使用，它是一种特殊的对象。

> 你可以简单理解为：函数 = 一个“可以被调用的对象”

既然函数是一种值，那么它就可以做所有“值”能做的事情，比如：

* 赋值给变量
* 作为参数传递
* 作为返回值返回

我们先从最简单的开始，我们来看看**函数赋值**是怎么个玩法：

```js
function sayHello() {
    console.log("Hello World")
}

const fn = sayHello   // 注意：这里没有加 ()，只使用了函数名字
```

此时，变量 `fn` 保存的就是这个函数本身，我们可以像调用原函数一样调用它：

```js
fn()   // Hello World
```

是不是感觉很神奇？相当于一个函数，可以同时拥有多个“名字”，这也是很多初学者第一次觉得 JS “有点怪”的地方，但请记住一句话：**函数名本质上就是一个变量名**，只是写法比较特殊罢了。

既然函数可以赋值，那它自然也可以作为参数传递给另一个函数：

```js
function sayHello() {
    console.log("Hello World")
}

function test(say) {
    say()  //拿到外部传入的函数，并且可以在这里调用
}
test(sayHello)  //直接将函数作为参数传递，同样只传递函数名称
```

在 JavaScript 中，这种“被传进去、再被调用的函数”（比如这里的`sayHello`函数）我们称为 **回调函数（callback）**，既然函数可以作为参数传递，有些时候为了简单，我们甚至还能这样写：

```js
function test(say) {
    say()
}
test(function () {   //直接编写一个匿名函数，传递进去
    console.log("Hello World")
})
```

同样的，函数不仅可以作为参数“被传进去”，还可以作为返回值“被返回出来”：

```js
function createFn() {
    return function () {
        console.log("我是被返回的函数")
    }
}

const fn = createFn()   //调用函数返回一个函数
fn()  //在调用函数返回的函数
```

可能各位小伙伴会觉得这种玩法好像没啥用，但是注意，这一点是后面学习**闭包**时的核心基础。

### 函数的属性

前面我们说，函数本质也是一个对象，只是比较特殊，那么它是否也存在一些属性呢？答案是肯定的，我们先来做个最简单的实验：

```js
function test() {
    console.log("Hello")
}

test.a = 10   //直接给函数设置属性，就像使用普通对象那样
test.b = "我是函数的属性"

console.log(test.a)
console.log(test.b)
```

你会发现，这种用法**完全没问题**，函数就像普通对象一样，可以随意添加属性。当然，除了我们手动添加的属性，JavaScript 还为函数**内置了一些常用属性**，我们来重点认识几个。

`length` 属性表示：**函数定义时的形参个数**：

```js
function sum(a, b, c) {}
console.log(sum.length)  // 3，因为函数形参列表中有3个参数
```

这个属性在一些函数工具库、参数校验场景中非常有用。

函数还有一个 `name` 属性，用来表示函数的名字：

```js
function test() {}
console.log(test.name)  // "test" 就是函数的名字
```

即使是匿名函数，在某些情况下也会自动拥有名字：

```js
const fn = function () {}  //使用变量fn接收一个匿名函数
console.log(fn.name)  // 使用变量调用，那么"fn"就是它的名字
```

是 JavaScript 引擎为了**调试友好**而做的优化，方便在报错或调用栈中定位函数来源。

除了属性之外，函数也具有一些自己的方法，虽然听着有些绕，但是确实是这样的：

```js
function test() {
    console.log("Hello World")
}
console.log(test.toString())   //函数对象也自带了toString方法
```

![image-20260203232146396](https://s2.loli.net/2026/02/03/XPw8urfhJl3WNKA.png)

函数类型的`toString()`方法会直接得到函数的源代码字符串。

此外，函数还有一个`call`和`apply`方法，它可以实现和调用函数一样的效果：

```js
test.call()   //效果和下面一样
test.apply()  //效果和下面一样
test()  //直接调用
```

```js
test.call(null, 1, "HHH")   //如果函数存在参数，这里第一个可以临时使用null（后面马上介绍这个是干嘛的）然后再依次填入要传递的实参即可
test.apply(null, [1, "HHH"])  //效果和上面一样，但是后续参数传递需要使用数组（目前还没学）
test()  //直接调用
```

是不是感觉JS越来越神奇，各种奇葩用法都是源于函数本质也是一个对象。

最后，这里需要注意的是，和前面的普通函数一样，对象的方法也可以赋值给一个变量，此时这个变量代表的就是对象的方法：

```js
const person = {
    name: "小明",
    say() {
        console.log(`大家好，我叫${this.name}`)
    }
}

const func = person.say   //直接让一个变量代表这个函数，得到函数类型值
func()
```

因为对象的方法本质上存储的也是一个函数，所以这也是允许的。

但是这个函数实际上是存在问题的，因为我们在对象方法中使用了`this`，而此时我们通过赋值操作已经把这个函数带到了对象的外面，那么对于对象的指代也会跟着失效：

![image-20260203164857691](https://s2.loli.net/2026/02/03/tMbT1YRxe8COFSs.png)

可以看到，由于`this`脱离了它原本的作用域，此时就无效了，要解决这种问题也很简单，我们可以手动使用函数对象的`call`或是`apply`方法来进行函数调用，并通过参数形式传递我们需要指定的`this`目标：

```js
const func = person.say   //直接让一个变量代表这个函数，得到函数类型值
func.call(person)   //call的第一个参数代表this的目标，我们手动吧需要作为this指代的对象传入即可
func.apply(person)  //效果和上面一样，但是后续参数传递需要使用数组（目前还没学）
```

通过手动指定`this`指代的对象，就可以使得这个函数能够正确得到结果了。除此之外，我们也可以使用`bind`方法来生成一个已指定目标对象的新函数：

```js
const func = person.say.bind(person)  //将拿出来的函数绑定到对象上，并生成一个内容一样的新函数
func()  //绑定之后，this直接代表的就是绑定时的对象了，所有可以直接调用
```

这种写法效果也是一样的，至此，有关函数类型的补充介绍就暂时到这里，在后续的课程中我们还会进一步介绍JS中的函数，让大家解锁更多高级玩法。

### 构造函数

在前面的内容中，我们已经学会了如何**通过对象字面量创建对象**：

```js
const person = {
    name: "小明",
    age: 18,
    say() {
        console.log(this.name)
    }
}
```

这种方式在创建**少量对象**时非常方便，但如果我们现在遇到这样一个需求：

* 我要创建很多“人”对象
* 他们的结构都一样：都有 name、age、say 方法
* 只是具体的数据不一样

比如：

```js
const p1 = { name: "小明", age: 18 }
const p2 = { name: "小红", age: 20 }
const p3 = { name: "小刚", age: 22 }
```

此时你会发现一个问题：**代码在不断重复，对象结构在不断复制**。这不仅写起来麻烦，也非常不利于后期维护，那有没有一种方式，可以像“生产模具”一样，用**一套预先制定好的规则**批量生产对象呢？就像一个工厂一样，这时候，就轮到 **构造函数（Constructor）** 出场了。

**构造函数，本质上就是一个普通函数**，但它的用途不是“执行某个任务”，而是 **用来创建对象的**。通过它，我们可以快速创建出**结构完全一致，但数据不同的对象**。我们先来看一个最简单的构造函数：

```js
function Person() {   //构造函数的名称首字母一般是大写（不强制要求）
    this.name = "默认名字"  //使用this关键字来为创建的对象中的属性赋值
    this.age = 0
}
```

编写好构造函数后，我们就可以使用这个构造函数来批量创建对象了，这里需要使用`new`关键字：

```js
const p1 = new Person()   //使用new+构造函数快速创建一个对象
console.log(p1)
```

![image-20260203215805304](https://s2.loli.net/2026/02/03/f4ijBCVSzQ7dJTq.png)

可以看到，此时生成出来的对象属性，正如我们在构造函数中预先定义的那样。有了构造函数，我们就可以快速创建对象了。

此外，我们也可以为构造函数增加参数，来让对象批量生产更加灵活：

```js
function Person(name, age) {   //可以外部传递参数，再作为对象的属性初始值
    this.name = name
    this.age = age
}

const p1 = new Person("小明", 18)  //此时就可以通过构造函数快速创建指定名称和年龄的对象
const p2 = new Person("小红", 17)
console.log(p1, p2)
```

这样我们就可以在创建对象时传入不同的数据，从而快速为对象的属性设置初始值，实际上，构造函数的构造过程类似于：

```js
const obj = {}   //创建一个空对象
Person.call(obj)   //把这个空对象作为 this，执行构造函数
return obj   //得到成品
```

所以，在构造函数中，对`this`添加属性，并不是在给函数本身加属性，而是在给 **即将被创建出来的那个对象** 加属性。

需要注意的是，如果在使用构造函数时忘记使用`new`关键字，那么没有新对象被创建：

```js
const p1 = Person("小明", 18)
console.log(p1)  //得到的实际上是一个undefined，因为没有正确创建对象
```

不仅如此，还有一个非常严重的问题就是，此时`this`指向的是 **全局对象**（在浏览器环境下，这个全局对象是`window`，我们会在后续的DOM章节进行讲解）此时会为全局对象增加一个属性：

![image-20260203234200801](https://s2.loli.net/2026/02/03/L8nkIKiFEcyCajA.png)

可以看到，我们直接在浏览器控制台访问name就能得到小明了，所以这个问题非常严重，各位小伙伴在使用构造函数时一定要记得添加`new`关键字。

我们也可以在构造函数中定义方法：

```js
function Person(name, age) {
    this.name = name
    this.age = age
    this.say = function () {
        console.log(`我叫${this.name}`)
    }
}
```

使用起来没有任何问题：

```js
const p1 = new Person("小明", 18)
p1.say()
```

但这里其实**埋了一个隐患**，由于函数本身是一个对象，这就导致每执行一次构造函数，都会重新创建一个新的 `say` 函数对象出来。我们可以比较一下：

```js
const p1 = new Person("小明", 18)
const p2 = new Person("小红", 17)
console.log(p1 === p2)   //结果为false，说明不是同一个对象
```

虽然功能完全一样，但它们是 **两个不同的函数对象**。当使用这种方式创建的对象数量一多时，就会导致内存占用增加、性能浪费，不利于复用。那有没有办法让所有实例共享同一个函数对象呢？我们会在下一节 **「原型链」** 中系统讲解。

### 原型链*

**注意：**本节难度较大，如果实在听不懂可以后续再来回顾，但是必须掌握。后续所有添加`*`星号的章节，表示建议完成原型链的学习之后在进行学习的部分，如果未理解原型链，可能部分知识点无法理解到位。

在 JavaScript 中，**每一个函数**（注意，是函数）天生都自带一个属性，叫做`prototype`，这个 `prototype` 本身是一个 **对象**，我们通常把它称为：**原型对象**：

```js
function Person(name, age) {
    this.name = name
    this.age = age
    this.say = function () {
        console.log(`我叫${this.name}`)
    }
}

console.log(Person.prototype)  //得到原型对象
```

![image-20260204001640580](https://s2.loli.net/2026/02/04/bUOd6izDINTt4sc.png)

你会发现，获取到的`prototype` 不是 `undefined`，而是一个对象，它们的关系是：

![image-20260204001608453](https://s2.loli.net/2026/02/04/jcv3ZuJb6gY2TxB.png)

构造函数的`prototype`指向的就是其原型对象，而我们发现，原型对象里面也自带了一个`constructor`函数，这个属性其实指向的就是构造函数本身，于是就形成了下面的一个关系：

![image-20260204001831875](https://s2.loli.net/2026/02/04/wmANlHkRZBb4DGp.png)

那么这个原型对象有什么用呢？实际上，构造函数是用来“存放对象自己的内容”的，而其对应的原型对象，则是用来“存放所有对象的公共内容”的，我们可以将一些公共的，所有对象都具有或者说共享的属性放到其原型对象上，比如我们向原型对象上新增一个属性：

```js
Person.prototype.gender = "男"

let person = new Person("小明", 18);
console.log(person.gender)  //这里访问的是原型对象上的gender
```

当访问一个对象的属性或方法时，若对象自身不存在该成员，JavaScript 会自动到其原型对象中查找。此时我们会发现，创建出来的对象也能访问`gender`属性，乍一看这不就跟我直接在构造函数里面增加属性是一样的吗？

实际上，这个属性是原型对象上存在的，它是所有对象共享的：

```js
Person.prototype.gender = { key: 666 }  //这里我们往原型链上扔一个属性，存放一个对象

let p1 = new Person("小明", 18);
let p2 = new Person("小红", 17);
p1.gender.key = 999   //直接改变原型对象上这个对象的key属性
console.log(p2.gender)
```

![image-20260204003037236](https://s2.loli.net/2026/02/04/U5t1ZBnJmaTkNqp.png)

此时我们会发现，虽然我们是通过`p1`进行修改的，但是当我们通过`p2`进行访问时，得到的结果居然是`p1`修改之后的，这就证明了原型链上的属性实际上是所有通过此构造函数生成的对象共享的：

![image-20260204003702526](https://s2.loli.net/2026/02/04/F3he51vUCRwM7VJ.png)

不过需要注意的是，如果我们通过对象，手动对原型对象上定义的属性赋值，并不会覆盖原本的值，因为原型链上的值是只读的，此时会在对象内部单独开辟空间存放：

```js
p1.gender = "Hello"  //并没有修改原型链，而是在当前对象内部自己创建了一个属性
console.log(p2.gender)  //得到的还是原型对象上的值
```

> **遮蔽效应（Property Shadowing）**在 JavaScript 的原型链中，当你在实例对象上定义了一个与原型对象**同名**的属性时，这个新属性会“遮住”原型上的属性。虽然原型上的属性依然存在，但通过该实例访问时，只能看到实例自己的那个属性。

在了解完原型对象后，现在我们回到之前那个“方法重复创建”的问题，现在我们只需把这个公共方法存放到原型对象上即可，这样所有创建出来的对象，都能共享这个方法：

```js
Person.prototype.say = function () {
    console.log(`我的名字是 ${this.name}`)
}

let p1 = new Person("小明", 18);
let p2 = new Person("小红", 17);
console.log(p2.say === p1.say)   //true，此时指向的都是同一个对象
```

对于一些公共的方法，由于所有对象都需要具有，所以使用原型对象存放是一个非常不错的方案，它能够解决我们之前出现的重复创建函数对象问题，省去了不少的内存开销。

此外，在JS中每一个对象（包括函数对象）内部也会有一个隐藏属性：

```js
[[Prototype]]
```

这个内部隐藏属性 `[[Prototype]]`，也是指向它的**原型对象**的，在代码层面，我们通常可以使用`__proto__`来访问这个隐藏属性：

```js
__proto__
```

它们的关系就像是：

![image-20260204005353061](https://s2.loli.net/2026/02/04/LBJncPrxYgK4UFH.png)

是不是感觉开始有点饶了，大家一定要跟紧我们的思路，不要掉队了，真正的难点现在才开始。既然所有对象都有一个指向自己原型对象的隐藏属性，那原型对象本身也不例外，原型对象也有自己的原型对象，我们来尝试访问一下：

```js
console.log(Person.prototype.__proto__)
```

![image-20260204010149675](https://s2.loli.net/2026/02/04/DKZxRUAfuFPikBT.png)

此时可以看到，这里得到的是一个`Object()`作为构造函数的原型对象，这是所有对象的最顶层原型对象，如果继续往上获取`__proto__`对象，得到的结果是` null`。在这个原型对象上，有很多我们之前用到过的方法，比如`toString`、`valueOf`，这些方法实际上在这里已经提前定义好了，所以，现在的关系是：

![image-20260204010724925](https://s2.loli.net/2026/02/04/s1JtEj9UMlSOpB8.png)

这里出现了一个新的构造方法`Object()`，它是万物之源，几乎你接触到的所有对象（除了那些特意通过 `Object.create(null)` 创建的）都是它的“子孙”，除了我们前面的字面量形式创建对象外，我们也可以使用`new`来创建：

```js
const obj = new Object()
console.log(obj)  //效果和我们直接写{}是一样的，都是创建空对象
```

无论是我们之前使用字面量形式创建的对象，还是使用这里的`new`关键字创建的对象，它们的原型对象都是这里的最顶层`Object.prototype`对象，所以：

![image-20260204011820980](https://s2.loli.net/2026/02/04/Cc9JV1WmKqYLIa3.png)

最后，我们来总结一下，比如现在我要调用对象的`say`这个方法，JavaScript 会按 **以下顺序查找**：

1. 先在对象自己身上找
2. 如果没找到，去 `p.__proto__`（也就是 `Person.prototype`）上找
3. 如果在原型对象上还没找到，继续找原型对象的原型对象
4. 直到找到为止，或者找到 `null` 为止
5. 如果最后都没找到，说明确实没有，如果是函数调用就抛异常，如果是属性值获取就返回一个`undefined`表示未定义，如果是属性赋值就在当前对象本身创建一个新的属性。

这个 **“一层一层往上找” 的过程**，就好像是在一条链路上逐步向上走，像这样层层相扣的关系，我们称之为**原型链**。

**扩展思考：**函数的也是一种特殊的对象类型，那么它的原型对象是什么？

### Object方法*

在上一节中我们学习了原型链，在原型链的顶端，是`Object.prototype`，其中定义了大量的公共方法，这些方法所有的对象都拥有，那这一节我们来探究一下这些方法有什么作用。

首先是`hasOwnProperty()`方法，它可以查询当前对象是否拥有某个属性：

```js
const obj = { name: "小明", age: 18 }
console.log(obj.hasOwnProperty("name"));
console.log(obj.hasOwnProperty("gender"));  //false，因为不存在这个属性
```

注意这个方法只能检测当前对象是否具有这个属性，不会顺着原型链向上进行检查。如果要判断某个属性是否在整个原型链上存在，我们可以使用`in`关键字进行判断：

```js
const obj = { a: 1 };
// 添加原型属性
Object.prototype.b = 2;
//如果要检测普通的属性，就直接使用字符串，当然这里也是支持Symbol的
console.log('a' in obj);          // true
console.log('b' in obj);          // true (来自原型链)
console.log(obj.hasOwnProperty('a')); // true
console.log(obj.hasOwnProperty('b')); // false
```

对于大型对象来说，使用 `in` 比 `hasOwnProperty()` 稍慢，因为它需要检查原型链，因此，在考虑性能优化的代码中，如果只需要检查自有属性，使用 `hasOwnProperty()` 更高效。

接着是`propertyIsEnumerable`方法，它可以判断某个属性是否是对象自己的属性且能够被遍历，默认情况下，对象所有的属性（包括从原型链上得到的）都是可以被`for..in`语句遍历的：

```js
console.log(obj.propertyIsEnumerable("age"));
```

不过这个方法用到的机会非常少，因为限制实在是太多了。

我们还可以使用`isPrototypeOf`来检测某个对象是否是指定对象的原型对象：

```js
console.log(Object.prototype.isPrototypeOf(obj))
```

```js
const person = new Person("小红", 20) 
console.log(Object.prototype.isPrototypeOf(person))   //只要是原型链上的原型对象，都可以视为它的原型对象
```

除了我们上面提到的两种用于类型转换的方法之外，还有一个`toLocaleString`方法，它也可以实现类似于`toString`的效果，但是返回的是本地化的字符串表示，根据不同的语言，会得到不同的结果，我们会在后续讲解日期时间对象的时候再进行介绍。

除了原型对象上的一些方法外，Object 中也存在一些**静态方法**，静态方法就是直接挂在构造函数上的方法，在实际开发中，我们经常会遇到一些更通用、更“工具化”的需求，这些方法可以帮助我们快速完成很多操作。

由于是挂在`Object`构造函数下的函数，所以我们可以直接`Object.xxx`进行调用。

我们首先来介绍一下`Object.keys()` ，它可以用来 **获取对象自身所有可枚举的属性名**，返回一个数组对象：

```js
const keys = Object.keys(person)
console.log(keys)   //由于还没学数组，只做了解即可
```

除此之外，我们也可以通过`getOwnPropertyNames`来获取对象中所有的属性名称：

```js
console.log(Object.getOwnPropertyNames(obj))
```

只不过，它比 `Object.keys()` 更细致，`keys`只能拿到可枚举的属性，而`getOwnPropertyNames`能拿到所有已定义的属性。除了获取字符串属性名称外，我们也可以使用`getOwnPropertySymbols`获取符号形式的属性：

```js
const s = Symbol()
obj[s] = "秘密"
console.log(Object.getOwnPropertySymbols(obj));  //上面两种方法都是无法拿到符号属性的，只能通过这种方式获取
```

同样的，还有`Object.values()`函数，它可以获取所有的值：

```js
const values = Object.values(person)
console.log(values)   //由于还没学数组，只做了解即可
```

既然可以单独拿属性名称和值，那么也可以使用`Object.entries()`函数，它可以获取所有的键值对：

```js
const entries = Object.entries(person)
console.log(entries)   //键值对以二维数组形式存储，目前只做了解
Object.fromEntries(entries)  //使用fromEntries可以将一个entries数组变回对象
```

除了获取对象的属性之外，`Object`还提供了对象的拷贝函数`Object.assign()`，我们可以快速将一个对象的属性拷贝到另一个对象中：

```js
const target = {}
Object.assign(target, obj)
console.log(target)   //得到obj中的所有属性
```

当然，源对象可以不止一个，我们可以继续添加更多源对象到后面：

```js
Object.assign(target, obj, person)
```

如果多个源对象的属性冲突，那么以最后一个源对象为准。这里需要注意一个非常重要的点：

> `Object.assign()` 是 **浅拷贝**，如果属性值是对象，复制的是**引用**，而不是新对象，这一点和前面讲的引用类型是完全一致的。

接着是`Object.is()`，它可以用来 **判断两个值是否完全相同**，它和 `===` 很像，但在一些特殊值上更准确：

```js
console.log(Object.is(NaN, NaN))     // true
console.log(NaN === NaN)             // false
```

```js
console.log(Object.is(+0, -0))       // false
console.log(+0 === -0)               // true
```

在绝大多数情况下，我们使用 `===` 就够了，`Object.is()` 更多是用在一些**底层判断或框架源码**中或特殊情况下。

下一个是`Object.hasOwn()`，它和`hasOwnProperty`功能一致，我们可以通过它来判断属性是否为当前对象自带而不是从原型链上得到的：

```js
console.log(Object.hasOwn(obj, "toString"))
console.log(Object.hasOwn(obj, "name"))
```

有些时候，我们希望一个对象 **创建后就不再被修改**，比如配置对象、常量对象，这时可以使用`Object.freeze()`函数：

```js
Object.freeze(obj)  //冻结吧
obj.key = "你干嘛"
console.log(Object.isFrozen(obj))  //isFrozen可以判断是否处于冻结昨天
console.log(obj)  //上面的修改虽然执行，但是最终结果还是之前的
```

你会发现，修改操作**不会生效**，它相当于把对象“冻结”了，不能修改属性值、不能添加新属性、不能删除属性。当然，除了冻结之外，我们也可以选择一些不那么激进的锁定效果，比如我们只想限制新增和删除，不限制修改，我们可以使用`seal`密封操作：

```js
Object.seal(obj)  //密封对象
obj.key = "你干嘛"
delete obj.age
obj.name = "哎哟"
console.log(obj)
console.log(Object.isSealed(obj))  //使用isSealed判断是否被密封
```

当然还有一种更加宽松的限制，使用`Object.preventExtensions()`来限制对象不可新增，但是可以删除和修改：

```js
Object.preventExtensions(obj)  //阻止继承
obj.key = "在山里我能听见各种各样的叫声"
delete obj.age
obj.name = "知识学爆"
console.log(obj)
console.log(Object.isExtensible(obj))  //使用isExtensible判断是否可以被继承
```

最后还有一个非常重磅的函数，`Object.create()`，基于指定对象，创建一个新对象，并设置为新对象的原型对象：

```js
let obj = Object.create(Object.prototype);  //得到一个新的空对象
console.log(obj)
console.log(obj.__proto__)  //结果确实是我们指定的Object.prototype
console.log(Object.getPrototypeOf(obj))  //使用getPrototypeOf也可以获取原型对象，效果和上面一样
```

只不过，这里我们也可以在创建一个没有原型对象的对象：

```js
let obj = Object.create(null);
console.log(obj)   //依然是一个新的空对象
console.log(obj.__proto__)   //但是没有原型
```

大家可以想象一个，如果一个对象没有原型对象，那么相当于它自己就是原型链最顶层的对象，我们之前说过，所有正常创建的对象原型链最顶层一定是`Object.prototype`，但是这个时候由于自己就是最顶层，所以它并不具有任何原型链上的属性：

```js
let obj = Object.create(null);
obj.toString()
```

![image-20260204120847559](https://s2.loli.net/2026/02/04/xgZH2uUCzp37hd5.png)

### 对象属性进阶*（选学）

接下来我们将介绍更多和对象属性相关的操作，由于这部分内容实际使用频率不高，仅作为选学内容。

前面我们学习过如何创建对象的属性：

```js
const obj = {
    name: "小明",
    age: 18
}

obj.key = ""   //直接对着需要创建的属性名字赋值即可，会自动完成创建
```

此外，我们也可以通过`Object.defineProperty`来完成这个操作，它可以实现对象属性的创建：

```js
Object.defineProperty(obj, "key", {})   //直接填写新属性的名称就行了，注意后面需要带一个对象，用于一会编写属性描述
console.log(obj)
```

这里我们创建了一个新的`key`属性：

![image-20260204121529425](https://s2.loli.net/2026/02/04/1YdXBPFkhtx8jV4.png)

我们可以对这个新建的属性进行一些细致化的配置，比如它的初始值，我们可以在后面的属性描述对象中进行配置：

```js
Object.defineProperty(obj, "key", {
    value: "初始值",  //使用value表示初始值
})
```

![image-20260204121720668](https://s2.loli.net/2026/02/04/XOWomyTlYR3SL7D.png)

除了设置初始值之外，和之前创建属性最大的区别是，它还可以设置属性的可迭代性、可配置性、可写性：

```js
Object.defineProperty(obj, "key", {
		value: "初始值",
    enumerable: false,   //如果可迭代性为false，那么在遍历对象属性时不会出现这个属性
    writable: false,    //如果改为false，表示属性只读，无法更新
    configurable: false   //如果可配置性为false，表示不可以被重新定义和删除，也就是上面这几个配置是锁死的，后续不能改
})
obj.key = 666 
console.log(obj)   //上面的修改没有生效，因为已经关闭写入属性了
```

如果我们在后续使用过程中，需要调整这些配置，可以再次调用此函数来设置：

```js
Object.defineProperty(obj, "key", {
    value: "初始值",
    enumerable: true,
    writable: true
})
```

不过需要注意的是，如果上面关闭了可配置性选项，那么这里是不允许重新调整的，哪怕是删除这个属性也是不允许的。

我们也可以手动将配置取出来：

```js
let descriptor = Object.getOwnPropertyDescriptor(obj, "key");
console.log(descriptor)
```

但是注意，这里虽然可以吧配置对象取出，但是修改配置的属性也是不会生效的，要进行修改只能使用上面的`defineProperty`函数重新调用。当然，我们也可以试试看取出之前正常创建的属性：

![image-20260204141838721](https://s2.loli.net/2026/02/04/bQZIq2EJmYXc59N.png)

可以看到，在默认情况下，如果是通过字面量创建的属性，默认所有选项都是开启的。

与获取属性和新增属性相关的还有`defineProperties`和`getOwnPropertyDescriptors`函数，它们可以实现一次性定义多个和获取多个属性配置，效果和上面完全一样，这里就不做演示了。

我们接着来介绍一下`get`和`set`函数，它们可以控制属性的获取和设置行为，正常情况下，我们直接为属性设置一个值，相当于让属性保存这个数据，这是默认的行为。除此之外，我们也可以自由控制：

```js
Object.defineProperty(obj, "key", {
    get() {
        return "结果: " + this._key   //返回临时变量
    },
    set(value) {
        this._key = value * value  //比如我们希望存储值的时候直接保存给到的值的相乘的结果
    }
})
obj.key = 5
console.log(obj.key)
```

可以看到，当我们手动配置属性的`get`和`set`函数时，在使用赋值运算时的行为就发生了改变：

![image-20260204144040424](https://s2.loli.net/2026/02/04/32DAiKmhxkt65gG.png)

我们成功干预了属性的设置过程和获取过程。需要注意的是，如果我们自行定义了`get`和`set`函数，那么是不允许手动设置`value`属性的，包括`writable`属性也是不能配置的。

![image-20260204145221895](https://s2.loli.net/2026/02/04/mT7wsP5X1v6IKYS.png)

打印对象可以看到，实际存储数据的是一个以`_`下划线开通的属性（按照约定，这种属性一般表示内部使用的属性，外部不应该去访问）然后下面的`key`属性实际上是由`get`和`set`两个函数组成的，控制台展示的`key`属性是虚拟的。

还有就是，不要尝试在`set`里面给这个属性本身设置值，因为现在设置值的操作已经变成函数的调用了：

```js
Object.defineProperty(obj, "key", {
    set(value) {
        this.key = value   //会导致递归调用，因为这里key = value本来就是调用这里的set函数
    }
})
```

### 类型判断*

在前面的学习中，我们已经接触过各种各样的数据类型，这时候就会不可避免地遇到一个非常现实的问题：**我怎么知道一个变量现在到底是什么类型？**比如在函数里面：

```js
function test(x) {
    // 我怎么判断 x 是不是数组？
    // 是不是对象？
    // 是不是函数？
}
```

这就是**类型判断**存在的意义，在一些强类型语言（如 Java）中，变量的类型在声明时就已经确定了，但 JavaScript 是 **弱类型 + 动态类型** 的语言，变量的类型**随时可能发生变化**。

我们最早接触到的类型判断方式，就是 `typeof`，这是我们上一章讲解的内容：

```js
console.log(typeof 10)        // "number"
console.log(typeof "hello")   // "string"
console.log(typeof true)      // "boolean"
console.log(typeof undefined) // "undefined"
console.log(typeof {}) // "object"
```

虽然在基本类型的判断上，`typeof`没什么大问题，但是到了对象这，就出问题了，因为 typeof 无法区分对象细分类型：

```js
function Student(name) {
    this.name = name
}

function Phone(id) {
    this.id = id
}

console.log(typeof new Student("小明"))
console.log(typeof new Phone("iPhone 17"))
```

![image-20260204212510559](https://s2.loli.net/2026/02/04/xgWYDpvFR7GSMwE.png)

可以看到，这里得到的都是`object`，根本看不出来它到底是哪个构造函数创建的，此时我们需要一个更加强大的运算符`instanceof`来解决对象类型的具体类型细分问题：

```js
对象 instanceof 构造函数
```

我们可以直接让对象和任意一个构造函数进行比较，判断它是否是某个构造函数创建的对象，更加准确的说法就是：**判断右侧构造函数的 prototype，是否存在于左侧对象的原型链上**，我们可以来尝试一下：

```js
const student = new Student("小明")
console.log(student instanceof Student)  //true，因为Student.prototype就是它的原型对象
console.log(student instanceof Object)   //true，因为Object.prototype是它原型对象的原型对象，在原型链上
console.log(student instanceof Phone)    //false，Phone.prototype不是它的原型对象，也不在原型链上
```

利用原型链关系，我们就可以使用`instanceof`很轻松地判断它具体是什么类型或者说是哪个构造函数创建的对象了。

那么，`instanceof`到底是如何进行判断的呢？实际上，当执行 `obj instanceof Constructor` 时，JavaScript 并不是“硬编码”去判断类型，而是 **调用了构造函数上的一个特殊方法**，这个方法就是：`[Symbol.hasInstance]()`，可以看到方法的名字是一个符号：

```js
console.log(Student[Symbol.hasInstance])   //打印结果是native代码，无法查看具体实现
```

我们作为开发者不需要关心它是如何实现的，只需要知道这个函数会返回一个真或假的结果来指出给定的对象是否是属于这个构造函数的。

## 包装对象

在JavaScript 里，“包装对象”指的是：**把基本数据类型临时“包装”成对象**，成为对象后，就具有个各种各样的属性和方法，使得基本类型也能很方便地进行使用。在 JavaScript 中，一共有三种常见的包装对象：

| 基本类型 | 对应的包装对象 |
| -------- | -------------- |
| number   | `Number`       |
| string   | `String`       |
| boolean  | `Boolean`      |

这一部分，我们来了解一下常见的基本类型包装对象以及其提供的属性和方法。

### 数字包装对象

经过前面的学习我们知道，JS里面的基本类型仅仅存储的是值本身，它不像是对象那样有各种各样的属性。

```js
let n1 = 10   //存储的就是10这个数字本身
```

针对于数字类型，JS推出了一种它的包装对象，构造函数是`Number`，我们可以使用以下方式来进行包装：

```js
let n2 = new Number(10)   //使用构造方法创建一个数字的包装对象，参数需要传入被包装的数字本身
let n3 = new Number("10")  //字符串也是没问题的，可以转换
let n4 = Number("10")  //注意如果不加new的话，是不会创建对象的，得到的是一个基本类型的数字
```

我们可以来打印一下这两种变量的类型：

```js
console.log(typeof n1)  // number 类型
console.log(typeof n2)  // object 类型
```

那么，从基本类型变成了对象类型的数字，现在有什么功能呢？我们先来看看原型上定义的一些方法，比如我们现在想让数字只保留一位小数：

```js
let n2 = new Number(6.666)
console.log(n2.toFixed(1))  //使用toFixed可以得到一个保留n位小数的字符串，采用四舍五入
```

是不是感觉很方便？再比如我们现在想要得到二进制的表示形式：

```js
let n2 = new Number(17)
console.log(n2.toString(2))   //Number的toString方法可以传入一个数表示进制，这里传入2表示二进制
```

![image-20260204160823763](https://s2.loli.net/2026/02/04/ZACwkT6MEfuLrUi.png)

我们还可以让数字按照千位分隔形式展示，使用`toLocaleString`即可：

```js
let n2 = new Number(10000000)
console.log(n2.toLocaleString())   //默认情况下，会采用国外的千位分隔形式展示，得到的也是字符串
```

![image-20260204161120462](https://s2.loli.net/2026/02/04/5iUlY4KXry9ELTA.png)

你以为这就完了吗，我们还可以让其表示一种货币格式：

```js
//第一个参数是地区，有关国家地区代号请参阅：https://blog.csdn.net/m0_63526467/article/details/150988706
//后续是展示选项，style控制类型，currency表示货币类型
console.log(n2.toLocaleString('zh-CN', { style: 'currency', currency: 'CNY' }))
```

![image-20260204163837293](https://s2.loli.net/2026/02/04/S9Az6motpOVgxkQ.png)

有关更多使用方法请参阅文档：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString

我们也可以使用`toExponential`来进行科学计数法表示：

```js
let n2 = new Number(660000000)
console.log(n2.toExponential())
```

![image-20260204164053092](https://s2.loli.net/2026/02/04/VraBcPi5UzebmM6.png)

这里的`e+8`表示就是我们科学计数法中的$10^8$，然后前面的6.6就是相乘的数字。此外，还有一个`toPrecision`方法可以限制底层浮点数的精度（JS底层是采用64位双精度浮点数表示的）得到限制精度后的字符串结果，有关浮点数的底层表示形式，可以参考我们的Java或C语言视频课程。

实际上，为了我们开发更加方便，JS会在我们使用基本类型时，自动将基本类型包装为对象类型：

```js
let n2 = 660000000 //即使不进行转换也可以直接调用函数，因为会自动帮你转换
console.log(n2.toLocaleString())
```

除了这里的数字包装对象之外，包括后面介绍的字符串和布尔包装对象都会自行进行包装，所以我们无需这么麻烦再去`new`一次了。

我们接着来介绍一下Number中的静态方法，这些方法为我们提供了很多用于直接处理数字的工具：

```js
let n = 66
console.log(Number.isNaN(n))   //判断值是否为 NaN
console.log(Number.isInteger(n))   //判断是否为整数
console.log(Number.isFinite(n))   //判断是否为有限数（就是非无穷大）
```

在`Number`上还挂载了一些静态属性，比如Number能表示的最大最小值：

```js
console.log(Number.MAX_VALUE)
console.log(Number.MIN_VALUE)
```

这里得到的其实就是64位双精度浮点数能够表示的最大和最小值。此外，在ES6之后，Number还针对整数进行了单独的安全处理，由于底层数据存在精度问题，因此整数也是有最大限制的：

```js
console.log(Number.MAX_SAFE_INTEGER)
console.log(Number.MIN_SAFE_INTEGER)
```

这里得到的是在整数形态下能表示的的最大和最小值。我们可以使用`isSafeInteger`来判断一个数是否已经超出JS的最大整数表示范围：

```js
console.log(Number.isSafeInteger(9007199254740991))
console.log(Number.isSafeInteger(9007199254740992))  //超出范围，返回false
```

此外，针对于精度问题，Number还有一个`EPSILON`存储了**1 与大于 1 的最小浮点数之间的差值**，也就是可能会出现的误差的最大值。比如我们之前遇到的`0.1 + 0.2 ≠ 0.3`的问题，只要在误差范围之内，就表示可以接受：

```js
console.log(0.1 + 0.2 < 0.3 + Number.EPSILON)
```

> **原因：** 计算机内部使用二进制存储数字，而 0.1 和 0.2 在二进制中是无限循环小数。相加时的舍入误差导致结果比 0.3 多了那么一点点。

Number中还内置了对于字符串和数字之间的转换函数，比如我们想要直接将一个字符串转换为数字形式：

```js
console.log(Number.parseFloat("1.234"))  //转换为小数
console.log(Number.parseInt("12.5"))   //转换为整数，不保留小数位（直接砍去，不是四舍五入）
```

不过，虽然Number已经很好用了，但是精度一直是困扰很多人的问题，直到大整数类型的出现，这一切都得到了解决。

### 大整数类型

在前面的部分中，我们学习了 `number` 类型以及它的包装对象，用来表示整数和小数，但是我们说它存在一个非常严重的问题，就是精度不够，我们先来看一个看似很正常的数字：

```js
const n = 9007199254740991
console.log(n)
```

这个数看起来没什么问题，但如果我们再加 1 呢？

```js
console.log(n + 1)
console.log(n + 2)
```

![image-20260204173502738](https://s2.loli.net/2026/02/04/OuQglvyHpGmNnJ2.png)

这是因为 JavaScript 中的 `number` 类型，本质上使用的是 **IEEE 754 双精度浮点数**，它能安全表示的最大整数是有限的，这也是我们前面提到的：**最大安全整数**，一旦超出最大数，它就出现精度丢失了。

那问题来了，如果我真的需要用到非常大的整数怎么办？为了解决这个问题，ES2020 正式引入了一种新的基本数据类型：**BigInt（大整数）**这也是到目前为止，我们认识的第七种基本数据类型，也是最后一种基本类型。

BigInt 是一种可以表示任意精度整数的基本数据类型，它不能表示小数，但整数可以大到“离谱”，创建 BigInt 非常简单，只需要在整数后面加一个 `n`：

```js
const a = 123n   //并非必须是很大的数才能变成bigint，小的也可以
const b = 900719925474099199999n
```

有了BigInt之后，我们就可以创建一个非常大的值了。不过需要注意的是，BigInt只能和BigInt类型的数据进行运算：

```js
let a = 11n, b = 2n, c = 6
console.log(a + b)  //正常运算
console.log(a + c)   //报错，因为类型不同
```

虽然 BigInt 不能和 number 一起算数，但**可以比较大小**：

```js
console.log(c > a)
console.log(10n === 10) // 不是同一类型 false
console.log(10n == 10)  // 自动类型转换 true
```

如果我们要让Bigint和其他的类型进行运算，和前面的Number一样，我们需要借助包装对象来进行类型转换：

```js
console.log(a + BigInt(b))
console.log(Number(a) + b)
```

使用包装对象之后，我们也可以使用BigInt中的一些函数：

```js
console.log(a.toString(2));  //由于bigint只能表示整数，所以只包含一些整数相关的方法
console.log(a.toLocaleString());
```

静态函数中有一些针对比特位进行计算的函数。`BigInt.asIntN` 方法用于将一个 `BigInt` 值转换为具有指定位数的有符号整数。它接受两个参数：**位数**（`bits`）和 **BigInt** 值（`bigint`），并返回一个 `Number`（如果结果在安全整数范围内）或截断后的有符号整数。

```js
console.log(BigInt.asIntN(4, 28n))  //考虑首位符号位
console.log(BigInt.asUintN(4, 28n))  //不考虑符号位
```

这里我们将数字28变为只保留4个bit位的值：

* 28 = 0011100 只保留4个位，得到 1100  = -4
* 28 = 0011100 只保留4个位，不考虑符号位得到 1100  = 12

虽然bigint能够表示很大的数字，但是日常情况下还是推荐各位小伙伴使用普通的number类型，一个是因为bigint使用起来比较麻烦，有些计算可能还要进行类型转换，同时后面介绍的很多的一些工具函数都不支持bigint类型，最最关键的是，JSON格式不支持这种类型（有关JSON格式我们会在后续介绍）

### 字符串包装对象

在前面的内容中，我们已经学习过 **字符串（string）是基本数据类型**，比如：

```js
let str = "hello"
let str = new String("HelloWorld")  //和之前一样，可以弄成包装对象
console.log(typeof str)   // "string"
```

既然字符串是**基本类型**，那它本质上应该只是一个“值”，和前面一样，**字符串在“需要的时候”，会自动包装成一个对象**，我们可以直接使用String构造函数和原型对象上挂载的函数和属性。实际上在之前，我们已经使用过一些了：

```js
let str = "Hello World"
console.log(str.length)  //获取当前字符串长度
console.log(str.charAt(1))  //使用charAt函数来获取字符串某个位置上的单个字符
```

需要注意的是，虽然这里的`length`是一个普通属性，但是它是不允许修改的，我们可以尝试获取它的属性配置：

```js
console.log(Object.getOwnPropertyDescriptor(str, "length"));
```

![image-20260204190710541](https://s2.loli.net/2026/02/04/omzsduaZFT3lpJY.png)

这个属性是不可写的，并且配置已经锁死无法更改了，所以这个属性单纯只能用来获取长度，无法通过它去更改字符串长度。

除了前面使用过的这两种方法，String原型上内置了非常多的方法用于对字符串进行处理，我们依次来介绍一下。除了使用`charAt`来获取某个位置上的字符之外，我们也可以使用ES2022推出的`at`方法获取，用法完全一样：

```js
let str = "Hello World"
console.log(str.at(0))
```

但是与前面不同的是，`at`方法支持使用负数来代表从后往前数：

```js
console.log(str.at(-2))  //这里表示取倒数第二个（倒数下标是从1开始）
```

我们除了可以获取某个位置上的字符，也可以直接取它的码点，使用`charCodeAt`方法：

```js
let str = "Hello World"
console.log(str.charCodeAt(0))   //直接获取第一个字符H的码点
```

当然，除了使用方法来获取某个字符外，我们也可以使用方括号来获取某个位置上的字符：

```js
console.log(str[2])   //等价于charAt(2)，访问第三个字符
```

注意这里虽然能通过这种方式获取到字符串的某个字符，但是不能进行修改，因为字符串是不可修改的一串字符。

除了主动获取，我们也可以去查找某个字符或子串在字符串中的位置：

```js
let str = "Hello World"
console.log(str.indexOf("e"))   // 1 也是下标形式，表示第二个字符
console.log(str.indexOf("World"))   // 6 表示第七个字符
console.log(str.indexOf("k"))   // -1 没找到
```

这里会返回指定的字符或子串在字符串中首次出现的位置，如果结果为`-1`表示没有找到。当然，我们也可以反着来，找最后一个出现的字符或子串：

```js
console.log(str.lastIndexOf("l"))  //9 最后一个l出现的位置
```

如果我们只是需要简单判断某个字符或子串是否存在于字符串中，那么我们也可以使用`includes`方法：

```js
let str = "javascript"
console.log(str.includes("script")) // true
console.log(str.includes("java"))   // true
```

实际开发 **强烈推荐** 用 `includes`，语义清晰，不用`indexOf`那样还需要自己判断 `-1`，来的更简单一些。

此外，我们还可以判断字符串是否以某个字符或子串开头或结尾：

```js
let str = "hello.js"
console.log(str.startsWith("hello")) // 判断以hello开头 true
console.log(str.endsWith(".js"))     // 判断以.js结尾 true
```

看完字符串的查询和搜索，我们接着来看字符串的编辑功能。首先是字符串的裁切功能，我们可以使用`substring`来对字符串进行截取，比如我们只需要前面的五个字符：

```js
let str = "庆祝的酒为你开好，千万不要膨胀得太早，把每一节课都录好，回重庆见你的家乡父老，加油你是最棒的"
console.log(str.substring(0, 8))  //从第一个字符开始截取子串直到第9位（不含）
console.log(str.substring(9))  //从第十个字符开始截取子串直到最后
```

注意，截取子串之后，会生成一个新的字符串返回，而不是在原本字符串上进行修改，再次强调JS里面的字符串是不可变的。

除了使用`substring`之外，我们也可以使用`slice`方法：

```js
console.log(str.slice(0, 8))
console.log(str.slice(9))
```

可以看到，效果和上面完全一样，它也可以实现字符串的裁切，但是不同的是，它还支持负数形式：

```js
console.log(str.slice(9, -8))  //从第9个到倒数第8个
console.log(str.slice(-9))  //倒数第9个
```

如果是负数，表示倒数第N个，这样就非常灵活了。但是注意，如果倒数计算的结果已经超过起始位置了，那么截取出来的字符串将会是一个空串。我们更推荐各位小伙伴使用这个方法进行字符串裁切，相比`substring`来说，它更加灵活。

接着是字符串的替换操作，我们可以使用`replace`方法直接替换字符串内的某个字符或子串：

```js
let str = "Hello World"
console.log(str.replace("l", "x"))   //把字符串里面的第一个l替换为x
console.log(str.replace("Hello", "Crazy"))   //把字符串里面的第一个Hello替换为Crazy
```

注意`replace`只能替换首个出现的字符或子串，如果需要实现全部替换，可以使用ES2021新出的`replaceAll`方法：

```js
console.log(str.replaceAll("l", "x"))   //替换所有l
```

在这之前，如果需要进行批量替换，必须使用**正则表达式**，有关正则表达式的内容，我们会在下一章进行介绍。

我们还可以对字符串中出现的空白字符进行处理，`trim`可以去除字符串首尾的空白字符，比如空格：

```js
let str = "  hello  "
console.log(str.trim()) // "hello"
```

当然，如果你只是希望去除首部或尾部的空白，也可以使用`trimStart`或是`trimEnd`方法。

既然能去除，那么也可以补齐，我们可以使用`padStart`方法指定一个字符来补齐字符串长度：

```js
let str = "5"
console.log(str.padStart(3, "0")) //补齐到3长度之后变成"005"
```

当字符串长度不足时，会自动使用指定的字符对内容长度进行补齐，与其相反的是`padEnd`，它是在尾部进行补齐。

我们也可以使用方法来快速把字符串的所有内容变成大写或小写形式：

```js
let str = "Hello"
console.log(str.toUpperCase())   //所有字母变大写
console.log(str.toLowerCase())   //所有字母变小写
```

介绍完字符串的编辑功能，我们接着来看着字符串的拆分和拼接，`split`方法可以以指定字符或子串对字符串进行分割，得到一个数组：

```js
let str = "Hello World You Like This"
console.log(str.split(" "))  //根据空格字符进行划分
```

![image-20260204195826552](https://s2.loli.net/2026/02/04/74FTiW9NeSMYVCD.png)

这对于根据空格划分单词这种操作来说，就非常好使了，不过，由于我们目前还没有学习过数组，这里仅做了解。

关于字符串的拼接，之前已经介绍过，我们可以使用`+`加法运算符来进行拼接：

```js
let str1 = "全民制作人们", str2 = "大家好"
console.log(str1 + str2)
```

当然，原型上也提供了一个`concat`方法来进行连接：

```js
let str1 = "全民制作人们", str2 = "大家好"
console.log(str1.concat(str2))   //连接顺序是谁调用谁在前面，和+是一样的
```

不过正常情况下，还是建议大家使用加法运算符，更加干脆直接一些。

我们还可以使用ES6新出的`repeat`函数来让字符串自己重复拼接：

```js
console.log(str.repeat(4))   //表示让字符串重复4次
```

![image-20260204204907056](https://s2.loli.net/2026/02/04/ObgpGvuNe2jrTLX.png)

除了原型上的方法，我们最后再来看看String上的静态方法，我们可以使用`fromCodePoint`来将码点转换为对应的字符：

```js
console.log(String.fromCodePoint(97))
console.log(String.fromCodePoint(97, 98, 99))
```

与其对应的还有一个`fromCharCode`，但是此函数不支持大于65535，不够现代化，更推荐使用ES6新增的`fromCodePoint`函数。

还有一个ES6新增的函数，`String.raw()`的主要用途是保持字符串的原始格式，不处理转义字符：

```js
console.log("Hello\nWorld")   //正常情况下会换行
console.log(String.raw`Hello\nWorld`)   //原样打印，怎么写的怎么打印
```

这里我们用到了还未讲解过的标签模板语法，标签模板可以使用函数对模版字符串进行解析，有关标签模板的详细内容我们会在下一章进行讲解。

### 布尔包装对象

这一节，我们来认识最后一个比较特殊、也**最容易踩坑**的包装对象：**Boolean**，在 JavaScript 中，布尔类型只有`true`和`false`这两个取值，它们属于 **基本数据类型**，通常用于条件判断、逻辑运算、控制程序流程：

```js
let isLogin = true
if (isLogin) {
    console.log("已登录")
}
```

但和 `Number`、`String` 一样，JavaScript 也为布尔值提供了一个对应的 **包装对象**，我们可以使用 `Boolean` 构造函数来创建一个布尔包装对象：

```js
const b1 = new Boolean(true)
const b2 = new Boolean(false)
```

老样子，我们来研究一下包装之后的对象有哪些方法，首先原型链上只有一个`valueOf`方法：

![image-20260204211343102](https://s2.loli.net/2026/02/04/rYQVyCs4jXUGktO.png)

除此之外没有其他任何方法了，同样的，它的构造函数上也没有挂载其他任何静态方法，所以这个类型算是最简单的一种。

不过，Boolean 包装对象最容易坑人、面试最爱考的地方不止如此，我们来看一段代码：

```js
const b = new Boolean(false)

if (b) {
    console.log("条件成立")
} else {
    console.log("条件不成立")
}
```

我们会发现这里，居然得到的是“条件成立”，为什么会出现这种情况呢？我们知道，基本类型包装之后就会变成对应的包装对象，那么既然是对象，回顾咱们之前介绍的真值和假值概念，就能知道，对象是一种真值，所以这里直接得到真的结果，跟包装的具体值没半毛钱关系。

## 数组

前面我们已经在很多地方都用到了数组，那么数组到底是什么东西？它能做什么？

我们在上一章介绍了变量，一个变量可以存储一个数据，但是现在有一个奇葩需求，现在需要你存储100个甚至1000个数据，这个时候怎么办呢？总不可能创建1000个变量吧。

为了解决这种问题，我们可以使用数组，什么是数组呢？简单来说，就是存放数据的一个组，所有的数据都统一存放在这一个组中，一个数组可以同时存放多个数据，数据的类型随意。数组也是前面介绍的对象的一种，但是它是一个特殊的对象。

### 数组的创建和使用

比如现在我们想保存12个月每个月的天数，那么我们只需要创建一个数组就可以了，创建数组需要使用`[]`进行定义：

```js
let arr = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]  //数组中的每个元素用逗号隔开
```

这里我们通过数组存放了12个数字类型的值，这些保存在数组中的数据，我们一般称为“元素”，数组就像一个连续的列表一样：

![image-20220613113423268](https://s2.loli.net/2022/06/17/ESJ5WmydXrxfwsU.jpg)

除了使用上面的字面量创建数组之外，我们也可以使用数组构造方法：

```js
const arr1 = new Array(1, 2, 3)  //创建包含1、2、3这三个数字元素的数组
const arr2 = new Array(5)   //如果只传递一个参数，表示数组的长度，这里是创建长度为5的空数组，虽然有长度但是没有存储任何东西
```

虽然通过这种方式也可以创建数组，但是实际开发中，还是强烈推荐使用数组字面量 `[]`，它更加简洁明确。

那么数组定义好了，如何去使用它呢？比如我们现在需要打印6月的天数，我们可以使用`[下标]`来访问数组对应位置上的元素：

```js
console.log(arr[5])   //注意下标位置和前面一样，从0开始，0是第一个元素
```

除此之外，我们也可以使用数组对象的`at`方法来访问指定下标位置的元素，它支持负数访问倒数的元素：

```js
arr.at(5)  //正数第六个元素
arr.at(-2)  //倒数第二个元素
```

结合之前的`for`循环，我们也可以一次性打印12个月的天数：

```js
for (let i = 0; i < 12; i++) {
    console.log(`${i + 1}月的天数有: ${arr[i]}`)
}
```

除了简单的`for`语句之外，我们还可以使用`for..of`语法，它可以更加简单地对数组中的元素进行遍历：

```js
for (let num of arr) {
    console.log(num)   //这里得到的num就是遍历的每一个对象，和之前对象的遍历如出一辙
}
```

除此之外，我们还可以使用Array的`forEach`方法来进行元素遍历：

```js
let arr = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
//这里需要传递一个回调函数，然后会自动进行遍历，当每次遍历时，都会自动调用这个函数，第一个参数就是遍历的元素
//第二个元素是当前遍历的下标位置
//第三个是当前遍历的数组对象
arr.forEach(function (item, index, array) {
    console.log(`${index + 1}月的天数有: ${item}`)
})
```

需要注意的是，如果我们访问一个超出数组长度的位置，那么会得到一个`undefined`作为结果，表示未定义：

```js
console.log(arr[12])
```

我们可以使用`in`关键字来判断某个下标的元素是否存在于数组中：

```js
const arr = ['a', 'b', 'c'];
console.log(0 in arr);  // true
console.log(3 in arr);  // false
console.log('length' in arr);  // true 如果不是数字，则判断的是属性是否存在于数组对象中，和前面是一样的
```

除了读取数组的元素之外，我们也可以修改数组上的元素：

```js
let arr = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
arr[5] = "Hello"   //拿到下标后，直接使用赋值运算符即可，就像使用变量那样
console.log(arr[5])
```

修改之后，对应位置上的元素就会更新成修改的值。

需要注意的是，如果我们对着一个超出数组长度的位置赋值：

```js
const arr = []
arr[3] = "World"
```

![image-20260204222520461](https://s2.loli.net/2026/02/04/vzKeFVrD8Zfn6R1.png)

此时，原本为空的数组会根据数据设置的位置**自动扩容**，然后再将值放到对应位置上，而中间的空位就是“空槽”，没有存储任何值。我们可以使用数组的`length`属性来查看当前数组的长度：

```js
arr[3] = "World"
console.log(arr.length)  //得到4
```

我们可以利用`length`属性来访问最后一个元素或是倒数的元素：

```js
console.log(arr[arr.length - 1])  //arr.length - 1 刚好就是最后一个元素的下标
```

比较有意思的是，数组的`length`属性不仅可以得到数组的长度，我们还可以使用它来修改数组的长度：

```js
const arr = [1, 2, 3, 4, 5, 6]   //原本长度为6
arr.length = 3   //修改长度为3
console.log(arr)   //数组被截断
```

有些时候，我们可能希望快速填充数组里面的值，比如我们希望吧数组里面的所有值全部变成`666`，可以使用`fill`方法：

```js
let arr = [10, 20, 30]
arr.fill("666")
console.log(arr)
```

![image-20260205002319150](https://s2.loli.net/2026/02/05/O8XiPRWDurleThM.png)

### 二维和多维数组

前面我们说，数组每一个位置上都可以存放任何类型的数据，那么能不能存放一个数组呢？答案是肯定的。

```js
const arr = [[1, 2], [3, 4], [5, 6]]
console.log(arr)
```

![image-20260204225339957](https://s2.loli.net/2026/02/04/jFaBMWCnXOvt8zi.png)

可以看到，这里得到是一个长度为3的数组，数组的每一个元素都是一个数组，这就是我们常说的二维数组。比如现在我们要存放2020-2022年每个月的天数，那么此时用一维数组肯定是不方便了，我们就可以使用二维数组来处理：

```js
const arr = [
    [31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
    [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
    [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
]
```

这样，我们就通过二维数组将这三年每个月的天数都保存下来了，它看起来就像是个矩形，类似于我们常用的Excel表格：

![image-20230814162042949](https://s2.loli.net/2023/08/14/IHdxlec4SXKZtyi.png)

那么二维数组又该如何去访问呢？首先我们需要先访问最外层的数据：

```js
console.log(arr[0])   //拿到最外层数组的第一个元素，也就是2020年的天数数组
```

当拿到2020年的天数数组之后，我们可以对着这个数组继续进行下标访问：

```js
console.log(arr[0][3])   //继续拿四月的天数
```

也就是说，如果是二维数组的情况，我们要访问矩形的其中一个格子，就必须进行两次下标访问，越排在前面的下标访问就是越外层的数组。同样的，既然有二维数组，那么肯定也有三维数组：

```js
const arr = [
    [
        [1,2,3], [4,5,6]
    ],
    [
        [6,7,8], [9, 10, 11]
    ]
]
```

此时它看起来就像是个立方体了，只不过，这种高维度数组的使用频率非常少，这里不做讲解。

### 添加和删除元素

前面我们已经学会了如何**读取和修改**数组中的元素，那么接下来就要解决一个更常见的问题：数组里的数据有些时候是动态变化的，我该怎么往里面加数据、删数据？比如现在我们创建了一个数组，但是我想继续新增内容，增加数组的长度。

JS 为数组对象提供了一组方法，用来**在数组头部或尾部添加 / 删除元素**，我们可以使用`push` 来**向数组最后面添加一个或多个元素**：

```js
const arr = [1, 2, 3]
arr.push(4)
console.log(arr)  // [1, 2, 3, 4]
```

这样，数据就自动添加到了数组的最后一位后面，数组长度也会增加。这里我们也可以一次性添加多个：

```js
arr.push(5, 6, 7)
console.log(arr)  // [1, 2, 3, 4, 5, 6, 7]
```

需要注意的是，`push` 操作会直接修改原数组，而不是生成一个新的数组。同时，`push`操作是有返回值的，返回值就是更新后的数组长度。我们还可以使用`pop` 来**删除数组最后一个元素**：

```js
const arr = [1, 2, 3]
const last = arr.pop()
console.log(arr)   // [1, 2] 此时尾部的3被移除
console.log(last)  // 3
```

可以看到，使用`pop`后，数组的最后一个元素被删除了。当`push`和`pop`方法结合使用时，它看起来就像是我们在数据结构课程中学习的栈结构，满足先进后出的规则。

当然，除了在尾部进行插入和删除之外，我们也可以使用`unshift` 来**在数组最前面添加元素**：

```js
const arr = [2, 3]
arr.unshift(1)
console.log(arr)  // [1, 2, 3] 此时1插入到首部
```

和上面的`push`一样，支持多个参数插入：

```js
arr.unshift(-1, 0)
console.log(arr)  // [-1, 0, 1, 2, 3]
```

我们也可以使用`shift` 来**删除数组的第一个元素**：

```js
const arr = [1, 2, 3]
const first = arr.shift()
console.log(arr)    // [2, 3] 此时首部1被移除
console.log(first)  // 1
```

为了方便大家记忆，只需知道 shift 系列操作的是头，pop 系列操作的是尾即可。

需要特别提及的是，`delete`操作也可以用于数组，它可以实现数组元素的删除：

```js
const arr = [1, 2, 3, 4, 5];
// 删除索引为2的元素
delete arr[2];
console.log(arr);        // [1, 2, empty, 4, 5]
console.log(arr.length); // 5 (长度不变！)
console.log(arr[2]);     // undefined
```

它并没有真正删除指定索引处的元素，也**不会改变数组的长度**，而是将该位置的值设置为`undefined`，这种行为实际上并不推荐，因为它创建了稀疏数组，可能导致意外行为，同时它不改变数组长度，可能造成混淆。

除了首尾操作之外，有时候我们也需要在数组的中间位置插入一个元素，但由于JS中并不存在插入相关的方法，我们只能选择其他方案，在JS中有一个`splice`方法，它是一个综合性的方法，很多人称它是“万能修改器”，因为它既可以实现元素的删除，也可以实现元素的插入，甚至还可以实现元素的替换。我们先来看最简单的删除元素：

```js
const arr = [1, 2, 3, 4]
arr.splice(1, 1)   //第一个参数是操作的下标位置，第二个参数是删除的元素数量
console.log(arr)
```

这里我们在下标`1`的位置上删除了1个元素，也就是这里的第二个元素`2`被删除了，当然，删除数量也可以设置为`0`，表示不删除任何元素。这里`splice`的返回值，就是被删除的元素列表，以数组形式返回：

![image-20260204232757637](https://s2.loli.net/2026/02/04/42Fftc3j8s5hmPW.png)

我们也可以填写后续的参数，表示在操作位置插入元素：

```js
arr.splice(1, 1, "A", "B")   //可以写一个或多个元素
```

这些元素会在对应位置的后面插入：

![image-20260204232555776](https://s2.loli.net/2026/02/04/V9JWkKajIdsmowg.png)

这样，就可以实现在数组的指定位置插入或删除元素了，当然，如果你不希望对原数组进行修改而是得到一个修改后的新数组，也可以使用ES2023新增的`toSpliced`方法：

```js
console.log(arr.toSpliced(1, 1, "A", "B"));
```

接着，我们再来介绍一下ES2023新增的`with`方法，它相比`splice`来说，对于元素的替换更加简便：

```js
const arr = [1, 2, 3, 4]
console.log(arr.with(2, "Hello"));  //直接将下标为2的元素修改为"Hello"
```

注意，调用`with`方法得到的结果是一个新的数组，不会修改原数组。

最后还要介绍的是，ES6新增的`copyWithin`方法，它可以实现在数组内部进行拷贝，且不改变数组的长度。

```js
const arr = [1, 2, 3, 4]
// target (必填): 复制出的数据要粘贴到的起始索引位置
// start (可选): 开始复制数据的源索引位置（默认为 0）
// end (可选): 停止复制数据的源索引位置（默认为 array.length，但不包含该索引本身）
arr.copyWithin(2, 0, 2)
console.log(arr)
```

这里相当于拷贝了数组中第1、2个元素，然后将其替换到数组下标位置为2的位置上。注意，即使拷贝的数组长度已经超出范围了，也不会改变原有数组的长度，当超出时拷贝的内容会被截取。

### 数组的查找

在实际开发中，我们经常会遇到这样的问题：

- 数组里**有没有**某个元素？
- 某个元素在数组中的**位置在哪**？
- 满足某个条件的元素是**哪一个**？

为了解决这些需求，JavaScript 为数组提供了一组专门用于“查找”的方法。

我们可以使用`indexOf` 来查找某个元素在数组中**第一次出现的位置**：

```js
let arr = [10, 20, 30, 20, 40]
console.log(arr.indexOf(20)) // 得到下标位置1
```

和字符串的查找比较类似，这里也是找的从前向后的第一个相等的元素，找到就返回下标位置，如果没找到这个元素则返回`-1`作为结果。需要注意的是，`indexOf` 内部使用的是 **严格相等（===）** 进行比较，如果我们使用不同类型：

```js
let arr = [10, 20, 30, 20, 40]
console.log(arr.indexOf('20')) // -1，找不到
```

这里还有一个`lastIndexOf` ，它和 `indexOf` 用法几乎完全一致，唯一的区别就是从后面往前找第一个满足条件的。

有些时候我们可能还希望根据自己的规则进行元素的查找，可以考虑使用`find`方法：

```js
let arr = [10, 25, 30, 5]
let result = arr.find(function (item) {   //需要一个回调函数，参数是数组里面的任意一个元素，需要返回一个布尔类型结果来表示当前这个元素是不是我们需要的这个
    return item > 20  //比如我们需要查找的是大于20的元素
})
console.log(result) //25就满足条件

```

和前面的`indexOf`一样，这里也是寻找第一个满足条件的元素，只不过返回的是元素本身，而不是下标，与其类似的还有一个`findIndex`方法，与`find`唯一区别就是它返回的是下标。

`includes` 是 ES6 新增的方法，可以直接判断数组中**是否包含某个元素**：

```js
let arr = [10, 20, 30]

console.log(arr.includes(20)) // true 包含
console.log(arr.includes(99)) // false 不包含
console.log(arr.includes(20, 2)) // 从第三个元素开始找 false 不包含
```

针对于数字类型，一个比较有趣的是：

```js
console.log([NaN].indexOf(NaN))   // -1
console.log([NaN].includes(NaN))  // true
```

`includes`能正确判断 `NaN`，而`indexOf`却不行，实际开发中，如果只是判断“有没有”某个元素，建议优先使用 `includes`。除此之外，JS还提供了一些其他的方法来进行更多的存在性判定，比如`every`方法用于判断所有的元素是否都满足我们给定的条件：

```js
arr.every(function (item) {  //依然是传入回调函数来指定我们自己的规则
    return item > 0  //只要是满足大于0就达标
})
```

当所有的元素都满足这里的条件`return`真时，那么就会得到`true`，否则只要有一个元素不满足条件，就是`false`。我们也可以使用`some`方来判断是否存在至少一个元素满足我们给定的条件：

```js
arr.some(function (item) {
    return item >= 20
})
```

### 数组的排序

默认情况下，数组的元素是按照我们的定义顺序或是插入顺序来排列的：

```js
let arr = [1, 5, 2, 7, 3, 8, 0]
```

如果我们希望让数组的元素变得有序，比如这里的数字从小到大排列，那么就可以使用`sort`方法，排序规则由我们自己定义：

```js
arr.sort()  //不传递参数表示采用默认排序规则，按字符串顺序来排序
arr.sort(function (a, b) {
    //这里给到的a和b就是待比较的两个数字
    //排序的返回值需要是一个数字，如果结果为0表示两个值相等，如果结果大于0，表示a排在前，反之b排在前
    //对于数字类型，直接返回两者相减的结果就可以很轻松进行比较了，a - b就是正序，b - a就是倒序
    return a - b
})
console.log(arr)
```

排序之后，原数组的元素顺序会被重新排列。如果你不希望改变原数组，只是得到排序之后的新数组，可以使用ES2023新增的`toSorted`方法。

如果只是想把数组顺序完全反过来，也可以直接使用 `reverse`方法：

```js
let arr = [1, 2, 3, 4]
arr.reverse()
console.log(arr) // [4, 3, 2, 1]
```

此时数组内的元素也会被重新排列，得到一个相反的顺序。如果你不希望改变原数组，只是得到排序之后的新数组，可以使用ES2023新增的`toReversed`方法。

### 数组的拼接与截取

这一部分我们接着来介绍数组的拼接和截取操作。首先，`concat` 用于将多个数组或元素合并成一个新数组：

```js
let arr1 = [1, 2]
let arr2 = [3, 4]

let newArr = arr1.concat(arr2)
console.log(newArr) // [1, 2, 3, 4]
```

注意`concat`连接之后，会生成一个新的数组，不会对原有数组进行修改，这和字符串的`concat`类似。

`join`方法可以将数组内的元素拼接起来，形成一个字符串：

```js
console.log(arr.join());
```

![image-20260205004206453](https://s2.loli.net/2026/02/05/wSW4smFcEa9VUn2.png)

默认情况下，数组内的元素会使用逗号进行连接，组成一个字符串，我们也可以自定义连接符号：

```js
console.log(arr.join(""));   //1527380
console.log(arr.join("-"));  //1-5-2-7-3-8-0
```

`slice` 用于从数组中**截取一段内容**，并返回一个**新数组**：

```js
let arr = [10, 20, 30, 40, 50]

let newArr = arr.slice(1, 4)  //同样支持负数，反向下标
console.log(newArr) // [20, 30, 40]
```

规则和字符串的 `slice` 几乎一致，这里就不多做介绍了。

最后我们来介绍一下`Object.groupBy()`，它 是 JavaScript 的一个静态方法，虽然并不属于数组类型，但是它的功能是用于根据指定的分组函数将数组元素分组到一个对象中。它是在 ES 2024 中引入的，为数组分组操作提供了更简洁、更直观的语法：

```js
const inventory = ["AAA", "AA", "BBBB", "BB", "CCC"];
const result = Object.groupBy(inventory, function (item, index) {
    //比如这里我们想要按照字符串的长度进行分组，直接将length作为键
    return item.length
});
console.log(result)
```

我们可以通过自定义的函数来返回一个用于分组的键，只要计算出来是相同的键的元素，表示属于同一个组：

![image-20260205014033226](https://s2.loli.net/2026/02/05/Rs8PSiqIflE5KDV.png)

可以看到，这里确实是按照字符串的长度进行分组。

### 数组其他方法

前面我们已经学习了数组的创建、访问、遍历以及一些基础操作方法。本节我们来学习几个**在实际开发中出现频率非常高**的数组方法，它们都属于**“函数式数组方法”**，常用于**数据加工、转换、统计**等场景。

首先是`map` ，它的作用是，把数组中的每一项“加工”一次，生成一个新的数组。比如我们现在有一个数字数组：

```js
let arr = [10, 20, 30, 40, 50]
```

现在要求你吧这个数组里面的每一个数字全部变成字符串形式的，聪明的你一定想到了：

```js
for (let i = 0; i < arr.length; i++) {
    arr[i] = arr[i] + ""   //遍历每一个元素，并重新设置为字符串格式
}
console.log(arr)
```

现在，我们有一个更简单的方法做这件事情，使用`map`方法，我们可以定义一个函数来表示自己的映射规则，接着让`map`自己完成对每一个元素的转换：

```js
//注意map返回的是一个新数组，不是原有数组上修改
arr = arr.map(function (value, index, array) {  //循环对每个元素进行处理
    return value + ""   //这里的返回值就是映射的值，也就是加工之后替换上去结果
})
console.log(arr)   //效果和上面完全一样
```

是不是感觉很方便？接下来是`flat`，它 的作用是把“嵌套数组”展开成一层，比如我们常见的二维数组：

```js
const arr = [1, [2, 3], 4]
```

我们可以使用`flat`将这个数组中存在的深层嵌套数组给展平：

```js
console.log(arr.flat())   //得到[1, 2, 3, 4]
```

可以看到，原本是以数组形式存在的第二个元素，里面的内容被自动拆开并存放到外层数组中了，这在很多用到二维数组需要做扁平化处理的情况下，非常好用。它还支持多级嵌套的展平：

```js
const arr = [1, [2, [3, [4]]]]
console.log(arr.flat(2))   // 只展平2级 [1, 2, 3, [4]]
console.log(arr.flat(Infinity))   // 无限级展平 [1, 2, 3, 4]
```

`flatMap` 是`flat`和`map`的结合体，你可以理解为先 map 再 flat 一级，比如说：

```js
const arr = [{ a: 1, b: 2 }, { c: 3, d: 4 }]
```

现在要求你对上面这个数组进行处理，取出所有对象的属性名并合成一个新的数组返回，就像这样：

```js
const arr = ['a', 'b', 'c', 'd']
```

此时我们就可以考虑使用`flatMap`来处理这种情况：

```js
console.log(arr.flatMap(function (value, index, array) {  //我们可以自由编写如何得到深层数组
    return Object.keys(value)   //返回我们自己计算得到的深层数组，之后会按照`flat(1)`自动展平
}))
```

接着是`filter`，它可以实现对数组中的元素过滤：

```js
arr.filter(function(element, index, array) {
    return element > 20
})
```

比如我们这里只想要保留大于20的元素，就可以使用这种方法，它会按照我们的条件判断进行取舍。

最后，`reduce` 是数组方法中**最灵魂、也最容易劝退新手**的一个，不过一旦理解，就非常强大。它的核心思想是：把一堆数据，慢慢“合并”成一个结果，其中最常见的场景就是计算数组中所有数字之和：

```js
// acc：累计结果（累加器）
// cur：当前元素
// 初始值：第一次计算时 acc 的值
arr.reduce(function (acc, cur, index, array) {
    return 新的 acc
}, 初始值)
```

计算规则为，从初始值开始，进行数组长度轮循环：

1. 初始时将acc设置为初始值，然后cur为第一个元素，返回值为累加的结果。
2. 将上一轮累加的结果作为新一轮acc的值，cur此时变为第二个元素，如此往复。
3. 最后一轮结束时，acc的值就是`reduce`的计算结果。

比如现在有一个数组：

```js
const arr = [1, 2, 3, 4]
const sum = arr.reduce((acc, cur) => {
    return acc + cur   //返回单次累加的结果
}, 0)  //起始为0
console.log(sum) // 结果是10
```

执行过程可以理解为：

```
acc = 0
acc = 0 + 1
acc = 1 + 2
acc = 3 + 3
acc = 6 + 4
```

至此，有关数组的原型对象方法就全部介绍完毕了。

Array中还有一些静态函数，这里只介绍一个`isArray`函数，它可以判断目标对象是否为数组：

```js
const arr = [1, 2, 3, 4]
console.log(Array.isArray(arr));   //true
console.log(Array.isArray({}))  //false
```

其他的静态函数这里暂时不做介绍，后续章节中会逐步扩展。

### 类型数组（选学）

**注意：**本节为**选学内容**，日常业务开发几乎用不到，不想看可以跳过。

在前面的章节中，我们已经学习了普通数组（Array），它非常灵活，什么类型的数据都能往里塞：

```js
const arr = [1, "hello", true, {}, []]
```

这种“啥都能装”的特性很方便，但在**某些对性能、内存要求非常高的场景**下，就会显得有点“松散”。为了解决这类问题，JavaScript 在 ES6 中引入了一类新的数组家族：**类型数组（TypedArray）**，类型数组是一种特殊的数组，它只能存放指定类型的数据，类型数组在创建时就已经“定死了规则”：

- 它只能存某一种数值类型，类型明确
- 每个元素在内存中占用固定字节，内存开销少，好管理
- 连续内存，性能更高
- 不能随意 push 各种类型的数据

这种数组更加类似于一些强类型语言的数组，比如Java和C的数组，类型是固定的。**TypedArray 并不是一个具体的构造函数**，它是一个**统称**，下面这些才是真正可以用的类型数组构造函数：

| 构造函数       | 含义            |
| -------------- | --------------- |
| `Int8Array`    | 8 位有符号整数  |
| `Uint8Array`   | 8 位无符号整数  |
| `Int16Array`   | 16 位有符号整数 |
| `Uint16Array`  | 16 位无符号整数 |
| `Int32Array`   | 32 位有符号整数 |
| `Uint32Array`  | 32 位无符号整数 |
| `Float32Array` | 32 位浮点数     |
| `Float64Array` | 64 位浮点数     |

创建类型数组无法使用我们前面的`[]`字面量，我们只能通过对应的构造函数来创建：

```js
const arr = new Int8Array(5)   //创建了一个长度为5的8位有符号整数数组
console.log(arr)
```

在使用这个数组时，也只能按照其最大的存储空间去使用：

```js
arr[0] = 255
console.log(arr)   //[-1, 0, 0, 0, 0]
```

为什么这里得到的是`-1`呢？因为Int8只能存储8位有符号整数，所以：

* 255 = 11111111 只保留8位，且第一位作为符号位 11111111 = -1

这在一些对内存优化非常苛刻的场景很有效，它能够最大程度地利用数组空间，就像是C语言里面的byte数组一样。

实际上，这些类型数组都是依靠一个ArrayBuffer对象在存储数据，它是底层二进制数据容器，比如我们可以创建：

```js
const buffer = new ArrayBuffer(8)  // 表示一个8字节的缓冲区
```

ArrayBuffer 不能直接操作，只能通过视图（TypedArray 或 DataView）访问，假设在8字节的缓冲区下，不同的 TypedArray 视图长这样：

- `Uint8Array`：可以存放8个元素，每个占据1字节空间
- `Int16Array`：可以存放4个元素，每个占据2字节空间
- `Float32Array`：可以存放2个元素，每个占据4字节空间

总之，**ArrayBuffer 是"内存块"**，**TypedArray 是"读写这个内存块的接口"**，TypedArray 让 JavaScript 能够高效地处理二进制数据，为 Web 平台提供了接近计算机底层原生性能的数据处理能力。

## 本章练习

本章我们介绍了JavaScript的函数和对象，以及各种包装对象和数组类型，这一部分我们来做一些练习，对之前的内容进行加深巩固。

### 统计元素出现的次数

现在有一个数组，里面存放了很多重复的元素：

```js
const arr = ["a", "b", "a", "c", "b", "a"]
```

现在请你设计一个JavaScript程序，来统计数组里面不同元素的出现次数。

### 二分搜索算法

现在有一个从小到大排序的数组，给你一个目标值`target`，现在我们想要找到这个值在数组中的对应下标，如果数组中没有这个数，请返回`-1`：

```js
const arr = [0, 1, 2, 3, 5, 7, 9]
```

请你设计一个JS程序实现这个功能。

### 爬楼梯（力扣竞赛）

原题地址：https://leetcode.cn/problems/climbing-stairs/description/

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**示例 1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例 2：**

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

### 有效的括号（力扣竞赛）

原题地址：https://leetcode.cn/problems/valid-parentheses/description/

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。

**示例 1：**

> **输入：**s = "()"
>
> **输出：**true

**示例 2：**

> **输入：**s = "()[]{}"
>
> **输出：**true

**示例 3：**

> **输入：**s = "(]"
>
> **输出：**false

**示例 4：**

> **输入：**s = "([])"
>
> **输出：**true

**示例 5：**

> **输入：**s = "([)]"
>
> **输出：**false

### 选择题精选

1. 下列关于函数的说法，正确的是（ ）

   A. 函数必须先声明才能调用
   B. 函数调用时，形参和实参类型必须一致
   C. 函数可以没有返回值
   D. 函数只能在声明位置之后使用

2. 关于 `return` 的描述，错误的是（ ）

   A. return 可以返回任意类型的数据
   B. return 会结束函数的执行
   C. 一个函数必须写 return
   D. 没有 return 时默认返回 undefined

3. 执行下面代码，输出结果是（ ）

   ```js
   function test(a) {
       console.log(a)
   }
   test()
   ```

   A. 报错
   B. null
   C. undefined
   D. 0

4. 下面哪一项是递归函数必须具备的条件（ ）

   A. 必须有循环语句
   B. 必须有返回值
   C. 必须有递归出口
   D. 必须调用其他函数

5. 执行下面代码，输出结果是（ ）

   ```js
   function test() {
       for (let i = 0; i < 5; i++) {
           if (i === 3) return
           console.log(i)
       }
   }
   test()
   ```

   A. 0 1 2 3
   B. 0 1 2
   C. 1 2 3
   D. 0 1 2 3 4

6. 下列哪种方式可以正确创建一个对象（ ）

   A. `const obj = ()`
   B. `const obj = []`
   C. `const obj = {}`
   D. `const obj = new Object[]`

7. 访问对象中**属性名包含特殊字符**的属性，必须使用（ ）

   A. 点运算符
   B. 方括号
   C. typeof
   D. delete

8. 访问一个对象中不存在的属性时，返回值是（ ）

   A. null
   B. false
   C. 报错
   D. undefined

9. 关于 `delete` 删除对象属性的说法，正确的是（ ）

   A. delete 会删除变量
   B. delete 不会影响性能
   C. delete 会破坏对象结构优化
   D. delete 删除失败会报错

10. 关于对象方法中 `this` 的指向，正确的是（ ）

    A. this 永远指向函数定义的位置
    B. this 永远指向 window
    C. this 由函数调用方式决定
    D. this 不能出现在对象方法中

11. 执行下面代码，输出结果是（ ）

    ```js
    const person = {
        name: "小明",
        say() {
            console.log(this.name)
        }
    }
    const fn = person.say
    fn()
    ```

    A. 小明
    B. undefined
    C. 报错
    D. person

12. 下列哪种方式可以修正上题中的 this 指向（ ）

    A. fn()
    B. fn.call(person)
    C. fn.bind()
    D. person.say()

13. 关于 Symbol 的描述，错误的是（ ）

    A. Symbol 是基本数据类型
    B. Symbol 可以作为对象属性名
    C. Symbol.for 每次都会创建新 Symbol
    D. Symbol 可以避免属性名冲突

14. 关于基本类型和引用类型的区别，正确的是（ ）

    A. 引用类型存储的是值本身
    B. 基本类型存储的是引用
    C. 引用类型比较的是地址
    D. 基本类型存储在堆内存

15. 执行下面代码，输出结果是（ ）

    ```js
    const a = { x: 1 }
    const b = a
    b.x = 2
    console.log(a.x)
    ```

    A. 1
    B. 2
    C. undefined
    D. 报错

16. 对象参与隐式类型转换时，优先级最高的是（ ）

    A. toString
    B. valueOf
    C. constructor
    D. Symbol.toPrimitive

17. 下列哪一项**不是**函数的特点（ ）

    A. 可以作为参数传递
    B. 可以作为返回值
    C. 不能拥有属性
    D. typeof 结果为 function

18. `function sum(a, b) {}`，`sum.length` 的值是（ ）

    A. 0
    B. 1
    C. 2
    D. undefined

19. 关于构造函数的说法，错误的是（ ）

    A. 构造函数本质是普通函数
    B. 构造函数必须有 return
    C. 构造函数一般首字母大写
    D. new 会创建新对象

20. 关于原型对象的描述，正确的是（ ）

    A. prototype 只存在于对象上
    B. prototype 用来存放私有属性
    C. prototype 中的属性可被实例共享
    D. prototype 与构造函数无关

    

————————————————
版权声明：本文为柏码知识库版权所有，禁止一切未经授权的转载、发布、出售等行为，违者将被追究法律责任。
原文链接：https://www.itbaima.cn/zh-CN/document/95jc6sjyjwcp9pvp