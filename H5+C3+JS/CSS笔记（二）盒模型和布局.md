![image-20250823211541992](https://s2.loli.net/2025/08/23/rgcFa7LT3xRZGyV.png)

# CSS 盒模型和布局

上一章我们介绍了CSS的基本语法和文本样式调整，相信各位小伙伴应该对CSS逐步上手了，这一章我们将继续深入学习CSS的盒子模型和布局，并带给大家有关UI设计方面的教学，让大家能够写出更加精美的页面。

## 盒子模型

盒子模型（Box Model）是网页布局中的一个核心概念，用于描述每个HTML元素在页面上的表现方式，它是我们后续学习网页布局的重要前置知识点。

### 什么是盒子

在HTML中，我们可以将任意一个元素视为一个矩形盒子（不仅仅是指之前介绍的`div`盒子）无论这个元素是行内元素还是块级元素：

```html
<span>十七张牌你能秒我</span>
```

![image-20250823220331923](https://s2.loli.net/2025/08/23/McDm6dLeyIKJ9AS.png)

只要是一个元素，浏览器在渲染时都会将其当做一个矩形盒子渲染，而这样的一个盒子，一共包含4个部分，它由内容区域（Content）、内边距（Padding）、边框（Border）和外边距（Margin）组成，这种模型能够有助于我们理解和控制元素的布局和空间占用。

我们可以打开浏览器并按下键盘上的F12（或是鼠标右键点击"检查"）在开发者工具中查看盒子的几个区域：

![image-20250823222116815](https://s2.loli.net/2025/08/23/tXnJDCMY1rO6ZAK.png)

默认情况下，由于我们并未设置任何内容，所以盒子的大小是由内容和标签属性来确定的。并且内边距、外边距、边框的宽度默认都是0，而这一部分，我们的主要学习目标就是如何去调整这几个部分来让元素的大小和样式随心变化。

### 内容区域

前面我们说到盒子的宽高是由内容来确定的，实际上用于控制盒子宽高的是`width`和`height`属性，他们的默认值都是`auto`表示自动确定宽高，不过，针对于行内元素和块级元素来说，存在一些差异，在只考虑内容宽高的情况下：

* **行内元素：**盒子的宽度由内部文本或其他行内元素的总宽度决定，盒子的高度由内部文本的字体大小和行高决定。
* **块级元素：**盒子的宽度默认直接占满整行，盒子的高度由内部其他元素高度总和而决定。

在内部不存在任何元素的情况下，无论是**行内元素**还是**块级元素**高度都为0。**行内元素**的宽度也为0，**块级元素**的宽度依然是占满整行。

除了像这样通过内容来让盒子获得一个高度和宽度，我们也可以手动设置元素的高度和宽度：

```html
<div class="box">十七张牌你能秒我</div>
```

```css
.box {
  width: 200px;
  height: 100px;
}
```

![image-20250824005453529](https://s2.loli.net/2025/08/24/83pKVZ5Q2SEqmFM.png)

此时，这个盒子的宽就变成了200个像素点，高就变成了100个像素点，注意其内部元素依然是从上往下排列，文本内容会在顶端。但是注意，宽高设置仅针对于块级元素生效，行内元素是无法设置宽高的，我们可以将`div`换成`span`来观察：

```html
<span class="box">十七张牌你能秒我</span>
```

同时，我们在HTML阶段提到，块级元素会排斥其他元素，导致其他元素另起一行展示，这是块级元素的一大特性。同样的，虽然现在我们可以任意调整块级元素的大小，但是这并不影响它对于其他行内元素或块级元素的排斥性：

```html
<div class="box">大花洒大街上</div>
<span>圣诞节啊手动滑稽卡萨丁合计卡萨</span>
```

![image-20250824012022933](https://s2.loli.net/2025/08/24/a9bmNGXpL1JqUOl.png)

除此之外，我们在HTML阶段还认识了一个`img`标签，它是一个**行内块元素**，所谓行内块，就是既具有行内元素的特性（比如不会占据一行宽度，而是穿插在文本中），也具有块级元素的特性（允许设置宽高）。因此，实际上对于图片这种行内块元素，`width`和`height`也是可以生效的：

![image-20250824010836526](https://s2.loli.net/2025/08/24/I3sXVqzlKyOr2RD.png)

可以看到，图片既不会排斥行内文本另起一行，又可以通过CSS设置宽高，这也是行内块元素的一个非常重要的性质（不过图片标签本身也自带了`width`和`height`属性，在哪里设置都可以调整宽高，效果一样）

此外，我们也可以让盒子的宽度或高度自动适应内部元素的尺寸，比如内部文本只有`200px`，此时我们可以设置`fit-content`来自动适应内部宽度：

```css
.outer-box { width: fit-content; }
```

针对于盒子内部文本的自适应调整，除了`fit-content`之外，还有`max-content`和`min-content`属性，由于不是很常用，这里不做详细介绍，具体使用效果请参阅MDN文档：https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference/Values/max-content

介绍完盒子的宽度和高度，我们接着来说一下嵌套的情况：

```html
<div class="outer-box">
  <div class="inner-box"></div>
</div>
```

```css
.outer-box {
  width: 400px;
  height: 100px;
  background-color: green;
}

.inner-box {
  background-color: yellow;
}
```

当两个盒子发生嵌套的时候，外部盒子的宽高并不会继承给内部盒子（不同于前面的文本相关属性，会自动向内继承）内部盒子的默认宽度遵循上面所说的规则。此时，我们可以手动设置内部盒子的宽高来指定内部盒子的大小：

![image-20251130234907770](https://s2.loli.net/2025/11/30/vJBDOzEqhGyFw8s.png)

此外，除了设置一个固定值之外，我们也可以使用百分比的形式设置盒子的尺寸，比如，我们将其宽度设置为`50%`，高度设置为`90%`：

![image-20251130235155417](https://s2.loli.net/2025/11/30/qFl29OuEn5eYIHG.png)

此时盒子将按照外部盒子的相对尺寸进行展示，比如外部盒子的宽度是`200px`，那么此时按照`50%`计算，就会变成`100px`的宽度进行展示，这在很多需要保持固定比例展示的情况下就显得非常灵活。当然，百分比的上限并非`100`，你可以设置一个超出`100%`的值，CSS的世界是非常自由且包容的，不要让固定思维限制了你的想象力：

![image-20251130235426296](https://s2.loli.net/2025/11/30/NdmBYILWist6wHG.png)

前面我们说到，由于块级元素的独特性质，即使我们不去手动设置它的宽度，也会默认占据一整行，相当于自带了`width: 100%`这个属性，不需要我们手动进行编写：

![image-20251201000139013](https://s2.loli.net/2025/12/01/myxNv46bLthj1fz.png)

在前面的学习中，我们知道，整个页面实际上是由`body`所包裹的，而`body`则是我们整个可见页面最外层的盒子：

![image-20251130235704199](https://s2.loli.net/2025/11/30/fAGCbQUcXtJV73v.png)

它是包含所有**可见页面内容**（文本、图片、链接等）的容器，而`body`的上一级就是页面的根元素`html`，默认情况下，`html`和`div`一样，高度初始为0且由内容撑开，但是宽度占据整个浏览器窗口。在默认情况下，`<body>` 默认也是一个块级元素，它的宽度会自动填满父元素`<html>`的宽度，高度同样是初始0然后内容撑开，并且浏览器的**用户代理样式**（后面介绍）通常会给它设置一个默认的`margin`（外边距，后面进行介绍），这也是为什么上面我们的盒子没有贴边展示的原因。

正是因为`body`默认占据整个页面的宽度，因此内部的所有块级元素根据性质，默认都会占据页面的一整行了。

**提问：**最外层的`body`作为整个页面的可视内容容器，是否也可以通过CSS修改它的大小呢？那`html`呢？

### 背景样式

前面我们介绍了盒子的大小如何进行控制，这一节我们接着来看盒子的背景。背景有两种形式：

* **背景颜色：**使用某一种特定颜色作为盒子的背景。
* **背景图片：**使用一张图片作为盒子的背景。

我们先来看最简单的一种背景颜色，可以使用`background-color`来设置颜色：

![image-20250824025533064](https://s2.loli.net/2025/08/24/jTWpxXg9Rlo7UeP.png)

和我们前面介绍的`color`属性一样，颜色可以使用十六进制表示或是使用`rgb`和`rgba`函数来表示。如果我们未给`background-color`设置颜色时，它的默认值是`transparent`，代表透明无颜色。

有些时候，我们的页面中可能需要出现一些重点标注的词语或段落，此时使用背景颜色就可以达成非常不错的效果：

```html
<span>
  我是普通文本中的
  <span class="highlight">重点文本</span>
  需要标记出来
</span>
```

![image-20250824030456812](https://s2.loli.net/2025/08/24/oBHMaRCVNzngEeD.png)

我们接着来介绍一下背景图片，不同于纯色的背景颜色，背景图片可以让背景显示为一张图片的形式，我们可以将需要展示的图片放到项目目录下或是使用URL访问网络图片，就像我们使用`img`标签那样，我们需要使用`background-image`属性来：

```css
<div class="test-box"></div>
```

```css
.test-box {
  width: 200px;
  height: 200px;
  /* 使用url函数引用内部或外部图片地址 */
  background-image: url("https://img2.baidu.com/it/u=4082245214,2139971588&fm=253&fmt=auto&app=120&f=JPEG?w=889&h=500");
}
```

可以看到页面上确实出现了一个盒子，并且背景使用的是我们指定的图片：

![image-20251130231430528](https://s2.loli.net/2025/11/30/EWbkwmBQrUx8t35.png)

不过，这个图片只出现了一部分，这是因为默认情况下，背景图片显示的大小是按照图片实际像素大小展示的，这里的图片由于比我们设定的盒子宽高更大，因此一部分内容就超出了盒子导致无法展示。要解决这种问题也很简单，我们可以使用`background-size`属性来控制背景图片的尺寸：

```css
background-size: cover;  /* 尽可能缩放且保持比例 */
```

这里需要介绍一下`background-size`的几个预设值：

- **auto**  -  默认值，根据背景图片的**固有尺寸**来显示，如果图片没有固有尺寸，则根据容器尺寸来显示。
- **cover**  -  将背景图片放大或缩小，使其宽高比保持不变，并使其**完全覆盖**整个背景区域（优先选择窄的一边进行缩放并占满）
- **contain**  -  将背景图片放大或缩小，使其宽高比保持不变，并使其**完全包含**在背景区域内（优先选择宽的一边进行缩放，但可能出现重复展示）

使用`cover`后，背景图片确实进行了一定程度的缩小，可以看到更多内容，但是仍然有一部分内容被裁剪。而使用`contain`之后，背景图片的大小被强制缩小到盒子能够完整展示的大小，但是此时图片存在重复渲染的问题：

![image-20251130232425909](https://s2.loli.net/2025/11/30/wVCOlR4jhiBD3Ts.png)

要解决重复渲染的问题，我们可以使用`background-repeat`来防止背景图片进行重复渲染，默认情况下只要背景图片小于盒子尺寸，都会进行重复渲染，这里只需设置到`no-repeat`即可：

![image-20251130232520417](https://s2.loli.net/2025/11/30/eFNETfqOslo6cGJ.png)

此外，除了使用预设的几种`background-size`之外，我们也可以手动指定背景图片的宽高：

![image-20251130233323482](https://s2.loli.net/2025/11/30/eho3KSszdRkCNJ2.png)

除了设定固定值之外，我们也可以使用百分比来控制背景的宽度大小等，不过这里的百分比相对的是盒子的大小。

我们还可以通过`background-position`属性来控制背景图片在盒子中的位置：

![image-20251130233813219](https://s2.loli.net/2025/11/30/KPSL8UAytDXsFZB.png)

`background-position` 属性通常接受一个或两个值，第一个值定义**水平位置** (X 轴)，第二个值定义**垂直位置** (Y 轴)，比如：

| **水平关键字 (X)** | **垂直关键字 (Y)** | **描述**                       |
| ------------------ | ------------------ | ------------------------------ |
| `left`             | `top`              | 将图片左上角对齐容器的左上角。 |
| `center`           | `center`           | 将图片中心点对齐容器的中心点。 |
| `right`            | `bottom`           | 将图片右下角对齐容器的右下角。 |

一些常用的组合示例：

- **居中：** `background-position: center;` (相当于 `center center`)
- **左上角：** `background-position: left top;`
- **右下角：** `background-position: right bottom;`
- **垂直居中，水平靠左：** `background-position: left center;` (或 `center left`，顺序通常是水平在前)

此外，我们也可以使用百分比值可以更灵活地定位，百分比是图片的中心相对于盒子的中心的偏移量：

* `background-position: 50% 50%;`效果与`background-position: center center`是相同的。
* `background-position: 0% 0%;`效果与`left top`是相同的。

当然，除了百分比也可以使用固定的尺寸，如`px`或是`rem`等，不过是以图片的右上角为原点开始进行计算，各位自行尝试就明白了，这里不详细介绍。

最后需要注意的是，`background`是以上所有属性的简写属性，和之前介绍的`text-decoration`一样，可以直接将上面这堆属性写到一起，就像这样：

```css
.test {
  background: red no-repeat url("https://img2.baidu.com/it/u=4082245214,2139971588&fm=253&fmt=auto&app=120&f=JPEG?w=889&h=500") 10px 20px / contain;
}
```

```css
.test {
  background-color: red;
	background-image: url("https://img2.baidu.com/it/u=4082245214,2139971588&fm=253&fmt=auto&app=120&f=JPEG?w=889&h=500");
	background-repeat: no-repeat;
	background-position: 10px 20px;
	background-size: contain;
}
```

顺序可以自由排列，但是注意`background-position` 和 `background-size` 必须用 `/` 分隔。

### 盒子的边框

前面我们介绍了盒子的大小，而边框也是盒子的一个重要属性，边框用来包围内容区域和内边距，形成一个可见的外框。要创建边框非常简单，需要两个必要的属性`border-width`和`border-style`：

![image-20251201171358742](https://s2.loli.net/2025/12/01/To4mhbfvI3jsM8F.png)

其中`border-width`用于控制边框的宽度，`border-style`用于控制边框的样式，这里详细介绍一下`border-style`属性：

* **none**  -  无样式，默认值
* **solid**  -  实线样式，一根普通的边框线，也是用的最多的一种
* **dashed**  -  虚线形式
* **dotted**  -  点线形式，一堆小点点围绕盒子
* **double**  -  双实线形式，两条实线围绕盒子（边框宽度至少需要`2-3px`才能正确显示）
* **ridge/groove**  -  相框形式，比较古老的样式，不符合现代审美

默认情况下，浏览器会为边框设置黑色作为默认颜色，我们也可以手动控制边框的颜色，可以使用`border-color`设置边框颜色：

![image-20251201182703049](https://s2.loli.net/2025/12/01/YgJkcniZezf97Oh.png)

通过为盒子添加边框，可以让盒子的边界感更加明确，视觉上更加凸显。和上面一样，`border`属性是以上三者的简写属性，我们可以直接写成这样：

```css
.test { border: red 1px solid; }
```

```css
.test { 
	border-width: 1px;  /* 原来的写法 */
	border-color: red;
	border-style: solid;
}
```

当然，为了更加灵活地设置盒子边框，我们也可以针对盒子的四边单独进行边框样式设置，比如现在我们只需要设置盒子的上边框，我们可以使用`border-top`属性：

![image-20251201185704939](https://s2.loli.net/2025/12/01/fmyYPFKrJsLWqoE.png)

```css
.test {
	border-top: red 1px solid;
  /* 或是拆分成下面的写法 */
  border-top-color: red;
  border-top-width: 1px;
  border-top-style: solid;
}
```

除了`border-top`之外，我们还可以使用`border-bottom`、`border-left`和`border-right`分别控制下左右边距的样式。

需要注意的是，如果我们在一个地方同时使用了边框总设置和单边设置，此时会按照声明的顺序进行覆盖，比如：

```css
.test-box {
  border: green 2px solid;
  border-top: red 2px solid;  /* 后者会覆盖前者的设置 */
}
```

此时就会出现，三边为绿色边框顶部为红色边框的情况。当然，除了边框样式支持这样的写法之外，后续遇到的很多属性都是支持这样进行总体+特例进行设置的。

这里需要特别注意的是，默认情况下（非IE浏览器）盒子的边框会使得盒子的大小进一步增加：

![image-20251201222808401](https://s2.loli.net/2025/12/01/1Pkuqsoe8yr7jm5.png)

```css
.test-box {
  width: 300px;
  height: 100px;
  background-color: cornflowerblue;
  border: red 2px solid;   /* 由于四边都增加了一个2px的边框，导致盒子最终变成 304x104 的大小了 */
}
```

除了为盒子设置边框外，我们还可以对盒子进行进一步修饰，比如现在各大操作系统争先使用的圆角设计。我们可以使用`border-radius`属性来将盒子的四个角磨圆，看起来不那么尖锐甚至视觉上会很舒服：

![image-20251201191953323](https://s2.loli.net/2025/12/01/Aitw7W6vS3EgJPD.png)

`border-radius`的尺寸决定了圆角的半径大小，注意，如果圆角半径设置过大，就会使得盒子的四角变成完全的圆形：

![image-20251201220837917](https://s2.loli.net/2025/12/01/pYlZCHEnaztrAyN.png)

此时的设定为`border-radius: 100px;`这代表盒子的圆角半径设定为100px的宽度，这已经远超四角能展示的最大半径了（这里盒子的高度是100px，由于上下都要做圆角处理，因此四角的圆角半径最大只能达到50px）因此就会按照盒子能够达到的最大半径进行展示。

和边框一样，我们可以单独控制某一个角的圆角效果：

![image-20251201221311445](https://s2.loli.net/2025/12/01/Lzr1G3hp27TPIlq.png)

比如我们需要控制左上角的圆角半径，就可以使用`border-top-left-radius`属性来单独控制左上角的圆角半径效果，同样的，我们也可以使用`border-top-right-radius`、`border-bottom-left-radius`和`border-bottom-right-radius`等属性来控制，或是直接使用下面的写法：

```css
.test {
  border-radius: 100px 10px; /* 2 个值：分别作用于（左上+右下）、（右上+左下） */
  border-radius: 100px 20px 10px; /* 3 个值：分别作用于（左上）、（右上+左下）、（右下） */
  border-radius: 50px 20px 10px 30px; /* 4 个值：分别作用于四个角（顺时针） */
}
```

除了设置一个固定大小之外，我们也可以通过百分比的形式进行设置，百分比的形式会同时考虑盒子角的两条边，分别计算两边的圆弧半径，形成椭圆的效果：

![image-20251201222409266](https://s2.loli.net/2025/12/01/vb5BgfJ3HVrGslA.png)

这里就分别按照长和宽的`20%`进行圆角处理，反正我个人觉得这种方式调出来的圆角是很难看的。当然如果希望固定值也能实现分别调整两边的圆角半径，也可以使用`/`分割：

```css
.test { border-radius: 10px 20px 30px 40px / 5px 15px 25px 35px; }
```

### 内边距

我们接着来介绍盒子模型中最重要的边距属性，分为内边距和外边距两个属性。

我们先来研究一下内边距，内边距指的是 **内容（content）与边框（border）之间的空白区域**，可以使用`padding`属性进行设置：

```css
.test-box {
  width: 200px;
  height: 50px;
  background-color: cornflowerblue;
  padding: 20px;  /* 控制盒子的内边距 */
  border: red 2px solid;
}
```

![image-20251201233317443](https://s2.loli.net/2025/12/01/cBPEG7RwjHfOYC9.png)

可以看到，盒子的实际区域和边框之间出现了一个`20px`宽度的空白区域，和边框一样，默认情况下它会使得盒子变大。

我们可以尝试在盒子内添加一些文本之类的便于观察：

```html
<div class="test-box">
  <span>你干嘛，能不能不要再黑我们家鸽鸽了</span>
</div>
```

![image-20251201233457000](https://s2.loli.net/2025/12/01/hZ48UGuEm1yLeix.png)

由于设置了`padding`属性，盒子具有了内边距，因此实际的内容显示区域会距离盒子的边缘会有些距离（注意内边距也算作盒子大小的一部分，我们为盒子设置的背景会包含在内边距的尺寸范围内）即使我们去掉边框也是一样的效果：

![image-20251201233641347](https://s2.loli.net/2025/12/01/JXjskQK2u1TW9nw.png)

这对于一些卡片类型的界面元素来说，就非常合适了，我们会在接下来的UI设计课程中为大家介绍如何编写一个漂亮的卡片。和我们之前介绍的`border`属性一样，我们也可以单独控制四个方向的边距：

```css
.test {
  padding-top: 10px;
  padding-right: 20px;
  padding-bottom: 10px;
  padding-left: 20px;
}
```

或是使用简写写法独立设置：

```css
.test {
  padding: 10px;             /* 四边都是10px */
  padding: 10px 20px;        /* 上下10px，左右20px */
  padding: 10px 20px 30px;   /* 上10px，左右20px，下30px */
  padding: 10px 20px 30px 40px; /* 上10px，右20px，下30px，左40px */
}
```

有些时候，我们在计算盒子宽度的时候，往往还需要考虑边框和内边距的宽度，这就给我们造成了很大的麻烦，比如我们希望创建一个大小为`100x300`的盒子（包含内边距和边框在内）我们就得先去计算边框和内边距的宽度，再进行减法运算。那有没有能够节省我们计算时间同时又能满足我们要求的方案呢？

这里就需要提到一个比较特殊的属性`box-sizing`了，它是 CSS 中用来**控制盒子模型宽高计算方式**的属性，它决定了元素的 width、height 是如何计算的。默认情况下，盒子模型采用的是`content-box`模式，width / height **只包含内容区（content）**，不包含 padding 和 border，所以当我们添加边距和边框时，盒子的尺寸会变大：
$$
元素实际宽度/高度 = content + padding + border
$$
如果希望边距和边框算在盒子总宽度之内，我们可以使用`border-box`作为值：

```css
.test-box {
  width: 300px;
  height: 100px;
  background-color: cornflowerblue;
  padding: 20px;
  border: 3px solid red;
  box-sizing: border-box;   /* 此时盒子的高度就是height设定的高度 */
}
```

而盒子内容的实际大小，将会按照如下公式来计算：
$$
content = width/height - padding - border
$$
比如这里的盒子大小就是：

![image-20251202000257794](https://s2.loli.net/2025/12/02/DW5RAwlYtyuF2Gg.png)

这样，我们就可以只关心盒子最终大小，而无需再花时间去计算边框和内边距的尺寸了。

除了使用固定大小作为`padding`的值之外，我们也可以使用百分比的形式，但是注意，padding 的百分比始终相对于**父元素的宽度**来计算，不管是水平还是垂直方向。

![image-20251202170218442](https://s2.loli.net/2025/12/02/EaFXclWDA1h263Q.png)

最后需要注意的是，对于行内元素来说，内边距的使用受到一些限制，我们在前面说到，行内元素高度是由文本和行高决定的，即使我们手动对齐进行高度设置也是无效的，不同于块级元素和行内块元素，我们即使可以通过内边距的形式去撑大盒子，也无法改变行内元素最终展示的高度：

```html
<span style="padding: 20px">我是一段文本内容</span>
```

![image-20251202155511221](https://s2.loli.net/2025/12/02/huwpZE5TCjldKVL.png)

可以看到，行内元素虽然也可以通过设置`padding`属性来实现内边距效果，但是仅限于`padding-left`、`padding-right`会正常生效并撑开内容区域，而上下的内边距会出现一些问题，`padding-top`和`padding-bottom`虽然会生效，但不影响行高设定，不会像块级元素或是行内块元素那样把周围元素推开：

```html
<span style="padding: 20px">我是一段文本内容</span>
<span>我是另一段文本内容，很长很长很长很长很长很长很长很长很长很长很长很长很长很长</span>
```

![image-20251202155748750](https://s2.loli.net/2025/12/02/9N5aZeRiDSWovwt.png)

因此，针对于行内元素来说，**盒模型不参与垂直方向上的布局计算**。不过有意思的是，由于我们通过内边距的形式强行撑大了盒子，行内元素撑开的盒子大小依然是有效的，也就是说背景还是会按照被撑开的大小展示：

```html
<span style="padding: 20px;background-color:red;">我是一段文本内容</span>
```

![image-20251202160215704](https://s2.loli.net/2025/12/02/T5ler6xLCBtvj9H.png)

那么有没有办法让行内元素也可以像块级元素那样正确展示`padding`的效果呢？后面我们会为大家介绍如何在行内和块级元素之间进行转换。

### 外边距

介绍完内边距之后，我们接着来介绍一下外边距`margin`，它也是 CSS 盒模型中的一部分，用来控制**元素外部与其他元素之间的距离**。和`padding`不同，它不会影响元素本身的大小，而是影响它与周围元素的间隔。我们可以使用`margin`属性来设置盒子的外边距：

```html
<body>
  <div class="test-box">我是一号盒子</div>
  <div class="test-box" style="margin: 10px">我是二号盒子</div>
</body>
```

![image-20251202154625347](https://s2.loli.net/2025/12/02/qoaLwBvMkfTWdCe.png)

可以看到，在设置外边距之后，盒子以外的部分和其他盒子之间或是父元素的边框之间存在一定的间距，这就是外边距。

和内边距一样，我们可以自由设置四个方向的外边距：

```css
.test {
  margin: 20px; /* 这意味着四个方向（上、右、下、左）的外边距都是 20 像素 */
	margin: 20px 10px; /* 上下外边距为 20 像素，左右外边距为 10 像素 */
  margin: 20px 10px 15px; /* 上外边距为 20 像素，左右外边距为 10 像素，下外边距为 15 像素 */
  margin: 20px 10px 15px 5px; /* 上外边距为 20 像素，右外边距为 10 像素，下外边距为 15 像素，左外边距为 5 像素 */
}
```

或是单独设置每一个：

```css
.test {
  margin-top: 20px;
  margin-bottom: 20px;
  margin-left: 20px;
  margin-right: 20px;
}
```

同样的，行内元素也可以设置外边距来与其他元素保持距离：

```html
<span style="margin-right: 20px">我是一段文本内容</span>
<span>我是另一段文本内容，很长很长很长很长很长很长很长很长很长很长很长很长很长很长</span>
```

![image-20251202165338290](https://s2.loli.net/2025/12/02/ZuFvy5EONDeRJkX.png)

但是注意，和前面的内边距一样，即使我们为行内元素添加上下外边距，也是没有任何效果的：

![image-20251202165655448](https://s2.loli.net/2025/12/02/k4VZWLH9ziryaBN.png)

同样是只有左右会生效，因为行内元素高度始终是由文本和行高决定，虽然盒子模型上显示确实存在上下的外边距设定，但是最终并不会生效。而块级元素和行内块元素则不受到影响。

除了设定固定尺寸外，我们也可以使用百分比或是一些预设值，使用百分比和`padding`效果一样，都是按照父元素的宽度进行计算。不过，这里有一个比较特殊的值，我们可以将`margin`设置为`auto`来实现居中效果：

![image-20251203163358971](https://s2.loli.net/2025/12/03/1ofBqSrmuQnZhE9.png)

当`margin`设置为`auto`时，浏览器会自动计算盒子两边距离父盒子边框的宽度，从而实现横向居中的效果。注意，`auto`只会影响横向效果，这对纵向是没有作用的。

最后还有一个比较大的坑点需要注意，首先是盒子之间的间距可能会存在问题，我们看下面这个例子：

```html
<div class="test-box"></div>
<div class="test-box"></div>
```

```css
.test-box {
  width: 200px;
  height: 50px;
  margin: 20px;   /* 此时两个盒子都具有外边距 */
  background-color: cornflowerblue;
}
```

![image-20251203171508542](https://s2.loli.net/2025/12/03/OaMRWKYSNqwb9eL.png)

我们发现，虽然两个盒子都设置了外边距，但是在最终展示出来的效果上，却只存在一个外边距（上面盒子底部的外间距和下面盒子顶部的外间距重合了）难道不应该是两个外边距加一起吗，应该是`40px`才对啊，为什么这里会重合呢？

这种情况我们一般称为**"margin折叠"**，它往往出现在相邻**块级元素**的垂直方向上，就和上面图中一样（这种情况针对行内块和行内元素无效，仅块级元素有效）

此外，除了相邻盒子存在折叠现象外，在父子元素之间也会出现这种情况：

```html
<div class="outer-box">
  <div class="inner-box"></div>
</div>
```

```css
.outer-box {
  width: 200px;
  height: 50px;
  margin-top: 10px;
  background-color: cornflowerblue;
}

.inner-box {
  width: 100px;
  height: 30px;
  background-color: red;
  margin-top: 20px;
}
```

![image-20251204155130283](https://s2.loli.net/2025/12/04/9C78ieQBKaHGSsm.png)

当我们在内层盒子中使用`margin-top`属性时，它的会直接连带父盒子一起产生边距效果（仅限`margin-top`会出现这种情况）这种特性有些时候非常恶心，导致我们设置边距出现问题。不过，针对这种情况来说，只要父盒子满足以下要求，就可以阻止折叠行为：

1. 父元素有 padding-top（哪怕是 1px）
2. 父元素有 border-top（哪怕是透明边框）
3. 父元素形成新的块格式化上下文（BFC，后续会介绍，比如设置overflow属性）

此外，即使是在同一个盒子下，自己本身的上下外边距也会出现折叠的情况：

```html
  <div class="outer-box"></div>
  <div>我是下面的盒子</div>
```

```css
.outer-box {
    width: 200px;
    margin: 20px 0;
}
```

![image-20251211174135261](https://s2.loli.net/2025/12/11/k8QEcwnWjmMG6FI.png)

最后，这里总结一下发生 `margin` 折叠的三种情况

1. **相邻的兄弟元素**: 相邻的块级兄弟元素，上面元素的 `margin-bottom` 和下面元素的 `margin-top` 会发生折叠。 
2. **父元素和第一个子元素**: 如果父元素没有上边框、上内边距，并且没有内容将它和它的第一个子元素分开，那么父元素的 `margin-top` 和第一个子元素的 `margin-top` 会发生折叠。 
3. **空的块级元素**: 如果一个块级元素没有内容、`padding`、`border`，并且 `height` 为 `auto`，那么它自己的 `margin-top` 和 `margin-bottom` 会发生折叠。 

### 用户代理样式

还记得我们在一开始的时候介绍到，引入CSS的方式主要有三种，分别是：

1. **内联样式/行内样式** （Inline CSS）直接在HTML元素的`style`属性中编写CSS代码。
2. **内部样式** （Internal CSS）在HTML文档的`<head>`标签内使用`<style>`标签定义CSS规则。
3. **外部样式** （External CSS）将CSS代码写在独立的`.css`文件中，然后通过`<link>`标签引入到HTML页面中。

此外，在计算机发展的早期，浏览器本身为了能让一些缺少自定义CSS样式的网站有一个大致的排版，通常会有一些自己默认的样式设置，比如`p`标签就存在浏览器预设的一些默认边距设置：

![image-20251204175738933](https://s2.loli.net/2025/12/04/recTPylz2Odp4Z8.png)

包括我们前面介绍的`body`标签，在Chrome下同样具有默认外边距设置，我们可以选中`body`标签，然后查看外边距：

![image-20251205123859667](https://s2.loli.net/2025/12/05/dJMwf6n3WAb9U4C.png)

可以看到，`body`的默认样式实际上是来源于用户代理样式表的。

虽然用户代理样式表很多情况下能够帮助一些没有CSS样式的网站更好看地显示内容，但是在有些时候，不同浏览器的用户代理样式不一样，可能会导致网页的某些元素以不一样的尺寸或边距进行展示，从而出现不同用户看到的页面样式不同的情况。因此，为了避免这个问题，我们可以引入一些用于消除不同浏览器用户代理样式差异的CSS文件，从而实现统一。

这里我们使用前面提到的`link`标签进行引入：

```html
<link rel="stylesheet" href="css/normalize.css">
```

可以看到`normalize.css`将一些常见的元素样式进行了统一，从而消除不同浏览器之间的差别：

```css
html {
  line-height: 1.15; /* 统一行高设置 */
  -webkit-text-size-adjust: 100%;
}

body {
  margin: 0;  /* 统一消除了body的外边距 */
}

...
```

不过，我们每次引入CSS都要像这样先去下载对应的文件再进行导入实在是太累了，有些时候我们也可以尝试使用一下不同厂商CDN为我们提供的远程地址。

> **CDN（Content Delivery Network）**是一组分布在全球各地的加速节点，用来缓存和分发网站的静态资源，让用户能从最近的节点加载内容，从而大幅提升访问速度。
>
> 一些大厂会将常用的CSS、JS、字体等资源存放到CDN加速节点上，我们可以直接使用CDN上的资源，这样不仅可以减少自己服务器数据传输消耗的带宽，也能利用这些加速资源让用户更快加载网站。

其中比较常见的CDN提供站点有：Cloudflare、百度、字节、阿里云、Akamai等。这里我们以Cloudflare的CDN加速站点为例，引入`normalize.css`文件，首先前往官网：https://www.jsdelivr.com，接着搜索`normalize.css`：

![image-20251205125225419](https://s2.loli.net/2025/12/05/qA1Y2IRe4gfTirc.png)

我们直接选择第一个，然后找到引入CSS的HTML标签：

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/normalize.css@8.0.1/normalize.min.css">
```

接着就可以将其加入到我们的页面上了，效果和本地引入是一样的。

### 滚动区域

在网页开发中，内容超出容器范围是非常常见的情况，而当内容超出容器但我们又希望保持布局整洁、不让内容溢出到外面时，就需要使用 **滚动区域（scroll area）**滚动区域本质上是利用 CSS 的 `overflow` 属性控制内容的可视范围。

默认情况下，如果内容超出了盒子的大小，会直接显示到盒子外部：

```html
<div class="box">
  我是盒子里面的字，很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
</div>
```

```css
.box {
  width: 200px;
  height: 100px;
  background-color: #f0f0f0;
  white-space: nowrap;
}
```

可以看到文字已经跑到盒子外面去了：

![image-20251209155812692](https://s2.loli.net/2025/12/09/gxZkGqjCX4cbI1l.png)

默认情况下盒子的`overflow`属性为`visible`，表示当内容溢出盒子后，依然会展示出来。我们可以将其修改为`auto`来实现内容溢出时自动添加滚动条：

```css
.box {
  width: 200px;
  height: 100px;
  background-color: #f0f0f0;
  white-space: nowrap;
  overflow: auto;  /* 未溢出时不展示滚动条，溢出时自动展示 */
}
```

![image-20251209160202372](https://s2.loli.net/2025/12/09/nz3gKVJY8yFrHkx.png)

可以看到，现在盒子里面的文本超出部分被自动隐藏，需要通过盒子底部出现的滚动条滚动查看。此外，我们也可以将`auto`改为`scroll`来让滚动条常驻。

如果我们希望直接隐藏掉内容超出的部分，也可以使用以下两种值：

* `hidden`  -  直接隐藏超出内容（但是可以被JS或浏览器强制滚动）
* `clip`  -  裁切掉超出内容（同上，但是不能强制滚动）

虽然这两种都可以实现超出部分的隐藏效果，但是前者依然会创建滚动容器（你可以理解为依然可以滚动，只是滚动条被隐藏了）而后者是实打实的按照盒子宽度对内容进行了裁切，超出盒子部分相当于不存在了（注意`clip`选项比较新，老旧浏览器可能不支持）所以在大部分需要裁切内部元素且不保留滚动效果的情况下，选择`clip`性能会更好。

> 想要查看哪些CSS特性受支持可以访问 https://caniuse.com 一键查询

除了统一设置滚动外，我们也可以针对于垂直和水平方向进行分别控制滚动条：

```css
.box {
  width: 200px;
  height: 100px;
  overflow-y: auto;   /* 只允许垂直滚动 */
  overflow-x: hidden; /* 禁止水平滚动 */
}
```

最后我们再来研究一下`body`和`html`的滚动，和我们自己创建的盒子不同的是，当元素超出整个页面的大小时，浏览器窗口会自动创建一个滚动条，这里我们把盒子高度弄大一点：

```css
.box {
  width: 200px;
  height: 5000px;
  background-color: #f0f0f0;
}
```

由于`body`和`html`的高度会被内容撑开，此时页面无法容纳全部信息。在默认情况下，当页面内容超出视口高度时，会自动显示滚动条，我们不需要对任何标签设置`overflow`属性，为了方便开发者设置，这个滚动条默认是附加在`html`上的，实际的滚动容器是`html`。

因此，你可以为`html`（`body`也可以但部分浏览器不支持）设置`hidden`属性来实现隐藏超出视口大小部分：

```css
html {
  overflow: hidden;
}
```

此时浏览器窗口的滚动条就消失了，无法滚动页面。

### UI设计系列（课程一）

欢迎来到我们的UI设计系列课程，在本系列中，我们将会针对一些现代化网站常见的UI样式进行讲解剖析，让大家有能力设计出一个更漂亮，更有个性，更符合现代化设计的网站。现在很多开发者的通病，就是缺乏一个良好的审美，和页面UI设计思路，导致最终设计出来的网站页面样式粗糙，用户操作复杂，层次不明确，文本看不清等问题。

UI设计就像是商品的包装，一个优秀的UI设计往往更能吸引用户的眼光，我们就从用现在的最多的卡片元素开始介绍，这里有两个卡片：

![image-20251205131219465](https://s2.loli.net/2025/12/05/faXLFN96VcwhSWI.png)

第一眼看来，各位觉得是1号卡片好看，还是2号卡片好看呢？相信绝大部分用户可能会更加青睐2号卡片，那么2号卡片为什么相比1号卡片，看起来更舒服呢？我们这节课就从几个角度来分析一下原因。

1. **卡片的背景和边框：**卡片的背景是最为重要的一个因素，既然要让用户知道我们这里展示的是一个卡片，那么卡片肯定需要有一个自己的"边界"，而卡片的背景就是一个很好的边界划分，因为外部是白色背景，此时我们只需要将卡片设置为一个区别于白色的颜色即可，这样卡片的形态就出来了。但是注意，卡片的颜色需要有一定的**对比度**，否则看起来就很容易和内容或是外部区域的颜色打架：

   ![image-20251205142424512](https://s2.loli.net/2025/12/05/tdQzyvlSDmhMwsE.png)

   上面的例子中，一号卡片由于颜色与页面背景相似，看起来就像是在一起的一样，三号卡片颜色虽然和外部形成鲜明对比，但是由于颜色过深，导致卡片内部字体可读性变差。因此，卡片的背景颜色需要保证在有对比度的情况下同时还要兼顾内容的可读性。

   当然，有些场景下可能浅色背景确实比较好看，背景越浅，内容就能够越凸显，可读性就更好，我们也可以保留浅色背景设计，为卡片设置一个边框来增加边界感：

   ![image-20251205160304883](https://s2.loli.net/2025/12/05/f7swXEk5bWgQImh.png)

   现在看来，是不是一号卡片就不像之前那样没边界感了，同时卡片的背景也能够更加凸显内容，这就是边框的作用。除了边框外，我们也可以使用盒子的阴影来增加边界感，有关阴影的设置我们会在后续的章节中进行介绍。

2. **卡片的边距：**除了背景颜色，边距也是影响卡片美观的一个非常重要因素，当卡片的内部没有边距时，会显得很拥挤，给人一种非常窒息的感觉，强烈的压迫感，因为文字没有**"呼吸空间"**，就像下面这样：

   ![image-20251205160819679](https://s2.loli.net/2025/12/05/CjxeVMdAs9q16w3.png)

   一号卡片完全没有边距，看起来非常拥挤，就像装不下似得，但实际上内部有很多空间容纳这些文本。三号卡片虽然有了内边距，但是似乎有点用力过猛，导致边距实际内容区域挤压得没有空间了，周围区域又显得很空洞。

   因此，设置一个合理的内边距，在视觉上能给人一种轻松愉快的感觉，不会显得压抑。边距设置一般可以考虑按照**4的倍数**进行调整，比如如` 8 / 12 / 16 / 20 / 24 px `等。

3. **卡片的圆角：**直角卡片有强烈的"边界感"，多个直角卡片并排时会显得密集、紧绷，直角有抢夺内容注意力的副作用（像这类容易抢夺人注意力的内容，一般将其称为**"视觉噪音"**）现代化的UI设计，更多地会考虑使用圆角来让边界自然过度，防止抢夺过多注意力。就像Windows10到Windows11的变化一样，圆角的设计更受用户的喜爱。

   ![image-20251206000455437](https://s2.loli.net/2025/12/06/ty9akxbme1GEKrF.png)

   可以看到，一号卡片由于没有设置圆角，四个尖尖看着就非常难受，有点明明是跑龙套的非要出头秀一波的感觉。三号卡片虽然添加了圆角，但是似乎有点过度圆滑了，看着有点像丸子或气泡的感觉，如果再大一点就会导致圆角处的外边距被彻底磨平，就又出现上面没有边距的压迫感了。

   因此，圆角的大小应该根据盒子的内边距进行合理调整，越大的圆角，就需要越大的内边距来进行匹配，而不是一味地去打磨。一般情况下圆角的半径不建议超过内边距的大小。

### UI设计系列（课程二）

颜色在 UI 设计中绝对不是“好看就行”，而是承担着重要的功能：**信息传达 + 视觉引导 + 品牌表现 + 层级结构**。在这一部分，我们将从设计的角度，结合 CSS 的实际写法，深入讲解配色的原则、情绪化表达以及视觉层级建立。

很多初学者设计页面的最大问题，就是 **颜色乱选**，不合理的颜色会使得界面变得：

- 不统一（像从不同页面拼起来的）
- 对比不足（看不清，就像上面卡片的例子一样）
- 对比过强（刺眼）
- 情绪混乱（颜色表达与内容不符）
- 层级不明显（不知道哪里是重点）

颜色不是越丰富越好，尽量不要去选择过多的颜色，尤其是彩虹色，会使得页面主题不明，不同颜色都在争抢，一个优秀 UI 设计师必须理解：**颜色本质是信息载体，而不是装饰品**，95% 的现代 UI 都使用 **一主一辅一强调**的颜色体系：

```
主色（Primary） —— 品牌、主按钮、重点视觉
辅色（Secondary）—— 辅助内容、背景分区，一般不会和主色调差别过大
强调色（Accent） —— 强调、提醒、交互反馈，建议采用主色调的反色或相差较大的颜色
```

我们可以来看下面这个例子：

![image-20251206224245654](https://s2.loli.net/2025/12/06/SUQiCntTuK5Bm9r.png)

可以看到，当我们选用绿色清新风格的卡片时，不同的颜色搭配会产生不同的效果。其中一号卡片的主色和辅色颜色差异过大，存在冲突的情况，看起来就非常奇怪，而三号卡片的主辅色调过于相近，对比度不足，看起来里面的重点内容被弱化，很容易让人找不到重点。相比之下，二号卡片主次色调分明，对比度合理，颜色相近且属于同一个色系，视觉效果是最好的。

这里给几种比较合理的案例供大家参考：

![image-20251206225627049](https://s2.loli.net/2025/12/06/slBQiVuXkFN3CME.png)

这里也推荐一个选色网站，可以自由选择颜色搭配：https://colordrop.io，也可以选择好颜色之后在这里尝试：https://colorable.jxnblk.com，除了注重颜色搭配外，我们还要注意颜色也会产生**情绪、暗示和氛围**，不同的颜色也会带给人不同的感觉，比如：

- 蓝色：科技、可信赖、冷静
- 红色：警告、禁止、热情
- 绿色：安全、通过、自然、清新
- 黄色：提醒、高能注意

## 布局

在前面我们已经掌握了盒模型，它让我们理解了 *一个元素自身的尺寸结构*：内容（content） → 内边距（padding） → 边框（border） → 外边距（margin）但是仅仅知道一个盒子长什么样还远远不够，因为：**网页不仅仅是一个盒子，它是由成百上千个盒子组成的布局系统**，从这节课开始，我们进入到 CSS 最重要的部分：**布局**。

布局指的是：决定网页中元素如何排列、位置如何分布、如何换行、如何伸缩、如何对齐的规则和技术。

在默认情况下，浏览器使用的是**普通文档流（Normal Flow）**，这是浏览器的默认排版方式，也是最基本的布局规则，在普通流中：

| **元素类型**           | **排列方式**                   |
| ---------------------- | ------------------------------ |
| 块级元素（block）      | **从上往下**依次排列，独占一行 |
| 行内元素（inline）     | **从左往右**排列，不会独占一行 |
| 行内块（inline-block） | 像文字一样排列，但可以有宽高   |

而想要实现很多网页的高级样式，仅仅依靠普通流是无法实现的，这一章我们将逐步介绍布局。

### 定位布局

如果你想让一个元素实现诸如：固定在屏幕顶部、贴在右下角、叠在另一个元素上或是跟随滚动等效果，就需要用到定位布局。定位布局一共有四种，我们可以根据不同的情况进行选择。我们首先来介绍第一种`relative`布局，它可以实现相对于自身位置偏移，且不脱离文档流的效果。

要设定布局，我们可以使用`position`属性：

```css
.test { position: static; }  /* 默认是static类型，也就是静态布局 */
```

#### 相对定位

我们来测试一下`relative`属性，看看如何控制定位：

```html
<div class="outer-box">
  <div class="inner-box"></div>
</div>
```

```css
.inner-box {
  width: 100px;
  height: 50px;
  background-color: cornflowerblue;
  position: relative;   /* 使用相对布局 */
  left: 10px;  /* 使用定位布局后，我们需要手动指定盒子上下左右的位置，使用left、right、top、bottom */
}

.outer-box {
  margin-top: 50px;
  width: 400px;
  height: 200px;
  border: 1px solid black;
}
```

此时，我们为内层盒子设定了相对布局，并且向左偏移`10px`，现在我们来看看最终的效果：

![image-20251207150445886](https://s2.loli.net/2025/12/07/BZGQ1u7C5LYagqf.png)

可以看到，内部盒子和之前一样，是在外部盒子之内包裹的，且跟随外部盒子的边距一起移动。不过，内部盒子由于设置了定位，所以就从默认的左上角位置跑到了距离原本位置左侧`10px`的位置，也就是在原本位置的基础上进行偏移。这就是`relative`布局，实现在原本位置上相对定位的效果。同样的，如果我们设置`right: 10px`那就相当于原本位置向右偏移：

![image-20251207150839453](https://s2.loli.net/2025/12/07/zPFQJUC6EprZtXB.png)

注意，虽然我们可以使用相对定位来移动盒子的位置，但是其盒子所占据的大小和位置还是停留在原地，只是视觉效果上发生了位移。我们可以将其`top`属性设置一下：

```html
<div class="outer-box">
  <div class="inner-box"></div>
  <div>我是后面的文字</div>
</div>
```

![image-20251207233032610](https://s2.loli.net/2025/12/07/dDScnAZrtypsMLH.png)

可以看到，虽然我们使用相对定位移动了盒子，但是后续的盒子排列还是按照原本的尺寸和位置进行的。

#### 绝对定位

我们接着来看，下一个布局，`absolute`可以让盒子完全脱离文档流，采用绝对定位，和`relative`一样，我们来测试一下，这里直接把`relative`改成`absolute`：

```css
.inner-box {
  width: 100px;
  height: 50px;
  background-color: cornflowerblue;
  position: absolute;  /* 使用绝对布局 */
  right: 10px;
}
```

此时我们会发现，内部的盒子位置发生了变化：

![image-20251207151817637](https://s2.loli.net/2025/12/07/K27CIipoG6a9tkm.png)

发生了什么，为什么盒子直接跑到外层盒子的外面去了？`absolute`会让元素 **脱离文档流**，并根据**最近的定位布局（position 非 static）祖先元素**进行精确位置定位，如果所有的祖先都没有设置定位布局，那么默认以`html/body`的可视区域（可以理解为浏览器窗口有多大，可视区域就有多大）来定位。

所以这里的效果就是相对于整个可视窗口的最右侧距离`10px`，这里需要注意的是，如果我们不去设置横向或纵向上的距离，比如这里就没有设置`top`，那么此时会采用默认值，它们的默认值为`auto`，元素会停留在它原本在正常文档流中的横向或纵向位置上，就继续按照未设置布局时的纵向位置了。

和相对定位不同的是，绝对定位会导致元素脱离文档流，我们可以来看下其他兄弟元素会变成什么样：

![image-20251207233212049](https://s2.loli.net/2025/12/07/uNc59xlJsZQjKCh.png)

可以看到，盒子变为绝对定位后，盒子原本应该会占据的位置被闲置出来了，这和之前的`relative`效果不太一样。

> 这种情况我们就称之为 **脱离文档流（out of normal document flow）** 它是指元素不再按照正常的文档排版规则参与布局，而是**从页面常规排列中抽离出来**，不再占据原本应有的位置，也就是我们常说的“脱离文档流”。

那要是我们希望内部盒子还是依照外部盒子一起移动，在外部盒子的内部进行绝对定位呢？前面我们说到，`absolute`会根据最近的定位布局进行定位，所以，最简单的方法就是将外层盒子设置一个`relatvie`定位（可以无需设置上下左右距离，效果就和不设置定位是一样的）这样就可以让内层盒子按照外层盒子进行绝对定位了，同时又保留了绝对定位的脱离文档流特性：

![image-20251208122526336](https://s2.loli.net/2025/12/08/7m9cCiPrH8TGdfY.png)

可以看到，此时内层盒子的位置就会按照父盒子进行绝对定位，这也是我们常说的**子绝父相**，这种布局方式在很多时候都能派上用场。

需要注意的是，如果`absolute`没有定位祖先，默认以视口进行定位，当`body`或`html`存在滚动条时，将会跟随滚动条一起向上滚动：

![image-20251209153049021](https://s2.loli.net/2025/12/09/ykrplPiDJceub3f.png)

因此，如果我们需要实现诸如回到顶部按钮或是一些浮动公告等效果，使用`absolute`是无法达到预期的。

#### 固定定位

我们接着来看下一个`fixed`固定布局，它与`absolute`类似，能够固定到一个指定位置上，同样会使元素脱离文档流。与`absolute`不同的是，它是以整个视口（默认是浏览器窗口）进行定位：

```css
.inner-box {
  width: 100px;
  height: 50px;
  background-color: cornflowerblue;
  position: fixed;  /* 采用固定布局 */
  top: 10px;
}
```

![image-20251209151714621](https://s2.loli.net/2025/12/09/NFCVLgcWH2XJRkr.png)

可以看到，`fixed`定位不受影响，并且也不受到页面滚动条影响，它将始终以窗口为参照，始终固定在页面的某个位置上，这对于实现回到顶部按钮或是一些浮动公告等效果就非常适合。

#### 粘滞定位

我们接着来看粘滞定位，它也是 CSS 中一个非常有用的定位布局，它可以让元素在页面滚动时，先是像普通元素一样（`position: relative`），当滚动到屏幕的特定位置时，就会“粘”在那里，表现得像 `position: fixed` 一样。

简单来说，`position: sticky` 是 `position: relative` 和 `position: fixed` 的结合体。  元素在跨越特定阈值前为相对定位，之后为固定定位。 这个“特定阈值”是通过 `top`, `right`, `bottom` 或 `left` 属性来设置的。 像我们常见的一些导航栏，使用这种定位方式就非常合适。

要使用粘滞定位，我们需要保证至少设置 `top`, `bottom`, `left`, `right` 四个值中的一个：

```html
<div class="outer-box">
  <div class="simple-box">
    我是文本，很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
    很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
    很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
  </div>
  <div class="sticky-box"></div>
</div>
```

```css
.outer-box {
  height: 2000px;  /* 注意父元素的高度不能低于粘滞元素的高度 */
}

.sticky-box {
  width: 100px;
  height: 100px;
  background-color: red;
  position: sticky;
  top: 30px;
}
```

![image-20251210152714520](https://s2.loli.net/2025/12/10/woXBM6IZz8Cy7hU.png)

当页面滚动到`top`指定的位置时，盒子将采用`fixed`固定布局，始终定在那个位置，除非我们将滚动条重新拉回去。

这里有一个需要注意的点，`sticky`虽然可以表现得像`fixed`脱离文档流，但是盒子本身所占据的区域依然存在，我们可以通过下面这个例子来验证：

```html
<div class="outer-box">
  <div class="simple-box">
    我是文本，很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
    很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
    很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
  </div>
  <div class="sticky-box"></div>
  <div class="simple-box">
    我是文本，很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
    很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
    很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长
  </div>
</div>
```

![image-20251210153414863](https://s2.loli.net/2025/12/10/yU6upAMO8JbGw3S.png)

因此，在`sticky`定位下，无论元素当前是处于“相对定位”状态还是“固定”状态，**它在文档流中原本占据的空间始终会被保留**。

#### 包含块

前面我们讲解了定位布局，在继续讲解其他布局方式之前，我们先来补充一个概念：**包含块**，它是决定一个元素尺寸和位置参照的区域（可以理解为是子元素的“参考系”或“坐标系”）通过之前的学习我们发现，元素的许多属性值计算都直接依赖于它的包含块：

- **尺寸（Size）**：当元素的 `width`, `height`, `padding`, `margin` 设置为**百分比（%）**时，这些值是相对于包含块计算的。
- **定位（Position）**：当元素使用绝对定位（`absolute`）或固定定位（`fixed`）时，它的偏移属性（`top`, `right`, `bottom`, `left`）是相对于包含块的边缘进行定位的。

那么，一个元素的包含块究竟是如何确定的呢？这主要取决于该元素的 `position` 属性：

1. 静态与相对定位 (`static` / `relative`)

   对于大多数情况，也就是在普通文档流中，元素的 `position` 属性为 `static`（默认值）或 `relative`（相对定位）时，它的包含块由其**最近的块级祖先元素**的**内容区域（Content Box）**构成。

   简单来说，就是你外面套着哪个块级盒子，哪个块级盒子就是你的参考系。

2. 绝对定位 (`absolute`)

   当一个元素的 `position` 设置为 `absolute` 时，它的包含块会变成**最近的、`position` 值不为 `static` 的祖先元素**的**内边距区域（Padding Box）**。

   这个规则非常重要，它正是我们之前学习的“**子绝父相**”布局模式的底层原理。为了让绝对定位的子元素相对于父元素来定位，我们必须给父元素添加一个 `position: relative`（或其他非 `static`的值），从而为子元素创建一个定位参考系。

   如果所有祖先元素都没有设置定位，那么这个绝对定位的元素就会一直往上找，直到找到最顶层的**初始包含块 (Initial Containing Block)** 为止，这个初始包含块通常就是浏览器窗口（视口）

3. 固定定位 (`absolute`)

   当元素的 `position` 设置为 `fixed` 时，规则最简单：它的包含块**始终是视口（viewport）**，也就是你的浏览器窗口。

   这就是为什么固定定位的元素会“钉”在屏幕的某个位置，不会随着页面滚动而移动。

理解了包含块，你就能更准确地预测和控制元素在页面上的尺寸和位置，尤其是在处理复杂的百分比布局和定位时。

后续我们还会学习一些诸如3D变换相关的属性，这些属性往往会为子元素创建一些新的的**包含块**，导致子元素在定位时的参照发生变化。

#### Z轴顺序

在前面我们学习了多种定位布局，但在实际开发中，还经常会遇到另一个问题：**当多个元素发生重叠时，谁在上面？谁被挡住？**这就涉及到 CSS 中的 **Z 轴顺序（Z-index stacking）**那么，什么是 Z 轴？在网页中，我们通常只关注 **X 轴（左右）** 和 **Y 轴（上下）**，但实际上页面是一个**三维空间**：

- **X 轴**：从左到右
- **Y 轴**：从上到下
- **Z 轴**：从里到外（离你远 → 离你近）

你可以将 Z 轴简单理解为：**Z 轴决定“谁盖在谁上面”**，我们先看一个最基础的例子：

```html
<div class="box a"></div>
<div class="box b"></div>
```

```css
.box {
  width: 100px;
  height: 100px;
  position: absolute;
}

.a {
  background-color: red;
  left: 50px;
  top: 50px;
}

.b {
  background-color: blue;
  left: 80px;
  top: 80px;
}
```

通过分析可以得出，这两个盒子必定**发生了重叠**，那现在的问题是，谁在上面呢？经过实际测试，我们会发现，盒子B覆盖掉了盒子A，实际上，在默认情况下，DOM 顺序（也就是HTML里面标签的顺序）靠后的元素，层级更高。如果我们希望**明确指定谁在上面**，就需要使用 z-index 属性：

```css
.a {
  z-index: 20; /* z-index越大，就越靠前 */
}

.b {
  z-index: 10;
}
```

![image-20251214140106258](https://s2.loli.net/2025/12/14/RdWCuiUVrBQIG6N.png)

使用`z-index`之后，优先级越高的盒子排在前面，`z-index`的值只能是整数（包括负数）需要注意的是，`z-index` 不会对普通文档流的元素生效，必须是以下定位的元素才有效果：`relative`、`absolute`、`fixed`和`sticky`，也就是非`static`定位的元素。

当然，如果两个盒子的`z-index`一样，那么又会重新按照DOM顺序进行比较。在默认情况下，`z-index`的值为`auto`（不是0）如果是一个z-index为0的元素和auto的元素重叠，依然会按照DOM顺序进行排列，但是这两者是由本质区别的，我们会在下一节为大家介绍**层级上下文**概念。

#### 层级上下文

前面在 **Z 轴顺序** 中我们提到，当多个元素在页面中发生 **重叠** 时，浏览器需要决定：**谁在上面，谁在下面**。

很多同学第一反应是：“不是有 z-index 吗？数值大就在上面呗？”，实际上，这句话只对了一半，真正决定上下顺序的，并不是单纯的 z-index 数值，而是一个叫做 **层级上下文（Stacking Context）** 的东西。

那么，什么是层级上下文？你可以把 **层级上下文** 理解成：**一个独立的空间（分层空间）**，在这个小空间里：

- 里面的元素，只能和 **同一个空间里的兄弟元素** 比较 z-index

也就是说，`z-index` 只在 “同一个层级上下文” 里才有意义，那么什么情况下会创建层级上下文呢？

* 盒子存在非`static`定位，且使用了`z-index`属性（即使是`z-index: 0`）此时盒子会创建一个新的层级上下文
* 盒子使用了`fixed`定位（即使没有`z-index`），会直接创建一个新的层级上下文
* 盒子使用了`sticky`定位且处于粘滞状态时，会创建一个新的层级上下文

其他的一些情况我们暂时没有学到过，但是也会导致出现层级上下文，这里仅做了解即可：

* 盒子使用了`filter`、`perspective`、`clip-path`、`isolation: isolate`和`will-change`属性时。
* 盒子使用了`transform`进行了三维变换（下一章介绍）
* 盒子使用了`opacity`属性改变了盒子的透明度

以最简单的情况为例，我们可以来看下面这个例子：

```html
<div class="parent">
  <div class="child"></div>
</div>

<div class="other"></div>
```

```css
.parent {
  position: relative;
  z-index: 1;
}

.child {
  position: absolute;
  z-index: 999;
  background: red;
}

.other {
  position: relative;
  z-index: 2;
  background: blue;
}
```

可以看到，这里的`child`盒子被`other`盒子给盖住了，为什么会发生这种情况呢？明明它的`z-index`更大，怎么还反而被压在下面了？这其实因为`parent`盒子创建了一个新的层级上下文，此时`child`盒子只能在这个新的层级上下文中（也就是`parent`盒子中）与其他元素进行比较，而它上一级的`other`盒子只能与它的父盒子进行比较，当父盒子的`z-index`比不过其他盒子时，即使父盒子里面的元素`z-index`再高，也是没用的。

**简而言之，拼爹拼不过，那么从一开始你就输了。**

但是，如果父盒子不存在层级上下文，那么情况就不一样了：

```css
.parent {
  position: relative;  /* 此时父盒子不存在层级上下文 */
}

.child {
  position: absolute;
  z-index: 999;
  background: red;
}

.other {
  position: relative;
  z-index: 2;
  background: blue;
}
```

![image-20251214140058259](https://s2.loli.net/2025/12/14/CWx7v8RcTVhr6Lm.png)

可以看到，当我们去掉父盒子的`z-index`后，就不满足创建新的层级上下文条件，此时，层级会继续向内进行比较，`child`盒子会变成与`other`盒子比较的对象，此时明显`child`盒子更胜一筹。

### 网格布局

前面我们介绍了4种定位布局，这一节我们接着来看网格布局。网格布局（Grid Layout）也是CSS中非常强大的布局之一。与我们之前接触的定位布局不同，网格布局是一个**二维布局系统**，它可以针对行和列进行单独处理，非常适合一些卡片列表的场景。

要使用网格布局非常简单，我们需要修改盒子的`display`属性：

```css
.grid-box {
  display: grid;  /* 表示盒子是一个网格 */
}
```

> `display`属性用于控制盒子的展示效果，我们可以通过它来改变盒子的显示模式，比如一个块级元素，我们可以通过`display: inline`将其显示模式改为行内元素，此时，盒子就会按照行内元素的效果进行展示了：
>
> ```html
> <div style="display: inline">我是块级元素，但是被改成行内了</div>
> <span>我是标准的行内元素</span>
> ```
>
> ![image-20251210162214187](https://s2.loli.net/2025/12/10/KCpi3nYjVafhEGk.png)
>
> 可以看到，这里的`div`变得不再是块级元素的样子，而是以行内元素的形式展现，自然也就不会排斥其他行内元素了。相反的，我们还可以将行内元素改成块级元素的展现形式：
>
> ```html
> <div style="display: inline">我是块级元素，但是被改成行内了</div>
> <span style="display: block;height: 50px">我是标准的行内元素，被改成块级了</span>
> ```
>
> ![image-20251210162408226](https://s2.loli.net/2025/12/10/NJuvlhdeVxWwDp9.png)
>
> 当然，除此之外，介于行内元素和块级元素之间的行内块元素效果也可以使用`display: inline-block`来实现。

#### 基础设置

回到网格布局，在修改`display: grid`后，盒子就会按照网格的形式进行展示，那么应该如何去使用呢？我们可以向这个网格里面添加一些元素，来观察一下最终的效果，这里添加的每一个**直接子元素**会被自动放入到划分出来的网格中，我们一般称其为**网格项（Grid Item）**：

```html
<div class="grid-box">
  <div class="grid-item">1</div>
  <div class="grid-item">2</div>
  <div class="grid-item">3</div>
  <div class="grid-item">4</div>
  <div class="grid-item">5</div>
  <div class="grid-item">6</div>
</div>
```

```css
.grid-box {
  display: grid;
}

.grid-item {
  width: 100px;
  height: 100px;
  background-color: red;
}
```

![image-20251210162822186](https://s2.loli.net/2025/12/10/jqiPLhDgo45GR9Z.png)

默认情况下，Grid 只有一列，所以我们的每一个元素都是纵向排列的，我们的盒子（网格项）自动分配到生产出来的每一个网格中。我们可以通过 `grid-template-columns` 和 `grid-template-rows` 属性来告诉浏览器，我们网格的行和列信息，从而变成我们想要的排列：

```css
.grid-box {
  display: grid;
  grid-template-columns: 100px 100px 100px;  /* 表示从左往右一共3个宽度为100px的列 */
}
```

![image-20251210163233046](https://s2.loli.net/2025/12/10/iBMAZhYCVt4v82b.png)

此时网格内的元素也会按照3列的样式进行从左往右排列。同样的，我们还可以继续限制网格的行：

```css
.grid-box {
  display: grid;
  grid-template-columns: 100px 100px 100px;   /* 表示从左往右一共3个宽度为100px的列 */
  grid-template-rows: 100px 100px 100px;   /* 表示从上往下一共3个高度为100px的行 */
}
```

此时就会变成一个`3x3`的网格：

![image-20251210163628714](https://s2.loli.net/2025/12/10/Ucumi8yNptloZOH.png)

由于我们已经明确指定了网格的宽度和高度，即使不设置每个格子盒子的大小，也会自动采用网格大小：

```css
.grid-item {
  background-color: red;  /* 只留一个背景颜色就行了 */
}
```

有些时候，可能我们希望的是表格直接占满整个页面宽度，而不是手动去指定每列的大小，此时我们就可以考虑使用`fr`单位，它代表了网格容器中**剩余空间**的一份：

```css
.grid-box {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;  /* 将按照当前网格盒子的大小，进行3等分 */
  grid-template-rows: 100px 100px 100px;
}
```

![image-20251210165155836](https://s2.loli.net/2025/12/10/OSrxiYLkMW4v2ls.png)

这对于我们想要快速创建等分网格的情况来说，就非常好使，比如这个固定大小的网格：

```css
.grid-box {
  display: grid;
  width: 500px;
  height: 300px;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr;
}
```

当然我们也可以按比例分配，比如中间的格子占两倍大小：

```css
.grid-box {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;  /* 中间格子2倍大小 */
  grid-template-rows: 100px 100px;
}
```

虽然很方便，但是有一个问题，如果需要创建很多相同的列，重复写 `1fr 1fr 1fr ...` 非常麻烦，CSS 提供了一个 `repeat()` 函数来简化这种一连串的相同值：

```css
.grid-box {
  display: grid;
  grid-template-columns: repeat(3, 1fr);  /* 和连续写3个1fr是等价的 */
  grid-template-rows: repeat(3, 100px);  /* 和连续写3个100px是等价的 */
}
```

现在来了一个新的需求，每个格子紧紧相连看着不是很舒服，能不能稍微搞点间距？在之前的课程中，想要给元素之间增加间距，我们通常使用 `margin`，但在 Grid 布局中，我们有更优雅的方式：`gap`，它可以直接定义行与行、列与列之间的沟槽宽度：

```css
.grid-box {
  display: grid;
  gap: 10px;   /* 表示横向和纵向的格子之间距离都是10px大小 */
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 100px);
}
```

![image-20251210171944520](https://s2.loli.net/2025/12/10/hFauORjVt7SEinB.png)

设置`gap`属性后，会牺牲一部分格子的大小来为格子之间留出空间，从而实现间距效果。

配合一些CSS函数，我们还能实现网格自动扩列或是收缩，比如宽度达到多少自动增加一列，宽度减少后自动减少一列，类似于：https://www.itbaima.cn/zh-CN/resource 的效果：

```css
.grid-box {
  display: grid;
  gap: 1px;
  /* 实现网格自动扩列效果 */
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
}
```

这里用到的东西有：

- **`minmax(200px, 1fr)`**: 定义了每一列宽度的范围
  - **最小值 `200px`**: 每一列最窄不会小于 200 像素
  - **最大值 `1fr`**: `fr` 是“分数单位”（fraction unit）。`1fr` 表示如果还有剩余空间，该列会占满剩余空间
  - **含义**: 列宽至少是 200px；如果空间够大，它们会拉伸填满行宽。
- **`auto-fill`**: 这是一个智能关键字，不同于直接设置固定列数
  - 它告诉浏览器：**在不换行的情况下，尽可能多地往一行里塞入列**
  - 如果容器很宽，浏览器会计算：`容器宽度 / 200px` 能放下多少列，就生成多少列

是不是感觉网格布局非常强大？别着急，这才只是开胃菜。

#### 显式网格和隐式网格

现在有一个新的问题：如果我们定义了一个 `2行 x 3列` 的网格（总共6个格子），但在 HTML 里写了 9 个子元素，多出来的 3 个元素会怎么样？我们来测试一下：

```html
<div class="grid-box">
  <div class="grid-item">1</div>
  <div class="grid-item">2</div>
  <div class="grid-item">3</div>
  <div class="grid-item">4</div>
  <div class="grid-item">5</div>
  <div class="grid-item">6</div>
  <div class="grid-item">7</div>
  <div class="grid-item">8</div>
  <div class="grid-item">9</div>
</div>
```

```css
.grid-box {
  display: grid;
  gap: 10px;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, 100px);
}
```

![image-20251211105032626](https://s2.loli.net/2025/12/11/RfljJ3eXsDxP8vh.png)

可以看到，这里我们只定义了6个网格，但是实际存在9个元素，显然已经超出网格的容纳范围了。但是即使在超出网格容纳范围的情况下，后续三个元素依然会显示出来，这其实是因为除了我们自己定义的 **显式网格 (Explicit Grid)** 外，还存在 **隐式网格 (Implicit Grid)**，网格布局非常智能，它会自动处理这些多出来的元素：

- **显式网格 (Explicit Grid)：** 由我们通过 `grid-template-columns` 和 `grid-template-rows` 手动定义好的格子区域（比如前 2 行）
- **隐式网格 (Implicit Grid)：** 当内容超出我们定义的网格范围或是只设置了横向或纵向的网格数时，浏览器会自动创建新的行来容纳这些内容，这就构成了隐式网格

> 比如上面只写了`grid-template-columns`分了多少列但是未分行的情况，或是元素超出的情况，都算做隐式网格。

我们可以使用 `grid-auto-rows` 和 `grid-auto-columns` 属性，来规定这些"自动生成出来的格子"应该有多大：

```css
.grid-box {
  display: grid;
  gap: 10px;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, 100px);
  grid-auto-rows: 80px;  /* 表示超出显式网格范围后，隐式网格高度按80px展示 */
}
```

![image-20251211105902774](https://s2.loli.net/2025/12/11/t1DEVcQ8Mxpf2l3.png)

注意，如果当前网格自动排列流向是从左往右，那么设定 `grid-auto-columns` 是不会生效的，默认自动采用`grid-template-columns`的宽度设定。我们也可以尝试手动修改网格的流向：

```css
.grid-box {
  display: grid;
  gap: 10px;
  grid-auto-flow: column;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, 100px);
  grid-auto-columns: 80px;  /* 此时需要控制超出列的宽度 */
}
```

![image-20251211111239651](https://s2.loli.net/2025/12/11/sgj6YIS38DK42bx.png)

由于我们修改了网格元素流向，超出后会以列的形式扩展，此时自动扩展的格子高度会按照`grid-template-rows`的设定继续，而宽度只能由`grid-auto-columns`来控制。

#### 元素的定位与合并

Grid 最强大的地方在于它可以让一个元素跨越多个格子（合并单元格）。默认情况下，每个子元素占据一个格子。我们可以通过网格线（Grid Lines）的编号来指定元素的位置。在一个 3列 的网格中，会有 4条 垂直的网格线（从左到右依次是 1, 2, 3, 4）也就是下面图中画蓝色线条区域：

![image-20251211112058262](https://s2.loli.net/2025/12/11/TPn4vKbpokxYZjf.png)

我们可以使用以下属性来控制子元素的位置：

- `grid-column-start` / `grid-column-end`: 指定列的开始和结束网格线。
- `grid-row-start` / `grid-row-end`: 指定行的开始和结束网格线。

或者使用简写属性：

- `grid-column`: 开始线 / 结束线
- `grid-row`: 开始线 / 结束线

比如现在我们想让第一个元素横跨前两列：

```css
<div class="grid-box">
  <div class="grid-item first">1</div>
  <div class="grid-item">2</div>
  <div class="grid-item">3</div>
  <div class="grid-item">4</div>
  <div class="grid-item">5</div>
  <div class="grid-item">6</div>
</div>
```

```css
.grid-item.first {
  /* 从第1条线开始，到第3条线结束（也就是占据第1列和第2列） */
  grid-column: 1 / 3;
}
```

![image-20251211112444510](https://s2.loli.net/2025/12/11/BUsxcrouk4FWPS6.png)

我们也可以使用 `span` 关键字，表示“跨越几个格子”：

```css
.grid-item.first {
  /* 从当前位置开始，跨越 2 列 */
  grid-column: span 2;
}
```

如果想让某个元素占满整个第一行（假设是3列网格）：

```css
.grid-item.first {
  grid-column: 1 / -1; /* -1 表示从后往前数的第一条网格线 */
}
```

通过这些属性的组合，我们可以轻松实现非常复杂的网格布局，从而不需要通过嵌套多个网格布局 `div` 来实现。

#### 网格对齐

我们前面说，Grid 布局是一个二维系统，它同时处理列和行。这一节我们接着来研究格子的对齐，这里需要先扩展一个知识点，在网格布局中，存在两个方向的轴：

- **行轴 (Inline Axis)**: 在默认的书写模式下，这是水平方向的轴，从左到右。
- **块轴 (Block Axis)**: 在默认的书写模式下，这是垂直方向的轴，从上到下。

针对于不同方向上的轴，有不同的CSS属性来控制其对齐方式：

- **`justify-*`**相关属性，控制 **行轴（Inline Axis）** 上的对齐。
- **`align-*`**相关属性，控制 **块轴（Block Axis）** 上的对齐。

那么什么时候会用到对齐操作呢？比如当网格内的元素小于网格大小时，我们就可以考虑以什么方式对齐：

```html
<div class="grid-box">
  <div class="grid-item">1</div>
  <div class="grid-item">2</div>
  <div class="grid-item">3</div>
  <div class="grid-item">4</div>
  <div class="grid-item">5</div>
  <div class="grid-item">6</div>
</div>
```

```css
.grid-box {
  gap: 10px;
  display: grid;
  grid-template-columns: repeat(4, 150px);
  grid-auto-rows: 150px;  /* 自动扩展的列数高度也给到150px */
}

.grid-item {
  width: 100px;
  height: 100px;
  background-color: red;
}
```

![image-20251214181507621](https://s2.loli.net/2025/12/14/Afk7KTOFuvzEnLx.png)

可以看到，默认情况下，格子中的盒子会靠在左上角位置，比如现在我们希望让所有网格内的盒子横向居中（也就是行轴方向上居中）可以使用`justify-items`属性：

- `stretch` (默认值): 如果元素未设置宽度，将占满整个格子的宽度，如果有宽度则按照下面的`start`效果对齐。
- `start`: 以行轴起始方向对齐。
- `end`: 以行轴终点方向对齐。
- `center`: 以行轴方向的中点位置对齐。

![image-20251214182238139](https://s2.loli.net/2025/12/14/RuzvNn5sWYHlkfm.png)

可以看到，使用`center`后，每个盒子都跑到了单个网格的行轴中心位置。

此外，针对于单个元素，我们还可以独立控制其对齐行为，比如我们希望它可以按照居右对齐，使用`justify-self`即可进行控制：

![image-20251214184704793](https://s2.loli.net/2025/12/14/6MN2SBhV4mHsvXc.png)

接着，我们也可以让整个网格实现行轴居中，因为网格盒子本身就比较宽，这里的列数完全不足以填满整个网格的行轴空间，因此，使用`justify-content`属性可以进行网格整体控制，实现这种效果：

- `stretch` (默认值): 如果未设置格子具体宽度，网格将伸展宽度以占用剩余空间。
- `start`: 所有格子都向行轴的起点堆积。
- `end`: 所有格子都向行轴的终点堆积。
- `center`: 所有格子都朝行轴的中心位置堆积。
- `space-between`: 优先用格子填充两侧，再逐步填充中心，间距自动调整为一致。
- `space-around`: 保证每个格子两侧的间隔相等，类似于`margin`效果。
- `space-evenly`: 保证每个格子之间的间隔一致，类似于`gap`效果。

![image-20251214182519276](https://s2.loli.net/2025/12/14/ZcjXg9RzoP4JNSF.png)

可以看到，使用`center`后，所有的格子全部跑到盒子的行轴中心位置进行网格布局。

既然有这么多属性可以控制行轴上元素的顺序，同样的，针对于块轴，我们可以使用`align`相关属性来控制，比如控制盒子在纵向（块轴）上的对齐，我们可以使用`align-items`进行控制，用法和上面完全一样，只是方向发生了变化而已：

![image-20251214184139390](https://s2.loli.net/2025/12/14/PQXcBMbKw9kYG3E.png)

包括也有一个相同的`align-self`属性，可以针对单个元素进行对齐控制。

以及我们可以使用`align-content`来控制整个网格在块轴上的位置排布，此时我们需要将格子布局盒子高度设置一下，不然看不出来效果：

```css
.grid-box {
	...
  align-content: center;  /* 实现块轴居中效果 */
  height: 500px;  /* 把格子容器大小设置为500高 */
}
```

![image-20251214184505236](https://s2.loli.net/2025/12/14/y4nac3OXqYxQv5l.png)

可以看到，和之前的`justify-content`一样，所有的网格都跑到了块轴方向上的中心位置。

> 为了方便使用 Grid 还提供了成对的简写属性，按 **行轴 / 块轴** 的顺序书写：
>
> - `place-content: <align-content> <justify-content>`
> - `place-items: <align-items> <justify-items>`
> - `place-self: <align-self> <justify-self>`

这里总结一下上面用到的几种对齐类型：

- **`*-content`**: 控制 **整个网格** 在容器内的对齐。这在你定义的网格轨道（所有行和列）总尺寸小于容器尺寸时生效。
- **`*-items`**: 控制所有 **网格项（Item）** 在其所在的 **单元格（Cell）** 内的对齐。
- **`*-self`**: 控制某一个 **网格项（Item）** 在其所在的 **单元格（Cell）** 内的对齐。

对齐一般用于网格内元素小于单元格大小，以及网格整体排列等情况，在大部分情况下很少会用到。

#### 网格区域

想象一下，如果我们可以直接在 CSS 里“画”出页面的布局，是不是很快捷？而`grid-template-areas` 就是用来做这件事的，它可以让我们通过引用子元素的别名来定义网格模板。比如我们需要实现这样的布局：

![image-20251211113402268](https://s2.loli.net/2025/12/11/iDxzR4d9qGlHveI.png)

首先，我们需要给每一个子盒子起一个名字（别名），这需要用到 `grid-area` 属性：

```css
<div class="grid-box">
  <div class="grid-item header">1</div>
  <div class="grid-item nav">2</div>
  <div class="grid-item content">3</div>
  <div class="grid-item footer">4</div>
</div>
```

```css
.grid-item { background-color: gray; }
.header  { grid-area: header; }
.aside { grid-area: nav; }
.main    { grid-area: content; }
.footer  { grid-area: footer; }
```

有了名字之后，我们就可以在父容器（Grid Container）上使用 `grid-template-areas` 属性来引用这些名字，它的语法非常像是在写一个字符串矩阵：

```css
.grid-box {
  display: grid;
  gap: 1px;   /* 间距留窄一点方便观察 */
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: 40px 140px 40px;

  grid-template-areas:
    "header header header"   /* 从上往下从左往右，编写每个格子分别属于哪个盒子，连续的两个或多个格子会自动相连 */
    "nav    content content"
    "nav    footer footer";
}
```

![image-20251211113724826](https://s2.loli.net/2025/12/11/JbXopGiaRQN5UqL.png)

可以看到，上面的表格确实按照我们想要的效果展示出来了：

- 第一行 `header header header`：表示 `header` 元素横跨了整整 3 列（相当于之前的 `grid-column: 1 / -1`）。
- 第二行 `nav content content`：表示左边第 1 列是导航栏，右边剩下的 2 列都给内容区域。
- 第三行 `footer footer footer`：底部通栏。

这种写法最大的好处就是**语义化极强**。别人不需要看具体数值，光看这段 CSS 代码，脑海里就能浮现出网页的结构图。

如果某个格子我们不想放任何内容，想留空怎么办？在 `grid-template-areas` 中，我们可以使用**点号 (`.`)** 来表示一个空的网格单元：

```css
grid-template-areas: 
  "header header header"
  "nav    .      content" /* 中间留了一个格子的空白 */
  "nav    footer footer";
```

不过，虽然网格布局能够很好地完成网页整体布局，但是它不如我们接下来要介绍的`flex`布局灵活，我们更推荐各位小伙伴采用弹性布局对网页进行整体排版。

### 弹性布局

Flex布局是目前Web开发中使用最广泛、最流行的布局方式，它可以简便、完整、响应式地实现各种页面布局。和网格布局一样，要使用Flex布局，首先需要给父容器设置 `display: flex`：

```css
.grid-box {
  display: flex;  /* 采用弹性布局 */
}
```

一旦设置了这个属性，这个盒子就变成了**"Flex容器"**（Flex Container），而它内部的所有**直接子元素**就会自动变成**"Flex项目"**（Flex Item）我们可以来看一下盒子怎么排列：

```html
<div class="flex-box">
  <div class="flex-item">1</div>
  <div class="flex-item">2</div>
  <div class="flex-item">3</div>
  <div class="flex-item">4</div>
</div>
```

```css
.flex-box {
	display: flex;
}

.flex-item {
  width: 100px;
  height: 100px;
  background-color: red;
}
```

![image-20251212115949586](https://s2.loli.net/2025/12/12/bE12gRmjwsGcQIn.png)

可以看到，当我们采用弹性布局，盒子会呈线性排列，无论是块级还是行内元素，并且即使是块级元素，也会直接无视其排斥特性。默认情况下，盒子在弹性布局容器内，会按顺序横向排列，如果说前面学习的网格布局是二维布局，那么弹性布局就是一维布局，它专注于控制单个方向上的元素排列。

和网格布局一样，我们也可以使用`gap`属性来控制盒子之间的空隙：

```css
.flex-box {
  display: flex;
  gap: 10px;
}
```

![image-20251212142631298](https://s2.loli.net/2025/12/12/Wby5nrm9FiVqHzf.png)

这里需要注意的是，默认情况下，所有项目都会挤在同一行上，即使盒子已经多到装不下：

![image-20251213133152696](https://s2.loli.net/2025/12/13/XkWlLtIPzHueVB2.png)

可以看到，上面的盒子宽度并不是我们设定的值，实际上，当盒子过多且无法按照其本来的宽高进行展示时，弹性布局会强制将盒子进行压缩，通过改变每一个元素的宽度来适应。就像它的名字一样，既然是弹性的，那肯定是能屈能伸。

当然，我们也可以使用`flex-wrap`属性来控制当展示不下时是否自动换行：

```css
.flex-box {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;  /* 自动换行，也可以使用wrap-reverse向反方向换行 */
}
```

开启自动换行后，盒子将不再受到挤压，而是保持正常大小进行计算并换行。

#### 对齐方式

Flexbox 的核心在于它引入了两个非常重要的概念：**主轴（Main Axis）** 和 **交叉轴（Cross Axis）**

- **主轴（Main Axis）**：Flex项目排列的主要方向，默认是水平从左到右。
- **交叉轴（Cross Axis）**：与主轴垂直的方向，如果主轴是水平的，交叉轴就是垂直的。

![image-20251213141303327](https://s2.loli.net/2025/12/13/pZoX5J8y19TulgV.png)

我们可以调整主轴的方向来让元素以其他方向或是顺序进行排列，使用`flex-direction`属性来决定主轴方向：

- `row` (默认值): 主轴为水平方向，起点在左端。
- `row-reverse`: 主轴为水平方向，起点在右端。（项目从右向左排）
- `column`: 主轴为垂直方向，起点在上沿。
- `column-reverse`: 主轴为垂直方向，起点在下沿。（项目从下向上排）

```css
.flex-box {
  display: flex;
  flex-direction: column;  /* 以纵向排列 */
}
```

![image-20251212131735510](https://s2.loli.net/2025/12/12/iC8AuZB9nQL2tTJ.png)

由于我们通过`flex-direction`改变了主轴的方向，此时盒子的排列方向就变成从上往下了。

> 结合上面讲解的`flex-wrap`属性，有一个简写属性`flex-flow`可以结合两者：
>
> ```css
> flex-wrap: wrap;
> flex-direction: row-reverse;
> ```
>
> 等价于：
>
> ```css
> flex-flow: row-reverse wrap;
> ```

针对于主轴和交叉轴，我们可以分别控制其对齐方式，这里有两种CSS属性：

- **`justify-`** 系列：控制 **主轴 (Main Axis)** 上的对齐。在默认的 `flex-direction: row` 或 Grid 布局中，它就是 **水平方向**。
- **`align-`** 系列：控制 **交叉轴 (Cross Axis)** 上的对齐。在默认的 `flex-direction: row` 或 Grid 布局中，它就是 **垂直方向**。

这和我们之前在网格布局中介绍的对齐属性基本一致，但是注意某些属性无法使用：

| 属性 (Property)   | 作用于 (Applies to) | 作用轴 (Axis) | 核心作用 (Core Function)                                     | 适用布局 (Layout)    |
| ----------------- | ------------------- | ------------- | ------------------------------------------------------------ | -------------------- |
| `justify-content` | 容器                | 主轴 (水平)   | 分配**容器内所有项目**的整体间距和对齐。                     | Flex & Grid          |
| `align-content`   | 容器                | 交叉轴 (垂直) | 分配**多行/多列项目**之间的间距和对齐。 **(只在多行/列时生效)**. | Flex & Grid          |
| `justify-items`   | 容器                | 主轴 (水平)   | 设置容器内**所有项目**在“单元格”内的默认水平对齐。           | Grid (Flexbox中无效) |
| `align-items`     | 容器                | 交叉轴 (垂直) | 设置容器内**所有项目**在“单元格”内的默认垂直对齐。           | Flex & Grid          |
| `justify-self`    | 单个项目            | 主轴 (水平)   | **覆盖** `justify-items`，单独设置**某个项目**的水平对齐。   | Grid (Flexbox中无效) |
| `align-self`      | 单个项目            | 交叉轴 (垂直) | **覆盖** `align-items`，单独设置**某个项目**的垂直对齐。     | Flex & Grid          |

因为弹性布局和网格布局控制维度不同，且元素直接由弹性盒子控制，其中`justify-items`无法生效，盒子本身就是采用原始尺寸，无法做到和Grid一样的格子内控制，同理，`justify-self`也无法完成控制。

我们先来试试看主轴上的对齐控制，唯一可用的就是`justify-content`属性，它可以控制所有 FlexItem 在主轴上的对齐方式，它有以下预设值：

- `flex-start/start` (默认值): 从主轴起点开始对齐。
- `flex-end/end`: 从主轴终点开始对齐。
- `center`: 在主轴上居中对齐。
- `space-between`: 优先两端对齐，项目之间的间隔都相等。第一个项目贴近起点，最后一个项目贴近终点。
- `space-around`: 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
- `space-evenly`: 所有项目之间的间隔，以及项目与边框的间隔，都完全相等。

```css
.flex-box {
  display: flex;
  gap: 10px;
  justify-content: center;   /* 此时主轴上的元素会自动居中靠拢，两边留出空位 */
}
```

![image-20251213134956332](https://s2.loli.net/2025/12/13/orxtQqacL6mPhz7.png)

同样的，如果我们想要控制交叉轴上的元素排列，可以使用`align`相关属性，之前在Grid提到的三个属性都可以使用，比如我们现在希望盒子在垂直方向上居中：

```html
<div class="flex-box">
  <div class="flex-item">1</div>
  <div class="flex-item">2</div>
  <div class="flex-item">3</div>
  <div class="flex-item">4</div>
</div>
```

```css
.flex-box {
  display: flex;
  gap: 10px;
  height: 200px;  /* 给个高度，不然会由内部盒子撑开，看不到效果 */
  background-color: lightblue;
  align-items: center;  /* 表示元素在交叉轴上以居中排列 */
}
```

可以看到，和之前Grid一样，所有盒子确实在交叉轴方向上居中了。我们也可以试试看改变Flex容器的方向，观察此时元素会如何进行排列并居中。

不过，由于Flex布局只有一维方向，针对于进行整体控制的`align-content`属性，因为其效果是控制交叉轴上的多行元素，我们需要开启弹性布局的换行功能才能正常使用，此属性只有在换行的情况下才能使用，类似于 `justify-content` 的多行版本：

- `stretch` (默认值): 各行将伸展以占用剩余空间。
- `flex-start`: 所有行都向交叉轴的起点堆积。
- `flex-end`: 所有行都向交叉轴的终点堆积。
- `center`: 所有行都朝交叉轴的中点堆积。
- `space-between`: 首行在起点，末行在终点，其他行平均分布。
- `space-around`: 每行两侧的间隔相等。

比如我们希望出现换行时，靠顶部和底部排列：

```css
.flex-box {
  display: flex;
  height: 300px;   /* 得设置一个高度，不然看不出来 */
  background-color: lightblue;
  gap: 10px;
  flex-wrap: wrap;
  align-content: space-between;
}
```

![image-20251213142730311](https://s2.loli.net/2025/12/13/9pfuPQedZCH6TgM.png)

当然，除了统一控制对齐方式之外，我们也可以对弹性盒子里面的元素单独设置对齐方式：

```html
<div class="flex-box">
  <div class="flex-item" style="align-self: end">1</div>
  <div class="flex-item">2</div>
  <div class="flex-item">3</div>
  <div class="flex-item">4</div>
  <div class="flex-item">5</div>
</div>
```

```css
.flex-box {
  display: flex;
  height: 200px;
  background-color: lightblue;
  gap: 10px;
  align-items: center;
}
```

![image-20251213143217116](https://s2.loli.net/2025/12/13/yIA5zrXH4VKRsa1.png)

此时，1号盒子就采用自己定义的对齐方式，而其他元素依然采用弹性容器定义的对齐方式。

#### 空间分配

我们接着来介绍一下 Flex Item 的空间分配控制。在前面的学习中我们发现，默认情况下，如果弹性盒子的空间不足以容纳其中的元素，此时会导致盒子被自动挤压。

这其实是`flex-shrink`属性，它可以控制项目的缩小比例，也就是当父容器在主轴方向上空间不足时，盒子如何收缩：

- 默认为 `1`，表示当空间不足时，所有盒子都将等比例缩小。
- 如果一个盒子的 `flex-shrink` 为 `0`，表示禁止缩小，保持原始大小。
- 如果一个盒子的 `flex-shrink` 为 `2`，而容器内其他其他盒子为 `1`，那么它在收缩时会比其他盒子多收缩一倍，以此类推。

比如我们现在不希望盒子被挤压：

```css
.flex-item {
  width: 100px;
  height: 100px;
  flex-shrink: 0;
  background-color: red;
}
```

![image-20251213145729642](https://s2.loli.net/2025/12/13/TbF5CkmWQL3HUAD.png)

可以看到，此时盒子就按照正确的大小进行排列了，并且浏览器底部出现了滚动条。

同样的，我们不仅可以控制弹性压缩，也可以控制弹性伸缩，我们可以使用`flex-grow`来使盒子自动扩大，它决定当父容器在主轴方向上有剩余空间时，项目如何瓜分这些空间：

- 默认为 `0`，表示即使有剩余空间，也不放大。
- 如果所有盒子的 `flex-grow` 都为 `1`，它们将平分剩余空间。
- 如果一个盒子的 `flex-grow` 为 `2`，其他盒子都为 `1`，那么它将获得比其他盒子多一倍的剩余空间，以此类推。

```css
.flex-item {
  width: 100px;
  height: 100px;
  flex-grow: 1;   /* 自动增长 */
  background-color: red;
}
```

![image-20251213172335562](https://s2.loli.net/2025/12/13/zea3TXERtGOhmnj.png)

此外还有一个`flex-basis`属性用于控制，它可以统一控制元素在主轴上占据的空间大小：

- 默认值是 `auto`，表示盒子的大小由其内容或 `width`/`height` 属性决定。
- 也可以设置一个具体的长度值，如 `100px`，那么盒子在主轴上的尺寸也会自动调整为`100px`。

最后，针对于以上三个属性，我们还可以使用简写属性`flex`来一步到位，比如：

```css
.flex-item {
  flex: 1 0 auto;
}
```

等价于：

```css
.flex-item {
  flex-grow: 1;
  flex-shrink: 0;
  flex-basis: auto;
}
```

这里还有一些常用的例子：

- `flex: 0 1 auto;` (默认值) - 不放大，会缩小，大小由内容决定。
- `flex: 1;` - 这是 `flex: 1 1 0%;` 的简写。意思是：会放大，会缩小，基础大小为0。这常用于让项目等分容器。
- `flex: auto;` - 这是 `flex: 1 1 auto;` 的简写。
- `flex: none;` - 这是 `flex: 0 0 auto;` 的简写。不放大也不缩小。

我们可以尝试使用弹性布局来实现之前网格阶段学习的页面布局：

![image-20251211113402268](https://s2.loli.net/2025/12/11/iDxzR4d9qGlHveI.png)

```html
<div class="main-window">
  <div class="header">Header</div>
  <div class="main-content">
    <div class="aside">Aside</div>
    <div class="main-content__body">
      <div class="main">Main</div>
      <div class="footer">Footer</div>
    </div>
  </div>
</div>
```

```css
/* 弹性布局容器 */
.main-window {
  height: 240px;
  display: flex;
  flex-direction: column;
}

.main-content {
  flex: 1;
  display: flex;
}

.main-content__body {
  flex: 1;
  display: flex;
  flex-direction: column;
}

/* 各板块容器 */
.header {
  height: 50px;
  background-color: #CFE0F9;
}

.aside {
  width: 200px;
  background-color: #DFEAFB;
}

.main {
  flex: 1;
  background-color: #EFF5FD;
}

.footer {
  height: 50px;
  background-color: #CFE0F9;
}
```

![image-20251213180534264](https://s2.loli.net/2025/12/13/WdYfH1QS2yz98LT.png)

至此，有关弹性布局的内容我们就介绍完了。

### 浮动布局（选学）

**注意：**此布局方式已过时且不推荐使用，仅作为选学内容，不强制要求掌握。

在 Flex 和 Grid 布局成为主流之前，`float`（浮动）曾是网页多列布局的主要实现方式。尽管现在我们有了更现代、更强大的工具，但理解 `float` 依然很有价值，因为它不仅存在于许多老旧的网站代码中，而且在其设计的初衷——**图文环绕**效果上，至今仍然是最直接的实现方式。

浮动的本质是让一个元素脱离正常的文档流，并移动到其父容器的左侧或右侧，直到它的边缘碰到**父容器的边缘**或**另一个浮动元素的边缘**，要使用浮动布局，我们可以使用`float`属性：

```css
.float-box {
  float: left; /* 元素向左浮动 */
}
```

`float`属性有以下几个值：

- `left`: 元素向左浮动。
- `right`: 元素向右浮动。
- `none`: (默认值) 元素不浮动，按照正常文档流显示。

前面已经说了，它主要用于实现文字环绕图片的效果，这里我们来测试一下：

```html
<div class="container">
  <img src="test-image.png" class="floated-image">
  <p>
    这是一段很长很长的文本，它会自动环绕在浮动的图片周围。当一个元素浮动后，它就不再占据正常文档流中的空间，后续的块级元素会无视它，但行内内容（如文本）会感知到它的存在，并围绕它进行排列，从而形成图文环绕的效果。这就是浮动布局最原始也是最核心的用途。
  </p>
</div>
```

```css
.floated-image {
  float: right;
  width: 150px;
  margin-left: 15px; /* 给图片左侧留出一些空隙，避免文字贴得太近 */
}

.container {
  width: 500px;
  border: 1px solid #ccc;
}
```

![image-20251213222251323](https://s2.loli.net/2025/12/13/N2T7U9pM3j6eJ8B.png)

可以看到，图片向右浮动后，`p` 标签中的文字很自然地围绕在了图片的右侧和下方，这在一些新闻报纸上很常见，浮动布局可以很好地实现文字环绕效果。浮动会使盒子不再遵循正常文档流排列，同样出现了之前遇到的脱离文档流效果。

那如果是两个块级元素相邻呢？

```html
<div class="container">
  <div class="float-1">我是一号盒子</div>
  <div class="float-2">我是二号盒子</div>
</div>
```

```css
.float-1 {
  float: right;
  margin-left: 5px;
}

.container {
  width: 400px;
  border: 1px solid #ccc;
}
```

此时在一个大盒子中，其中一个小盒子设置了浮动，而另一个小盒子没有设置，那么此时会变成什么效果呢？

![image-20251213223013798](https://s2.loli.net/2025/12/13/UknaigZo2wGNxyz.png)

可以看到，一号盒子由于设置了右浮动，直接跑到最右边去了，同时，二号盒子会直接占满大盒子的宽度，就好像是无视了第一个浮动盒子一样，这就是浮动带来的效果。不过有趣的是，这种效果仅限于块级元素，如果遇到了行内元素，那么依然会感知到浮动元素的存在，从而刻意绕开，比如我们可以为二号盒子增加文本：

```html
<div class="container">
  <div class="float-1">我是一号盒子</div>
  <div class="float-2">我是二号盒子我是二号盒子我是二号盒子我是二号盒子我是二号盒子我是二号盒子我是二号盒子我是二号盒子</div>
</div>
```

![image-20251213223848759](https://s2.loli.net/2025/12/13/zX89IE6PqhVQgSo.png)

可以看到，虽然文本位于二号盒子内，并且二号盒子也占据了整个宽度，但是里面的行内元素（直接编写的文字）依然会感知并避开浮动元素。

浮动布局虽然功能强大，但是也存在一些问题，浮动布局有一个非常著名的副作用：**父元素高度塌陷**。当一个容器内的所有子元素都设置了浮动时，由于这些子元素都脱离了正常文档流，父容器会认为内部没有任何内容，导致其高度计算为 0，比如：

```html
<div class="container">
  <div class="float">我是一号盒子</div>
  <div class="float">我是二号盒子</div>
</div>
```

```css
.float {
  float: right;
}

.container {
  width: 400px;
  border: 1px solid #ccc;
}
```

![image-20251213224351020](https://s2.loli.net/2025/12/13/rsRNHWTuELBVmQg.png)

你会发现，大盒子的边框“塌陷”成了一条线，由于所有元素都脱离了文档流使得它没有高度，这会导致后续布局出现严重问题，因为父容器没有被正确地撑开。

为了解决这个问题，我们需要 “清除浮动”。核心思想是在浮动元素的末尾，告诉浏览器：“在这里，不要再让任何元素环绕在浮动元素的旁边了”，这可以通过`clear`属性实现，`clear`属性规定元素的哪一侧不应该紧挨着浮动元素：

- `clear: left;`：元素的左侧不能有浮动元素。
- `clear: right;`：元素的右侧不能有浮动元素。
- `clear: both;`：元素的左右两侧都不能有浮动元素，这是最常用的值。

以下是三种主流的清除浮动的方法：

1. 最古老、最直接的方法是在所有浮动元素的末尾添加一个空的`<div>`，并为其设置 `clear: both`。

   ```html
   <div class="parent-box">
     <div class="child-box">子元素1</div>
     <div class="child-box">子元素2</div>
     <div style="clear: both;"></div> <!-- 清除浮动的空元素 -->
   </div>
   ```

   这个空`div`会被推到所有浮动元素的下方，从而将父容器的高度撑开，**缺点：** 增加了无意义的HTML结构，不符合语义化。

2. 此外，为父元素创建一个新的BFC（Block Formatting Context，格式化上下文）也能解决高度塌陷问题。BFC的一个特性就是其内部的浮动元素也会被计算高度。**缺点：** 如果内容真的溢出了，`overflow: hidden`可能会裁剪掉重要内容，需谨慎使用。

3. 目前最流行、最优雅的解决方案是，利用CSS的`::after`伪元素，在父容器内容的末尾动态地添加一个看不见的“清道夫”元素，来完成清除浮动的工作。

总而言之，`float` 在实现文字环绕效果时依然非常有用。但在进行整体页面布局时，强烈推荐使用 **Flex** 或 **Grid**，因为它们是为布局而生的，功能更强大，也避免了浮动带来的各种问题。

### 表格布局（选学）

**注意：**此布局方式已过时且不推荐使用，仅作为选学内容，不强制要求掌握。

在 Flexbox 和 Grid 布局出现之前，开发者们为了实现多列布局，尝试了各种“奇技淫巧”。除了前面介绍的 `float` 之外，另一种流行的方法是滥用 CSS 的 `display` 属性，模拟 HTML的 `table` 标签行为来进行布局。

其核心思想是，将一个普通的 `div` 容器通过 CSS 设置为 `display: table`，其子元素设置为 `display: table-row`（模拟 `tr`），孙子元素设置为 `display: table-cell`（模拟 `td`）我们来看一个基本示例，如何用它来创建一个简单的三列布局：

```html
<div class="table-layout-container">
  <div class="table-row">
    <div class="table-cell">第一列</div>
    <div class="table-cell">第二列，这列的内容会多一些，使得高度增加。</div>
    <div class="table-cell">第三列</div>
  </div>
</div>
```

```css
.table-layout-container {
  display: table;
  width: 100%;
  border-collapse: collapse; /* 类似表格，让单元格边框合并 */
}

.table-row {
  display: table-row;
}

.table-cell {
  display: table-cell;
  border: 1px solid #ccc;
  padding: 10px;
}
```

![image-20251213230320252](https://s2.loli.net/2025/12/13/4hkftTaQ9vWASxR.png)

可以看到，虽然表格布局能轻松模拟HTML中的表格，也可以利用它来实现之前介绍的一些布局样式，但是既然我都用表格了，为啥我不直接用之前的网格布局呢？还不需要考虑表格的一些样式处理。

此外，表格布局的缺点也非常多，比如：

1. **结构冗余**：为了实现布局，通常需要添加额外的、无语义的 `div` 标签来模拟 `table-row`，这让 HTML 结构变得臃肿。
2. **响应式差**：表格布局天生是为规整的二维数据设计的，在不同屏幕尺寸下调整布局（比如在手机上根据屏幕宽度判断修改CSS，从三列变为一列）非常困难，远不如 Flex 或 Grid 灵活。
3. **渲染性能**：浏览器需要读取整个“表格”的结构后才能开始渲染，对于大型布局，这可能会比现代布局方法更慢。

### 布局行内展示

我们已经知道，通过设置 `display: flex`，一个容器就变成了“Flex容器”，它的直接子元素就成了“Flex项目”。不过，这种容器依然是一个**块级元素（block-level element）**，它会占据一整行，就像 `div` 一样：

```html
<div class="flex-box">
  <div class="flex-item"></div>
  <div class="flex-item"></div>
</div>
<span>我是字谢谢</span>
```

```css
.flex-box {
  gap: 10px;
  display: flex;
}

.flex-item {
  width: 20px;
  height: 20px;
  background-color: red;
}
```

![image-20251213181150519](https://s2.loli.net/2025/12/13/xblVanoFfRi5XkU.png)

考虑到这个问题，CSS也支持将这些常用的布局设置为行内形式，让其变为行内特性：

```css
.flex-box {
  gap: 10px;
  display: inline-flex;  /* 保留flex布局且盒子本身变为行内状态 */
}
```

![image-20251213230822062](https://s2.loli.net/2025/12/13/4ep3Wax9R7X5vtC.png)

可以看到，此时弹性布局容器变成了行内状态，不再与其他行内元素相排斥了。当然，不仅仅是`flex`包括之前提到的`grid`和`table`布局也有对应的行内状态。

### 块级格式化上下文

前面我们一直提到，BFC (Block Formatting Context)，即块级格式化上下文，是网页CSS布局中一个非常重要的概念。 简单来说，它就像一个独立的 “盒子”，盒子内部元素的布局不会影响到外部，反之亦然。通过BFC我们能解决很多布局带来的问题，这一节我们就来研究一下什么是BFC。

BFC具有一些独特的特性，理解这些特性有助于我们更好地掌控页面布局：

- **隔离的独立容器**：BFC像一个结界，内部元素的布局不会影响外部元素，比如前面遇到的`margin`折叠问题。 
- **垂直外边距合并**：在同一个BFC中，相邻的块级盒子的垂直外边距会发生合并 (margin collapse)。 
- **包含浮动元素**：BFC可以包含其内部的浮动元素，从而解决因浮动元素导致父元素高度塌陷的问题。 
- **不与浮动元素重叠**：BFC的区域不会与浮动元素的区域重叠，这使得我们可以利用BFC来实现两栏或多栏布局。

那么如何创建BFC元素呢？很简单，我们可以使用以下方式来将一个盒子变为BFC元素，只需要满足其中之一：

- 根元素 `<html>` 
- 浮动元素 (元素的 `float` 属性不为 `none`) 
- 绝对定位元素 (元素的 `position` 值为 `absolute` 或 `fixed`)
- `display` 值为 `inline-block`, `table-cell`, `table-caption`, `flex`, `inline-flex`, `grid` 或 `inline-grid` 的元素
- `overflow` 值不为 `visible` 的块级元素（如 `hidden`, `auto`, `scroll`) 

其中最简单的方式就是设置`overflow`的属性为`hidden`，这将会使得盒子立即变成BFC容器：

```html
<div class="test-box">
  <div class="flex-item"></div>
</div>
```

```css
.test-box {
  overflow: hidden;
}

.flex-item {
  width: 50px;
  height: 50px;
  margin-top: 20px;
  background-color: red;
}
```

包括我们前面介绍的垂直方向上外边距合并问题，我们只需要将其中一个元素包裹在BFC盒子内即可：

```html
<div class="test-box">
  <div class="flex-item"></div>
</div>
<div class="flex-item"></div>
```

```css
.test-box {
  overflow: hidden;
  width: fit-content;
}

.flex-item {
  width: 50px;
  height: 50px;
  margin: 20px;
  background-color: red;
}
```

此时之前出现的问题就迎刃而解了：

![image-20251213232842309](https://s2.loli.net/2025/12/13/hc7ej39GzUdaMS8.png)

至此，有关布局的内容就全部介绍完成。

### UI设计系列（课程三）

很多网站首页有一种高级效果：页面滚动时背景固定不动，内容向上滚动，或是内容和背景的滚动幅度不一致，比如柏码官网：https://www.itbaima.cn/zh-CN

这种效果我们一般称其为**视差 (Parallax) 质感**，虽然真正的视差需要 JS才能实现，但是 CSS 的 `background-attachment` 属性也能实现轻量版的质感滚动，我们可以来尝试实现一下。这个属性决定背景图片是随着内容滚动、保持固定，还是局部滚动。

这里我们直接为`body`设置一个背景：

```html
<body>
  <div style="height: 2000px"></div>
</body>
```

```css
body {
  margin: 0;
  background-size: contain;
  background-repeat: no-repeat;
  background-image: url("/img/background.jpeg");
}
```

`background-attachment` 属性用于决定背景图片是应该随着页面其余部分滚动，还是保持固定，亦或是随着其父元素滚动，可选预设值如下：

- **`scroll` (默认值):** 背景图片会随着页面的滚动而移动。
- **`fixed`:** 背景图片相对于视口（浏览器窗口）固定，不会随着页面滚动而移动。这是实现视差效果的关键。
- **`local`:** 背景图片会随着元素内容的滚动而滚动。如果元素没有滚动条，则效果与 `scroll` 相同。

这里我们直接将其设置为`fixed`效果，再稍微调整一下首页的元素排列：

```html
<body>
  <div style="height: 500px;padding-top: 100px;box-sizing: border-box">
    <h1>Hello World</h1>
    <h2>欢迎来到我们的官方网站</h2>
  </div>
  <div style="height: 2000px;background-color: white;"></div>
</body>
```

```css
body {
  margin: 0;
  background-size: 100% 500px;
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-image: url("/img/background.jpeg");
}
```

可以看到，此时首页的背景将始终固定在顶部，即使我们滑动滚动条依然存在。

**思考：**可以采用其他方式实现这种视差效果吗？

当然，这种方式只能实现简单的视差效果，要实现前景滚动背景缓慢跟随的效果，使用CSS无法实现，我们更建议使用JavaScript进行控制，效果会更好。

### UI设计系列（课程四）

本节重点是实现网页的顶部导航栏效果，创建一个顶部固定的导航栏：

![image-20251213235641081](https://s2.loli.net/2025/12/13/7GhXLxqrYIfR9Pc.png)

图标库推荐使用Fontawsome图标库，简约大气：

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@7.1.0/css/fontawesome.min.css">
```

推荐图标库网站：

* https://www.iconfont.cn  -  阿里巴巴图标库
* https://fontawesome.com  -  Fontawsome图标库
* https://iconpark.oceanengine.com/official  -  字节跳动图标库

本案例课程讲解代码保存至网盘。

## 选择器进阶

在上一章，我们介绍了超多选择器，不同的选择器有着不同的选择方式，不过它们都是选择某一个指定元素，而这一部分，我们将继续深入选择器，探索更多关于选择器的使用方法。

### 伪类选择器

伪类选择器用于向选择器添加特殊的效果，它不选取元素本身，而是选取处于特定“状态”的元素。比如，当用户鼠标悬停在元素上时，或者一个链接已经被访问过，像这一类具有状态性质的选择，我们就可以考虑使用伪类选择器。伪类的语法以一个冒号 (`:`) 开头。

我们从最简单的开始，与用户的交互行为相关，比如鼠标操作、链接状态等伪类：

- **`:link`** 和 **`:visited`**: 分别用于设置链接未被访问和已被访问时的样式。
- **`:hover`**: 用于设置鼠标悬停在元素上时的样式。这是最常用的伪类之一，可以用于任何元素，不仅仅是链接。
- **`:active`**: 用于设置元素被激活时的样式（例如，鼠标在元素上按下但还未释放）。
- **`:focus`**: 用于设置元素获得焦点时的样式，通常用于表单输入框、按钮等可交互元素。

我们可以来尝试一下，修改一个`a`标签在不同状态下的颜色：

```html
<a href="#">这是一个链接</a>
```

```css
a:link {
  color: green; /* 未访问的链接为绿色 */
}
a:visited {
  color: red; /* 已访问的链接为红色 */
}
a:hover {
  color: purple; /* 鼠标悬停时变为紫色 */
  text-decoration: none;  /* 鼠标悬停时移除下划线 */
}
a:active {
  color: orange; /* 点击时变为橙色 */
}
```

此时，针对不同状态的链接，就可以展现出不同的效果了。

我们也可以来试试看`input`输入框，上面提到的伪类同样适用：

```html
<input type="text" placeholder="点击这里输入">
```

```css
input:focus {
  background: green;   /* 选中时使用绿色 */
}
```

**注意：** 当这几个伪类一起使用时，为了保证它们都生效，需要遵循 **LVHA** 顺序：`:link` — `:visited` — `:hover` — `:active`，因为前面我们说过，越在后面的CSS优先级更高，因此保证这里的顺序才能正确进行样式覆盖，你可以尝试修改上面CSS选择器的顺序看看会发生什么。

我们接着来看，结构类伪类，它们会根据元素在文档树（就是HTML代码）中的位置来选取元素：

- **`:first-child`**: 选取其父元素下的第一个子元素。
- **`:last-child`**: 选取其父元素下的最后一个子元素。
- **`:nth-child(n)`**: 选取其父元素下的第 `n` 个子元素。`n` 可以是：
  - 数字：如 `li:nth-child(3)` 选取第3个 `li`。
  - 关键字：`even` (偶数) 或 `odd` (奇数)。
  - 公式：`an+b` 的形式，如 `2n+1` 表示所有奇数项。
- **`:only-child`**: 选取父元素中唯一的子元素。
- **`:nth-of-type(n)`**: 与 `:nth-child(n)` 类似，但它只在同类型的兄弟元素中计数。例如，`p:nth-of-type(2)` 会选取父元素下的第二个 `<p>` 元素，而不管它前面有多少个其他类型的兄弟元素。

比如我们现在想直接针对一个列表中指定位置上的元素进行调整：

```html
<ul>
  <li>第一项</li>
  <li>第二项</li>
  <li>第三项</li>
  <li>第四项</li>
  <li>第五项</li>
</ul>
```

```css
/* 选取第一个 li */
li:first-child {
  font-weight: bold;
}

/* 选取最后一个 li */
li:last-child {
  font-style: italic;
}

/* 选取所有偶数行的 li，并设置背景色，实现斑马纹效果 */
li:nth-child(even) {
  background-color: #f2f2f2;
}
```

是不是感觉伪类选择器功能很强大？通过附加伪类选择器，可以让页面上的元素样式更加随心地变化。

当然可能会有小伙伴说，那要是我现在希望除了第一个元素之外的其他所有元素都生效某个样式怎么办呢？我们还可以继续使用`:not`伪类，来实现反向判断的效果：

```css
/* 选取非第一个 li */
li:not(:first-child) {
  font-weight: bold;
}
```

此时，除了第一项的所有其他项目都会变成加粗样式。

包括我们之前讲过的并集选择器（选择器列表）有些时候非常麻烦，比如，我们要同时选择`header`、`main`和`footer`下的被鼠标覆盖的`p`标签，那么只能像这样写：

```css
header p:hover,
main p:hover,
footer p:hover {
  color: red;
  cursor: pointer;
}
```

而有了伪类之后，我们可以使用`:where`来实现合并效果：

```css
:where(header, main, footer) p:hover {
  color: red;
  cursor: pointer;
}
```

`:where`表示判断只要是其中几个选择器的其中一个满足条件即可，效果完全等价于上面的写法（还有一个类似的`:is`伪类，但是会计入到CSS的优先级计算中）当然，我更建议这种情况使用我们后续讲解的嵌套选择器，它写起来更加简单易理解，且现在大部分浏览器都支持。

还有一个比较新的`:has`伪类（截止到2023年所有浏览器完全支持）它可以用于判定某个元素是否存在满足条件的子元素或是兄弟元素，如果存在，则样式生效。比如现在我们希望只要包含一个`a`标签的`div`盒子，就采用绿色背景，此时就可以考虑使用`:has`伪类：

```html
<div>
  <a>我是链接</a>
</div>
```

```css
div:has(> a) {
  background-color: green;
}
```

我们还能继续进一步判断，比如必须要求是第一个元素为`a`标签才能设置为绿色：

```css
div:has(> a:first-child) {
  background-color: green;
}
```

此外，结合之前的`:not`伪元素，我们还能实现状态取反，只要不包含直接子元素`a`标签就设置为绿色：

```css
div:not(:has(> a:first-child)) {
  background-color: green;
}
```

由于篇幅有限，这里仅介绍常用伪类，有关更多伪类请参阅：https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference/Selectors/Pseudo-classes

### 伪元素选择器

与伪类选择器不同，伪元素选择器不是选取处于特定“状态”的元素，而是用于创建一些不存在于文档树中的“虚拟”元素，或是选取某个元素的特定部分。伪元素让我们能够对元素的某些特殊部分进行样式调整，而这些部分是无法通过常规选择器选中的。

伪元素的标准语法以两个冒号 (`::`) 开头，以区别于伪类选择器。

> **注意：** 在早期的CSS版本中，伪元素也使用单个冒号，如 `:before`。为了向后兼容，现代浏览器仍然支持这种写法。但最佳实践是使用双冒号 `::` 来明确区分伪元素和伪类。

我们先从一些常见元素的伪元素选择器说起，它们可以轻松控制特殊部分的样式。比如输入框：

```html
<label>
  <input placeholder="请输入内容">
</label>
```

```css
input {
  border: 1px solid #a200d8;
  border-radius: 20px;
  line-height: 2;
  padding: 0 12px;
  width: 200px;
}
```

![image-20251215115812162](https://s2.loli.net/2025/12/15/xEDXdRIZiNwWsG8.png)

虽然这里我们可以很轻松地修改输入框的边框、内边距之类的样式，但是里面的提示文本好像无法直接设置，这里我们就可以通过伪元素来设置：

```css
input::placeholder {
  color: #bc8cd3;
}
```

这里的`::placeholder`就是`input`中的一个伪元素，它表示输入框中的占位文本（提示文本）当然，并非只有`input`标签存在这类伪元素，如`textarea`这种多行文本输入框同样包含这个伪元素。

再比如我们之前使用过的`li`元素，想要自定义每一个项目前面的标记，也可以使用`::marker`伪元素来进行控制：

```html
<ul>
  <li>我是第一个元素</li>
  <li>我是第二个元素</li>
  <li>我是第三个元素</li>
  <li>我是第四个元素</li>
</ul>
```

```css
li::marker {
  content: '🚀';
}
```

这里我们使用到了`content`属性，它可以控制伪元素的内容，比如这里我们可以将其替换为一个Emoji表情，那么最终展示出来的效果就是：

![image-20251215125653942](https://s2.loli.net/2025/12/15/mHJAT2Md6xrLOf4.png)

请注意，虽然我们可以随意地选择伪元素，但是在伪元素里面使用某些CSS属性是有限制的，并非所有的CSS属性都可以在伪元素中进行设置，不同的伪元素有着不同的规则。具体请参阅：https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference/Selectors/Pseudo-elements

再比如我们用的最多的文本，现在我们希望某个`span`或是`p`标签里面的首字母有独特样式，可以使用`::first-letter`伪元素来控制首个字母：

```html
<p>
  Scientists exploring the depths of Monterey Bay unexpectedly encountered a rare and unique species of dragonfish. This species is the rarest of its species.
</p>
```

```css
p::first-letter {
  font-size: 1.5rem;
  font-weight: bold;
  color: brown;
}
```

![image-20251215132417250](https://s2.loli.net/2025/12/15/Dsj6EbwP5rxGakK.png)

针对于我们通过鼠标选中的那些文本，我们还可以通过伪元素来自定义其样式，`::selection`用于处理被选择的文本：

```css
p::selection {
  color: white;
  background-color: #c582f1;
}
```

![image-20251215142808789](https://s2.loli.net/2025/12/15/OLqj2WxNvKtQDf3.png)

前面我们说到，伪元素除了可以控制部分元素的特殊属性，也可以凭空产生不存在的元素，这里我们隆重介绍一下`::before`和`::after`伪元素，它们分别代表元素的内容前后插入的伪元素。

比如我们想在一段文本之前添加一个小盒子做装饰：

```html
<p>这是一段文本</p>
```

```css
p::before {
  content: "";
  width: 20px;
  height: 20px;
  margin-right: 5px;
  display: inline-block;
  background-color: red;
}
```

`::before`选择器默认会为元素的内容添加一个在它之前的伪元素，这个伪元素默认为行内元素，注意通过这种方式创建的伪元素必须填写`content`属性，否则等于没有添加内容。它的限制相比之前提到的一些伪元素更少，我们可以通过多种CSS属性组合来改变它的展示效果、大小、颜色等：

![image-20251215141649462](https://s2.loli.net/2025/12/15/OIRcvTby7xqFBY9.png)

可以看到，此时文本前面多出了一个我们在HTML从未定义的元素，同时在开发者工具中可以直接看到`p`标签内文本的最前面添加了一个`::before`的伪元素。这对于添加装饰性元素、实现特殊布局效果以及简化 HTML 结构非常有用。但是同时它也存在一些弊端，比如它是CSS动态生成，不利于SEO优化，一些重要信息不建议使用伪元素实现，包括后续学习的JavaScript也是无法控制伪元素的，因为它**不是在DOM中真实存在的元素**。

同样的，我们也可以使用`::after`在元素的内容后面添加伪元素，使用方法和效果和上面完全一样，这里不详细演示了。

### 嵌套选择器

我们接着来看嵌套选择器，它是一种比较新的语法，在2024年之后版本的浏览器才会支持，详细支持列表请见：https://developer.mozilla.org/zh-CN/docs/Web/CSS/Nesting_selector

由于比较新，对于旧版浏览器兼容性不是特别好，当然，各位小伙伴无需过多担心兼容性问题，因为在后续的学习中我们还会接触到各种CSS处理器程序，比如`Less`、`PostCSS`等，它们会自动将一些不支持的语法编译为旧浏览器支持的样式，来实现良好的兼容性，这些在后续的工程项目里面都是非常常见的。

嵌套选择器的核心思想是**允许将一个CSS规则嵌套在另一个CSS规则内部**，从而减少代码的重复性，并使CSS结构更直观地反映HTML的结构。

在没有嵌套语法之前，如果我们想为一个复杂的组件（比如一个卡片）编写样式，代码通常是这样的：

```css
.card {
  background-color: white;
  border-radius: 8px;
}

.card h2 {
  font-size: 1.5rem;
  color: #333;
}

.card p { line-height: 1.6; }
.card a { color: blue; }
```

可以看到，`.card` 这个选择器被重复了很多次，既繁琐又容易出错。而使用嵌套选择器，我们可以这样写：

```css
.card {
  background-color: white;
  border-radius: 8px;

  h2 {
    font-size: 1.5rem;
    color: #333;
  }
  
  p { line-height: 1.6; }
  a { color: blue; }
}
```

在上面的代码中，`h2`, `p`, `a` 规则都嵌套在 `.card` 内部，这等同于之前的 `.card h2`, `.card p`, `.card a`，嵌套一级相当于直接省略掉一个选择器的空格，这样不仅结构性和 HTML 能够保持大致相同，便于我们快速寻找并修改想要的样式，同时写起来也更加简洁。

多级的情况下，我们可以一直向内进行嵌套：

```css
.card div a { color: #fff; }
.card div > p { color: #fafafa; }
```

```css
.card {
  div {
    a { color: #fff; }
    > p { color: #fafafa }
  }
}
```

可能会有小伙伴说，虽然这样写着很简单，但是只适用于选择器存在空格的情况，那要是我们现在控制的是伪元素或是伪类选择器呢？

```css
.card::before { content: "sadadasda"; }∂
.card::after { content: "xXXX"; }
```

```css
.card {
  ::before { content: "sadadasda"; }
  ::after { content: "xXXX"; }
  /* 这种写法其实等价于 (.card *::after) 这种效果 */
}
```

可以看到，当我们使用这两种特殊选择器时，即使采用嵌套写法也是没有效果的，这是因为伪元素选择器的双冒号必须紧贴前置选择器，中间不能出现空格，否则意义就完全不一样了，所以这里我们需要用到一个特殊占位符。

在嵌套选择器中，`&` 符号是一个特殊的占位符，它代表**父选择器**本身。这在为父元素自身添加伪类、伪元素或组合选择器时尤其有用：

```css
.card {
  &::before { content: "sadadasda"; }
  &::after { content: "xXXX"; }
}
```

这样我们就可以准确表示是父元素的伪元素选择器或是伪类选择器了。

包括这种情况，也是需要使用`&`来明确表示是和父选择器链接的选择：

```css
.card.child { background: red }
```

```css
.card {
  &.child { background: red }
}
```

嵌套选择器的引入极大地提升了CSS代码的可读性和可维护性，让样式代码的组织方式更加模块化和组件化。

### UI设计系列（课程五）

本节重点是模拟企业官网布局，分析并抄一个类似的，锻炼CSS技术的最好办法就是抄别人的网站模拟布局，研究人家是如何进行配置的，本案例保存至网盘

苹果官网：https://www.apple.com.cn

## 本章练习

### 选择题

1. 关于标准盒模型（默认`content-box`）下列说法正确的是？

   A. width 包含内容、padding 和 border

   B. width 只包含内容区域

   C. width 包含内容和 padding

   D. width 包含内容和 margin

2. 如果一个元素设置了 `box-sizing: border-box;`，那么 width 表示的是

   A. 内容宽度

   B. 内容 + padding

   C. 内容 + padding + border

   D. 内容 + padding + border + margin

3. 下列哪种定位方式 不会脱离文档流？

   A. absolute

   B. fixed

   C. relative

   D. float

4. 关于 position: sticky，下列说法正确的是？

   A. 一开始就脱离文档流

   B. 固定后会脱离文档流

   C. 整个过程中都不脱离文档流

   D. 只要设置了 top 就会脱离文档流

5. 在 Flex 布局中，justify-content 默认作用的轴是？

   A. 交叉轴

   B. 块级轴

   C. 主轴

   D. 行内轴

6. 关于 ::before，以下说法正确的是？

   A. 不需要 content 也能显示

   B. 是 DOM 中真实存在的节点

   C. 默认是行内元素

   D. 只能用于 `div`

7. 下列哪个伪类表示 第一个子元素？

   A. :first-child

   B. :first-of-type

   C. :nth-child(1n)

   D. :only-child

### (蓝桥杯)  电影院排座位

随着⼈们⽣活⽔平的⽇益提升，电影院成为了越来越多的⼈休闲娱乐，周末放松的好去处。各个城市的 电影院数量也随着市场的需求逐年攀升。近⽇，⼜有⼀个电影院正在做着开张前期的准备，⼩蓝作为设计⼯程师，需要对电影院的座位进⾏布局设计。 本题需要在已提供的基础项⽬中，使⽤ CSS 完成⻚⾯中电影院座位布局。

题目地址：https://www.lanqiao.cn/problems/5133/learning

### (蓝桥杯) 新鲜的蔬菜

厨房里新到一批蔬菜，被凌乱地堆放在一起，现在我们给蔬菜分下类，把相同的蔬菜放到同一个菜板上，拿给厨师烹饪美味佳肴吧。

题目地址：https://www.lanqiao.cn/problems/2439/learning

### (蓝桥杯) 智能停车系统

蓝桥社区停车场正式对外开放，但由于停车位线标记不够完善，车主总是乱停乱放。为了使车辆有序的停放，现在给停车场绘制了醒目的停车位线。

**题目提示：** 设置子元素为（`flex-wrap`）换行 ，在交叉轴上使用（`align-content`）两端对齐分布，在主轴上使用（`justify-content`）两端对齐分布。两端对齐分布效果即均匀排列每个元素，首个元素放置于起点，末尾元素放置于终点



————————————————
版权声明：本文为柏码知识库版权所有，禁止一切未经授权的转载、发布、出售等行为，违者将被追究法律责任。
原文链接：https://www.itbaima.cn/zh-CN/document/ap5ixyomoejuw4ue