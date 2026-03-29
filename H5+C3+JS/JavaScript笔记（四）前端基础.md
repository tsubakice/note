![image-20260212203819527](https://files.seeusercontent.com/2026/02/12/pR5r/image-20260212203819527.png)

# DOM核心知识

**注意：** 本章知识仅适用于Web前端开发学习路线或全栈开发路线，如果仅使用JavaScript做后端开发，不强制要求学习。

前面我们为大家介绍了JS的基础语法部分，这一章我们将继续深入Web浏览器环境，学习然后控制页面上的元素并实现各种高级操作。

## 元素和属性

在实际开发中，我们经常需要动态控制页面上的元素样式，比如当鼠标点击元素时，我们可以改变它的颜色，点击按钮时可以展示弹窗，用户滑动窗口时实时展示当前滚动的进度。这些效果都需要通过DOM提供的方法来实现，我们可以利用JavaScript与其进行交互，让我们的网站动起来。

### DOM介绍

DOM（Document Object Model，文档对象模型）可以理解为：**浏览器把 HTML 页面转换成的一棵“对象树”**。这棵树里的每一个节点，都是 JavaScript 可以操作的对象。比如下面的代码：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <div>我是页面上的一段普通内容</div>
    <p>我是一个段落</p>
  </body>
</html>
```

浏览器会把上面的代码自动转换为下面的树形结构：

![image-20260212212153383](https://files.seeusercontent.com/2026/02/12/Sfg2/image-20260212212153383.png)

在HTML中，这种结构叫做DOM树，我们需要将这个树形结构倒过来看，HTML元素就是这棵树的树根，而它的子元素，实际上就是这棵树上的分支。所有的元素，在浏览器加载页面后，会在内存中生成一个对应的 DOM 节点对象，此时 JavaScript 就可以直接操作这个节点。

在JS中，节点分为很多种类型，一共有14种类型，但其中比较常见的是以下五种类型：

1. **元素节点**（`ELEMENT_NODE`）：操作 HTML 元素的核心。
2. **文本节点**（`TEXT_NODE`）：处理元素内的文本内容。
3. **注释节点**（`COMMENT_NODE`）：有时用于模板标记或调试。
4. **文档节点**（`DOCUMENT_NODE`）：全局 `document` 对象，用于访问整个页面。
5. **文档片段节点**（`DOCUMENT_FRAGMENT_NODE`）：高效批量添加 DOM 元素。

比如我们编写的元素就是一个节点，也就是 HTML 标签本身，它是页面结构的核心组成部分，下面这些都是元素节点：

```html
<div class="container"></div>
<p>Hello World</p>
<a href="https://example.com">链接</a>
```

而文本节点，就是包含在元素或属性中的纯文本内容，包括空格、换行等空白字符：

```html
<div>
  这里是文本节点
  <p>Hello <span>World</span></p>
</div>
```

此外，HTML 中的注释内容，虽然不会在页面中显示，但可以通过 DOM 访问，这些是注释节点：

```html
<!-- 这是一个注释节点 -->
```

整个文档的根就是文档节点，一般情况下是`html`标签：

```html
<html lang="en">
  ...
</html>
```

几乎在HTML文件中出现的所有内容，都会变成一个JS可以控制的节点，哪怕是注释也会被算在其中。因此，**DOM 让 JavaScript 有了“操作页面的能力”**。下一节我们将介绍如何通过JS获取DOM上的节点。

### 获取元素

在操作页面之前，第一步永远是：**先找到元素**，通过JS查找元素有很多种方式，浏览器为我们提供了多种“查找 DOM 元素”的方式，下面我们从最基础、最容易理解的方法开始，一步步往后讲。

我们需要用到`doucment`对象，它是整个DOM的总管，代表当前这个HTML文档，我们可以通过它来获取DOM上的任何内容。第一种是通过 id 获取元素（`getElementById`）这是**最常用、也是新手最先学会的方法**：

```js
document.getElementById(id)
```

这里的参数是 **元素的 id 名称（字符串）**，返回值是 **一个元素对象**，如果找不到元素，则返回 `null`：

```html
<div id="box">我是一个盒子</div>
```

```js
const box = document.getElementById('box')
console.log(box)
```

可以看到，这里得到了一个HTML元素对象，类型是`HTMLElement`，HTML中所有的元素类型都是`Node`的子类，继承关系和顺序如下：

1. **EventTarget**：最顶层的基类，让对象拥有处理事件的能力（比如 `addEventListener`）。
2. **Node**：基础的节点类，DOM 树中的所有内容（标签、文本、注释等）都是 Node。
3. **Element**：更具体的节点，它专门指代“标签”，提供了处理属性（Attribute）的方法。
4. **HTMLElement**：HTML 专用的元素，它包含了所有 HTML 标签共有的属性（比如 `id`, `className`, `style`）。
5. **具体标签**（如 `HTMLDivElement`）：特定标签的属性（比如 `<a>` 标签独有的 `href`）

我们如果直接打印它，会以HTML代码的形式在控制台展示：

![image-20260213012627718](https://files.seeusercontent.com/2026/02/12/8nnS/image-20260213012627718.png)

我们可以通过`nodeType`来获取它的具体类型：

```js
console.log(box.nodeType === Node.ELEMENT_NODE)
```

由于得到的结果是一个数字，我们直接使用`Node`构造函数中提供的常量来进行比较即可。

当然，我们也可以在一个单独的JS文件中编写这段代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <script src="js/script.js"></script>
</head>
```

```js
//script.js
const box = document.getElementById('box')
console.log(box)
```

此时我们会发现，这样好像不太行，控制台中获取到的元素是一个`null`：

![image-20260213115140379](https://files.seeusercontent.com/2026/02/13/V0zf/image-20260213115140379.png)

这其实是因为我们的JS引入是写在`head`标签内的，而页面内容是在`body`中，网页的加载顺序是从上往下进行的，当JS加载的时候，页面内容还未渲染完成（DOM树还未完成构建）所以就无法拿到需要的元素了。

这里推荐几种做法，最简单的就是将 `<script>` 标签移到 `</body>` 结束标签之前：

```html
<body>
  <div>我是页面上的一段普通内容</div>
  <div id="box">我是一个盒子</div>
  <!-- 放在最后执行，此时前面的元素都已经完成加载 -->
  <script src="js/script.js"></script>
</body>
```

还有一种方法是给 `<script>` 添加 `defer` 属性，这个属性可以使得JS延迟加载，确保脚本执行时能访问到所有 DOM 元素：

```html
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <script defer src="js/script.js"></script>
</head>
```

我们也可以使用`getElementsByTagName`根据标签名字一次性获取所有元素：

```js
document.getElementsByTagName("div")
```

![image-20260213123903844](https://files.seeusercontent.com/2026/02/13/ahJ0/image-20260213123903844.png)

这里得到的是一个`HTMLCollection`，它是一个类数组（array-like）集合对象，包含了一组有序的 DOM 元素，使用方法非常简单：

```js
const elements = document.getElementsByTagName("div")
console.log(elements.item(0))   //使用item访问指定下标的元素
console.log(elements[1])   //也可以和数组一样玩
```

![image-20260213141603054](https://files.seeusercontent.com/2026/02/13/9kvD/image-20260213141603054.png)

此外，`HTMLCollection`还提供了一个`namedItem`用于快速查找集合中`name`或`id`属性为指定值的元素：

```js
console.log(elements.namedItem("box"))
```

我们也可以使用`getElementsByClassName`通过 `class` 名称来获取元素：

```js
const elements = document.getElementsByClassName("item")
console.log(elements)
```

```html
<div class="item">A</div>
<div class="item">B</div>
```

我们还可以根据`name`属性来获取元素，这个方法在现代开发中**用得不多**，主要用于表单：

```html
<input type="text" name="username">
<input type="password" name="password">
```

```js
const inputs = document.getElementsByName('username')
```

除了根据名字属性获取外，`document`上直接提供了根元素的获取，`documentElement`直接代表页面根元素，也就是`html`元素：

```js
document.documentElement
```

![image-20260213153648979](https://files.seeusercontent.com/2026/02/13/eF6o/image-20260213153648979.png)

此外，`body`属性也是直接代表`body`这个元素：

![image-20260213160213836](https://files.seeusercontent.com/2026/02/13/tFt9/image-20260213160213836.png)

最后我们来介绍一个最重要的，也是开发中最常用的一种元素获取方式，`querySelector`可以使用 CSS 选择器获取单个元素：

```js
document.querySelector("#box")
```

要判断我们的选择器是否使用正确，也可以使用`matches`进行判断：

```js
box.matches("#box")   //当CSS选择器匹配时，返回真
```

利用这个方法，可以替代前面讲到的全部方法，只要CSS选择器编写合理，就能直接选择需要的元素，非常方便。不过需要注意的是，这个方法只会返回匹配到的第一个元素，如果需要一次性获取所有满足CSS选择器的元素，可以使用`querySelectorAll`方法：

```js
document.querySelectorAll(".item")
```

它会返回一个`NodeList`对象，和我们前面介绍的`HTMLCollection`不同的是，**HTMLCollection** 仅包含 **Element（元素节点）**比如 `<div>`、`<p>` 等标签。而 **NodeList** 包含所有 **Node（节点）**元素，所以还包括**文本节点**（换行、空格）、**注释节点**以及属性节点，因为CSS选择器选择的范围更加广泛，所以它应该包含更多种类。它提供了`forEach`用于快速遍历子节点：

```js
document.querySelectorAll(".item").forEach((value, key, parent) => {
    console.log(key, value, parent)   //key就是顺序下标，value就是节点本身，parent就是当前这个节点列表
})
```

此外，还有一个非常重要的是，`HTMLCollection`是动态的，一旦DOM发生变化，这个对象会跟着变化，而NodeList是获取时产生的静态快照，我们可以尝试在打印对象之后删除页面上的一个元素：

![image-20260213152017011](https://files.seeusercontent.com/2026/02/13/1qaO/image-20260213152017011.png)

此时`HTMLCollection`的长度会变化成`1`，而`NodeList`无感。

以上讲解的方法，除了在`document`上使用，我们也可以在任意一个元素上使用，来实现快速查找子元素：

```js
const box = document.getElementById('box')
box.querySelector("p")   //直接在这个box元素上查找内部符合条件的子元素
```

此外，我们还可以使用：

```js
element.closest('.container')   //用于向上查找符合选择器的祖先元素
```

查找到元素之后，我们就可以开始愉快的操控它了。

### HTML属性

在前面我们已经学会了如何**获取元素**，但仅仅拿到元素还不够，真正让页面“动起来”的关键，是**读取和修改元素的属性**。**属性（Attribute）**，简单理解就是写在 HTML 标签上的那些“信息”，比如：

```html
<div id="box" class="item" title="提示文本"></div>
```

这里的 `id`、`class`、`title`，都是元素的属性，这是我们在HTML中为大家介绍的。需要获取HTML标签上的属性，我们可以直接使用`getAttribute`方法：

```js
console.log(element.getAttribute("id"))
```

注意返回值**永远是字符串**，因为属性都是直接写的值，如果属性不存在，会返回 `null`。除了获取之外，我们也可以使用`setAttribute`来修改某个属性：

```js
element.setAttribute("class", "666")
```

![image-20260214170321600](https://files.seeusercontent.com/2026/02/14/6Mwu/image-20260214170321600.png)

如果这个属性不存在，那么将会新增这个属性：

```js
box.setAttribute("data-v-25565", "666")
```

![image-20260214171513288](https://files.seeusercontent.com/2026/02/14/gMc7/image-20260214171513288.png)

不过，对于一些非标准的HTML标签属性，我们更建议大家使用后续DOM属性中的`dataset`来实现。

对于一些布尔属性来说，设置不需要填写特别的内容，如果要使用直接设置值为空即可，如果不需要则移除属性，比如：

```html
<input disabled="disabled">
```

```js
input.removeAttribute('disabled') // 表示 false
input.setAttribute('disabled', '') // 表示 true
```

这里需要注意的是，**HTML 属性名本身不区分大小写**，所以即使我们写一个大写的也会变成小写形式，覆盖也会按照小写的形式去进行覆盖：

```js
box.setAttribute("CLASS", "666")
```

![image-20260214171915265](https://files.seeusercontent.com/2026/02/14/Kda2/image-20260214171915265.png)

不过，这里还是建议大家在 **JavaScript 中调用方法时，属性名统一推荐使用小写**，这是业界的通用约定，避免出现兼容性和可读性问题。

有时候我们并不关心属性的值，只关心它**有没有被写在标签上**，这时可以使用`hasAttribute`来判断：

```js
element.hasAttribute('title')
```

我们也可以使用 `removeAttribute` 删除某个属性：

```js
element.removeAttribute('title')
```

如果属性本身不存在，**不会报错**，但也不会有任何效果。

### DOM属性

在上一节中我们讲了 **HTML 属性（Attribute）**，它们是**写在标签上的信息**。而这一节要讲的 **DOM 属性（Property）**，是**浏览器在内存中为每一个元素创建的 JavaScript 对象属性**。所以，HTML 属性是“写给浏览器看的”，DOM 属性是“写给 JavaScript 用的”。

当浏览器加载 HTML 页面时，会做两件事：

1. 解析 HTML 标签上的属性（Attribute）
2. 基于这些属性，**生成一个对应的 DOM 对象**

比如一个`input`标签：

```html
<input type="text" value="hello">
```

浏览器会创建一个 `HTMLInputElement` 对象，其中包含大量 DOM 属性：

```js
input.value
input.type
input.disabled
input.checked
input.placeholder
```

这些属性并不一定全部写在 HTML 中，但 **只要元素类型支持，就一定存在于 DOM 对象上**。想要直接观察DOM元素的各个属性，可以直接打开开发者工具，然后再元素界面选择需要观察的元素，接着在下面找到属性：

![image-20260124000847227](https://s2.loli.net/2026/01/24/VTxgbqYJ6FhGLBE.png)

DOM 属性的访问方式非常直观，**就像普通 JS 对象一样**，比如我们想要修改元素的`id`属性，直接对其进行修改即可，修改后会立即生效到元素上：

```js
const box = document.getElementById('box')
box.id = 'crazy'
```

![image-20260214210437028](https://files.seeusercontent.com/2026/02/14/nlS9/image-20260214210437028.png)

对于一些非标准HTML属性，我们也可以使用`dataset`来实现：

```js
box.dataset.role = 'guest'
```

![image-20260215153512552](https://files.seeusercontent.com/2026/02/15/3sqR/image-20260215153512552.png)

此外，它还可以实现自动解析驼峰的效果：

```js
console.log(box.dataset.userId)  //获取的其实是data-user-id
```

不过，和前面介绍的 HTML 属性的一个区别是，DOM 属性的值不一定是字符串。比如一些布尔属性，它们的值就是布尔类型的：

```js
console.log(box.draggable)  //false
```

这里我们着重介绍两个比较常用的DOM属性，首先是用于设置`class`的`className`和`classList`属性：

```js
console.log(box.className)   //字符串结果
box.className = "crazy item"
```

修改`className`后，元素的`class`属性也会一起跟着变化，但是如果我们需要追加新的类，还需要手动进行拼接，这显然是非常麻烦的，所以我们更推荐大家使用下面的`classList`属性来修改类，它使用起来更加方便：

```js
console.log(box.classList)   //得到一个类列表
```

这里得到的是一个`DOMTokenList`对象，它可以实现对所有以空格分割的属性进行快捷编辑，我们可以直接通过它来进行类的增加和删除，直接使用`add`就可以插入新的类了，就像使用数组那样：

```js
box.classList.add('crazy', 'thursday', 'vivo', '50yuan')
```

![image-20260214215036006](https://files.seeusercontent.com/2026/02/14/Jj7s/image-20260214215036006.png)

我们也可以使用`remove`来对属性进行删除：

```js
box.classList.remove("item")
```

![image-20260214215306162](https://files.seeusercontent.com/2026/02/14/Kkw0/image-20260214215306162.png)

其他方法也很好用：

```js
box.classList.toggle('active')   //切换类，如果有则删除，没有就新增
box.classList.toggle('active', true)   //切换类，强制设置
box.classList.contains('active')   //判断是否包含类
box.classList.replace("item", "crazy")  //把一个类替换成另外一个类
```

我们接着来看如何更改内联样式，使用`style`即可：

```html
<div id="box" style="background: pink">我是文本</div>
```

```js
console.log(box.style)
```

`style`属性中包含了目前几乎所有可用的CSS属性，需要调整哪个属性，只需要直接对其进行赋值即可。比如我们现在需要修改文本的颜色：

```js
box.style.color = 'white'   //等价于 color: white; 白色
```

![image-20260214225933760](https://files.seeusercontent.com/2026/02/14/eQp7/image-20260214225933760.png)

此时页面上的元素`style`属性也会跟着发生变化，每个属性的设置都是独立的，不会影响其他属性。

当然，DOM属性不仅包含HTML标签上的属性，还有一些元素自己本身的属性，比如前面介绍的`innerText`、`nodeType`等，其实都是DOM属性。有关其他DOM属性，我们会在后续课程中逐步进行讲解，大家在使用时也可以在MDN文档上自行查询需要的属性。

### 元素内容

在前面我们已经学习了如何获取元素，以及元素的属性，这一节我们接着来介绍几个比较特殊的属性，它们可以操控元素的内容。在所有的元素对象中，`innerHTML` 用来 **获取或设置元素内部的 HTML 结构**：

```html
<div id="box">
  <span>hello</span>
</div>
```

```js
const box = document.getElementById('box')
console.log(box.innerHTML)
// <span>hello</span>
```

可以看到，`innerHTML`代表的是元素内部的HTML文档结构，打印出来的值也是一个HTML格式的代码。同样的，既然可以获取，我们也可以使用这个属性来修改内部的HTML代码：

```js
box.innerHTML = '<p>新的内容</p>'
```

![image-20260213164153780](https://files.seeusercontent.com/2026/02/13/9qYs/image-20260213164153780.png)

虽然这种做法在修改HTML结构上简单粗暴，但是它存在诸多问题，其中最主要的就是安全和性能问题：

1. 当你修改 `innerHTML` 时，浏览器并不会只更新你改动的那一小部分，浏览器必须销毁该元素下所有的旧 DOM 节点，重新解析 HTML 字符串，并创建新的 DOM 对象，大规模的 DOM 变更会触发昂贵的浏览器重排（Reflow）和重绘（Repaint），在循环中使用 `innerHTML += '...'` 更是性能自杀。
2. 如果你直接把用户输入的数据赋给 `innerHTML`，就相当于给黑客开了后门，因为客户可以利用这种方式在你的网页上直接插入`javascript`标签来执行恶意JS代码。
3. `innerHTML` 对格式要求极严。如果你不小心传入了未闭合的标签（比如 `<div>文字`），浏览器会尝试自动修复，但这往往会导致最终生成的 DOM 结构和你预期的完全不同，甚至引发布局崩坏。
4. 如果你用 `innerHTML` 覆盖了一个容器，即便你只是改了一行文字，原本绑定在容器内部子元素上的所有 **Event Listeners**（事件监听器）都会随之消失（后续章节会介绍事件监听机制）

因此，如果实在需要增加删除或是修改元素，我们更推荐使用后续章节中讲到的方法来操作，而不是直接修改HTML内容。如果仅仅只是修改页面上的文字内容，建议使用下面的`textContent`方式。

除了`innerHTML`之外，还有`innerText`，它可以用来获取或设置 **“人眼能看到的文本”**。

```html
<div id="box">Hello World</div>
```

或是这种嵌套写法，都只会解析可视文本部分：

```html
<div id="box">
  Hello <span>World</span>
</div>
```

```js
console.log(box.innerText)   //Hello World
```

注意，如果文本内容是不可见的，那么这里得到的结果也会是一个空字符串：

```html
<div id="box" style="visibility: hidden">Hello World</div>
```

最后，还有一个`textContent`，它的功能和`innerText`差不多，也可以用来获取或设置 **纯文本内容**，但是它不会受到可视性影响：

```js
console.log(box.textContent)   //即使不可见依然可以得到正确结果
```

此外，针对于HTML中的换行也会一起保留下来，而`innerText`是渲染之后的实际展示文本，不会保留换行：

```html
  <div id="box" style="visibility: hidden">Hello
    World</div>
```

![image-20260213172145204](https://files.seeusercontent.com/2026/02/13/hOk8/image-20260213172145204.png)

相比`innerText`，它获取内容的性能更高，因为前者需要计算布局（Reflow）来确定文本是否可见。但是，一些特殊标签里面的内容也会被`textContent`拿到：

```html
<div id="box">
  <script>
      for (let i = 0; i < 6; i++) {
          console.log("孩子们，这并不好笑")
      }
  </script>
</div>
```

![image-20260213172358089](https://files.seeusercontent.com/2026/02/13/rdA6/image-20260213172358089.png)

而`innerText`则会绕过这些实际不可见的内容。无论使用`textContent`还是`innerText`，我们都可以对内容进行修改，但是注意，这里设置的是纯文本内容，也就是普通文本元素：

```js
box.textContent = "卡布奇诺今犹在，不见当年倒茶人"
```

```js
box.innerText = "卡布奇诺今犹在，不见当年倒茶人"
```

![image-20260213172609962](https://files.seeusercontent.com/2026/02/13/7Eei/image-20260213172609962.png)

注意，由于这里仅仅只是文本内容设置，所以即使我们设置一段HTML代码，也会被自动转义为文本形式的内容，变成普通文本元素：

```js
box.innerText = "<div>卡布奇诺今犹在，不见当年倒茶人</div>"
```

![image-20260213173110970](https://files.seeusercontent.com/2026/02/13/r2kR/image-20260213173110970.png)

由于`textContent`不考虑实际渲染样式，所以在外面修改内容时浏览器不需要重新计算布局，在性能上相比`innerText`更加稳定，如果要频繁对页面内容进行修改，建议优先考虑`textContent`。

除了使用`inner`获取内容外，我们还可以使用`outer`来获取包含元素本身在内的全部内容：

```js
console.log(box.outerHTML)
```

![image-20260215143630779](https://files.seeusercontent.com/2026/02/15/As5r/image-20260215143630779.png)

注意，当我们替换`outerHTML`内容时，会连带整个标签一起变化。

还有`outerText`，但是这个属性默认情况下和`innerText`其实是一致的，不过，如果我们尝试修改它，它会连带着整个标签一起变成普通文本：

```js
box.outerText = "牛逼啊"
```

![image-20260215145045049](https://files.seeusercontent.com/2026/02/15/3nZr/image-20260215145045049.png)

### 创建和操作元素

前面我们介绍了如何**修改内容**，只需要利用`innerHTML / innerText / textContent`属性即可。但是实际上，直接对`innerHTML`进行修改会导致一些性能问题，这一节我们就介绍一下更多关于 **DOM 的增删改操作**。

想在页面中新增一个元素，**第一步一定是创建它**，`document`对象为我们提供了一个`createElement`方法可以快速创建一个新的元素：

```js
const div = document.createElement('div')
div.textContent = '我是新创建的元素'   //和普通元素一样，可以正常设置属性
```

不过，这仅仅只是在内存中创建了一个新的元素，此时在页面上还看不到任何变化。创建完元素之后，**一定要插入到 DOM 树中**，我们可以使用`appendChild`方法来将它添加到某个元素的内部，作为子元素存在：

```js
const box = document.getElementById('box')
const div = document.createElement('div')
div.textContent = '我是新创建的元素'
//调用父元素的appendChild方法为其添加这个新创建的元素到内部
box.appendChild(div)
```

![image-20260213192117331](https://files.seeusercontent.com/2026/02/13/e4bT/image-20260213192117331.png)

注意，`appendChild`只会在原有基础上新增子元素，如果原本就存在子元素，则会自动添加到原本存在元素的后面：

![image-20260213192700748](https://files.seeusercontent.com/2026/02/13/rZ5b/image-20260213192700748.png)

还有一个比较有意思的是，如果这个元素不是我们创建的，而是原本在页面上就存在的，那么将其添加到新的位置后，相当于是从原来的位置移动到这个新的位置上，而不是复制一个新的元素插入：

```js
const box = document.getElementById('box')
const item = document.querySelector('.item')
//实际上是将这个元素移动到box的下面
box.appendChild(item)
```

除了原本的`appendChild`，我们也可以使用`append`方法来实现元素插入，效果是完全一样的，但是它支持填入多个元素，按照顺序一次性插入：

```js
const box = document.getElementById('box')
const div = document.createElement('div')
div.textContent = '我是新创建的元素'
const item = document.querySelector('.item')
box.append(div, item)
```

此外，除了插入元素之外，`append`还可以直接插入一个字符串，这个字符串直接作为文本元素存入。

```js
const box = document.getElementById('box')
box.append("我是文本内容")
```

![image-20260213193511262](https://files.seeusercontent.com/2026/02/13/zy1V/image-20260213193511262.png)

这里除了创建简单元素之外，实际上`document`还为我们提供了很多不同的节点类型创建：

```js
document.createTextNode('hello')   //可以实现创建一个文本节点
document.createComment("这是注释")   //创建注释
```

这里需要注意的是，虽然`append`可以实现节点插入，但是依然会出现前面提到的性能问题，如果我们需要一次性插入多个节点，建议使用`DocumentFragment`，它就像是一个临时的小DOM树，也可以增删DOM元素，我们可以先把要插入的元素放入其中，之后一次性进行插入：

```js
const fragment = document.createDocumentFragment();  //创建一个临时dom树
//添加元素到fragment中
box.append(fragment)
```

这也可以极大地优化批量插入元素的情况，相比`for`循环每次插入都会导致元素重新计算发生重排，这种方式会大大减少重排次数，提高性能。

当然，既然可以在尾部插入元素，我们也可以在首部插入元素，新元素会出现在 **第一个子节点的位置**：

```js
const box = document.getElementById('box')
box.prepend(div, item, "我是文本内容")   //效果和上面一样，但是在元素内部的前面插入
```

我们还可以实现精确位置插入，使用`insertBefore`可以实现在指定元素的前面插入，比如我们现在想要插入到这个`p`子标签的前面：

```html
<div id="box">
  <a>你干嘛</a>
  <p>哎哟</p>
</div>
```

```js
const box = document.getElementById('box')

const div = document.createElement('div')
div.textContent = '我是新创建的元素'
//除了直接使用document来查询元素之外，我们还可以对着任意一个元素进行查询
const p = box.querySelector('p')

box.insertBefore(div, p)  //这里的意思将div插入到p之前
```

不过，这种方式只能往某个元素的前面插入，用起来不是很方便。这里更建议大家使用更加万能的元素插入操作`insertAdjacentElement`，它的参数可以自由控制插入点，相比前面几种更加方便：

```js
const box = document.getElementById('box')
const div = document.createElement('div')
div.textContent = '我是新创建的元素'
const p = box.querySelector('p')
//第一个参数用于控制插入点,beforebegin 就是在它本身之前
box.insertAdjacentElement('beforebegin', div)
```

![image-20260214000419704](https://files.seeusercontent.com/2026/02/13/9Zlt/image-20260214000419704.png)

| position        | 位置说明                     | 示意           |
| --------------- | ---------------------------- | -------------- |
| `"beforebegin"` | 在 target **前面**（当兄弟） | target 之前    |
| `"afterbegin"`  | 在 target **内部最前**       | 第一个子元素   |
| `"beforeend"`   | 在 target **内部最后**       | 最后一个子元素 |
| `"afterend"`    | 在 target **后面**（当兄弟） | target 之后    |

如果只是单纯插入一个文本元素，那么直接使用`insertAdjacentText`，它可以实现普通文本插入：

```js
box.insertAdjacentText('beforebegin', "我是文本内容")
```

当然，如果你希望插入一段HTML代码，也可以使用`insertAdjacentText`：

```js
box.insertAdjacentText('beforebegin', "我是文本内容")
```

除此之外，还有`insertAdjacentHTML`方法可以实现对HTML代码的插入，但是注意，这种方式和`innerHTML`一样，存在一些性能问题和XSS攻击问题。

介绍完元素的插入，我们接着来看元素的删除，删除非常简单，只需要直接对着元素自己调用`remove`方法即可：

```js
const box = document.getElementById('box')
box.remove()   //移除自己
```

当然，我们也可以通过`removeChild`来移除子元素，但是这里需要传入子元素（那我干嘛不直接对着子元素`remove`呢）

```js
const box = document.getElementById('box')
const p = box.querySelector('p')
box.removeChild(p)
```

然后是元素的替换操作，使用`replaceChild`即可替换某个指定元素：

```js
const box = document.getElementById('box')
const p = box.querySelector('p')
const a = document.createElement('a')
a.textContent = '点我啊哥哥'
//第一个参数是新的元素，第二个参数是原本需要被替换的
box.replaceChild(a, p)
```

我们还可以使用`replaceChildren`来实现对所有子元素的替换：

```js
box.replaceChildren(a, "我是普通文本元素")
```

`replaceChildren`会移除所有子元素，并将给到的元素替换进去。

除了对子元素进行替换之外我们也可以直接对某个元素进行替换，使用`replaceWith`替换：

```js
const box = document.getElementById('box')
const a = document.createElement('a')
a.textContent = '点我啊哥哥'
//直接替换调用元素
box.replaceWith(a)
//也可以一次性传入多个元素，包括文本元素也是可以的，注意不会进行HTML转换
box.replaceWith(a, "我是普通文本元素")
```

### 元素关系

在真实开发中，我们**几乎不可能只操作一个元素**，更多情况是：批量修改列表样式，遍历表格、菜单、卡片，找某个元素的父 / 子 / 兄弟节点。实现这一切的关键就是在 DOM 树中，根据关系找到其他节点。

常见的关系有 3 种：**父节点**、**子节点**、**兄弟节点**

首先我们来看看如何通过元素找到自己的父元素，使用`parentElement`即可直接得到父元素：

```js
const box = document.getElementById('box')
console.log(box.parentElement)
```

![image-20260214124924131](https://files.seeusercontent.com/2026/02/14/npS7/image-20260214124924131.png)

由于这里返回的是`HTMLElement`对象，所以我们还可以继续使用`parentElement`向上找：

```js
console.log(box.parentElement)  //body
console.log(box.parentElement.parentElement)   //html
console.log(box.parentElement.parentElement.parentElement)   //null
```

当寻找到DOM的最顶层时，无法再继续获得父元素了。除了`parentElement`外，我们也可以使用`parentNode`来获取父节点，但是相比`parentElement`，它支持更多类型，不仅仅是Element对象，包括Document也是能拿到的：

```js
console.log(box.parentNode)   //body
console.log(box.parentNode.parentNode)   //html
console.log(box.parentNode.parentNode.parentNode)  //document对象
```

我们接着来看子元素的获取，使用`childNodes`来得到所有子节点，注意这里得到的是Node，也就是说会包含多种类型的节点，包括文本和注释：

```html
<div id="box">
  <!--  我是个很搞笑的注释  -->
  <a>你干嘛</a>
  <p>哎哟</p>
</div>
```

```js
const box = document.getElementById('box')
console.log(box.childNodes)
```

![image-20260214163140004](https://files.seeusercontent.com/2026/02/14/fp9W/image-20260214163140004.png)

可以看到这里得到了7个节点，不对啊，怎么会有7个呢？算上注释这里明明只有3个节点啊，这其实是因为空白部分也被算作文本节点导致的：

![image-20260214163300393](https://files.seeusercontent.com/2026/02/14/1osX/image-20260214163300393.png)

如果需要精确获取HTML元素，不包含无关的文本和注释，我们可以使用`children`属性：

```js
console.log(box.children)
console.log(box.childElementCount)   //使用childElementCount还能快速得到子元素数量
```

![image-20260214163352501](https://files.seeusercontent.com/2026/02/14/Xtz0/image-20260214163352501.png)

得到的结果是一个`HTMLCollection`，只包含HTML元素。有时候为了方便，我们还可以快速获取第一个或最后一个子元素：

```js
console.log(box.firstChild)   //第一个子元素，拿到的是Node对象，包含文本和注释
console.log(box.lastChild)    //最后一个子元素，拿到的是Node对象，包含文本和注释
console.log(box.firstElementChild)   //第一个子元素，拿到的是Element对象
console.log(box.lastElementChild)    //最后一个子元素，拿到的是Element对象
```

除了查找子元素外，我们也可以实现对兄弟元素的查找，`Sibling`代表兄弟元素，和上面一样，我们也可以获取包含文本或注释的或是纯HTML元素的：

```js
console.log(box.previousSibling);  //上一个兄弟元素，拿到的是Node对象，包含文本和注释
console.log(box.nextSibling);  //下一个兄弟元素，拿到的是Node对象，包含文本和注释
console.log(box.previousElementSibling);  //上一个兄弟元素，拿到的是Element对象
console.log(box.nextElementSibling);   //上一个兄弟元素，拿到的是Element对象
```

综上，建议各位小伙伴尽量使用返回结果是`Element`的属性，实际开发中它们更加常用。

### 元素位置和滚动

在真实开发中，我们经常会遇到这些需求：判断一个元素**在页面的什么位置**、获取元素**距离窗口顶部/左侧的距离**、判断元素是否**出现在可视区域**等等，这些能力，都和 **元素位置（Position）** 与 **滚动（Scroll）** 有关。

`offset` 系列属性，是 **最容易理解、也是新手最常用的一组**，它反映的是元素在**页面布局中的位置和尺寸**，常用的有 4 个：

```js
element.offsetWidth   //元素宽度
element.offsetHeight   //元素高度
```

这里的元素高度是按照`border-box`进行计算的，也是就包含了边框、内边距、内容在内的总宽度，不包含外边距，它可以反应盒子本身占据的空间。

```js
element.offsetLeft   //元素x坐标
element.offsetTop    //元素y坐标
```

`left`和`top`坐标表示**元素左上角**相对于 **offsetParent** 的距离。`offsetParent` 指的是离当前元素最近的、设置了定位（position 不为 static）的祖先元素，如果一路往上都没找到，那么默认是 `body`（其实就是CSS中介绍的定位方式）我们可以直接打印`offsetParent`查看这个定位父元素是谁：

```js
console.log(box.offsetParent)   //body
```

我们也可以稍加修改：

```html
<div style="position: relative">
  <div id="box">我是文本</div>
</div>
```

![image-20260215022931547](https://files.seeusercontent.com/2026/02/14/5Otg/image-20260215022931547.png)

注意，以上提到的属性都是由CSS进行控制的，为只读属性，无法手动进行修改。

接着是`client` 系列，它们描述的是 **元素内部可视区域**，和滚动条强相关。

```js
element.clientWidth  //元素宽度
element.clientHeight  //元素高度
```

这里的元素高度是按照`padding-box`进行计算的，也就是只包含了内边距、内容在内的总宽度，不包含边框和外边距，包括滚动条占据的宽度也不包含。

```html
<style>
  #box {
    overflow: auto;
    height: 100px;
    width: 200px;
    border: 2px solid black;
  }

  .inner-box {
    background: #f0f0f0;
    height: 2000px;
  }
</style>
```

```html
<div id="box">
  <div class="inner-box"></div>
</div>
```

```js
console.log(box.offsetWidth)   //204，整个盒子大小
console.log(box.clientWidth)   //185，实际内容区域大小
```

同样的，clientLeft / clientTop可以表示`padding-box`的坐标，但是注意这个坐标是相对的盒子的边框的距离，一般情况下就是盒子边框的大小：

```js
console.log(box.clientLeft)   //2，距离左边框大小
console.log(box.clientTop)   //2，距离上边框大小
```

这个属性一般很少单独用，知道它是 **border 的厚度** 就够了，同样的，这里提到的4个属性无法被手动修改，由CSS控制的只读属性。

我们接着来介绍滚动状态下，内容的这四个属性又会如何变化，首先是`scrollWidth / scrollHeight`属性，它代表实际的滚动区域宽度和高度：

```js
console.log(box.scrollWidth)  //实际的滚动区域宽度，也就是总的滚动宽度
console.log(box.scrollHeight)   //实际滚动区域高度，也就是总的滚动高度
```

而`scrollLeft / scrollTop`表示的则是当前滚动的左边位置和顶部位置：

```js
console.log(box.scrollTop)   //内容滚动顶部位置
console.log(box.scrollLeft)   //内容滚动左侧位置
```

![image-20260215025953866](https://files.seeusercontent.com/2026/02/14/Bdr3/image-20260215025953866.png)

和前面不同，针对于`scrollLeft / scrollTop`属性，我们可以手动为其设置一个值，使得滚动位置可以动态调整：

![image-20260215030330493](https://files.seeusercontent.com/2026/02/14/Hwo7/image-20260215030330493.png)

手动赋值后，滚动条会自动跳到我们指定的位置上。不过，这里更推荐大家使用专门的滚动控制方法`scrollTo`或是`scroll`，这两个方法效果完全一样，只是名字不同，文档更推荐使用`scroll`方法：

```js
//使用数字直接控制在x轴和y轴上滚动到哪个位置，px为单位
box.scroll(0, 0)
//这里需要传入一个对象，其中top和left属性控制滚动目标位置（可以缺一）
//behavior控制滚动行为，目前只有立即滚动和平滑滚动
box.scroll({ top: 0, behavior: 'smooth' })
```

如果希望滚动过程更加流畅丝滑，建议使用平滑滚动效果，不同浏览器会有不同的展现方式，但是都大差不差。

出了让滚动条直接跳到指定位置，我们也可以控制滚动条相对滚动，使用`scrollBy`来实现：

```js
box.scrollBy(0, 1000)
```

这里会使得滚动条向`y`轴方向向下滚动1000个像素点。

此外，还有一个滚动方法，`scrollIntoView`可以实现滚动到指定元素位置，使得指定元素在可视区域中：

```html
<div style="height: 2000px">我是页面上的一段普通内容</div>
<div id="box">我是内容</div>
```

```js
const box = document.getElementById('box')
box.scrollIntoView()
```

此外，除了获取元素在页面上的绝对位置之外，我们也可以获取元素位于视口上的信息，它会按照元素实际距离视口位置进行取值，当滚动位置发生变化的时候，元素视口位置也会跟着发生变化：

```js
const box = document.getElementById('box')
console.log(box.getBoundingClientRect())
```

![image-20260215113852456](https://files.seeusercontent.com/2026/02/15/t4jW/image-20260215113852456.png)

这里的`top`指的是元素顶部和视口顶部距离，当元素滚动到视口顶部时，`top`值也会变成0，而如果已经滚到上面去了，那么`top`会变成负数，`left`也是同理。`y`和`x`值和这里的`top`和`left`是一样的，不多做介绍了。

除了上面和左边，`bottom`也可以为我们提供元素底部与视口底部的距离（等价于 `top + height`），`right`同理，和`left`是相对应的。

这里的`width`和`height`默认情况下和之前的`offsetWidth`一样，表示元素的总宽度（包括 padding、border，不包括 margin）但是，如果元素应用了 CSS 变换（如 `scale`、`rotate`），返回的尺寸和位置也会是变换后的最终结果：

```html
<div id="box" style="width:fit-content;scale: 1.5">我是内容</div>
```

```js
const box = document.getElementById('box')
console.log(box.offsetWidth)   //元素变换前本身占据空间的尺寸 64px
console.log(box.getBoundingClientRect())   //width = 96px
```

可以看到，这里发生了CSS变换，虽然元素本身占据的位置大小不变，但是实际展示的大小不一样。这对于需要获取元素实际展示尺寸的情况，就非常好用。

## 事件

在讲 DOM 事件之前，我们先回顾一下 **DOM（Document Object Model）** 是什么。当浏览器解析 HTML 时，会把页面结构转换成一棵“节点树”，这棵树就是 DOM。而 **DOM 事件（DOM Event）**，就是当用户或浏览器对页面做出某种行为时，浏览器通知 JavaScript 的一种机制。

事件就是页面上发生的动作，例如：用户点击按钮、鼠标移动、键盘按下、页面加载完成、表单提交。

### 事件处理

理解事件时，通常可以拆解为三个部分：

- **事件源（Event Target）：** 谁触发了事件？（例如：一个 `<button>` 按钮）
- **事件类型（Event Type）：** 发生了什么？（例如：`click` 点击、`keydown` 按键）
- **事件处理程序（Event Handler）：** 发生了之后要做什么？（通常是一个 JavaScript 函数）

比如，我们现在需要处理按钮的点击事件，那么事件源就是一个按钮元素，事件类型就是点击事件，在明确事件源和事件类型后，我们就可以针对这个事件进行处理，当发生这个事件时，就执行我们自定义的JS代码。

处理事件一共有三种方式，我们先来看最简单的一种，我们可以直接在标签上进行编写：

```html
<button onclick="alert('你点击了按钮')">我是一个大按钮</button>
```

这种方式叫做 **内联事件**，一般来说，事件属性名称都是以`on`开头的，比如这里的点击事件，就是`onclick`，当用户点击元素时就会触发这个事件，浏览器为我们准备了多种多样的事件，以便我们可以在各种情况下都能处理用户的交互行为。

在`onclick`属性的内部，需要填写我们想要执行的JS代码，和之前一样，不能出现语法错误，同时，由于直接在行内编写，出现多行JS代码时，需要使用`;`隔开：

```html
<button onclick="console.log('我是测试');alert('你点击了按钮')">我是一个大按钮</button>
```

虽然这种方式写起来非常直观，但是只适合单行JS代码编写，如果有很多行写起来会非常麻烦，建议如果存在多行的情况下，最好封装成函数的形式使用：

```js
function test() {
    console.log('我是测试')
    alert('你点击了按钮')
}
```

```html
<button onclick="test()">我是一个大按钮</button>
```

除此之外，我们也可以使用DOM属性来进行事件处理：

```js
const btn = document.getElementById('test');
//直接为元素的onclick属性赋值，即可将函数绑定到此事件上
btn.onclick = () => {
  console.log('我是测试');
  alert('你点击了按钮');
}
```

这种方式和前面的写法效果完全一样，并且我们可以在赋值（绑定）的函数中自由编写代码。

不过，虽然这种方式可以更方便的绑定事件处理逻辑，但是这个事件的处理逻辑只能存在一个，重复赋值只会覆盖原本的事件处理逻辑，如果我们需要为某个时间添加多个处理操作，就需要用到更加高级的事件监听器了：

```js
//添加事件监听器来实现事件处理
btn.addEventListener('click', () => {
  console.log('我是测试');
  alert('你点击了按钮');
})
```

我们可以为指定事件添加事件监听器，当触发事件时，事件监听器中编写的函数也会自动执行，这是现代开发的标准方式。同时，事件监听器可以无限制添加，当触发时，会按照添加的顺序依次执行：

```js
btn.addEventListener('click', () => {
  console.log('我是测试1');
})
btn.addEventListener('click', () => {
    console.log('我是测试2');
})
btn.addEventListener('click', () => {
    console.log('我是测试3');
})
```

![image-20260224164303807](https://files.seeusercontent.com/2026/02/24/7Bgn/image-20260224164303807.png)

同样的，如果我们不需要某个事件监听器了，也可以移除它：

```js
const handler = () => {
    console.log("我是测试")
}

btn.addEventListener('click', handler)
btn.removeEventListener('click', handler)   //注意移除的必须是同一个函数对象
```

有些时候，我们可能只希望事件监听器只执行一次，也可以进行参数配置：

```js
btn.addEventListener('click', handler, { once: true })  
//最后一次参数是监听器配置，once表示只监听一次
```

以上三种事件处理方式都可以实现事件处理，各位小伙伴可以根据自己的实际需求进行选择。

需要注意的是，在事件监听中，非箭头函数的`this`的指向并不是全局对象（箭头函数依然是按照之前的规则绑定`this`）而是当前元素本身：

```js
function handler() {
    console.log(this)   //在调用时，由于是在事件中执行，this指向的就是事件源
}
btn.addEventListener('click', handler)
```

所以在事件里，如果你需要用到当前元素对象，可以直接使用`this`，但是注意箭头函数形式是无效的。

除了简单监听事件之外，我们还可以使用`event`对象，每次事件触发，浏览器都会自动传入一个事件对象，除了在标签属性上无法使用外，其他地方都可以直接作为参数使用：

```js
//直接修改onclick可以使用
btn.onclick = event => console.log(event)

//事件监听器可以使用
btn.addEventListener('click', (event) => {
    console.log(event)
})
```

这个 `event` 包含了本次事件的多种信息，比如触发的元素、鼠标位置、按键信息、是否按下`ctrl`、事件阶段、是否可取消等。比如点击事件，它包含了：

```js
event.target        // 真正触发事件的元素
event.currentTarget // 当前绑定事件的元素（至于为什么要分两个，下面接着介绍）
event.type          // 事件类型
event.clientX       // 鼠标X坐标
event.clientY       // 鼠标Y坐标
```

有关不同事件对象的详细信息，我们会在后续课程中逐步介绍。

### 事件流

事件流是事件章节最重要的核心知识，实际上浏览器在处理事件时，并不是简单的“点谁就执行谁”。想象一下，你点击了页面上的一个按钮，你不仅点击了按钮本身，同时也点击了它的父容器、甚至整个页面。

为了解决“到底谁先响应”的问题，浏览器采用了三阶段的流动模型。根据 W3C 标准，一个事件的生命周期按顺序分为以下三个阶段：

1. 捕获阶段 (Capture Phase)

   事件从最顶层的窗体对象（`Window`）开始，逐级向下传播，直到到达触发事件的具体元素。

   * **目的：** 在事件到达目标之前拦截它。
   * **顺序：** `Window` -> `Document` -> `<html>` -> `<body>` -> ... -> 目标元素的父级。

2. 目标阶段 (Target Phase)

   事件终于到达了真正触发它的那个元素（即 `event.target`）。

   - 在这个阶段，事件被触发并执行绑定的监听函数。

3. 冒泡阶段 (Bubbling Phase)

   事件从目标元素开始，逐级向上传播，直到回到最顶层的 `Window` 对象。

   - **意义：** 这是最常用的阶段，也是 JavaScript 默认监听的阶段。
   - **顺序：** 目标元素 -> 父级 -> ... -> `<body>` -> `<html>` -> `Document` -> `Window`。

比如下面的结构：

```html
<body>
  <div>我是页面上的一段普通内容</div>
  <div>
    <button>我是一个大按钮</button>
  </div>
</body>
```

![image-20260224174319642](https://files.seeusercontent.com/2026/02/24/3Cei/image-20260224174319642.png)

事件会从整个文档DOM树的最底层逐步传播到目标元素上，当找到目标元素时，开始按照冒泡顺序依次触发事件：

```html
<body onclick="console.log('3')">
  <div>我是页面上的一段普通内容</div>
  <div onclick="console.log('2')">
    <button onclick="console.log('1')">我是一个大按钮</button>
  </div>
</body>
```

![image-20260224175023351](https://files.seeusercontent.com/2026/02/24/w5Ab/image-20260224175023351.png)

可以看到，最内层点击的目标元素就是冒泡的最底部，从这个元素开始，逐步向外层元素触发`onclick`事件，所以事件触发并非是点击某个元素本身触发，包含此元素的盒子、页面都会一起触发点击事件，虽然从层级关系来说最上面展示的应该是按钮，但是点击按钮的同时也点击了压在下面的外层盒子。

这种机制很好地实现了想要一次性监听外层盒子的事件触发，而非局限于某个内部元素的效果。

理解了事件流，就能掌握前端开发中最重要的性能优化手段——**事件委托**，假如一个父容器（比如 `<ul>`）下面有 1000 个子元素（`<li>`），你不需要给每个 `<li>` 都绑定点击事件。相反，你可以利用 **冒泡机制**，只在 `<ul>` 上绑定一个监听器：

```html
<ul>
  <li>我是列表项1</li>
  <li>我是列表项2</li>
  <li>我是列表项3</li>
  <li>我是列表项4</li>
</ul>
```

```js
document.querySelector('ul').addEventListener('click', evt => {
    console.log(evt.target)  //直接通过target获取被点的li标签
  	console.log(evt.currentTarget)  //获取当前事件触发元素ul
})
```

当点击`ul`范围内的任何一个元素时，都会通过冒泡机制触发`ul`的点击事件，我们只需要通过`target`属性即可拿到具体点击的是哪一个元素了。

除了正常执行事件流之外，我们也可以通过 `addEventListener` 方法来决定在哪一个阶段“拦截”事件：

* **`capture` 为 `false`（默认值）：** 监听函数在 **冒泡阶段** 执行。
* **`capture` 为 `true`：** 监听函数在 **捕获阶段** 执行。

```html
<body onclick="console.log('3')">
  <div>我是页面上的一段普通内容</div>
  <div id="test">
    <button onclick="console.log('1')">我是一个大按钮</button>
  </div>
</body>
```

```js
btn.addEventListener('click', (event) => {
    console.log("2")
}, {   //第二个参数填写监听选项
    capture: true
})
```

![image-20260224184509168](https://files.seeusercontent.com/2026/02/24/6Yxi/image-20260224184509168.png)

可以看到，当`capture`开启之后，事件的处理会提前到捕获阶段执行。

有时候，我们可能希望点击子元素后不触发父元素的任何逻辑，这时你需要手动“截断”流，也就是阻止事件继续向后传播或是冒泡。我们可以使用`stopPropagation`方法：

```js
btn.addEventListener('click', (event) => {
    event.stopPropagation()   //停止事件继续传播
    console.log("2")
})
```

![image-20260224190037562](https://files.seeusercontent.com/2026/02/24/M1sp/image-20260224190037562.png)

可以看到，事件没有继续向上进行冒泡，父元素的事件处理不再执行，不过，由于阻止的是事件冒泡，所以如果我们将事件处理时机提前到`capture`，那么事件依然可以被处理。

需要注意的是，这里仅仅是阻止继续向下传播，但是当前元素绑定的其他同类型监听器仍会执行。如果需要彻底断绝后续所有监听器的事件处理，我们可以使用`stopImmediatePropagation`方法，它会立即结束所有事件冒泡并阻断后续监听器执行。

### 事件默认行为

这一节我们接着介绍事件相关的控制操作。首先是事件默认行为，在默认情况下，随着一些事件的触发，某些特殊的标签会产生一些默认行为，比如`a`标签在点击时会发生跳转：

```html
<a href="https://www.baidu.com" target="_blank" id="test">我是一个大按钮</a>
```

我们也可以通过事件监听来阻止其默认行为：

```js
a.addEventListener('click', event => {
    console.log("1")
    event.preventDefault()   //阻止事件默认行为
})
```

此时再次点击链接就无法进行跳转了。利用这种机制，我们还可以让用户在右键页面时无法弹出菜单，防止偷代码：

```js
//contextmenu是右键点击弹出菜单事件
document.documentElement.addEventListener('contextmenu', event => {
    event.preventDefault()
})
```

需要注意的是，并非所有事件都可以被阻止默认行为，我们可以通过`cancelable`属性来进行判断：

```js
console.log(event.cancelable)
```

它表示当前这个事件，是否“允许被取消默认行为”。常见可取消事件有：`click`、`submit`、`keydown`、`beforeunload`、`touchstart`、`wheel`。

在创建监听器时有一个非常有意思的参数，`passive`表示我们在Listener中永远不会调用`preventDefault`方法，但是如果我们又使用了这个方法的话，浏览器只会打印一个错误信息，而不会真的去执行`preventDefault`方法：

```js
a.addEventListener('click', event => {
    console.log("1")
    event.preventDefault()
}, {
    passive: true
})
```

![image-20260224223934192](https://files.seeusercontent.com/2026/02/24/oE1f/image-20260224223934192.png)

可以看到`preventDefault`并没有阻止事件行为触发，而是产生了一个错误信息到控制台（注意这里不是抛错误，不会导致后续代码执行失败）某些事件在一些浏览器上会默认将`passive`设置为`true`来保证一些性能上的优化。

### 鼠标事件

鼠标事件是前端开发中**最常用的一类事件**，几乎所有交互都离不开它。

常见鼠标事件如下：

| 事件名        | 说明               |
| ------------- | ------------------ |
| `click`       | 单击               |
| `dblclick`    | 双击               |
| `mousedown`   | 鼠标按下           |
| `mouseup`     | 鼠标抬起           |
| `mousemove`   | 鼠标移动           |
| `mouseenter`  | 鼠标进入（不冒泡） |
| `mouseleave`  | 鼠标离开（不冒泡） |
| `mouseover`   | 鼠标进入（会冒泡） |
| `mouseout`    | 鼠标离开（会冒泡） |
| `contextmenu` | 右键菜单           |

首先介绍一下`click`点击事件，最常见的点击事件：

```js
btn.addEventListener('click', function () {
  console.log('点击了')
})
```

注意，一次`click` = 按下 + 抬起 都发生在同一元素，如果按下在 A，抬起在 B，不算`click`。如果需要精细化控制，可以考虑`mousedown / mouseup`事件，它可以实现更底层的监听，分别针对鼠标按下和释放：

```js
btn.addEventListener('mousedown', () => {
  console.log('按下')
})

btn.addEventListener('mouseup', () => {
  console.log('抬起')
})
```

利用这种监听，就可以实现例如拖拽功能、长按检测、自定义按钮效果等操作。

`mousemove`事件当鼠标在元素上移动时会持续触发：

```js
btn.addEventListener('mousemove', (e) => {
  console.log(e.clientX, e.clientY)
})
```

然后是`mouseenter/mouseleave`，当鼠标进入/离开元素区域时，会触发一次：

```js
btn.addEventListener('mouseenter', () => {
  console.log('mouseenter')
})
```

与其类似的还有一个`mouseover/mouseout`事件，它们也可以在鼠标进入/离开元素区域时，触发一次，但是区别是，上面的`mouseenter/mouseleave`不会出现冒泡，而下面的会出现冒泡的现象。同时，`mouseover/mouseout`在进入子元素时也会触发，所以，实际开发中更推荐使用 `mouseenter`，它能更加准确反应鼠标进入元素。

在以上提到的所有鼠标事件中，常用的属性如下：

```js
  console.log(e.clientX) // 相对视口X坐标
  console.log(e.clientY)
  console.log(e.pageX)   // 相对页面X坐标
  console.log(e.pageY)
  console.log(e.button)  // 按下的是哪个鼠标键
```

我们的鼠标一共有三个按键，包含左中右：

| 值   | 按键 |
| ---- | ---- |
| 0    | 左键 |
| 1    | 中键 |
| 2    | 右键 |

### 键盘事件

键盘事件主要用于：表单输入、游戏控制和快捷键等功能。常见键盘事件包含：

| 事件名     | 说明                                       |
| ---------- | ------------------------------------------ |
| `keydown`  | 键按下                                     |
| `keyup`    | 键抬起                                     |
| `keypress` | 也是键按下，但不包含Ctrl这类控制键，不推荐 |

比如我们想要检测某个键是否被按下：

```js
//检测按键按下一般可以直接对着文档对象添加事件监听
document.addEventListener('keydown', e => {
  console.log(e.key)   //通过key属性来获取被按下的键
})
```

需要注意的是，如果我们对着某个子元素添加按键监听，那么是不会有效果的：

```html
<a href="https://www.baidu.com" target="_blank" id="test">我是一个<p>大按钮</p></a>
```

```js
a.addEventListener('keydown', function (e) {
    console.log(e.key)
})
```

这是因为，只有能成为**焦点元素 (focusable element)** 的 DOM 节点，才能响应键盘事件，比如，`<input>`/`<button>`/`<a>`/`<select>` 等表单 / 交互元素，就可以成为焦点元素。像 `<div>/<span>/<p>/<li>` 这类普通块级 / 行内子元素，则无法成为焦点元素。

我们可以试试看`input`标签：

```html
<input id="test">
```

![image-20260225101019073](https://files.seeusercontent.com/2026/02/25/Kp7k/image-20260225101019073.png)

当我们处于输入状态时，`input`标签就是聚焦状态，此时可以正确响应键盘事件。当然，除了通过上面的方式去聚焦之外，我们也可以使用JS代码来主动聚焦：

```js
const a = document.getElementById('test')
a.focus()
```

或是使用Tab键进行选择：

![image-20260225101956304](https://files.seeusercontent.com/2026/02/25/eJ7r/image-20260225101956304.png)

当然，如果是Tab键无法选中的元素，比如`div`这类，我们可以给需要监听按键的子元素添加一个核心属性，使其具备焦点能力：

```html
<div class="key-listener" tabindex="0">点击我后按任意键试试</div>
```

```js
document.querySelector('.key-listener').addEventListener('keydown', e => {
    console.log(e.key)
})
```

这里用到了`tabindex`属性，它能让普通元素具备焦点能力，现在使用Tab就可以快速聚焦了（`tabindex="-1"` 表示可通过 JS 手动聚焦，但不能通过 Tab 键选中，如果是正整数越大表示优先级越高）

接着是一些常用的键盘事件的属性：

```js
e.key        // 按下的字符
e.code       // 键盘物理位置
e.ctrlKey    // 是否按住ctrl
e.shiftKey
e.altKey
e.metaKey   //Mac键位专用
```

利用这些属性，我们可以通过判断来实现组合按键监听效果：

```js
document.addEventListener('keydown', e => {
  	//判断是否按下ctrl键（MacOS下command就是meta键）
    if((e.metaKey || e.ctrlKey) && e.key === 'c') {
        alert('你按下了Ctrl + C')
    }
})
```

结合前面介绍的阻止默认行为，我们还可以防止用户复制网页内容：

```js
document.addEventListener('keydown', e => {
    if((e.metaKey || e.ctrlKey) && e.key === 'c') {
        console.log("用户复制内容被阻止")
        e.preventDefault()
    }
})
```

### 表单事件

在实际开发中，**表单是最常见的交互区域**，一个表单中包含多种输入框和按钮。登录、注册、搜索、留言、提交订单……几乎所有业务都离不开表单。常见表单事件如下：

| 事件名   | 说明           |
| -------- | -------------- |
| `submit` | 表单提交       |
| `input`  | 输入时触发     |
| `change` | 失去焦点后触发 |
| `focus`  | 获得焦点       |
| `blur`   | 失去焦点       |
| `reset`  | 表单重置       |

首先是针对于表单中的输入框，有一个`input`事件，当输入框内容发生改变时立即触发（实时触发）

```html
<form id="loginForm">
  <label>
    <input type="text" id="username" placeholder="请输入用户名">
  </label>
  <label>
    <input type="password" id="password" placeholder="请输入密码">
  </label>
  <button>登录</button>
</form>
```

```js
const input = document.getElementById('username')

//监听input事件
input.addEventListener('input', () => {
    console.log('当前输入内容：', input.value)   //使用value属性来获取输入框中输入的内容
})
```

每输入一个字符都会触发`input`事件，包括删除、粘贴等，像实时校验用户输入内容是否合法就可以采用这种方式，比如判断用户输入的用户名是否是邮箱格式：

```js
input.addEventListener('input', () => {
  	//使用正则表达式进行判断
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    const isEmail = emailRegex.test(input.value)
    console.log('是否是邮箱:', isEmail)
})
```

这就是非常典型的一种表单校验案例。当然，还有一个`change`事件和 `input` 很像，但有一个关键区别，就是只有在内容变化且失去焦点后才触发。

针对于使用了中文输入法的情况，有时候非常恶心：

![image-20260225160152582](https://files.seeusercontent.com/2026/02/25/uSe7/image-20260225160152582.png)

由于中文输入法也是先输入每个字符再完成拼写，所以就会出现这种输入法过程中的文本输入也被监听到的情况，因此我们还可以更细致地进行事件监听：

1. compositionstart  -  输入法开始
2. compositionupdate  -  输入法正在打字
3. compositionend  -  输入法结束

```js
//只监听输入法结束时的事件
input.addEventListener('compositionend', () => {
    console.log(input.value)   //得到的就是输入法完成的结果
})
```

利用这种机制，我们就可以进行更加精确判断了：

```js
let composition = false

const showText = () => {
    if(!composition) {
        console.log(input.value)
    }
}

input.addEventListener('compositionstart', () => composition = true)
input.addEventListener('compositionend', () => {
    composition = false
    showText()   //因为compositionend不会一起触发input事件，所以完成输入法之后也得手动查看一次
})
input.addEventListener('input', showText)
```

除了普通的文本输入框之外，选择框也可以在选择之后触发`input`事件：

```html
<label>
  <select id="gender">
    <option>男</option>
    <option>女</option>
  </select>
</label>
```

```js
const select = document.getElementById('gender')
select.addEventListener('input', evt => {
    console.log(select.value)
})
```

接着是焦点获取相关的事件，在之前我们提到，部分元素是可以获得焦点的，键盘事件只有在取得焦点时才会正确响应，当元素聚焦或失焦时，也可以通过对应的事件来进行处理，`focus` 和 `blur` 就是聚焦和失焦时的事件：

```js
input.addEventListener('focus', function () {
  console.log('获得焦点')
})

input.addEventListener('blur', function () {
  console.log('失去焦点')
})
```

同样的，利用这种机制我们也可以在用户输入完成之后进行表单校验。

接着是非常重要的表单提交事件，在HTML阶段我们学习过，当点击表单内的按钮时，会自动提交表单内容，发起一个表单请求，默认为GET请求方式，对应的事件就是表单提交事件：

```js
const form = document.getElementById('loginForm')

form.addEventListener('submit', function (e) {
  console.log('表单提交了')
})
```

在默认情况下，表单提交会刷新页面并发送请求，我们也可以阻止其默认行为来防止页面刷新，并通过自行发起请求的方式来进行登录，有关发起XHR请求相关内容我们会在前端路线后续课程中介绍。

### 编辑事件

编辑事件主要用于可编辑内容区域，比如现在输入框中有一段文本：

```html
<textarea style="width: 300px;height: 200px" id="text"></textarea>
```

```js
const input = document.getElementById('text')
input.value = `1、祝有爱者有爱，无爱者自由。
2、不要靠近我，熟悉我，关心我，然后离开我。
3、人在刚认识的时候最好，虚伪且礼貌。
4、有空多爱自己，别人很忙。
5、世界上最危险的东西就是希望。
6、或许换个时间，有些人真的很合适。
7、我渴望你是救赎，也恐惧你是深渊。
8、想离开的人从来都不缺理由。
9、我对你的热情，在无数个夜晚里消耗殆尽。
10、也许最稳定的关系，就是没有关系。`
input.addEventListener('select', evt => {
    console.log(evt)
})
```

当我们选中其中一段文本之后，就会触发`select`事件：

![image-20260225172405128](https://files.seeusercontent.com/2026/02/25/xHa6/image-20260225172405128.png)

我们通过文本框的`selectionStart`和`selectionEnd`属性即可获得选择的起始和结束位置（就是选择的文本内容子串起始和结束下标）

```js
input.addEventListener('select', evt => {
    console.log("选择起始位置", input.selectionStart)
    console.log("选择结束位置", input.selectionEnd)
    console.log("选择内容:", input.value.substring(input.selectionStart, input.selectionEnd))
})
```

只不过，这个事件只能在一些可以编辑的元素中触发，如果是不可编辑的元素，是无法触发的：

```html
<div id="text">
  苹果正式官宣：2026年3月4日北京时间22:00
  举办「Apple Experience」特别活动
  纽约、伦敦、上海三地同步直播
  全场重点：iPhone 17e、全新平价Mac、M5全家桶
</div>
```

不过，还有一个事件`selectstart`可以在任意情况下触发，即使不是输入框，只要用户在页面上开始选择都会触发这个事件：

```js
div.addEventListener('selectstart', evt => {
    console.log(evt)
})
```

利用这个事件，我们可以实现阻止用户选择页面文本的效果：

```js
div.addEventListener('selectstart', evt => {
    console.log("用户尝试选择文本")
    evt.preventDefault()   //阻止选择开始
})
```

在前面我们为大家介绍了如何利用键盘事件来防止用户使用Ctrl+C复制页面内容，不过，虽然这种方式简单粗暴，但是如果用户有其他复制方式，比如右键菜单复制，依然可以实现内容拷贝，要彻底防止用户复制内容，我们可以使用`copy`事件来处理：

```js
div.addEventListener('copy', evt => {
    console.log("用户尝试复制文本")
    evt.preventDefault()
})
```

### 页面事件

前面我们讲了鼠标、键盘、表单等事件，它们大多是“用户行为触发”的。而页面级事件用于控制整个页面生命周期，通常是：

- 页面加载完成
- 页面关闭
- 页面尺寸变化
- 页面滚动
- 网络状态变化
- 页面可见性变化

这些事件往往和整个页面生命周期有关，页面事件主要绑定在：`window` 对象和`document` 对象上。

当 **DOM 树构建完成** 时会触发`DOMContentLoaded`事件，这个阶段仅仅只是HTML文档树解析完成，不等待图片加载，不等待 CSS 加载：

```js
document.addEventListener('DOMContentLoaded', () => {
    const div = document.getElementById('text')
    console.log('DOM结构已经构建完成')
    console.log(div)   //此时已经能够拿到DOM元素了
})
```

由于不等待CSS加载，有可能这个时候元素的最终渲染尺寸还没有确定，诸如`getBoundingClientRect`这类方法获取的结果并不一定是准确的。

接着就是`load`事件，它会在 **整个页面加载完成** 才会触发，包括DOM、图片、CSS以及一些其他的外部资源在内：

```js
window.addEventListener('load', () => {
    console.log('页面完全加载完成')
})
```

所以，我们前面提到的JS执行时DOM还未构建完成，导致无法获取到元素的问题，也可以通过事件监听的形式来完成，我们只需要在`load`事件中进行处理即可保证所有元素一定加载完成，这也算一种处理方式。

除了页面加载之外，页面在卸载时也会触发相关事件，当用户即将离开页面时，会触发`beforeunload`，比如关闭标签页，页面跳转，刷新页面等情况。`unload`就是页面完全卸载之后触发了。

接着是页面尺寸变化，当浏览器窗口大小改变时会触发`resize`事件：

```js
window.addEventListener('resize', evt => {
    console.log(evt)
})
```

这在响应式布局控制中非常常用。

还有页面滚动时，会触发`scroll`事件：

```js
window.addEventListener('scroll', evt => {
    console.log(evt)
})
```

在后续练习中，我们会利用滚动事件来实现真正的视差滚动效果。

此外，页面可见性变化时也能监听，比如我们想看当前页面是否被切到后台了，使用`visibilitychange`事件来监听：

```js
window.addEventListener('visibilitychange', evt => {
    console.log(document.visibilityState)   //通过visibilityState属性查看是否可见
})
```

当切换标签页或是最小化窗口时，事件就会触发。

我们甚至还可以监听页面是否发生错误，比如有JS发生了报错：

```js
window.addEventListener('error', function (e) {
    console.log('发生错误：', e.message)
})

console.log(a)
```

![image-20260225182451688](https://files.seeusercontent.com/2026/02/25/o6Ka/image-20260225182451688.png)

当然不仅仅是JS加载错误，比如图片加载错误也是可以监听到的：

```html
<img id="img" src="js.jpg">
```

```js
const img = document.querySelector('img')  //针对图片元素进行错误监听

img.addEventListener('error', function () {
    console.log('图片加载失败')
})
```

## 常用WebAPI

这一部分我们接着来介绍常用的WebAPI，在前端开发中，浏览器提供了一些强大的 API 供 JavaScript 与浏览器进行交互，我们将依次介绍一些常用的API以便大家使用。

### 窗口对象

`window` 是 JavaScript 中的一个全局对象，它是所有对象的顶层，它表示浏览器窗口（或框架），是浏览器环境的核心。它不仅是浏览器窗口的表示，还提供了访问浏览器相关功能的接口。同时，`window` 也是所有浏览器环境中全局对象的引用，我们之前没有使用`new`执行的`this`其实就是指向的`window`对象。

* `window` 是全局作用域的顶级对象，在浏览器中定义了一个“全局环境”。
* 当你在浏览器中执行代码时，你访问的全局变量（比如 `document`、`console`）都属于 `window` 对象的一部分。

在`window` 对象中有很多重要的属性：

* **`window.document`**：返回当前文档对象。通常用于操作 HTML 文档结构。
* **`window.location`**：返回当前页面的 URL 信息，并允许修改 URL 来重定向页面。
* **`window.navigator`**：返回浏览器的信息，如版本、平台、语言等。
* **`window.console`**：提供与开发者工具相关的功能，如 `console.log()`，`console.error()` 等。
* **`window.history`**：允许访问浏览器的历史记录，并可以执行前进、后退操作。
* **`window.innerWidth` / `window.innerHeight`**：返回浏览器视口的宽度和高度。
* **`window.localStorage` / `window.sessionStorage`**：提供本地存储（`localStorage`）和会话存储（`sessionStorage`）的接口，允许在浏览器中存储数据。

需要注意的是，在浏览器中，所有的全局变量和函数都是 `window` 对象的属性，实际上`var`声明（`let`和`const`不受影响）的全局变量 `x` 等同于 `window.x`，`function`声明的全局函数 `foo()` 等同于 `window.foo()`：

```js
function test() {
    console.log("牛逼")
}
```

![image-20260226162140134](https://files.seeusercontent.com/2026/02/26/yT6e/image-20260226162140134.png)

实际上我们之前使用的很多全局函数都是`window`提供的。

首先我们来介绍一下窗口尺寸相关的属性，在 JS 中，窗口尺寸主要分为两类：**包含滚动条的整体窗口尺寸** 和 **可视区域尺寸**。**可视区域尺寸**就是指浏览器窗口中实际能看到内容的区域大小（不包含浏览器的地址栏、工具栏、开发者工具页面，也不包含滚动条）

* `window.innerWidth`：获取浏览器可视区域的**宽度**（单位：px）
* `window.innerHeight`：获取浏览器可视区域的**高度**（单位：px）

示例代码：

```js
const viewportWidth = window.innerWidth;
const viewportHeight = window.innerHeight;
console.log(`可视区域宽度：${viewportWidth}px，高度：${viewportHeight}px`);
```

我们也可以使用**整体窗口尺寸**，它会完整包含整个浏览器窗口大小：

* `window.outerWidth`：整个浏览器窗口的**宽度**（包含浏览器边框、滚动条、工具栏等）
* `window.outerHeight`：整个浏览器窗口的**高度**（包含浏览器标题栏、状态栏等）

示例代码：

```js
const windowWidth = window.outerWidth;
const windowHeight = window.outerHeight;
console.log(`浏览器窗口整体宽度：${windowWidth}px，高度：${windowHeight}px`);
```

除了获取窗口尺寸之外，我们也可以获取滚动状态，当页面出现滚动条时，我们可以通过：

```js
window.scrollY  //纵向滚动位置
window.scrollX  //横向滚动位置
```

来获取整个页面的滚动位置，不过需要注意的是这两个值不能通过直接修改来改变页面的滚动位置，需要和之前使用元素的内部滚动一样，调用`scroll`相关方法：

```js
// 滚动到指定位置
window.scroll({ top: 0 })
```

接着是`window`对象的一些方法，可以用来控制浏览器窗口或执行其他操作：

* **`window.alert()`**：弹出一个警告框，显示指定的消息。
* **`window.confirm()`**：弹出一个确认框，返回用户的选择（`true` 或 `false`）。
* **`window.prompt()`**：弹出一个输入框，允许用户输入内容。
* **`window.open()`**：打开一个新的浏览器窗口或标签页。
* **`window.close()`**：关闭当前窗口。

```js
//因为是全局对象，调用其中的函数可以直接写成alert
window.alert("This is an alert!");  // 弹出警告框
let userConfirmed = window.confirm("Do you confirm this action?");
```

有关`window`对象中提供的其他属性，我们会在本节后续内容中继续介绍。

### 浏览导航和历史

`location` 是 `window` 对象的一个属性，它指向一个包含当前文档 URL 信息的对象，你可以通过它读取或修改浏览器地址栏的 URL，从而实现页面跳转、刷新等操作。

首先是`href`属性，它代表浏览器当前访问的完整 URL：

```js
console.log(location.href)
//当前浏览器地址栏的地址 http://localhost:63342/HelloHtml/index.html
```

当然完整地址可以拆分成几个部分单独获取，假设 URL 是 `https://www.example.com:8080/path/page.html?name=test&age=18#section1`：

| 属性名     | 作用                           | 示例                           |
| :--------- | :----------------------------- | :----------------------------- |
| `protocol` | 协议（含冒号）                 | `https:`                       |
| `host`     | 主机名 + 端口号                | `www.example.com:8080`         |
| `hostname` | 主机名                         | `www.example.com`              |
| `port`     | 端口号                         | `8080`                         |
| `pathname` | 路径（含斜杠）                 | `/path/page.html`              |
| `search`   | 查询参数（含问号）             | `?name=test&age=18`            |
| `hash`     | 锚点（含井号）                 | `#section1`                    |
| `origin`   | 协议 + 主机名 + 端口号（只读） | `https://www.example.com:8080` |

针对于`href`属性，我们可以对其进行修改来实现页面跳转：

```js
location.href = 'https://www.baidu.com'
```

注意修改`href`必须携带前面的协议，如果不携带会被认为是在当前站点进行跳转，会以路径的形式进行拼接，注意如果是跳转某个子页面，一定要区分最前面加`/`和不加的区别，前者会从根路径开始，后者会以当前路径继续深入。

> 通过这种方式跳转页面都会导致页面强制刷新，即使跳转到相同路径的页面也会刷新。

除了使用这种方式跳转页面之外，我们也可以使用`assign`方法，同样可以实现跳转到指定 URL 的效果：

```js
location.assign('https://www.baidu.com')
```

此外，还有一个与`assign`效果相同的`replace`方法，但是它们的区别在于历史记录，我们会在后面接着介绍。

除了页面跳转之外，我们也可以手动刷新页面：

```js
location.reload()   //简单刷新页面
location.reload(true)   //强制刷新页面(忽略缓存，重新加载)
```

我们接着来看历史记录，几乎所有现代化浏览器都有历史记录功能：

![image-20260226182015508](https://files.seeusercontent.com/2026/02/26/L8ii/image-20260226182015508.png)

历史记录中保存了所有你访问过的网站，你可以直接跳转到自己曾经访问过的站点，我们也可以在页面的顶部点击左右箭头符号来实现向前或向后跳转：

![image-20260226183408716](https://files.seeusercontent.com/2026/02/26/oV9b/image-20260226183408716.png)

当然，如果只是在某一个标签页内，前进后退的范围仅限于当前这个标签所访问过的站点或地址。在浏览器中也是使用的一个类似于栈结构的容器进行存放：

![image-20260226225444538](https://files.seeusercontent.com/2026/02/26/Sf5y/image-20260226225444538.png)

`history` 也是 `window` 对象的属性，它管理浏览器的会话历史记录（即你访问过的页面列表），可以实现 “前进”“后退”“新增 / 替换历史记录” 等操作，是前端路由（如 HistoryRouter）的核心：

```js
console.log(history.length)  //获取栈长度
```

长度就是当前标签页访问过的所有可以回退的页面数量，也就是历史记录的数量。除此之外，还有`scrollRestoration`属性，它保存的是滚动恢复策略：

```js
console.log(history.scrollRestoration)
```

它的作用是控制浏览器在历史记录之间导航时（后退 / 前进）是否自动恢复页面的滚动位置，默认情况下值为`auto`，浏览器自动恢复到上一次的滚动位置，我们也可以设置为`manual`来让浏览器不自动恢复滚动位置，需手动控制。

我们接着来看看历史记录的核心方法：

| 方法名      | 作用                                                         | 示例                          |
| :---------- | :----------------------------------------------------------- | :---------------------------- |
| `back()`    | 回退到上一条历史记录（等价于点击浏览器后退按钮）             | `history.back()`              |
| `forward()` | 前进到下一条历史记录（等价于点击浏览器前进按钮）             | `history.forward()`           |
| `go(n)`     | 跳转到指定偏移量的历史记录：- `n=1`：前进 1 步- `n=-1`：后退 1 步- `n=0`：刷新当前页 | `history.go(-2)` // 后退 2 步 |

```js
history.back() // 后退一步
history.forward() // 前进一步
history.go(-2) // 后退两步
history.go(1) // 前进一步
history.go(0) // 刷新当前页
```

注意历史记录的跳转（无论是JS还是浏览器直接点击返回前进按钮）可能会导致页面刷新，当两个历史记录的页面路径或地址不同时，都会导致页面刷新。

我们在前面提到的`assign`和`replace`方法，最大的区别就在于是否存在历史记录，如果是`replace`相当于直接将当前页面替换成了一个新的地址，也就是说会把当前页面的历史记录信息给覆盖，而`assign`相当于是进行一次新的调跳转，跳转之后之前的页面会作为历史记录存放。

除了进行页面跳转会增加历史记录，我们也可以手动插入一个新的历史记录：

```js
history.pushState(null, '', 'index.html')
```

这里需要传入3个参数：

* **`state` (对象)**：一个与新历史记录条目相关的状态对象。每当用户导航到该状态时，`popstate` 事件就会被触发，你可以从事件中或是`history.state`读取这个对象。如果不需要，传 `null` 即可。
* **`title` (字符串)**：目前大多数浏览器都会忽略这个参数，通常传空字符串 `""`。
* **`url` (字符串)**：新的历史记录条目地址。**注意：** 为了安全，这个 URL 必须与当前页面同源（同一个域名）

执行后，和之前一样，上一个页面的路径会作为历史，我们依然可以点击返回变回之前的页面，接着页面链接路径会替换为`pushState`传入的`url`参数，但不会刷新页面。这对于一些SPA（单页面应用）的实现，比如 VueRouter 就非常关键，如果我们通过`location.href`地址栏的 URL 会导致页面刷新。 而使用 `pushState`，你可以改变 URL，让用户觉得“跳转了页面”，但实际上网页内容是通过 JavaScript 动态更新的，整个过程非常平滑，没有白屏。

与其类似的还有一个`replaceState`，不过和之前的`replace`方法一样，它会直接替换当前的历史记录，而不是保存之前并插入一个新的，所以这种方式是无法回退之前路径的。

### 浏览器信息

`navigator` 用于获取**当前浏览器和设备环境信息**，简单来说就是浏览器的“自我介绍”。你可以通过它知道：浏览器类型、版本、操作系统、是否联网、用户使用的语言、摄像头/麦克风权限、地理位置、剪贴板等等。

很多常用的WebAPI属性都可以通过`navigator`来获取，我们先从浏览器信息开始介绍。其中最主要的就是`userAgent`属性，可以获取浏览器的 **用户代理字符串**：

```js
console.log(navigator.userAgent)
```

![image-20260226232916113](https://files.seeusercontent.com/2026/02/26/dgM3/image-20260226232916113.png)

可以看到这个结果里面包含了很多信息，包括当前使用的浏览器名称、操作系统版本等，只不过这种方式只能获取一个大概的信息，但实际版本和名称并不准确，并且部分浏览器是可以随意修改UA信息的，所以**不推荐用于精确判断**。要简单判断下是否移动端还是没问题的：

```js
if (/Mobile/.test(navigator.userAgent)) {
  console.log("移动端")
} else {
  console.log("桌面端")
}
```

我们接着来看下一个属性，`language`用于获取浏览器语言，比如要判断是否中文：

```js
if (navigator.language.startsWith("zh")) {
  console.log("中文环境")
}
```

`onLine`可以判断是否联网：

```js
console.log(navigator.onLine)
```

大家可以试试关闭电脑WIFI或是拔掉主机的网线，观察网络状态变化，还可以结合事件监听：

```js
window.addEventListener("online", () => {
  console.log("网络已恢复")
})

window.addEventListener("offline", () => {
  console.log("网络已断开")
})
```

我们还可以通过`geolocation`获取用户位置，不过此操作需要用户授权，所以我们只能以回调形式传入获取之后的处理逻辑：

```js
navigator.geolocation.getCurrentPosition(
  position => {   //用户允许执行，则进一步执行回调
    console.log(position.coords.latitude)  //通过coords获取经纬度
    console.log(position.coords.longitude)
    console.log(position.coords.accuracy)   //坐标精确度
  },
  error => {
    console.log("获取失败")   //如果用户不允许，则执行错误回调
  }
)
```

![image-20260226234407496](https://files.seeusercontent.com/2026/02/26/7Nct/image-20260226234407496.png)

这里我们可以点击访问该网站时允许（就和手机App授权是一样的）来让浏览器可以访问当前的地理信息。

接着是剪贴板，浏览器可以实现在用户不主动执行Ctrl+V的情况下，直接读取用户的剪贴板内容，不过和上面一样，由于剪贴板里面可能涉及到用户隐私信息，所以需要用户手动授权才能使用：

```js
navigator.clipboard.readText().then(text => {
    console.log(text)
})
```

![image-20260226235048260](https://files.seeusercontent.com/2026/02/26/Vu3v/image-20260226235048260.png)

这里`readText()`会返回一个Promise对象，当用户允许剪贴板之后，Promise才会正常resolve，如果用户拒绝了读取剪贴板的请求，会直接失败：

![image-20260226235225136](https://files.seeusercontent.com/2026/02/26/yWy7/image-20260226235225136.png)

除了读取剪贴板的内容之外，我们也可以对剪贴板内容进行修改：

```js
navigator.clipboard.writeText("Hello")
```

最后是摄像头和麦克风，我们可以通过`navigator.mediaDevices`来获取媒体设备：

```js
navigator.mediaDevices.getUserMedia({ 
    video: true,   //如果需要获取摄像头，就开启
    audio: true    //如果需要获取麦克风，就开启
}).then(stream => {
    console.log("获取成功")
}).catch(err => {
    console.log("获取失败")  //用户不授权或是根本没有对应的媒体设备就失败
})
```

由于也会涉及到用户隐私，所以这里依然需要用户手动授权，否则会导致获取失败：

![image-20260227000454298](https://files.seeusercontent.com/2026/02/26/Mw8a/image-20260227000454298.png)

如果成功获取到，这里会得到一个`stream`对象，它代表数据的输入流，大家可以理解为视频数据像水流一样不断涌入，我们可以把这个流绑定到一个`video`元素上：

```html
<video id="video" width="640" height="480" autoplay playsinline></video>
```

```js
navigator.mediaDevices.getUserMedia({
    video: { width: 640, height: 480 },  //视频可以进一步设置摄像头录制的宽度和高度以及其他属性
    audio: true
}).then(stream => {
    const video = document.getElementById('video')
    video.srcObject = stream  //将video元素的srcObject属性设置为这里的流即可
})
```

现在很多的一些视频会议都可以靠`mediaDevices`来实现。除了摄像头录制之外，我们也可以对屏幕进行录制，这里我们直接使用另一个方法`getDisplayMedia`来实现，注意屏幕录制也需要单独的权限：

![image-20260227001743410](https://files.seeusercontent.com/2026/02/26/lb2S/image-20260227001743410.png)

```html
<div style="position: relative;width: fit-content">
  <video style="position: absolute; bottom: 20px; right: 20px;border: black 2px solid;border-radius: 10px"
         id="user" width="320" height="240" autoplay playsinline></video>
  <video id="screen" width="1280" height="720" autoplay playsinline></video>
</div>
```

```js
navigator.mediaDevices.getDisplayMedia({
    video: {
        cursor: 'always', // 显示鼠标光标
        displaySurface: 'monitor' // 可选：monitor/window/browser
    },
    audio: false  // 屏幕音频就不获取了
}).then(stream => {
    const video = document.getElementById('screen')
    video.srcObject = stream
})
```

这样就可以实现会议室分享屏幕加在线聊天效果了。

### 文档对象

我们在前面的学习中认识了`document`对象，它代表我们整个页面文档，通过它，你可以用代码访问、修改甚至删除网页上的任何内容。这一部分，我们来深入了解一下它的更多内容。

首先就是它包含了页面的诸多元数据，如 `title`, `URL`, `characterSet`等：

```js
console.log(document.title)  //网页标题
console.log(document.characterSet)   //编码格式 
```

我们可以尝试修改网页的编码格式，此时`document`的对应属性也会跟着发生变化：

![image-20260227003430220](https://files.seeusercontent.com/2026/02/26/Uzr0/image-20260227003430220.png)

如果我们想要获取页面的地址，也可以通过URL属性来获得：

```js
console.log(document.URL)
```

![image-20260227003716723](https://files.seeusercontent.com/2026/02/26/hUr8/image-20260227003716723.png)

这个属性和`location.href`类似，但是这里的属性是只读无法修改的。

前面我们介绍了如何向页面添加元素，除此之外，我们还可以直接向页面打印文本内容，会以文本节点的形式进行插入：

```js
document.writeln("Hello World!")
```

![image-20260227004324417](https://files.seeusercontent.com/2026/02/26/mS9w/image-20260227004324417.png)

前面我们学习了如何获取元素，我们可以选择通过元素的`id`或`class`属性来快速拿到对应的元素，除此之外，我们还可以一次性获取所有指定类型的元素：

```js
console.log(document.links)  //获取所有的链接
console.log(document.images)  //获取所有的图片
console.log(document.forms)  //获取所有的表单  
```

### 数据存储

浏览器本身是“无状态”的，当页面一刷新，我们创建的所有变量、函数、数据都会烟消云散，为了保证用户每次访问网站时能够保留之前使用的一些数据，我们需要一种机制，把数据“保存下来”。

浏览器常见的数据存储方式有以下方式：

| 存储方式       | 大小      | 生命周期                 | 是否发给服务器 | 适用场景 |
| -------------- | --------- | ------------------------ | -------------- | -------- |
| Cookie         | 小（4KB） | 可设置                   | ✅ 会自动发送   | 登录状态 |
| localStorage   | 5~10MB    | 永久                     | ❌ 不会         | 本地缓存 |
| sessionStorage | 5MB       | 会话级，标签页关闭就消失 | ❌ 不会         | 临时状态 |
| IndexedDB      | 很大      | 永久                     | ❌ 不会         | 大型数据 |
| Cache Storage  | 很大      | 永久                     | ❌ 不会         | PWA缓存  |
| Memory         | 小        | 页面刷新就消失           | ❌ 不会         | 临时变量 |

Memory就是我们一直以来使用的数据存储方式，所有的数据都存储在内存中，页面关闭之后自动释放，不留下任何痕迹。这一节我们来介绍其他五种数据存储方式。

首先是最简单的`LocalStorage`，它是 **Web Storage API** 的一部分，用来在浏览器中**持久化存储数据**。所有存储在其中的数据都不会随着页面的刷新而消失，关闭浏览器后仍然存在。

在`LocalStorage`中，数据是以键值对形式存储的，类似于Map，我们可以直接使用：

```js
//注意键值对需要以字符串形式存放
localStorage.setItem("AAA", "建材王总")
```

由于键值对只能以字符串形式存放，所以这里建议将对象转换为JSON格式进行存储，直接存储对象会导致类型转换，变成`"[object Object]"`。

添加键值对后，我们可以直接在开发者工具中查看存放了哪些数据：

![image-20260227112034591](https://files.seeusercontent.com/2026/02/27/3tdZ/image-20260227112034591.png)

存储在`LocalStorage`的数据是永久保存的，即使我们刷新网页，下次依然可以访问到。我们可以使用`getItem`来获取之前保存的数据：

```js
console.log(localStorage.getItem("AAA"))
```

其他方法如下：

```js
localStorage.key(0)   //按下标位置获取Key
localStorage.removeItem("AAA")   //移除键值对
localStorage.length   //键值对数量
localStorage.clear()   //清空所有数据
```

不过需要注意的是，`LocalStorage`采用的是同源策略，如果我们跳转到其他站点上，在之前站点存储的数据是不通用的，每个站点的数据独立存放：

![image-20260227113451949](https://files.seeusercontent.com/2026/02/27/Wzx3/image-20260227113451949.png)

所以各位小伙伴无需担心其他网站污染了我们自己存储的数据，因为都是独立的。接着我们要介绍一个和`LocalStorage`非常类似的存储方案：`SessionStorage`，它的用法和前者非常类似，也是键值对：

```js
sessionStorage.setItem("AAA", "建材王总")
```

与`LocalStorage`的区别是，它是存储在会话存储空间中的：

![image-20260227113959061](https://files.seeusercontent.com/2026/02/27/lVf6/image-20260227113959061.png)

会话存储空间和本地存储空间不同，当我们关闭当前浏览器标签页时，会话会自动结束，同时也会清理会话空间存储的数据。注意，只要当前标签页不关闭，即使我们刷新网页也不会导致`SessionStorage`被清理，因为会话没有结束，所以一定要注意它和`Memory`数据的区别。

还有就是`SessionStorage`也遵循同源策略，但仅限于同标签页内：

1. 即使在同一个会话下跳转到其他页面也是无法读取的
2. 由于是会话级别的数据，所以不同标签页之间即使是同一个站点也是无法读取的

这种存储更适合一些表单临时数据、支付流程存储等场景。

接着我们来介绍一下**Cookie**，它是浏览器提供的一种**小型文本数据存储机制**，用于在客户端（浏览器）保存少量数据，并在之后的请求中自动携带给服务器。

Cookie的格式是：

```
name=value; expires=时间; path=/; domain=xxx; secure; HttpOnly; SameSite
```

它包含自己的名称、过期时间、路径、所属的域、安全性、仅网络可用、同源策略。

要添加Cookie有两种方式：

* 服务器通过 Set-Cookie 响应头来为客户端添加新的Cookie
* 通过`cookieStore`手动添加新的Cookie

由于我们还未解除到服务端环境，所以这里我们仅演示第二种方式，使用`set`方法即可：

```js
cookieStore.set({
    name: 'test',   //传入需要设置的Cookie名字
    value: '我是数据'   //Cookie的数据
})
```

![image-20260227121328343](https://files.seeusercontent.com/2026/02/27/Dhj2/image-20260227121328343.png)

可以看到，这里我们成功插入了一个新的Cookie，名称和值就是我们刚刚设定好的内容。需要注意的是，设置Cookie和之前的`LocalStorage`不同，它不是同步进行的，这里会得到一个Promise对象，当设置Cookie真正完成之后，才会结果。

我们可以尝试刷新网页重新请求页面，可以看到请求头会携带我们添加的自定义Cookie：

![image-20260227121839241](https://files.seeusercontent.com/2026/02/27/n5Ti/image-20260227121839241.png)

像一些用于表明身份的令牌数据，就可以通过Cookie来进行传递，所以你也可以把Cookie理解为是浏览器帮服务器“记住用户身份”的小纸条。当然，如果存在多条Cookie信息，那么会以分号分隔：

![image-20260227134548611](https://files.seeusercontent.com/2026/02/27/9Aat/image-20260227134548611.png)

我们也可以通过`docuemnt.cookie`来获取成品字符串：

```js
console.log(document.cookie)
```

默认情况下，我们创建的Cookie是存在过期时间的，如果什么都不设置那么过期时间就是会话周期内，这和前面介绍的`SessionStorage`是一样的。当Cookie过期之后，就会自动被移除，我们也可以手动为Cookie设置过期时间：

```js
cookieStore.set({
    name: 'test',
    value: '我是数据',
    expires: Date.now() + 3600 * 1000  //当前时间加上1个小时
})
```

![image-20260227132413418](https://files.seeusercontent.com/2026/02/27/kb6O/image-20260227132413418.png)

这里简单介绍一下后续几个参数：

* domain：当前Cookie可用的域名，注意只能是当前站点或子站点的域名，不能随意指定
* path：当前Cookie生效的路径起始位置，默认是`/`表示所有路径都适用
* secure：表示只有在安全环境下才能使用，比如localhost或是https站点
* HttpOnly：表示只用作请求，不能被JS随意读取或修改
* SameSite：控制跨站请求是否携带Cookie信息，默认是严格模式，完全禁止跨站携带（比如在某个网站流量数据，结果后台恶意发起一条银行网站的转账请求，如果携带Cookie将会导致账户盗刷）

不过，虽然前面介绍这三种存储方案都各有好处，但是都存在一个很大的问题，就是容量有限制。接下介绍一下**IndexedDB**，它也是浏览器提供的一种**本地数据库存储方案**，用于在客户端存储大量结构化数据。它比 `localStorage`、`sessionStorage` 更强大，它支持存储大量数据（MB ~ GB 级）就像后端的数据库一样，可以装下很多东西。

还有一种是专用于PWA渐进式应用程序页面缓存的`CacheStorage`，由于目前暂时接触不到，所以这里暂时不做详细介绍。

有关`IndexedDB`和`CacheStorage`的内容，我们会放到后续的《前端工程化》课程中进行详细介绍。

## 本章练习

### 实现真视差滚动

### 苹果官网完善

————————————————
版权声明：本文为柏码知识库版权所有，禁止一切未经授权的转载、发布、出售等行为，违者将被追究法律责任。
原文链接：https://www.itbaima.cn/zh-CN/document/sdhodlihphnpcg37