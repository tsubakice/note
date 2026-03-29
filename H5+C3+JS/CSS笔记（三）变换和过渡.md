![image-20251216143307451](https://s2.loli.net/2025/12/16/I4SePgJ8nlLGTQE.png)

# CSS3 变换和过渡

上一章我们介绍了盒子模型和布局相关知识点，并且为大家扩展了更多类型的选择器，帮助我们更好地调整页面样式。这一章我们将继续深入介绍CSS3的更多内容，比如二维、三维变换、函数等。

## 盒子模型进阶

上一章我们介绍了盒子模型，这一章我们接着介绍盒子模型中一些更加高级的用法。

### 最大宽度和最小宽度

在常规的`width`和`height`之外，CSS还提供了最大/最小宽度和高度的设置，这在响应式布局中非常有用，可以防止元素在不同尺寸的屏幕下变得过宽或过窄。

- `max-width`: 设置元素的最大宽度。当内容或浏览器窗口宽度小于`max-width`时，元素的宽度会自适应；但当其试图超过`max-width`时，宽度将被限制在这个最大值。
- `min-width`: 设置元素的最小宽度。即使内容很少，元素的宽度也不会低于这个值。
- `max-height`: 设置元素的最大高度。
- `min-height`: 设置元素的最小高度。

比如我们希望盒子的宽度始终保持为父元素宽度的80%，但是最大不能超过`500px`最小不能低于`320px`，此时我们就可以使用上面提到的几个属性：

```css
.container {
  width: 80%;         /* 宽度为父元素的80% */
  max-width: 500px;   /* 但最大不超过500px */
  min-width: 320px;   /* 最小不低于320px */
  margin: 0 auto;     /* 在大屏幕上水平居中 */
}
```

可以看到，当盒子的宽度超过设定的最大值时，就不会继续增加宽度了：

![image-20251216161343395](https://s2.loli.net/2025/12/16/lsTSopFJang4W79.png)

这种用法在很多时候都是非常实用的，我们的网站内容不可能跟随页面的宽度无限增加，否则其中的内容会变形得越来越奇怪，一般常见的网站都会设置一个最大的页面宽度来保证内容不会过度变形，一旦浏览器窗口超出最大限度，页面会自动居中并留出两边的空间：

![image-20251216161545744](https://s2.loli.net/2025/12/16/mU2qkRNzFvMpEG5.png)

注意，如果我们要设置盒子的固定大小，请正常使用之前的`width`和`height`属性，最大最小宽度虽然在某些情况下能够和普通的宽度效果一致，但是难免会存在特殊情况。

### 盒子轮廓

轮廓（outline）是绘制在元素`border`（边框）之外的一条线，用于突出元素。它与边框非常相似，但有几个关键区别：

1. **不占据空间**：轮廓是在元素之上绘制的，不会影响元素的尺寸或布局，它不是盒子模型的一部分，仅仅只是装饰，因此不会增加元素的总宽度或总高度。
2. **在盒子的边框外进行绘制**：轮廓是在盒子的边框外进行绘制，不会占用盒子内部的区域，但是可能会遮挡外部的其他元素。

我们可以使用`outline`属性来控制盒子的轮廓：

```css
.container {
  height: 100px;
  width: 200px;
  background-color: gray;
  outline-style: solid;  /* 轮廓采用实现绘制，可用的属性和border一样 */
  outline-width: 2px;  /* 创建一个宽度为2px的轮廓 */
  outline-color: black;  /* 边框的颜色 */
}
```

![image-20251216162232279](https://s2.loli.net/2025/12/16/p5Wrw1iSamHAVvO.png)

可以看到，此时盒子的边上就出现了一圈轮廓，看起来非常像边框，但是实际上并不是边框，盒子所占据的区域依然是上面设定的宽度。

同样的，如果盒子出现了圆角，那么轮廓也会按照盒子的实际形状展示：

![image-20251216162548623](https://s2.loli.net/2025/12/16/5JQufwrRxXUGF8j.png)

针对于以上三个属性，我们也可以使用边框的简写属性`outline`一步到位：

```css
outline: 2px solid black;
```

是不是感觉用起来和边框差不多，当然，轮廓也有一些比较特殊的属性，比如我们可以控制轮廓和盒子边缘的距离：

```css
.container {
  height: 100px;
  width: 200px;
  background-color: gray;
  outline: 2px solid black;
  outline-offset: 5px;  /* 表示边框距离盒子边缘的距离 */
  border-radius: 10px;
}
```

![image-20251216163005657](https://s2.loli.net/2025/12/16/uLUJQb1MW4DPEp6.png)

注意这个值可以是负数，负数表示轮廓往盒子里面跑：

![image-20251216163247421](https://s2.loli.net/2025/12/16/wfhLRvPgpoOtXBQ.png)

实际上我们在之前的学习中，就已经遇到过出现轮廓的情况了，比如`input`标签，在默认情况下，浏览器用户代理样式会为其添加一个蓝色的轮廓：

```html
<input type="text" placeholder="请输入文本">
```



![image-20251216163616691](https://s2.loli.net/2025/12/16/P5RBLTNDt8qCj9Q.png)

我们也可以通过前面学习的伪类来取消掉聚焦状态下的轮廓：

```css
input:focus {
  outline: none;
}
```

### 盒子阴影

使用 `box-shadow` 属性可以为元素的框添加阴影效果，这里需要用到几个属性：

```css
box-shadow: offset-x offset-y blur-radius spread-radius color;
```

这里我们分别介绍一下这些属性有什么用：

* **offset-x**  -  水平方向的偏移，默认情况下阴影在盒子的正下方。
* **offset-y**  -  垂直方向的偏移，默认情况下阴影在盒子的正下方。
* **(可选) blur-radius**  -  模糊半径，值越大，阴影越模糊。
* **(可选) spread-radius**  -  扩展半径，正值使阴影扩大，负值使阴影缩小，不填等于没有阴影。
* **color**  -  阴影的颜色。

比如，下面这个例子就是：

```css
.container {
  width: 200px;
  height: 100px;
  background-color: #d5d5d5;
  box-shadow: 0 0 10px gray;   /* 这里我们创建了一个模糊半径为10px的灰色阴影 */
}
```

这里填写 `0 0 10px gray` 表示创建一个水平和垂直方向上都不偏移，扩展半径为`10px`的灰色阴影：

![image-20251216171032129](https://s2.loli.net/2025/12/16/7Al9WIgGndxB14f.png)

注意，阴影和前面介绍的轮廓一样，不会占用盒子空间，但是可能会受到父盒子尺寸的影响，比如下面这种情况：

```css
.outer-box {
  width: 220px;
  height: 120px;
  overflow: hidden;  /* 隐藏超出部分 */
}

.container {
  width: 200px;
  height: 100px;
  background-color: #d5d5d5;
  box-shadow: 0 0 50px gray;   /* 这里我们创建了一个模糊半径为50px的灰色阴影 */
}
```

![image-20251216172041282](https://s2.loli.net/2025/12/16/uQT1DIlXZiY9aNf.png)

当外层盒子的尺寸大小不足以容纳阴影且禁止超出部分显示时，我们的阴影会被截断，然后变得很难看。

当然，除了编写一个阴影之外，我们还可以同时给盒子添加多个阴影：

```css
.container {
  width: 200px;
  height: 100px;
  background-color: #d5d5d5;
  box-shadow:   /* 多个阴影可以逐行编写，但是注意需要用逗号分割 */
      0 -4px 3px rgba(0, 42, 255, 0.12),
      0 4px 8px rgba(255, 0, 0, 0.2);
}
```

此外，阴影既然可以作为外阴影展示，同样的也可以在内侧展示，我们只需要添加一个`inset`即可表示这是一个内阴影：

```css
.container {
  width: 200px;
  height: 100px;
  background-color: #d5d5d5;
  box-shadow: 0 0 10px gray inset;   /* 这里我们创建了一个模糊半径为10px的灰色内侧阴影 */
}
```

![image-20251216172531895](https://s2.loli.net/2025/12/16/2R3MoAik18mCz4c.png)

阴影和前面介绍的轮廓一样，是跟随盒子的形状进行变化的：

![image-20251216173915213](https://s2.loli.net/2025/12/16/ZNpsI6lKVvfcOkM.png)

除了将阴影作为一个普通的阴影使用之外，我们也可以使用阴影来解决之前遇到的边框宽度被系统缩放影响的问题。

> 不同系统的 DPI / 缩放比例，会导致 CSS 的“1px”并不总是等于屏幕上的 1 个物理像素。

```css
.box {
  box-shadow: 0 0 0 1px #ccc;
}
```

这里我们只需要创建一个扩展半径为`1px`且无模糊半径的阴影即可，它实际的展示效果等价于宽度为`1px`的边框。

除了盒子可以创建阴影之外，还有一个`text-shadow`属性，它能实现文字的阴影效果：

![image-20251217152453018](https://s2.loli.net/2025/12/17/fyjsRIN8a39dQ4H.png)

可以看到它是在文字的周围增加描边阴影，用法和盒子阴影大致相同。

### 行内纵向对齐

经过HTML阶段的学习，我们知道，行内元素是可以任意穿插的，我们直接编写的文本内容、`a`标签、`b`标签等都算行内元素。包括一些既具有行内元素特性也具有块级元素特性的比如`img`标签，也能和文本实现穿插效果：

```html
<div class="test">
  带回家撒大家卡仕达手机卡三等奖哈三等奖卡仕达酱卡好的卡仕达可接受的
  <a href="index.html">点我啊</a>
  <img width="120" src="https://img1.baidu.com/it/u=1005000719,311741344&fm=253&fmt=auto&app=138&f=JPEG">
</div>
```

![image-20250823180045333](https://s2.loli.net/2025/08/23/huVzcW2K7wg936O.png)

不过虽然行内元素和行内块元素都可以穿插使用，但是有些时候可能并不是我们想要的效果。比如这里的图片，它展示的位置似乎并不是以文字的底部进行对齐的，看着像是有点漂移的感觉。

这实际上是因为`vertical-align`属性在控制纵向对齐规则：

```css
.test img {
  vertical-align: baseline;  /* 默认情况下，行内元素的纵向对齐是baseline */
}
```

那么什么是`baseline`呢？这里需要介绍一下`vertical-align`的几种值：

![image-20250823193100171](https://s2.loli.net/2025/08/23/RpHlVkQWg94hujT.png)

这里出现了四种对齐方式，分别对应不同的标准线：

* **顶线：**这一行文本行高的顶部，注意不是文字顶部，而是行高的顶部，随行高变化而变化。当行高和文字大小一致时，才是中文文字的顶部（英文文字会小一些）
* **中线：**文字的垂直中心点，也就是小写字母`x`的中心交叉位置。
* **基线：**英文小写`x`字母的下边缘，设置基线是为了给注入`g`、`j`这种带尾巴的字母留出空间。
* **底线：**同顶线，只不过相反，跑到行高的底部去了。

当我们修改行内或行内块元素的`vertical-align`属性时，会产生如下效果：

* `baseline` - 让行内或行内块元素的底线和文本的基线对齐。
* `top` - 让行内或行内块元素的顶线和文本的顶线对齐。
* `bottom` - 让行内或行内块元素的底线和文本的底线对齐。
* `middle` - 让行内或行内块元素的中线和文本的中线对齐。

注意`vertical-align`只针对于行内元素或者行内块元素有效，它对于块级元素是无效的。

### 精灵图

精灵图（也称“雪碧图”）是一种网页图片应用处理方式。它将一个页面涉及到的所有零星图片都包含到一张大图中，然后利用 CSS 的 `background-image`、`background-position` 和 `width`、`height` 属性来显示大图中的特定部分。

> **优点**：通过将多个图片合并成一张，减少了HTTP请求的数量，从而加快了页面加载速度。

这种用法在计算机发展的早期，很多网页、游戏都喜欢采用，比如早期的微博：

![image-20251216175230198](https://s2.loli.net/2025/12/16/TjPdUbwgnAeVOIh.png)

可以看到所有的小图标都被做到了一张图中，这样就可以反复使用同一个背景图，通过定位来选取自己需要的区域。

```css
.vip-icon {
  display: inline-block;
  width: 40px;
  height: 40px;
  background-position: -57px 0;
  background-image: url("/img/sprites.png");
}
```

![image-20251216175951128](https://s2.loli.net/2025/12/16/Sz14ZVTA2O6J8Ri.png)

如果网站存在大量的小型图标需要展示，也可以考虑使用这种方式去节省网络资源。

### 颜色渐变

CSS3 渐变 (gradients) 让你可以在两个或多个指定颜色之间平稳地过渡，实现渐变色效果。渐变色需要使用 `background-image` 属性来进行设置，列举一些常用的颜色渐变函数：

* **线性渐变 (Linear Gradients)**：颜色沿着一条直线过渡。
* **径向渐变 (Radial Gradients)**：颜色从一个中心点向外辐射过渡。

这里我们先来看看最简单的线性渐变：

```css
.container {
  width: 300px;
  height: 50px;
  background-image: linear-gradient(red, yellow);  /* 使用红色和黄色进行线性渐变 */
}
```

默认情况下，颜色渐变方向是从上往下进行渐变：

![image-20251216183331367](https://s2.loli.net/2025/12/16/6fhDNOjGtmsY7Bx.png)

我们可以添加`to right`表示采用从左往右的线性渐变：

```css
.container {
  width: 300px;
  height: 50px;
  background-image: linear-gradient(to right, red, yellow);
}
```

![image-20251216183411565](https://s2.loli.net/2025/12/16/qhKHGZs4TyLx2du.png)

同样的，`to bottom right`还能实现从左上角斜着渐变到右下角的效果，所有的情况如下：

| **写法**        | **含义**        |
| --------------- | --------------- |
| to bottom       | 上 → 下（默认） |
| to top          | 下 → 上         |
| to right        | 左 → 右         |
| to left         | 右 → 左         |
| to right bottom | 左上 → 右下     |

针对于自定义的角度，我们也可以使用角度单位来定义：

```css
linear-gradient(45deg, red, blue)
```

我们可以添加多种颜色让渐变看起来更丰富：

```css
background-image: linear-gradient(to right, red, orange, yellow, green, blue, purple);
```

![image-20251216184848190](https://s2.loli.net/2025/12/16/7FVE3XcMQrSGDwv.png)

我们可以自行控制不同颜色占有的比例，比如我们希望红色部分更多一些再进行黄色渐变，我们可以在颜色的后面添加一个百分比，表示颜色延伸的位置：

```css
background-image: linear-gradient(to right, red 80%, yellow);
```

![image-20251216183729148](https://s2.loli.net/2025/12/16/TypQvREbtWgxKuU.png)

或是黄色多一些：

```css
background-image: linear-gradient(to right, red, yellow 20%, yellow);
```

![image-20251216184630457](https://s2.loli.net/2025/12/16/sSx4aCMKvPgFhmk.png)

虽然颜色渐变使用的也是`background-image`属性，但是这并不代表我们就无法使用图片作为背景了，我们可以使用逗号分割不同类型的背景图片：

```css
background:
  linear-gradient(rgba(0,0,0,.4), rgba(0,0,0,.4)),
  url(bg.jpg);
```

此外，我们在上一章没有提到，`background-clip`还支持在盒子模型的指定区域内展示背景图片或渐变效果，比如我们只希望在盒子的内容区域展示渐变颜色，而不作用到边框和内边距上：

```css
.container {
  width: 300px;
  height: 50px;
  padding: 20px;
  background:
      linear-gradient(to right, red, blue) content-box;  /* 表示只有内容盒子变色 */
  border: 2px solid gray;
}
```

![image-20251216185600589](https://s2.loli.net/2025/12/16/iGgVfUekvQTlF8m.png)

此时盒子只有内容区域是渐变色，当然，除了`content-box`之外，还有`padding-box`表示只延伸到内边距区域，或是`border-box`表示完整的整个盒子，利用这种特性，我们就能实现彩色边框：

```css
.container {
  width: 300px;
  height: 50px;
  border-radius: 10px;
  background:
      linear-gradient(to right, white, white) padding-box,  /* pading+内容区域采用白色 */
      linear-gradient(to right, red, blue) border-box;  /* border+pading+内容区域采用渐变 */
  border: 2px solid transparent;  /* 注意边框的颜色会覆盖背景，这里弄个透明的 */
}
```

![image-20251216190415834](https://s2.loli.net/2025/12/16/4TKZo5q6QfcJXil.png)

这样，一个好看的渐变色边框就出现了。

我们接着来看径向渐变，径向渐变是由中心向外发散的渐变效果，比较适合球形盒子：

```css
.container {
  width: 200px;
  height: 100px;
  background-image: radial-gradient(red, yellow);
}
```

![image-20251216190837253](https://s2.loli.net/2025/12/16/bJVLvqxHAYnmGoz.png)

可以看到，径向渐变的颜色是有内而外，并且会以椭圆形样式适应盒子的宽高，我们还可以将渐变背景颜色直接改变为一个标准的圆形：

```css
background-image: radial-gradient(circle, red, yellow);
```

![image-20251216191039853](https://s2.loli.net/2025/12/16/rJ4L3oaduMliecC.png)

可以看到，现在渐变变成了标准圆形，但是由于我们的盒子并非一个正方形，导致这个渐变的圆形有一部分像是被遮挡一样，我们可以通过以下参数来调整扩散范围：

| **值**          | **含义**         |
| --------------- | ---------------- |
| closest-side    | 到最近边         |
| farthest-side   | 到最远边         |
| closest-corner  | 到最近角         |
| farthest-corner | 到最远角（默认） |

比如我们希望颜色渐变以最矮的一边为准：

```css
background-image: radial-gradient(circle closest-side, red, yellow);
```

![image-20251216191207753](https://s2.loli.net/2025/12/16/S1Q7N4IGZCnRgJe.png)

和线性渐变一样，我们可以使用百分比来控制渐变位置：

```css
background-image: radial-gradient(red 5%, yellow 15%, green 60%);
```

![image-20251216191420423](https://s2.loli.net/2025/12/16/hb7iwofUumDjtAz.png)

我们也可以通过`at`来改变圆的中心点位置，比如我们希望从左上角开始径向渐变：

```css
background-image: radial-gradient(circle at left top, red, yellow);
```

![image-20251216191539862](https://s2.loli.net/2025/12/16/3L8GJVBS2O7CxFr.png)

常见位置写法：

- center
- top / bottom / left / right
- x y（百分比或长度）

我们可以自由编写指定的百分比来控制位置，或是固定位置。

### 过滤

`filter` 是 CSS 中**用于给元素添加图形处理效果**的属性，常见于图片、背景、组件状态、UI 细节增强等场景。你可以把它理解为：**在浏览器里对元素做 “PS 后期处理”**。

比如现在有一个盒子：

```html
<div class="container">
  我是盒子里面的字，看着很漂亮很美丽
</div>
```

```css
.container {
  color: #227fd8;
  width: 200px;
  height: 100px;
  padding: 10px 15px;
  border-radius: 10px;
  border: 2px solid #5aa9fd;
}
```

![image-20251216192402204](https://s2.loli.net/2025/12/16/cpmDUA6sqBkg9oV.png)

我们可以使用多种滤镜效果，来对盒子进行处理，比如高斯模糊效果，我们可以使用`blur`效果，它会使得我们的内容变模糊，我们可以自行调整模糊的强度：

```css
.container {
  filter: blur(5px);   /* 添加高斯模糊滤镜 */
  ...
}
```

![image-20251216192351353](https://s2.loli.net/2025/12/16/IHZC8DOcuWenv37.png)

添加滤镜效果后，相信大家第一眼看到肯定分不清楚到底是自己近视了还是盒子问题。

我们接着来看下一个滤镜，`brightness`可以控制亮度，亮度越大，盒子的颜色就会变得越亮，亮度越小，盒子的颜色就会变得越灰暗：

```css
.container {
  filter: brightness(0.5);
  ...
}
```

![image-20251217145321293](https://s2.loli.net/2025/12/17/yQcjArZYPMmWn89.png)

默认情况下，亮度为`1`，当我们调整亮度为`0.5`之后，盒子颜色会变得昏暗，就像是把蓝色调的更黑一样。

我们接着来看下一个，`contrast`可以调节对比度，对比度越强，颜色展现就越鲜艳，反之颜色就越灰：

![image-20251217145904706](https://s2.loli.net/2025/12/17/GxW52A4zgsyRwCI.png)

在一些特殊日子，常常有些网站喜欢把所有内容都调整为灰白色，以纪念历史上的今天，我们可以使用`grayscale`属性来控制页面的灰度效果，当调整为`100%`表示完全变为黑白效果：

![image-20251217150128544](https://s2.loli.net/2025/12/17/kf21JjhEwAKgtCT.png)

类似的还有`invert`滤镜，它可以使得盒子变为相反的颜色，`sepia`会为盒子添加复古棕色效果，比较适合一些老照片，柏码官网还用到了`hue-rotate`效果，它可以将某种颜色整体变换为另一种颜色，以实现多种主题的切换。

此外，`drop-shadow`滤镜可以为盒子增加投影效果，这个与我们之前提到的`box-shadow`类似，但是它会根据盒子的具体绘制内容进行投影，而`box-shadow`只会按照盒子形状进行投影，我们可以对比一下两者：

```css
.container {
  /* 参数和box-shadow基本一致 */
  filter: drop-shadow(0 0 4px gray);
  color: #227fd8;
  width: 200px;
  height: 100px;
  padding: 10px 15px;
  border-radius: 10px;
  border: 2px solid #5aa9fd;
}
```

![image-20251217151723567](https://s2.loli.net/2025/12/17/rhgMyYBGFzfnPN1.png)

![image-20251217151742426](https://s2.loli.net/2025/12/17/uNFdaWTY3cPrjI9.png)

可以看到，普通的盒子阴影只能针对于盒子的边框产生阴影效果，而滤镜则是更细致化的，针对于所有内容的阴影效果，只要是透明背景的区域，都会产生阴影。

以上介绍的滤镜并非只能使用其中一种，我们也可以将多种滤镜组合起来使用，实现更加高级的效果。

有关更多滤镜效果，请参阅：https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference/Properties/filter

### 背景过滤器

backdrop-filter 是 **CSS 中用于“模糊/处理元素背后内容”** 的属性，常见于毛玻璃、半透明浮层、弹窗背景等现代 UI 设计中。它与前面介绍的`filter`效果差不多，不过，它是作用于其之后的元素，而不是盒子本身。

我们可以来尝试一下：

```html
<div class="card">我是卡片里面的文本</div>
```

```css
body {
  background-repeat: no-repeat;
  background-size: cover;
  background-image: url("https://img0.baidu.com/it/u=4012604816,607436490&fm=253&app=138&f=JPEG?w=889&h=500");
}

.card {
  width: 150px;
  height: 200px;
  padding: 10px 15px;
  border-radius: 15px;
  background-color: #ffffff60;
  backdrop-filter: blur(12px);  /* 添加高斯模糊效果 */
}
```

![image-20251218212937399](https://s2.loli.net/2025/12/18/YZsvfiQlTmre3Ah.png)

可以看到，此时被盒子遮挡的背景部分，变成了高斯模糊效果。同样的，我们也可以试试看其他滤镜效果，比如灰度：

![image-20251218213136272](https://s2.loli.net/2025/12/18/j51Ebcv9JsWuU7k.png)

backdrop-filter 支持的函数和 filter 基本一致，这里各位可以自行尝试。

## 二维和三维变换

在 CSS3 中，`transform` 属性允许我们在不改变文档流的情况下，对元素进行移动、旋转、缩放和倾斜。这不仅能改变元素的外观，配合过渡和动画还能制作出丰富的交互效果。这一部分，我们就来研究一下`transform`的使用。

### 二维变换

二维变换（2D Transform）是指在 X 轴和 Y 轴组成的平面上进行的变换。所有的二维变换都是通过 `transform` 属性来实现的。

使用 `translate()` 函数可以将元素从当前位置沿着 X 轴和 Y 轴移动：

```html
<div>
  <div class="box"></div>
  <div>熊大，坐直升机的感觉可真舒服，就跟牢大一样</div>
</div>
```

```css
.box {
  width: 100px;
  height: 100px;
  background-color: red;
  /* 表示横向移动10px，纵向移动20px */
  transform: translate(10px, 20px);
}
```

![image-20251217154553183](https://s2.loli.net/2025/12/17/m68aJiutcvSdkKB.png)

可以看到，盒子在平面内发生了位置偏移，遮挡住了后续的文本，并且值得注意的是，盒子原本所占据的空间并没有发生改变，这与我们前面提到的`relative`定位布局类似，它们都能实现偏移又不脱离文档流效果。但是，`translate`的不同之处在于，它仅仅是视觉上的偏移，并非盒子本体的移动。两种情况下，使用JavaScript获取其位置时，会出现差异：

![image-20251217161158891](https://s2.loli.net/2025/12/17/xz2NTuEroReS7MX.png)

`relative`定位相当于盒子自己发生了移动但又留着原来的位置，而`translate`则是盒子依然在原地，只是我们自己看起来像是被移动了而已。因此，`translate`更适合我们后面要介绍的动画功能，因为它仅仅是改变盒子渲染的位置，而非真正移动了盒子，我们后续介绍的所有变换效果实际上都是视觉变换，而非真正作用于盒子上。

除了一次性控制盒子在横向或纵向上的移动，我们也可以单独使用：

``` css
transform: translateX(10px);
```

我们接着来看下一个，使用 `scale()` 函数可以放大或缩小元素，同样的，它也是视觉效果，并非真的放大了盒子：

```css
.box {
  width: 100px;
  height: 100px;
  background-color: red;
  transform: scale(1.5);   /* 变为原本1.5倍的大小 */
}
```

![image-20251217161751543](https://s2.loli.net/2025/12/17/UD2G4XPiu7Zyx3K.png)

可以看到，盒子以自己的中心位置进行放大，当然，我们也可以分别控制横向或纵向的缩放比例：

```css
transform: scale(1.5, 2);
```

使用 `rotate()` 函数可以让元素围绕中心点进行旋转：

![image-20251217162349991](https://s2.loli.net/2025/12/17/mstJ9qMdnNuBCSk.png)

`skew()` 函数可以让元素产生倾斜变形效果，看起来像是一个平行四边形：

![image-20251217162732593](https://s2.loli.net/2025/12/17/fk765NGojqhABD4.png)

通过这些三维变换，我们就可以实现丰富多彩的效果，结合后续的动画和过度效果，就能让网页灵动起来。

### 原点变换

默认情况下，所有的变换（特别是旋转和缩放）都是围绕元素的**中心点**（`center center`）进行的。我们可以通过 `transform-origin` 属性来改变这个基点。

```css
.box {
  /* 将旋转中心改为左上角 */
  transform-origin: left top;
  transform: rotate(45deg);
}
```

这时候，元素就会像挂在墙上的画框歪了一样，围绕左上角的钉子旋转：

![image-20251217163233941](https://s2.loli.net/2025/12/17/YTOFQwpfCs9lU7H.png)

### 组合变换

如果我们需要同时对元素进行平移、旋转和缩放，可以将这些函数写在同一个 `transform` 属性中，用空格隔开。

```css
.box {
  /* 先移动，再旋转，最后缩放 */
  transform: translate(100px) rotate(45deg) scale(1.2);
}
```

**注意**：变换的顺序很重要！先旋转再平移，和先平移再旋转，最终的位置可能完全不同，因为旋转会改变坐标轴的方向。

比如我们这里先进行平移再旋转：

![image-20251217211928289](https://s2.loli.net/2025/12/17/gd3JiLzwyrkZFEP.png)

但是先选择再平移：

![image-20251217211949524](https://s2.loli.net/2025/12/17/WRHOEiMwF3XQK5q.png)

原因也很简单，因为先进行了旋转操作，盒子的的移动方向也会跟着旋转，导致盒子斜着平移。

### 三维变换

三维变换（3D Transform）在二维的基础上引入了 **Z 轴** 的概念。Z 轴是垂直于屏幕向外的轴，通过它我们可以实现物体“近大远小”的立体效果。

除了平面旋转 `rotate()`（实际上是围绕 Z 轴旋转）我们现在可以围绕 X 轴和 Y 轴旋转：

```html
<div class="inner-box"></div>
```

```css
.inner-box {
  width: 100px;
  height: 100px;
  background-color: red;
  transform: rotateY(80deg);
}
```

当盒子围绕Y轴旋转时，随着角度的变化，盒子的宽度会变细再变宽，各位小伙伴可以拿一张A4纸模拟一下，让纸张以垂直方向为轴进行旋转，只从正面去观察是否和浏览器上的盒子选择一致：

![image-20251217215138442](https://s2.loli.net/2025/12/17/bgWOhYuMdqxH9Xz.png)

在开发者工具中，我们可以直接通过鼠标控制旋转变换的旋转角度，快速进行旋转观察。

可能会有小伙伴说，这样看着好像没有那种立体的感觉，只有加上了景深，才能看到立体的翻转效果，否则`rotateY` 看起来只是单纯的变窄了。想要在平面屏幕上看到 3D 效果，首先需要开启“景深”功能，`perspective` 属性定义了观察者（人眼）距离屏幕的距离（视距）**视距越小，景深效果越强烈（近大远小越明显）视距越大，越接近平面效果。**

通常我们将 `perspective` 加在**父元素**（舞台）上，这样，父元素就扩展了一个Z轴方向上的空间出来，其中的子元素就有3D变换的空间了：

```html
<div class="outer-box">
  <div class="inner-box"></div>
</div>
```

```css
.outer-box {
  width: 300px;
  height: 300px;
  border: 1px solid gray;
  perspective: 500px;

  .inner-box {
    width: 100px;
    height: 100px;
    background-color: red;
    transform: rotateY(80deg);
  }
}
```

![image-20251217215505138](https://s2.loli.net/2025/12/17/bAylBtqG6rZTcIu.png)

景深效果会为盒子的观察视角添加一定的倾斜角度，此时盒子旋转看起来就有一点点的3D效果了。

同样的，针对于平移效果，`translateZ()` 允许元素沿着 Z 轴移动：

- **正值**：向屏幕外移动，离眼睛更近，元素看起来**更大**。
- **负值**：向屏幕内移动，离眼睛更远，元素看起来**更小**。

![image-20251217220515718](https://s2.loli.net/2025/12/17/WNeA1uBlmSD5rdj.png)

注意必须在添加景深效果时盒子的Z轴移动才会有效果，否则无论怎么移动都看不出来效果。

不过，当一个元素进行了 3D 变换，并且它内部还有子元素也进行了 3D 变换时，默认情况下，子元素会“扁平化”贴在父元素的平面上，失去自己的 3D 空间关系：

```html
<div class="outer-box">
  <div class="inner-box">
    <div class="inner-box-2"></div>
  </div>
</div>
```

```css
.outer-box {
  width: 300px;
  height: 300px;
  border: 1px solid gray;
  perspective: 500px;

  .inner-box {
    width: 100px;
    height: 100px;
    background-color: rgba(255, 0, 0, 0.8);
    transform: rotateX(45deg);

    .inner-box-2 {
      width: 50px;
      height: 50px;
      background-color: #0080ff;
      transform: rotateY(45deg);
    }
  }
}
```

![image-20251217221656904](https://s2.loli.net/2025/12/17/645vub8AIlnUcpt.png)

可以看到，里面的蓝色盒子又变成2D变换效果了，3D效果消失了，为了让子元素保持 3D 立体空间，我们需要在父元素上设置 `transform-style`属性：

```css
transform-style: preserve-3d;
```

这个属性在处理复杂模型3D效果时极为有用。

### 透明效果

在网页设计中，透明效果非常常用，比如模态框背后的半透明遮罩、悬浮时变淡的按钮等。CSS 提供了多种方式来实现透明效果，最常见的有两种：`opacity` 属性和 Alpha 通道颜色（如 `rgba`）其中`rgba`我们在之前的学习中已经使用过，这里我们来介绍一下新朋友：`opacity`，此属性用于设置整个元素的透明度。

```css
.box {
  background-color: red;
  opacity: 0.5;   /* 设置 50% 透明度 */
}
```

这样，我们的盒子就会自动变成半透明状态（包括盒子的背景、文本、以及所有的内容）它会影响盒子及其子元素的整体展示效果，全部变为透明。

### 过渡效果

在没有使用过渡属性之前，元素的样式变化是瞬间完成的。比如我们设置一个按钮在鼠标悬停时背景色变红，它会“突变”成红色。而 CSS3 的 `transition` 属性允许我们在两个样式状态之间建立一个平滑的过渡动画，让页面交互看起来更加自然和高级。比如：

```html
<div class="simple-box"></div>
```

```css
.simple-box {
  width: 200px;
  height: 200px;
  background-color: dodgerblue;
  
  &:hover { background-color: rebeccapurple; }
}
```

要实现过渡效果，我们需要定义两个要素，首先是需要针对于什么属性进行过度，其次是过度需要持续多久，接着我们就可以在设置下面的CSS属性了：

```css
/* 设置需要过度的属性 */
transition-property: background-color;
/* 过度动画的持续时间 */
transition-duration: .3s;
```

此时我们再将鼠标放到盒子上，就会产生背景颜色渐变的过度效果了。

同样的，比如我们希望当鼠标移动到按钮上时自动放大尺寸，产生灵动效果：

```css
button {
  color: white;
  padding: 6px 12px;
  border-radius: 6px;
  border: none;
  background-color: rebeccapurple;
  /* 可以直接在简写属性中一步到位 */
  transition: transform 0.2s;

  &:hover {
    transform: scale(1.05);
  }
}
```

此时我们将鼠标移动到按钮上时，就会产生放大过渡效果。

除了这种简单的过渡效果外，我们还可以进一步控制过渡的速度曲线，比如我们希望过渡效果一开始慢，然后逐步变快，就可以使用`transition-timing-function`属性来控制：

- `linear`: 匀速（从头到尾速度一样）。
- `ease`: （默认）慢速开始，然后变快，最后慢速结束。
- `ease-in`: 慢速开始。
- `ease-out`: 慢速结束。
- `ease-in-out`: 慢速开始和结束。
- `steps(n)`: 分步执行，像逐帧动画一样。

```css
button {
  ...
  transition: transform 0.2s ease-in;  /* ease-in表示缓慢进入逐步变快 */

  &:hover {
    transform: translateX(20px);
  }
}
```

此外，我们还可以使用自定义速度曲线来定义更多过渡效果：

```css
/* 起步稍微慢一点（斜率低），中间很快（斜率高），然后减速缓冲（斜率低），最后加速 */
transition: transform 0.2s cubic-bezier(0.25, 0.8, 0.25, 1);
```

当然，速度可以调整为负数，表示回放动画，从而实现回弹效果，就像果冻一样：

```css
transition: transform 0.2s cubic-bezier(0.68, -0.55, 0.27, 1.55);
```

有些时候，我们可能还希望过渡动画存在一定的延迟，如果延迟时间结束依然满足过渡效果的条件（比如鼠标指针依然停留在按钮上）此时可以使用`transition-delay`属性来控制：

```css
button {
  ...
  transition: transform 0.2s 1s ease-in;

  &:hover {
    transform: translateX(20px);
  }
}
```

这样，当我们鼠标放到盒子上时，会等待1秒之后再变从初始状态过渡到`:hover`定义的CSS效果。

虽然渐变效果很强大，但是有些地方存在坑点，比如高度就是一个非常难受的点，比如我们希望实现在默认情况下，盒子处于折叠状态，内容暂时被遮挡，当鼠标移动到盒子上时，再调整高度完整展示全部内容：

```css
.test-box {
  width: 320px;
  height: 20px;
  padding: 5px 10px;
  border-radius: 10px;
  overflow: hidden;
  border: 2px solid dodgerblue;
  /* 为高度添加过渡效果 */
  transition: height 0.3s ease-in-out;

  &:hover { height: auto; }  /* 鼠标经过时，高度按照内容来决定 */
}
```

可以看到，子盒子的高度变化确实会影响父盒子的高度，但是父盒子即使设置了高度的过渡属性，也是没有任何效果的。这是一个非常经典的坑，很多小伙伴会觉得，盒子高度变化了那不就应该触发`height`属性的过渡吗，为什么这里变化了也没用呢？

> 根本原因是**浏览器无法计算“不确定值”的中间状态**，简单来说，当您告诉浏览器“在 1秒内 从 `0px` 变到 `100px`”时，浏览器是非常开心的，因为这是一道简单的数学题。但是，当您说 “从 `0px` 变到 `auto`” 时，浏览器就懵了，浏览器必须先计算布局（Reflow）才能知道 `auto` 到底等于多少像素。而在过渡开始的那一瞬间，浏览器还不知道终点在哪里，自然也就无法计算中间该怎么走。

虽然这种情况确实很难处理，但是我们可以曲线救国，其中一个比较经典的做法就是使用`max-height`来解决问题，我们可以直接将初始高度设置为`max-height`属性，然后当鼠标经过时，给一个非常大的值（一定能装下内容的高度）这样，浏览器得到最大高度就能准确预测过渡终点，我们就可以实现过度效果了：

```css
.test-box {
  ...
  max-height: 20px;
  transition: max-height 0.3s ease-in-out;

  &:hover { max-height: 999px; }
}
```

这样就可以解决自适应高度过渡动画的问题了。

### UI设计系列（课程六）

本节讲解如何利用阴影编写漂亮的按钮：

![image-20251217234545216](https://s2.loli.net/2025/12/17/4Iizd6NwQUWVlJG.png)

使用同色阴影有时候能够给人一种玻璃折射光线的感觉，会使得按钮看起来更轻更通透，当然，不仅仅是按钮，实际上页面上很多的一些卡片、选择框等都可以采用这种风格，看起来就会很高级。

```html
<div style="display: flex; gap: 20px">
  <button class="btn-1">点击购买</button>
  <button class="btn-2">点击购买</button>
  <button class="btn-3">点击购买</button>
  <button class="btn-4">点击购买</button>
</div>
```

```css
button {
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: transform 0.3s ease, box-shadow 0.5s ease;

  &:hover {
    transform: scale(1.05);
  }
}

.btn-1 {
  background-color: dodgerblue;
  box-shadow: 0 0 10px rgba(0, 130, 255, 0.5);

  &:hover { box-shadow: 0 0 15px rgba(0, 130, 255, 0.8); }
}

.btn-2 {
  background-color: #ac1eff;
  box-shadow: 0 0 10px rgba(123, 0, 255, 0.5);

  &:hover { box-shadow: 0 0 15px rgba(153, 0, 255, 0.8); }
}

.btn-3 {
  background-color: #18af2c;
  box-shadow: 0 0 10px rgba(34, 255, 0, 0.5);

  &:hover { box-shadow: 0 0 15px rgba(17, 255, 0, 0.8); }
}

.btn-4 {
  background-color: #f88a19;
  box-shadow: 0 0 10px rgba(255, 136, 0, 0.5);

  &:hover { box-shadow: 0 0 15px rgba(255, 106, 0, 0.8); }
}
```

### UI设计系列（课程七）

本节我们将利用刚刚学到的 **3D 变换** 和 **过渡效果**，制作一个现代网站中非常常见的组件 —— **3D 翻转卡片**。

这个效果的核心原理在于：我们需要两个“面”（正面和背面），它们重叠在一起，背面的初始状态是旋转了 180 度的。当鼠标悬停时，我们将整个容器旋转 180 度，这样正面转到了后面，背面转到了前面。

在开始之前，我们需要补充一个专门配合 3D 旋转使用的属性：`backface-visibility`。

默认情况下，当我们把一个盒子旋转 180 度背对着屏幕时，我们看到的是这个盒子正面的“镜像”或者说是它的背面。但是，如果我们设置了 `backface-visibility: hidden`，那么当盒子背对屏幕时，它就会变得不可见（透明）。利用这个特性，我们就可以把两个盒子背对背贴在一起。

我们需要三层结构：

1. **外部容器 (`flip-card`)**：提供透视效果（景深）。
2. **内部容器 (`flip-card-inner`)**：这是实际进行旋转的物体，包含正面和背面。
3. **正面和背面 (`flip-card-front`, `flip-card-back`)**：实际展示的内容。

```html
<div class="flip-card">
  <div class="flip-card-inner">
    <!-- 正面 -->
    <div class="flip-card-front">
      <h2>前端开发</h2>
      <p>HTML & CSS</p>
    </div>
    <!-- 背面 -->
    <div class="flip-card-back">
      <h2>课程详情</h2>
      <p>那么这一时刻又有老铁问了，羊哥羊哥，你嘚技术，到底是怎么练的。</p>
      <button>立即学习</button>
    </div>
  </div>
</div>
```

```css
/* 1. 最外层容器：定义尺寸和景深 */
.flip-card {
  background-color: transparent;
  width: 300px;
  height: 200px;
  /* 开启 3D 空间感，数值越小立体感越强 */
  perspective: 1000px; 
}

/* 2. 内部容器：承载正面和背面，负责旋转 */
.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  /* 添加过渡效果，让翻转动作平滑进行 */
  transition: transform 0.6s;
  /* 关键属性：保留子元素的 3D 空间位置，否则子元素会变扁平 */
  transform-style: preserve-3d;
}

/* 3. 鼠标悬停交互：旋转内部容器 */
.flip-card:hover .flip-card-inner {
  transform: rotateY(180deg);
}

/* 4. 正面和背面的公共样式 */
.flip-card-front, .flip-card-back {
  position: absolute; /* 绝对定位，让正反面重叠在一起 */
  width: 100%;
  height: 100%;
  /* 关键属性：隐藏背面。当面朝后时不可见 */
  backface-visibility: hidden;
  
  /* 一些装饰样式 */
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}

/* 5. 正面特有样式 */
.flip-card-front {
  background: linear-gradient(to right, #4facfe, #00f2fe);
  color: white;
}

/* 6. 背面特有样式 */
.flip-card-back {
  background-color: #2980b9;
  color: white;
  /* 重点：背面初始状态必须先旋转 180 度 */
  /* 这样当父容器旋转 180 度时，背面正好转回来变成 0 度（正面朝上） */
  transform: rotateY(180deg);
}

.flip-card-back button {
  margin-top: 10px;
  padding: 5px 15px;
  background: white;
  color: #2980b9;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
```

通过这种巧妙的组合，我们就实现了一个平滑、具有原生 App 质感的 3D 翻转卡片。

## 函数和变量

**CSS 函数（CSS Functions）**，指的是在 CSS 属性值中使用的一种 **“带参数的表达式”**，用来完成**计算、取值、变换、颜色处理、图像生成**等工作。

实际上，我们在之前的课程中已经使用过多种函数了，比如颜色函数（如 `rgb`、`rgba`、`linear-gradient`）和变换函数（如 `rotate`、`translate`）以及滤镜函数（如 `blur`、`grayscale`），通过使用这些函数，我们就可以更加灵活地配置CSS参数值。在CSS中，函数的统一形式：

```css
属性: 函数名(参数1, 参数2, …);
```

这一小节我们来认识和学习更多实用的函数。

### 动态计算

`calc()` 是 "calculate"（计算）的缩写，它允许我们在声明 CSS 属性值时执行一些数学计算。这在响应式布局中简直是神一般的存在。

比如，我们希望一个侧边栏的宽度固定为 `200px`，而右侧的内容区域占据剩余的全部宽度。在以前，我们可能需要用百分比去凑。但在 CSS3 中，我们可以直接这样写：

```css
.content {
  /* 也就是：总宽度 减去 200px */
  width: calc(100% - 200px); 
}
```

`calc()` 支持加（`+`）、减（`-`）、乘（`*`）、除（`/`）四则运算。

> **注意**：在使用加号和减号时，**运算符前后必须有空格**，否则无效。

我们甚至可以混合不同的单位进行计算，比如百分比减去像素，或者是 `vw` 减去 `em`，浏览器会自动帮我们要处理单位换算。

### 属性获取

`attr()` 函数用于获取 HTML 元素上的属性值，并将其用于 CSS 中。不过目前它主要用于伪元素的 `content` 属性中。

比如我们想直接获取`a`标签上的`herf`属性：

```html
<a href="https://www.baidu.com">我是链接</a>
```

```css
a::after {
  content: attr(href);
  display: inline-block;
  text-decoration: none;
  margin-left: 5px;
  font-size: 0.8em;
  color: #aaa;
}
```

![image-20251218113118184](https://s2.loli.net/2025/12/18/pDdbyWYKvQIXezG.png)

不过这种用法，我们自己一般很少用到，在一些组件库中比较多。

### 智能比较

这三个函数是现代 CSS 响应式设计的利器，它们允许我们根据条件动态选择值，而无需编写繁琐的媒体查询。

比如`min()` 函数接受多个值，并返回其中**最小**的那个：

```css
.container {
  width: min(500px, 90%);
}
```

这行代码的意思是：浏览器会比较 `500px` 和 `90%`（当前父宽度的90%），哪个小就用哪个。

- 如果屏幕很宽，`90%` 超过了 `500px`，那么宽度就是 `500px`。
- 如果屏幕很窄（比如手机），`90%` 小于 `500px`，那么宽度就是 `90%`。
  这相当于自动实现了 `width: 90%; max-width: 500px;` 的效果。

再比如`max()` 函数则相反，它返回其中**最大**的那个：

```css
width: max(300px, 50%);
```

这表示盒子宽度至少是 `300px`。如果 `50%` 的宽度算出来大于 `300px`，那就用更宽的那个。

`clamp()` 综合了上面的两者，它接受三个参数：**最小值**、**首选值**、**最大值**：

```css
font-size: clamp(1rem, 2.5vw, 2rem);
```

它可以实现同时控制最大和最小值，比如上面的计算方式就是：

- **首选**：字体大小倾向于视口宽度的 `2.5%`（`2.5vw`），这样字体会随屏幕变大而变大（流体排版）。
- **下限**：但是，无论屏幕多小，字体**最小**不能小于 `1rem`，防止文字太小看不清。
- **上限**：无论屏幕多大，字体**最大**不能大于 `2rem`，防止文字大得离谱。

这类函数在响应式布局中非常有效。

### CSS 变量

这是一个跨时代的特性，`var()` 函数允许我们使用自定义属性（也就是 CSS 变量）这使得我们可以更方便地管理网页的主题颜色、字号等重复使用的值。

首先，如果需要在CSS中定义变量，变量名必须以两个连字符 `--` 开头，通常我们将变量定义在 `:root`（文档根元素）中，这样全局都可以使用：

```css
:root {
  --primary-color: #099bf6;   /* 非常适合统一定义主色调，后期可以统一在这里更改 */
  --secondary-color: #7bbfed;
  --text-color: #373737;
}
```

定义好之后，我们就可以在任何地方使用 `var()` 来引用它：

```css
.main-btn {
  border: none;
  border-radius: 8px;
  padding: 10px 15px;
  color: var(--primary-color);
  background-color: var(--secondary-color);
}
```

![image-20251218113955836](https://s2.loli.net/2025/12/18/NQwad1IC7rDAJyP.png)

如果我们觉得蓝色不好看，也可以改变主色调和辅色调，这样所有使用了变量的地方都可以立即切换到新的颜色：

```css
:root {
  --primary-color: #8605ef;
  --secondary-color: #e4caff;
  --text-color: #373737;
}
```

![image-20251218114710029](https://s2.loli.net/2025/12/18/DgzdYU4u2Kkilxr.png)

这种方式非常实用，我们只需要在一个地方控制颜色和一些通用的属性。很多的一些组件库，包括柏码官网实际上就是采用这种方式进行色彩变化的：https://www.itbaima.cn/zh-CN

此外，`var`还支持备选属性，比如当CSS中没有设置副色调，可以直接采用备选值：

```css
background-color: var(--secondary-color, #fff);
```

当然，除了颜色之外，基本上我们之前用到过的所有属性都可以使用变量的形式进行存储：

```css
:root {
  --primary-color: #8605ef;
  --secondary-color: #e4caff;
  --font-size: 16px;
  --line-height: 1.5;
  --font-family: 'Roboto', sans-serif;
}
```

注意，和CSS属性一样，我们可以通过重复定义变量来覆盖优先级更低的同名变量：

```css
body button {
  --primary-color: #ef0505;
}
```

此时，由于这里的选择器优先级更高，导致`--primary-color`覆盖掉了`:root`中定义的同名变量，那么此时就会按照这个新的变量值来使用。

### UI设计系列（课程八）

本节我们结合 **CSS 变量** 函数，做一个简单的 **暗黑模式切换** 模拟。

> **浏览器暗黑模式（Dark Mode）**，指的是浏览器或网页在显示时，整体采用**深色背景 + 浅色文字**的配色方案，用来替代传统的“白底黑字”的亮色模式。

通常暗黑模式是通过给 `body` 增加一个类名（比如 `.dark-mode`）来实现的。利用 CSS 变量，我们可以轻松定义两套颜色主题。

```html
<div class="card">
  <h2>CSS 变量很棒</h2>
  <p>利用变量，我们可以轻松实现主题切换。</p>
  <button>切换主题</button>
</div>
```

```css
/* 1. 定义默认主题（亮色） */
:root {
  --bg-color: #f5f5f5;
  --card-bg: #ffffff;
  --text-color: #333333;
  --btn-bg: #3498db;
}

/* 2. 定义暗色主题 */
/* 当 body 拥有 dark 类时，覆盖上面的变量 */
body.dark {
  --bg-color: #2c3e50;
  --card-bg: #34495e;
  --text-color: #ecf0f1;
  --btn-bg: #e74c3c;
}

/* 3. 应用变量 */
body {
  background-color: var(--bg-color);
  color: var(--text-color);
  transition: background-color 0.3s, color 0.3s; /* 添加过渡让切换更丝滑 */
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

.card {
  width: 300px;
  padding: 20px;
  background-color: var(--card-bg); /* 使用变量 */
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  transition: background-color 0.3s;
}

button {
  background-color: var(--btn-bg);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
}
```

后续只需要通过 JS 稍微切换一下 `body` 的类名，整个页面的配色就会瞬间改变，这就是 CSS 变量的魅力。

## AT规则

在 CSS 中，**At 规则（@规则，At-rules）** 是一类**以 @ 开头的特殊语句**，用于告诉浏览器**如何解析、加载或应用样式**，而不仅仅是“给元素设置样式”。这一节我们来认识一下不同类型的AT规则，看看如何使用它们。

实际上我们之前在自定义字体时用到的`@font-face`就是一种AT规则，它可以实现远程或本地引入字体。

### 媒体查询

CSS **媒体查询（Media Queries）** 是用来**根据设备特性或环境条件，动态地应用不同 CSS 样式**的一种机制，是实现**响应式设计**的核心技术之一。

```css
@media 媒体类型 and (媒体特性) {
  /* 满足条件时生效的 CSS */
}
```

默认情况下，媒体类型是可选的，我们可以不进行特别指定，常用的媒体类型有以下这些：

| **类型** | **含义**                     |
| -------- | ---------------------------- |
| screen   | 屏幕设备（手机、平板、电脑） |
| print    | 打印预览或打印时             |
| all      | 所有设备（默认）             |

如果不指定媒体类型，会自动采用`all`作为初始值。

我们可以使用多种媒体特性来创建精确的查询条件，比如现在我们想根据当前屏幕宽度进行判断，是否采用指定样式，就可以使用媒体查询来实现，这里需要使用`screen`类型：

```css
.container {
  width: 200px;
  height: 50px;
  background-color: red;
}

/* 对页面尺寸进行媒体查询，当小于768px宽度时，采用下面的样式 */
@media (max-width: 768px) {
  .container {
    background-color: green;
  }
}
```

当我们查询屏幕尺寸时，我们可以使用以下媒体特性进行判断：

* `width`  -  屏幕尺寸处于指定宽度时切换样式
* `max-width`  -  屏幕尺寸小于等于指定宽度时切换样式
* `min-width`  -  屏幕尺寸大于等于指定宽度时切换样式

这里只列举了宽度，高度也是同理的。在上面的例子中，我们用到了`max-width`来判断屏幕的最大宽度，当未达到最大宽度时，就采用其中的样式。

我们也可以尝试一下在打印时产生的特殊样式，使用指定的媒体类型即可：

```css
@media print {
  .container {
    background-color: blue;
  }
}
```

![image-20251219150117931](https://s2.loli.net/2025/12/19/E2OVm5NwtjCrfFi.png)

可以看到，当打印时，盒子的颜色会变成蓝色。

此外，还有一些常用的媒体查询特性，比如`orientation`可以检查当前是否处于横屏或是竖屏状态，`resolution`可以检测当前屏幕的分辨率和缩放等。有关更多媒体特性请参阅：[媒体查询](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference/At-rules/@media#媒体特性)

需要注意的是，AT规则虽然看着像是嵌套了一层CSS选择器，但是其本身并不参与任何优先级判断，在比较优先级时，依然是其中编写的选择器在进行判断，因此，为了能够保证样式在特定情况下准确覆盖，建议大家把媒体查询卸载后面，以保证同优先级下样式不会被覆盖。

### 深色模式

`@media` 还可以检测用户的系统偏好，例如是否开启了深色模式，深色模式是现在很多网站都会使用的颜色主题，因为深色模式在夜间能有效降低屏幕的亮度，保护眼睛，同时深色模式有着非常完美的对比度，很多颜色都能搭配。

我们可以使用 `prefers-color-scheme` 这个媒体特性来为网站提供深色模式的样式：

```css
@media (prefers-color-scheme: dark) {
  body {
    background-color: #121212;
    color: #eeeeee;
  }
}
```

切换深色模式可以尝试在系统设置中选择：

![image-20251219152100832](https://s2.loli.net/2025/12/19/zqLw7pa1yiPoCYZ.png)

这样，浏览器也会自动采用深色模式，我们可以观察一下深色模式下会有什么变化。

配合我们之前介绍的深色模式适配样式，就可以实现根据用户系统设置来自动切换浅色或是深色效果了。

### 动画

前面我们介绍了CSS过渡（`transition`）虽然过渡效果能带来非常平滑的动画切换，但它只能实现从一个状态到另一个状态的简单变化。如果我们想创建更复杂的、多步骤的动画，比如让一个元素来回移动、闪烁或者改变多次颜色，就需要使用CSS动画（`animation`）

动画的核心是 `@keyframes` 规则，它用来定义动画的各个关键帧（即动画过程中的关键状态）首先，我们需要使用 `@keyframes` 创建一个动画，你可以为它指定一个名字，然后在内部定义动画的起始状态（`from` 或 `0%`）、中间状态（任意百分比）和结束状态（`to` 或 `100%`）

比如现在我们想创建一个盒子左右移动的动画：

```css
@keyframes Test {
  0% { transform: translateX(0px) }
  50% { transform: translateX(100px) }
  100% { transform: translateX(0px) }  /* 最终状态为不移动 */
}
```

这里我们写了个3个关键帧，来表示在动画进行到不同的进度时所具有的三种状态，这样浏览器就可以自动推算出中间的过渡动画了。

这样，我们就定义好了在不同进度下的样式，当动画播放时，会自动从一个阶段过渡到另一个阶段的样式。我们需要使用`animation-name`属性来指定盒子要采用的动画效果名称，然后再使用`animation-duration`指定动画的播放时间，使用`animation-delay`控制动画播放延迟：

```css
.container {
  width: 100px;
  height: 100px;
  background-color: red;
  animation-name: Test;  /* 动画名称要和刚刚创建的保持一致 */
  animation-duration: 3s;  /* 动画持续时间越长，播放越慢 */
  animation-delay: 0s;  /* 播放延迟 */
}
```

接着，我们就可以在盒子中看到动画自动播放了。和之前一样，我们可以控制动画的速度曲线：

```css
animation-timing-function: ease-out;
```

此时动画的每一个关键帧就是先快后慢地播放。

默认情况下，动画只会在页面加载时播放一次，我们也可以控制播放次数，只需设置`animation-iteration-count`属性即可控制：

```css
animation-iteration-count: 3;
```

如果我们希望动画永远播放下去，也可以直接设置为`infinite`，表示无限次数播放。

我们还可以控制动画的播放方向，默认情况下是按照正常的方向在进行，也可以让其反着来：

| **值**            | **说明**     |
| ----------------- | ------------ |
| normal            | 正向（默认） |
| reverse           | 反向         |
| alternate         | 正 → 反 → 正 |
| alternate-reverse | 反 → 正      |

```css
animation-direction: reverse;
```

最后这里还有一个比较重要的属性，它可以决定动画开始前或是结束后，元素保持什么样的状态，使用`animation-fill-mode`来控制：

| **值**    | **效果**                      |
| --------- | ----------------------------- |
| none      | 不保留（默认）                |
| forwards  | 停在最后一帧                  |
| backwards | 立即应用第一帧                |
| both      | 同时具备 forwards + backwards |

注意，使用停留最后或第一帧之前，一定要关闭动画的循环播放效果，否则动画重复播放结束之后才会使用这里的状态。

除了让动画正常播放外，我们还可以让它定格在某一个进度下，比如我们希望鼠标移动到盒子上时自动暂停动画播放，鼠标移开时自动恢复动画播放，`animation-play-state`属性可以直接控制：

```css
.container {
  ...

  &:hover { animation-play-state: paused; }
}
```

除了设置一个动画外，我们也可以同时增加多个动画，让它们叠加展示，比如：

```css
@keyframes Test2 {
  0% { background-color: red; }
  50% { background-color: blue; }
  100% { background-color: yellow; }
}
```

```css
.container {
  ...
  animation-name: Test, Test2;
  animation-duration: 3s, 2s;
  animation-timing-function: ease-in;
  animation-fill-mode: forwards;

  &:hover { animation-play-state: paused; }
}
```

注意，当我们使用多个动画时，上面提到的这些属性也可以通过逗号分隔来分别控制不同动画的时间、速度曲线、填充模式等，如果只写一个表示所有动画采用一样的设置。

针对于以上属性，我们可以使用`animation`简写属性，一步到位：

```css
animation: name duration timing-function delay iteration-count direction fill-mode play-state;
```

比如：

```css
animation: move 1s ease-in-out 0.3s infinite alternate forwards;
```

表示使用名字为`move`的动画，持续时间1秒钟，先慢后快再慢结束，延迟0.3s播放，无限循环播放，动画方向先正后反，保留最后一帧状态。

### 层叠优先级控制

`@layer` 是 **CSS Cascade Layers（层叠层）** 相关的 AT 规则，用来**显式控制样式在层叠中的优先级顺序**，解决“样式越来越多、权重越来越乱”的问题。简单来说就是：@layer 用来给 CSS 规则分层，并决定“哪一层的样式优先级更高”

在没有 @layer 之前：

- 后加载的 CSS 通常会覆盖先加载的
- 为了覆盖样式，不断提高选择器权重
- 为了提高优先级导致出现大量 `!important`
- 第三方样式（如组件库）很难控制优先级

我们可以通过`@layer`创建多个层，写得越靠后的层，优先级越高，比如：

```css
@layer reset, base, components, utilities;
```

这里我们一共定义了4个层，其中`utilities`的优先级最高，`reset`的优先级最低。

那么定义好之后如何使用呢？我们可以使用`@layer`指定需要使用的层，并在其中编写样式：

```css
@layer utilities {
  .container { background-color: dodgerblue; }
}

@layer base {
  .container { background-color: red; }
}
```

可以看到，虽然这里的两个选择器的内容完全一样，并且背景颜色为红色排在后面，但是实际上，最终生效的是上面的蓝色，因为它和下面的样式不在同一个层级上，由于`utilities`的优先级更高一些，它就更能覆盖其他的样式。

同样的，即使较前层级的优先级更高，靠后的层级也能实现覆盖：

```css
@layer utilities {
  .container { background-color: dodgerblue; }
}

@layer base {
  div.container { background-color: red; }
}
```

这类似于我们前面提到的“层叠上下文”概念，优先级的比较只存在于同一个层级中，跨越层级时会直接按照更高优先级的层级样式进行展示。

不过，如果我们使用了`!important`，它就会使得样式跨越层级，变为最高优先级：

```css
@layer utilities {
  .container { background-color: dodgerblue; }
}

@layer base {
  .container { background-color: red !important; }  /* 此时虽然层级不如上面，但是被强制提高优先级 */
}
```

比较有趣的是，如果大家都使用了`!important`的话，它会反转层的优先级顺序：

```css
@layer utilities {
  .container { background-color: dodgerblue !important; }
}

@layer base {
  .container { background-color: red !important; }
}
```

可以看到，此时生效的是下面的红色盒子，这也是为了保证最低层级的CSS使用`!important`来覆盖其他样式。

那如果是没有采用分层的样式呢？比如我们直接编写的选择器：

```css
@layer utilities {
  .container { background-color: dodgerblue; }
}

.container { background-color: red; }
```

综上，完整优先级排序结果如下：

| 优先级   | 类型                                     |
| -------- | ---------------------------------------- |
| 1 (最低) | 先声明的 `@layer` 中的普通样式           |
| 2        | 后声明的 `@layer` 中的普通样式           |
| 3        | 未分层的普通样式                         |
| 4        | 未分层的  `!important` 样式              |
| 5        | 后声明的 `@layer` 中的 `!important` 样式 |
| 6 (最高) | 先声明的 `@layer` 中的 `!important` 样   |

可以看到，如果存在未分层的样式，那么浏览器会直接采用它，因为未使用`@layer`的样式默认处于最顶层。所以，要让元素实现分层管理，必须采用`@layer`进行编写，否则就会失去意义。因为比较麻烦，所以这种方式不推荐小型或个人项目使用。

### 其他常用AT规则

这一节我们接着来介绍一些常用的AT规则，首先是`@import`，它可以实现在CSS文件中引入其他CSS文件：

```css
@import "css/normalize.css";
```

当我们使用后，只要页面引入了此CSS就会自动将其中所有`@import`所关联的CSS样式一并引入。同时，在引入CSS时，我们可以直接指定此CSS文件内定义内容的层级：

```css
@import url("reset.css") layer(reset);
@import url("components.css") layer(components);
```

不过我们还是更推荐大家选择`link`标签进行CSS引入，因为这种方式会阻塞渲染，加载完一个CSS之后才去加载另一个，而`link`标签可以实现并行加载，不阻塞。

针对于页面上的一些CSS属性，可能在老旧浏览器中尚不支持，我们可以使用`@supports`来进行判断，只有在支持的情况下才启用其中的样式：

```css
@supports (backdrop-filter: blur(10px)) {
  .card {
    backdrop-filter: blur(10px);
  }
}
```

这样浏览器就会进行判断，不支持则不生效此样式。

前面我们介绍了`@media`能够根据浏览器窗口尺寸进行判断，同样的，我们也可以根据父元素的窗口尺寸进行判断。使用`@container`进行容器查询：

```css
@container (min-width: 400px) {
  .item {
    font-size: 18px;
  }
}
```

根据父容器的尺寸进行查询，满足条件则生效里面的样式。

### UI设计系列（课程九）

这一节我们来编写一个自定义的浏览器弹窗，弹窗包含背景和窗口部分。

```html
<div class="dialog-modal">
  <div class="dialog-card">
    <div class="dialog-card__header">
      <h4>重要通知</h4>
    </div>
    <div class="dialog-card__content">
      柏码将于2025年12月32日免费赠送各位学员一套
      小米17Pro手机 + 钢化膜套装，请于当天早上9点到群内
      领取，过时不候
    </div>
    <div class="dialog-card__footer">
      <button disabled>领取</button>
      <button>不领取</button>
    </div>
  </div>
</div>
```

```css
.dialog-modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);

  .dialog-card {
    display: flex;
    flex-direction: column;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 300px;
    height: 350px;
    background-color: white;
    border-radius: 10px;

    .dialog-card__header {
      padding: 10px 15px;
      border-bottom: 1px solid #eaeaea;
    }

    .dialog-card__content {
      flex: 1;
      font-size: 14px;
      padding: 10px 15px;
    }

    .dialog-card__footer {
      padding: 10px 15px;
      border-top: 1px solid #eaeaea;
      text-align: center;
    }
  }
}

h1, h2, h3, h4, h5, h6 {
  margin: 0;
}
```

### UI设计系列（课程十）

这一节我们根据前面讲解的媒体查询对前面编写的苹果官网做一个移动端适配，同时修复之前的背景图片定位问题。

### UI设计系列（课程十一）

这一节我们来实现一下苹果官网的顶部导航栏下拉菜单效果。

## 其他内容

随着CSS的课程来到尾声，我们也将和大家说再见了，在课程的最后时光里，我们继续来介绍一些CSS中比较常见但又不容易和前面串联的知识点，希望在这最后的时光里大家也能好好享受CSS带来的快乐。

### 表格样式

针对于我们前面学习的表格`table`标签，CSS也有很多属性可以控制其样式，我们来研究一下。

其中最常见的需求是给表格加上边框，让它看起来更清晰，在HTML阶段，我们并不推荐大家使用标签属性来控制边框，因为CSS里面控制会更加灵活：

```css
table, th, td {
  border: 1px solid #ccc;
}
```

在默认情况下，你会发现表格的边框是双线的，看起来有点老式。这是因为每个单元格（`td`, `th`）都有自己的边框。我们可以使用 `border-collapse` 属性将这些边框合并起来：

```css
table {
  width: 100%;
  border-collapse: collapse; /* 合并边框 */
}
```

`border-collapse` 有两个值：

- `collapse`：将相邻的边框合并为一条。
- `separate`：边框分离（默认值）。当使用此值时，还可以用 `border-spacing` 属性来设置边框之间的距离。

为了让表格内容不那么拥挤，我们可以给单元格添加一些内边距（`padding`），并设置文本对齐方式：

```css
th, td {
  padding: 10px;
  text-align: left; /* 标题和数据向左对齐 */
}
```

为了提高可读性，有些网站经常会制作“斑马条纹”表格，即奇数行和偶数行背景色不同。这可以通过我们前面学习的 `:nth-child` 伪类轻松实现：

```css
tr:nth-child(even) {
  background-color: #f2f2f2; /* 偶数行背景变灰 */
}
```

这样，一个美观、易读的现代风格表格就完成了。

### 比例尺寸

在响应式设计中，我们经常需要让一个元素（比如视频或图片容器）保持固定的宽高比，例如 16:9。在过去，这需要一些复杂的 “padding-top hack” 技巧。但现在，CSS 提供了一个非常简单的属性：`aspect-ratio`

`aspect-ratio` 允许你直接定义一个元素的宽高比，比如这里我们插入视频的CSS：

```html
<div class="video-container">
  <iframe src="//player.bilibili.com/player.html?isOutside=true&aid=115083162161828&bvid=BV1sQeEzFEKi&cid=31912692890&p=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
</div>
```

```css
.video-container {
  width: 80%; /* 宽度自适应 */
  aspect-ratio: 16 / 9; /* 宽高比设置为 16:9 */
  background-color: #f0f0f0;
}
```

这样，无论 `.video-container` 的宽度如何变化，它的高度都会被浏览器自动计算，以始终保持 16:9 的比例。这对于嵌入的视频、图片画廊、产品展示卡片等都非常有用。

你也可以用它来创建正方形：

```css
.profile-pic {
  width: 150px;
  aspect-ratio: 1 / 1; /* 宽高比 1:1，即正方形 */
}
```

只需设置一样的比例即可成为正方形状态。

### 滚动穿透

“滚动穿透”（Scroll Chaining）是一个常见的用户体验问题。想象一个场景：你在一个弹窗（Modal）内部滚动内容，当你滚动到顶部或底部时，如果你继续滚动，背后整个页面的 `<body>`也会跟着滚动。这通常不是我们想要的效果。

比如现在我们在页面上创建了一个弹窗：

```html
<div class="dialog-modal">
  <div class="dialog-card">
    沙井啊手动滑稽阿斯加德看哈手机扩大
    萨达好贱啊圣诞节阿是打卡机阿萨达
  </div>
</div>
<div style="height: 2000px"></div>
```

```css
.dialog-modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);

  .dialog-card {
    writing-mode: vertical-lr;
    white-space: nowrap;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 300px;
    height: 350px;
    background-color: white;
    border-radius: 10px;
    overflow: auto;
  }
}
```

我们会发现，弹窗里面的内容滚动到底部之后，如果继续使用鼠标滚轮，会导致其后面的元素也进行滚动。CSS 提供了 `overscroll-behavior` 属性来解决这个问题，它可以控制当滚动到达内容边界时的行为：

```css
overscroll-behavior: contain;
```

`overscroll-behavior` 主要有三个值：

- `auto`：（默认值）滚动会“穿透”到父级元素。
- `contain`：阻止滚动穿透，将滚动限制在本元素内。
- `none`：和 `contain` 效果类似，但它还会阻止在边界处的默认滚动效果（如回弹）。

通过在需要独立滚动的容器上设置 `overscroll-behavior: contain;`，你就可以轻松地优化滚动体验，防止页面出现意外的滚动。不过，这种方式并不完美，因为实际上我们在`modal`处滚动式依然可以发生滚动，最好的办法还是利用JS来阻止滚动事件。

### 用户选择

`user-select` 属性用于控制用户是否能够选择（即用鼠标拖蓝高亮）页面上的文本。

在某些场景下，禁用文本选择是很有用的：

- **按钮和图标**：用户点击按钮是为了触发操作，而不是复制按钮上的文字。
- **自定义UI组件**：在拖拽等交互中，不希望意外选中文本。

`user-select` 的常用值：

- `none`：元素及其子元素内的文本都不能被选择。
- `auto`：（默认值）浏览器根据情况决定，通常文本内容是可选择的。
- `text`：用户可以自由选择文本。
- `all`：只需单击一次，即可选中整个元素的所有内容，而不是像平常一样只选中一个词或一个字母。这在代码块等场景中非常方便。

例如，你可以给代码块设置 `user-select: all;`，让用户可以一键复制全部代码。

### 浏览器私有前缀

我们在编写CSS样式时，经常看到诸如 -webkit-、-moz- 这类前缀，实际上这种写法叫做 **浏览器私有前缀（Vendor Prefix）**主要用于 **CSS 属性、值或函数在标准尚未完全确定时**，让不同浏览器先行实现和测试新特性。不同的浏览器有不同的私有前缀：

| **前缀** | **对应浏览器内核 / 浏览器**                           |
| -------- | ----------------------------------------------------- |
| -webkit- | WebKit / Blink：Chrome、Safari、新版 Edge、iOS 浏览器 |
| -moz-    | Gecko：Firefox                                        |
| -ms-     | Trident / EdgeHTML：IE、老版 Edge                     |
| -o-      | Presto：老版 Opera（已淘汰）                          |

浏览器在规范稳定前，会用前缀避免将来 API 变动造成破坏，比如有些属性可以像这样写：

```css
.box {
  -webkit-user-select: none;
  -moz-user-select: none;
  user-select: none;
}
```

这在一些老旧的浏览器中非常有效，因为某些特性可能还尚未正式添加，需要添加特殊前缀才能正确启用，老版本浏览器只识别带前缀的写法。

不过，随着前端技术的不断完善，到目前来说已经不需要大家手动去添加这些前缀了，我们后续会学习的一些CSS处理器会在编译时自动帮我们添加对应带前缀的属性。

### 性能优化

`will-change` 属性可以提前告知浏览器，某个元素将要发生什么样的变化，这样浏览器就可以在元素真正发生变化之前，提前做好优化准备。当变化实际发生时，整个过程会更加流畅、快速，从而提升页面的渲染性能。这非常适合页面中存在一些过渡动画的情况。

简单来说，当你对一个元素使用了 `will-change`，浏览器会为这个元素创建一个独立的渲染层（也称为“合成层”）。之后，涉及这个元素的变化（如 `transform`, `opacity`）将在这个独立的层上进行，而不会影响或重绘页面上的其他部分。这个过程通常由GPU（图形处理器）来加速，因此效率非常高。

以下是一些适合使用 `will-change` 的场景：

| 场景               | 示例代码                                                | 说明                                                         |
| ------------------ | ------------------------------------------------------- | ------------------------------------------------------------ |
| **鼠标悬停效果**   | `.element:hover { will-change: transform; }`            | 当鼠标悬停在一个元素上，准备触发一个transform动画时，可以提前通知浏览器。 |
| **动态变化的元素** | `.sliding-element { will-change: transform, opacity; }` | 对于即将通过JavaScript进行位移或透明度变化的元素，比如一个轮播图的滑动项。 |
| **动画状态**       | `.element.animating { will-change: transform; }`        | 当通过添加一个class来启动CSS动画时，可以在该class中加入 `will-change`。 |

`will-change` 虽好，但不能滥用，过度使用会消耗过多内存，反而可能导致页面变慢甚至崩溃。

你可以指定一个或多个你期望变化的CSS属性：

- `transform`: 表示元素的几何变换即将发生。
- `opacity`: 表示元素的透明度即将发生变化。
- `scroll-position`: 表示元素的滚动位置即将发生变化（常用于可滚动容器）。
- `contents`: 表示元素的内容即将发生变化。
- `auto`: 这是默认值，表示浏览器不进行任何特殊的提前优化

总而言之，`will-change` 是一个强大的性能优化工具，但需要像手术刀一样精确地使用。

## 本章练习

### 选择题

**1. 关于 `max-width` 和 `min-width` 属性，下列说法错误的是？**
A. `max-width` 用于设置元素的最大宽度，当浏览器窗口小于该值时，元素宽度会自适应缩小。
B. `min-width` 用于设置元素的最小宽度，即使内容很少，宽度也不会低于此值。
C. 如果同时设置了 `width: 80%` 和 `max-width: 500px`，当父容器宽度为 1000px 时，该元素宽度为 800px。
D. 这些属性常用于响应式布局，防止元素在不同屏幕尺寸下过度变形。

**2. 关于 CSS 轮廓（outline）与边框（border）的区别，描述正确的是？**
A. `outline` 会占据盒子的空间，从而影响由于布局。
B. `outline` 总是绘制在 `border` 的内部。
C. `outline` 不占据空间，绘制在边框之外，可能会遮挡外部其他元素。
D. `outline` 不能像 `border` 一样设置颜色和样式。

**3. 在使用 `box-shadow` 设置阴影时，如果希望阴影显示在盒子内部（内阴影），需要添加哪个关键字？**
A. `inside`
B. `inset`
C. `inner`
D. `internal`

**4. 行内元素的 `vertical-align` 属性默认对齐方式是？**
A. `top` (顶线对齐)
B. `middle` (中线对齐)
C. `bottom` (底线对齐)
D. `baseline` (基线对齐)

**5. 下列关于“精灵图（Sprites）”的优点，描述最准确的是？**
A. 可以让图片在网页上自动旋转和缩放。
B. 通过将多张小图合并为一张大图，减少 HTTP 请求数量，加快页面加载速度。
C. 可以直接在 CSS 中修改图片的颜色和分辨率。
D. 是为了让图片具有 3D 效果。

**6. 若要实现一个从左到右的红色到黄色的线性渐变背景，正确的写法是？**
A. `background-image: linear-gradient(to right, red, yellow);`
B. `background-image: linear-gradient(to left, red, yellow);`
C. `background-image: linear-gradient(red, yellow);`
D. `background-image: radial-gradient(circle, red, yellow);`

**7. 想要给一个透明背景的 PNG 图片添加符合其不规则形状的投影，应该使用哪个属性？**
A. `box-shadow`
B. `filter: blur()`
C. `filter: drop-shadow()`
D. `text-shadow`

**8. 关于 CSS3 的二维变换 `transform: translate(10px, 20px)`，下列说法正确的是？**
A. 元素会在文档流中真正移动，原本的位置会被其他元素占据。
B. 元素只是视觉上发生了偏移，原本占据的空间依然保留，不脱离文档流。
C. 该属性只能用于块级元素，行内元素无效。
D. 它是通过修改 `margin` 值来实现移动的。

**9. 在制作 3D 翻转卡片时，为了让子元素在旋转时保持其 3D 立体空间（而不是扁平化贴在父元素上），需要在父元素上设置什么属性？**
A. `perspective: 1000px;`
B. `transform: rotateY(180deg);`
C. `transform-style: preserve-3d;`
D. `backface-visibility: hidden;`

**10. 为什么给元素的 `height` 属性设置 `transition` 过渡效果时，从 `0px` 变到 `auto` 不会产生动画？**
A. 因为 `height` 属性不支持过渡效果。
B. 因为 `auto` 是一个不确定的计算值，浏览器无法计算中间状态。
C. 因为必须配合 `width` 属性一起变化。
D. 因为过渡时间设置得太短了。

**11. 使用 `calc()` 函数进行计算时，下列写法正确的是？**
A. `width: calc(100%-20px);`
B. `width: calc(100% - 20px);`
C. `width: calc(100% +20px);`
D. `width: calc(100%* 20px);`

**12. 在 CSS 中定义变量（自定义属性）时，变量名必须以什么开头？**
A. `$` (例如 `$main-color`)
B. `@` (例如 `@main-color`)
C. `--` (例如 `--main-color`)
D. `var-` (例如 `var-main-color`)

**13. 如果希望一个 CSS 动画无限次循环播放，应该将 `animation-iteration-count` 属性设置为哪个值？**
A. `loop`
B. `always`
C. `infinite`
D. `100%`

**14. 为了检测用户系统是否开启了深色模式（Dark Mode），应该使用哪个媒体查询特性？**
A. `(theme: dark)`
B. `(prefers-color-scheme: dark)`
C. `(display-mode: dark)`
D. `(system-color: dark)`

**15. 如何使用 `aspect-ratio` 属性创建一个宽高比为 4:3 的容器？**
A. `aspect-ratio: 4 by 3;`
B. `aspect-ratio: 4, 3;`
C. `aspect-ratio: 4:3;`
D. `aspect-ratio: 4 / 3;`

**16. 根据 CSS 层叠层（`@layer`）的优先级规则，在不使用 `!important` 的情况下，下列哪个样式的优先级最高？**
A. 先声明的 `@layer` 中的样式。
B. 后声明的 `@layer` 中的样式。
C. 未分层的普通样式。
D. `:root` 中定义的样式。

**17. 为了防止在一个弹窗内部滚动到底部时，导致整个页面背景跟着滚动（即“滚动穿透”），可以在该弹窗容器上使用哪个 CSS 属性？**
A. `overflow: hidden;`
B. `scroll-snap-type: y mandatory;`
C. `overscroll-behavior: contain;`
D. `user-select: none;`

————————————————
版权声明：本文为柏码知识库版权所有，禁止一切未经授权的转载、发布、出售等行为，违者将被追究法律责任。
原文链接：https://www.itbaima.cn/zh-CN/document/4djgk5xy1lzpiuf2