![image-20241128010924534](https://s2.loli.net/2024/11/28/AeBr4Hd5m7CoZxU.png)

# HTML5 高级内容

这一部分我们接着来讲解HTML5的高级内容，包括表单、矢量图和无障碍。

## HTML表单

在我们常见的一些网站中，存在着各种各样的表单，比如我们登录一个网站时，需要填写登录表单、在网上购物时，需要填写收货信息表单、申请助学金时，需要填写助学金信息表单。表单是一个采集用户数据非常好的途径，也是很多网站都需要用到的功能。

![image-20241123195159784](https://s2.loli.net/2024/11/23/ZmqK4YHSw72zDdj.png)

这一部分我们就来学习一下表单的编写，和之前的表格一样，表单需要使用`form`标签来作为最外层：

```html
<form>
<!-- 各种表单组件 -->
</form>
```

表单中出现的各种输入框，选择框都有对应的标签来表示，我们依次来认识一下。

### 文本输入框

文本输入框是我们使用频率非常高的一种表单组件，几乎99%的场景都会用到它。我们可以使用`input`标签来表示一个文本输入框，它和`img`一样都是行内块元素（CSS阶段会详细介绍）它是一个单标签，直接加就完事：

```html
<form>
  <input>
</form>
```

![image-20241123205146741](https://s2.loli.net/2024/11/23/7zKhbfaYS96tHle.png)

可以看到，在页面中出现了一个可以输入内容的文本框，很简单吧？

> 输入框并不是必须在form中编写，根据情况也可以在页面的任何位置插入，后续学习的其他表单组件也是同理。

我们接着来认识一下`value`属性，它可以用于设置输入框的默认值，比如：

```html
<form>
  <input value="牛魔的">
</form>
```

![image-20241123232512710](https://s2.loli.net/2024/11/23/qawAYkLBVEhGxMm.png)

我们在什么都不输入的情况下，输入框会展示默认的文本。然后是`size`属性，它可以控制输入框的长度，单位是一个英文字符的宽度：

```html
<form>
  <input value="牛魔的" size="10">
</form>
```

不过这个属性我们一般使用的得比较少，控制宽度一般都是使用CSS来调整。还有一个`maxlength`属性，它可以控制输入框内文本的最大长度限制：

```html
<form>
  <input value="牛魔的" maxlength="5">
</form>
```

这样，当我们输入的字数超过指定的长度时将无法继续输入。有些时候我们可能还需要在用户未输入任何内容时添加一些提示信息来告诉用户这个输入框需要填写什么，我们可以使用`placeholder`属性：

```html
<form>
  <input placeholder="用户名">
</form>
```

![image-20241124000338111](https://s2.loli.net/2024/11/24/6TqD5ZvjxkhSl34.png)

此时输入框就会出现提示词。有些时候我们可能希望输入框里面的内容不可被修改，那么也可以为其添加`disbaled`属性：

```html
<form>
  <input value="你干嘛" disabled>
</form>
```

与其相似的还有`readonly`属性，它同样可以使一个输入框的内容只读，只不过它相比`disbaled`来说，可以被选中但不能编辑，且在表单提交时会包含在内，有关表单提交我们会在后面进行介绍。

> HTML某些属性被称为**布尔属性**（boolean attributes）。这些属性的特点是它们的存在与否决定了属性的值，而不需要显式地设置为特定的值。例如，`checked`、`disabled`、`readonly`等属性就是布尔属性。
>
> 由于这些属性只有启用和不启用两种情况，因此上面我们使用`disabled`就直接代表我们要启用某个功能，不用和其他属性那样还要编写具体结果。
>
> 对于布尔属性，如果在标签中使用该属性，那么它的值就被视为`true`。因此，`checked`和`checked="checked"`在HTML中是等效的。

除了这种普通文本框之外，我们还可以将文本框设置为多种类型的，比如我们输入密码时，往往需要使用`*`来代替真正的密码字符，从而防止用户的密码直接出现在屏幕上，保证一定的安全性。我们可以使用`type`属性来修改文本框的类型，默认情况下`type`属性为`text`，密码的话就是：

```html
<form>
  <input type="password">
</form>
```

![image-20241124001539239](https://s2.loli.net/2024/11/24/MBqKOUv4CLtRiak.png)

当然除了密码和数字之外，还有很多类型，这里我们全部列举出来：

- `email`：电子邮件地址输入，支持格式验证
- `url`：URL输入，支持格式验证
- `tel`：电话号码输入
- `number`：数字输入，可以使用上下箭头进行增减
- `range`：范围输入，通常以滑块形式呈现
- `date`：日期选择输入，通常显示日期选择器
- `time`：时间选择输入，通常显示时间选择器
- `datetime-local`：本地日期和时间输入
- `month`：月份选择输入
- `week`：周选择输入
- `color`：颜色选择输入，通常显示颜色选择器
- `file`：文件选择输入，允许用户上传文件
- `hidden`：隐藏输入，不在页面上显示
- `search`：搜索框输入，通常在样式上有所不同
- `button`：普通按钮
- `submit`：提交按钮
- `reset`：重置按钮

其中大部分类型都是普通的文本输入，比如`tel`、`email`、`url`，不过这些会携带一定的格式验证。

```html
<form>
  <input type="email">
</form>
```

不过这种格式验证只会在提交表单时进行校验。还有一些比较重要的输入框类型，我们会在接下来几节中进行详细介绍。

### 表单的提交

在继续介绍其他类型的输入框之前，我们先来讲解一下表单的提交。这里我们可以编写一个简单的登录表单：

```html
<form>
  电子邮件: <input type="email">
  密码: <input type="password">
</form>
```

不过我们发现，`input`标签一直都有一个警告，说表单输入没有相关的label，那么这个是什么东西呢？这其实HTML的一种规范，它要求所有的输入框都要被一个`label`标签囊括或是指向，以实现更明确的语义化，它也是一个行内元素：

```html
<form>
  <label>电子邮件: <input type="email"></label>
  <label>密码: <input type="password"></label>
</form>
```

![image-20241125164124310](https://s2.loli.net/2024/11/25/J2zgiHxmvZjcp9w.png)

实际上`label`标签加与不加，在展示效果上都是一样的。当然，此标签对于元素的选中有一定效果，当我们点击输入框前面的文本时，也可以激活输入框，包括后续的各种样式输入框也是支持这种效果的。

除了上面这种写法之外，为了更加灵活，我们也可以使用`for`属性来指定一个输入框：

```html
<form>
  <label for="username">电子邮件: </label>
  <input id="username" type="email">
  <label>密码: <input type="password"></label>
</form>
```

注意这里的`for`必须填写对应`input`标签的`id`名称，这与上面直接包含的写法是一样的。

我们接着来看表单，当用户填写好自己的用户名和密码后，实际上需要将填写的数据提交给后端进行校验，可能各位小伙伴对于前后端的概念还不是很熟悉，这里我们简单介绍一下：

> 在一个网站中，前端和后端是两个重要的组成部分，它们各自承担着不同的职责和功能。
>
> * **前端**：用户直接与之交互的部分，通常指网站的视觉和交互元素。它涉及到网页的布局、设计、以及用户体验。
> * **后端**：后端是用户看不见的部分，负责处理应用逻辑、数据库交互和服务器操作。

因此，我们作为前端开发来说，只需要考虑页面制作和用户操作逻辑即可，而登录验证以及数据的获取，则都是后端的工作，这里的登录实际上也是将数据提交给后端进行处理。

那怎么才能完成这样的一个提交操作呢？首先我们需要为所有的输入框命名，这样才能明确填写的表单项名称，我们需要指定`name`属性：

```html
<form>
  <label>电子邮件: <input name="username" type="email"></label>
  <label>密码: <input name="password" type="password"></label>
</form>
```

接着，当我们在完成表单的填写之后，我们可以使用一个类型为`submit`类型的输入框作为登录按钮使用：

```html
<form>
  <div>
    <label>用户名: <input name="username" type="email"></label>
  </div>
  <div>
    <label>密码: <input name="password" type="password"></label>
  </div>
  <input type="submit">
</form>
```

![image-20241125174517648](https://s2.loli.net/2024/11/25/tj5Ds7S46rMpPUh.png)

这种类型的输入框会直接显示为一个按钮的形式，当我们点击时也会触发一次表单提交操作。注意这种类型的输入框可以不需要配合`label`标签使用。

同样的，`button`标签也可以实现这样的效果，它代表一个按钮，是一个双标签且也是行内块元素：

```html
<button>你干嘛</button>
```

接着我们将其`type`属性设置为`submit`类型，这样就可以作为表单的提交按钮了：

```html
<button type="submit">你干嘛</button>
```

按钮标签可以编写更灵活的表单提交按钮样式，比如名称可以自定义，不过在我们学习JavaScript之前使用率不会很高，但是一定要记住有这样一个标签。

当然除了点击按钮之外，也可以通过敲击回车的方式提交表单，在表单提交前，如果您输入的内容与表单类型不符或校验未通过，那么将会弹出警告信息，比如我们这里的类型是`email`，但是输入内容并不是一个合法的电子邮件地址：

![image-20241125173749558](https://s2.loli.net/2024/11/25/SapiChlJDufjV3L.png)

> 我们还可以为`input`标签添加`required`属性来要求此输入框必须填写内容才可以提交表单。
>
> ![image-20241126011755753](https://s2.loli.net/2024/11/26/Sqyt16EKOgiRA8k.png)
>
> 这同样适用于之后讲解的`textarea`和`select`标签。

当输入内容无误时提交，使用F12打开开发者工具，看到旁边的“网络”板块出现了一个新的请求，且页面刷新了：

![image-20241125174057791](https://s2.loli.net/2024/11/25/r2o3FI8UtATgaLP.png)

此时我们就成功向后端发起了一个GET方法的表单登录请求，只不过由于现在我们没有学习后端相关知识和HTTP协议相关知识点，因此不需要深入研究，只做了解即可。

有些时候我们可能还希望重置表单内容，比如用户发现自己填写的内容有问题，我们可以提供一个`reset`类型的输入框或是`reset`类型的按钮来作为重置按钮，它可以直接将表单中填写的所有内容清除：

```html
<form>
  <div>
    <label>用户名: <input name="username" type="email"></label>
  </div>
  <div>
    <label>密码: <input name="password" type="password"></label>
  </div>
  <div>
    <input type="submit">
    <input type="reset">
  </div>
</form>
```

![image-20241125175428868](https://s2.loli.net/2024/11/25/uYapeQM8O6j3Igz.png)

当我们点击重置按钮时，表单中的数据就自动被清除了，还是很简单吧？

当然，可能有些时候我们的输入框并没有写到`form`标签之内，但是我们又希望它可以关联到某个表单那该怎么办呢，只需要使用`form`属性指定其id即可：

```html
<input form="666">
<form id="666">
  
</form>
```

这种规则同样适用于后续学习的`select`和`textarea`标签。

最后我们再来认识一下`form`标签中的一些属性，其实都是与表单提交相关，些常用属性列表如下：

1. **action**：指定表单数据提交的目标 URL。
2. **method**：指定表单提交方法。常用值包括 `GET` 和 `POST`。
3. **enctype**：指定表单数据编码类型，特别是在文件上传时。常用值包括 `application/x-www-form-urlencoded`、`multipart/form-data` 和 `text/plain`。
4. **target**：指定表单提交后响应的目标窗口或框架。常用值包括 `_self`、`_blank`、`_parent`和 `_top`。
5. **name**：为表单指定名称，便于在 JavaScript 中引用。
6. **id**：为表单指定唯一标识符，便于在 CSS 和 JavaScript 中引用。
7. **autocomplete**：指定表单是否启用自动完成功能。取值可以是 `on` 或 `off`。
8. **novalidate**：禁用表单的默认验证。

比如下面的代码：

```html
<form action="/login" method="post" novalidate>
  <div>
    <label>用户名: <input name="username" type="email"></label>
  </div>
  <div>
    <label>密码: <input name="password" type="password"></label>
  </div>
  <input type="submit">
</form>
```

此时会发送一个地址为`/login`的`POST`登录请求：

![image-20241125181137736](https://s2.loli.net/2024/11/25/OWYJZLIEkGSNR3P.png)

当然不出意外肯定会返回404，同样的，这里仅做了解即可。

### 日期选择

日期选择在很多时候都需要，这里有很多种类型的日期格式供选择，比如我们只希望选择时间，可以使用`time`：

![image-20241124011026428](https://s2.loli.net/2024/11/24/2hURBDow5Ceju1P.png)

它可以让我们选择一个时间点作为结果，不过比较拉胯的是，它只能选择时分，不能选择秒。当然也有选择日期的`date`类型输入框：

```html
<form>
  <input type="date">
</form>
```

我们也可以选择一个更强大的`datetime-local`类型，它不仅包含一个时间点，还包含一个具体的日期：

![image-20241124011559843](https://s2.loli.net/2024/11/24/jWI6bvDZuwh842O.png)

它可以进行日期和时间的完整选择。除此之外，还有单独的周数选择、月份选择：

```html
<form>
  <input type="week">
  <input type="month">
</form>
```

![image-20241124012441135](https://s2.loli.net/2024/11/24/AvFaN1TMhogmBzq.png)

不过这些日期选择框，在一些浏览器下并未完全适配，而且选择框的样式也无法修，因此我们在实际开发中也很少会用到日期相关的输入框，一般都是使用第三方组件来实现，这里仅做了解即可。

### 数字和范围选择

有些时候我们可能会要求用户只输入数字，可以使用`number`类型：

```html
<form>
  <input type="number">
</form>
```

![image-20241124112934730](https://s2.loli.net/2024/11/24/hIKYqPcnAM9xamT.png)

这种类型的输入框无法接受除了数字之外的其他字符，并且在输入框的右边有按钮可以调整输入的数字。

除了简单设置为数字类型之外，我们还可以限制数字输入的范围，使用`max`和`min`进行控制：

```html
<form>
  <input type="number" max="100" min="0">
</form>
```

![image-20241124115247955](https://s2.loli.net/2024/11/24/krzg9ibBfoTvm8W.png)

此时当我们点击右边的按钮进行数字调整时，达到最小或最大值时，将无法调整。

此外，我们还可以通过`step`属性来控制调整按钮的步长，也就是点击一次变化的差值，默认为1：

```html
<form>
  <input type="number" max="100" min="0" step="2">
</form>
```

除了数字输入框之外，我们也可以使用一个范围选择器，来选择一个数字范围：

```html
<form>
  <input type="range">
</form>
```

![image-20241124135054907](https://s2.loli.net/2024/11/24/52tzQXlUY3nBdJD.png)

我们同样也可以，为其设置一个最大范围：

```html
<form>
  <input type="range" max="100" min="0" step="2">
</form>
```

这样就只能在范围内拖动了，只不过这里并没有显示具体数值，有点不太好看，这个类型的输入框也不是很常用，只做了解就行了。

### 单选框和多选框

我们接着来介绍一个使用频率非常高的输入框类型，单选按钮。我们可以使用`radio`来表示一个单选框：

```html
<input type="radio">
```

![image-20241124181937305](https://s2.loli.net/2024/11/24/f8G9JhdXxU5TvVC.png)

接着就出现了一个小圆点，我们可以使用鼠标点击它使其变成选中状态。当然，像这样直接编写小圆点不太好看，我们可以为其丰富一下内容：

```html
<form>
  <div>请选择您的高考毕业礼物：</div>
  <input type="radio"> 100万元现金
  <input type="radio"> 清华大学录取通知书
</form>
```

![2a9cee0b5fc8ec3a09ca71c3ff095073](https://s2.loli.net/2024/11/24/MvPQyUKbmGfq3E9.png)

不过我们发现，这两个选项明明是单选框，但是为什么两个都可以同时个勾选呢？~~因为小孩子才做选择，我全都要~~，这是因为我们没有为其设置`name`属性，它是输入框的名称，其中一个作用是之前说到的提交表单时的表单项目的名称，还有一个作用就是为单选框进行分组，同一组单选框的`name`属性必须为同一个值：

```html
<form>
  <div>请选择您的高考毕业礼物：</div>
  <input type="radio" name="test"> 100万元现金
  <input type="radio" name="test"> 清华大学录取通知书
</form>
```

![image-20241124184131034](https://s2.loli.net/2024/11/24/ysFTBYS3IOPwKpH.png)

我们还是可以通过`checked`属性来默认勾选某个单选框：

```html
<form>
  <div>请选择您的高考毕业礼物：</div>
  <input type="radio" name="test"> 100万元现金
  <input type="radio" name="test" checked> 清华大学录取通知书
</form>
```

我们还需要为单选框设置一个`value`属性作为其表单提交的值：

```html
<form>
  <div>请选择您的高考毕业礼物：</div>
  <input type="radio" name="test" value="a"> 100万元现金
  <input type="radio" name="test" value="b" checked> 清华大学录取通知书
</form>
```

这样表单就可以提交我们当前所选中的值对应的`value`作为结果了。

很多时候可能我们并不是做单选题，我们也需要能够多选的表单。我们可以使用`checkbox`作为类型，但是同样需要和单选框一样使用相同的`name`用于分组，还有`value`作为值：

```html
<form>
  <div>感谢您参与本次校园招聘，请选择您的毕业Offer意向：</div>
  <input type="checkbox" name="test" value="a"> Java后端开发工程师|月薪2K
  <input type="checkbox" name="test" value="b"> 全干开发工程师996|月薪3K
  <input type="checkbox" name="test" value="c"> 服务器运维24小时随叫随到|月薪2K
  <input type="checkbox" name="test" value="d"> 美团大厂外卖骑手|1单5-10块
</form>
```

![image-20241125233904018](https://s2.loli.net/2024/11/25/6vqOkPfidar5uHe.png)

可以看到，我们可以自由选择喜欢的选项。在同时选择多个选项时，提交表单的值会同时携带勾选的所有属性：

![image-20241125234649370](https://s2.loli.net/2024/11/25/BNFtfVozvLSwdMi.png)

有关勾选框相关介绍就到这里。

### 下拉列表

虽然使用单选或多选框可以帮助我们解决选项类输入的问题，但是当选项很多时，将所有选项全部展示出来可能对用户来说并不友好，此时我们可以考虑另一种选择的方案，下拉选择框。

使用`select`标签来表示一个下拉选择框：

```html
<form>
  <select>
    
  </select>
</form>
```

不过光有一个选择框还不行，我们还要为其添加选项，所有的选项需要编写在`select`标签内部，并使用`option`标签囊括起来：

```html
<form>
  <select>
    <option>Java入门到入土</option>
    <option>C++就业到裁员</option>
    <option>C#的虚空岗位</option>
    <option>Web前端被AI取代</option>
  </select>
</form>
```

![image-20241126005213958](https://s2.loli.net/2024/11/26/liHkWV8Frgu4YLy.png)

可能各位小伙伴会比较疑惑下拉框样式问题，这是因为不同操作系统的下拉框样式不同。实际上很多网站为了统一样式一般都是使用第三方组件库实现下拉框。

选项的展示值也可以使用`label`属性指定，效果和直接编写到标签内部是一样的：

```html
<form>
  <select>
    <option label="Java入门到入土"></option>
    <option label="C++就业到裁员"></option>
    <option label="C#的虚空岗位"></option>
    <option label="Web前端被AI取代"></option>
  </select>
</form>
```

不过这种写法并不推荐，我们还是建议直接写到标签内部。除了单选之外，我们也可以为其添加`multiple`属性，来使得其可以多选：

![image-20241126005440495](https://s2.loli.net/2024/11/26/kWwADpcSjvl5E6J.png)

此时下拉框会自动变成上图的样式，我们可以按住Ctrl键，来点击实现多选。当然，如果选项比较多，全部展示出来不太好，我们也可以限制多选模式下展示的数量，使用`size`即可：

```html
<form>
  <select size="3" multiple>
    <option>Java入门到入土</option>
    <option>C++就业到裁员</option>
    <option>C#的虚空岗位</option>
    <option>Web前端被AI取代</option>
  </select>
</form>
```

> 比较有意思的是，`size`属性如果被设置，即使没有启用multiple也会展示为滚动框形式，只是无法进行多选。

和前面的多选框一样，我们必须要为每一个选择设置其`value`属性来作为表单提交的值：

```html
<select size="3">
  <option value="a">Java入门到入土</option>
  <option value="b">C++就业到裁员</option>
  <option value="c">C#的虚空岗位</option>
  <option value="d">Web前端被AI取代</option>
</select>
```

当我们想要默认选中某个值时，可以使用`selected`属性，将其直接写到需要作为默认值的`option`标签上即可：

```html
<form>
  <select size="3">
    <option value="a">Java入门到入土</option>
    <option value="b">C++就业到裁员</option>
    <option value="c" selected>C#的虚空岗位</option>
    <option value="d">Web前端被AI取代</option>
  </select>
</form>
```

有关下拉列表相关内容就介绍到这里。

### 文本域

单行文本框只能输入一行内容，这在很多时候都显得有点不太好使，比如我们想要编写一段200字的个人简介或者是一篇文章，此时使用普通输入框就感觉有点局限了。

`textarea`用于表示一个文本域，它可以实现多行文本的输入，它是一个双标签，也是一个行内块元素：

```html
<form>
  <textarea></textarea>
</form>
```

![image-20241126000509244](https://s2.loli.net/2024/11/26/hUIEHGDzebxsKL1.png)

我们可以使用`rows`属性来控制文本域的默认行数，用`col`控制文本域默认的列数：

```html
<textarea rows="5" cols="10"></textarea>
```

这样当文件域创建时会自动以5行10列的形式展示。

我们之前用在普通输入框`input`上的所有属性，在文本域中也可以使用，比如限制最大输入长度：

```html
<form>
  <textarea maxlength="20"></textarea>
</form>
```

包括我们之前设置的提示词`placeholder`属性：

```html
<form>
  <textarea placeholder="请输入您的个人简介..."></textarea>
</form>
```

包括`disabled`和`readonly`属性，也是可以使用的：

```html
<form>
  <textarea placeholder="请输入您的个人简介..." disabled></textarea>
</form>
```

不过需要注意的是，如果我们需要填写`textarea`的默认值，需要直接填写到标签内部：

```html
<form>
  <textarea placeholder="请输入您的个人简介...">我是默认值</textarea>
</form>
```

![image-20241126003405678](https://s2.loli.net/2024/11/26/sOtyhUv1SXncZeu.png)

至此，有关表单相关的标签至此就全部介绍完毕，其中`file`类型的`input`输入框，我们会在JavaScript阶段在进行介绍，它可以选择我们本地的一个文件，一般用于文件上传。

## HTML矢量图

**注意：** 从这部分开始全部作为选学内容，如果时间不是很充裕可以直接跳过。

我们接着来看HTML中有关图相关的扩展内容，学习这些知识有助于各位小伙伴对于一些不常用功能的见解。

### svg标签

SVG（Scalable Vector Graphics）是一种用于描述二维图形的XML（可扩展标记语言）格式。前面我们介绍了位图（像素图）格式，它是按照像素点的形式进行存储的，但是存在一个很大的问题就是放大就会失真。而矢量图就很好的解决了这个问题，它针对的是图片的形状进行存储，浏览器在展示矢量图的时候，就可以根据形状特征进行绘制，这样无论如何也不会出现失真问题。

我们可以使用XML语法（与HTML类似）来编写一个SVG矢量图，首先需要编写一个`svg`标签：

```xml
<svg>

</svg>
```

接着，我们就可以在`svg`中定义我们需要绘制的图形了，我们先来看最简单的矩形绘制，使用`rect`标签即可：

```xml
<svg>
  <rect/>
</svg>
```

不过需要注意的是，在XML语法中单标签需要在最后添加一个`/`来完成自闭合。

此时我们发现页面上好像并没有出现什么内容，这是因为我们还需要为这个矩形设置一些属性，首先是矩形的宽高，使用`width`和`height`属性来设置：

```xml
<svg>
  <rect width="30" height="30"/>
</svg>
```

此时页面上就会出现一个长宽都为30的正方形：

![image-20241127172754470](https://s2.loli.net/2024/11/27/8AVP2JmdOthjcYo.png)

整个`svg`元素会在页面中占据一个300x150的区域作为整块画布大小，我们可以在这个画布内自由发挥，注意svg也是一个行内块元素：

![image-20241127180437268](https://s2.loli.net/2024/11/27/lGYtyLCXeSx1HAi.png)

通过对`svg`设置`width`和`height`属性，可以对画布大小进行修改：

```xml
<svg width="100" height="100">
  <rect width="30" height="30" />
</svg>
```

我们还可以使用`fill`属性来修改正方形的填充颜色：

```xml
<svg>
  <rect width="30" height="30" fill="orange"/>
</svg>
```

这里的颜色支持一些比较常见色彩的英文单词，比如`blue`、`red`、`yellow`等，或是使用`transparent`来代表透明色，如果需要更加自定义化的颜色类型，可以使用16进制颜色代码来表示，我们会在后续的CSS阶段进行介绍。

除了绘制基本形状外，我们也可以使用`stroke-width`来设置边框绘制宽度，并使用`stroke`来设置边框颜色：

```xml
<svg>
  <rect width="30" height="30" fill="orange" stroke="red" stroke-width="3"/>
</svg>
```

![image-20241127181032126](https://s2.loli.net/2024/11/27/6MFgzEDRL2O5Zdu.png)

不过我们发现，这个图形的左上角好像被遮住了一部分，这是因为我们添加了边框之后，导致一部分内容超出了画布，我们可以使用`x`和`y`来设置图形左上角在画布上的坐标：

```xml
<svg>
  <rect y="10" x="10" width="30" height="30" fill="orange" stroke="red" stroke-width="3"/>
</svg>
```

注意这里的坐标与我们数学里学习的平面直角坐标系的坐标不太一样，这里的X轴是从左往右，而Y轴是从上往下。

我们还可以调整矩形的圆角，使用`rx`和`ry`属性进行控制：

```xml
<svg>
  <rect y="10" x="10" width="30" height="30" rx="5" ry="5"
        fill="orange" stroke="red" stroke-width="3"/>
</svg>
```

![image-20241127182010830](https://s2.loli.net/2024/11/27/5s2oRLfAQFknEBP.png)

看完了矩形，我们接着来看圆形如何绘制，方式也很简单，使用`circle`标签即可，其中`r`属性表示圆的半径：

```xml
<svg>
  <circle r="20"/>
</svg>
```

![image-20241127182957168](https://s2.loli.net/2024/11/27/LWUh9oavQNim1HP.png)

不过这个图形绘制出来，可以看到只显示出了四分之一的大小，这是因为默认情况下圆形是以中心为原点，且默认为0，所以就跑到左上角去了。我们可以使用`cx`和`cy`来调整圆心坐标：

```xml
<svg>
  <circle cx="20" cy="20" r="20"/>
</svg>
```

当然我们在圆形中也可以使用之前介绍的`fill`和`stroke`来设置，这里就不多说了。除了圆形之外，椭圆形也可以绘制的，我们可以使用`ellipse`来表示一个椭圆形：

```xml
<svg>
  <ellipse cx="20" cy="20" rx="20" ry="12"/>
</svg>
```

除了简单绘制一个图形外，我们还可以将不同图形给组合起来：

```xml
<svg>
  <ellipse cx="20" cy="20" rx="20" ry="12" fill="red"/>
  <circle r="10" cx="40" cy="15" fill="orange"/>
  <ellipse cx="30" cy="30" rx="20" ry="12" fill="green"/>
</svg>
```

![image-20241127184700454](https://s2.loli.net/2024/11/27/7ST4kpm3gejb9X6.png)

如果多个图形叠放，想要提现出更强烈的层级效果，我们还可以为其设置`opacity`属性表示透明度：

```xml
<svg>
  <ellipse cx="20" cy="20" rx="20" ry="12" fill="red"/>
  <circle r="10" cx="40" cy="15" fill="orange" opacity="0.5"/>
  <ellipse cx="30" cy="30" rx="20" ry="12" fill="green" opacity="0.5"/>
</svg>
```

![image-20241127184739689](https://s2.loli.net/2024/11/27/uIs2jHfGN78E3nC.png)

除了上述介绍的规则图形外，还有一些不规则图形也可以很轻松地定义和绘制。

绘制一段直线可以使用`line`标签，我们需要指定直线的起始点和结束点：

```xml
<svg>
  <line x1="0" y1="0" x2="20" y2="20" stroke="blue" stroke-width="2"/>
</svg>
```

![image-20241127185246606](https://s2.loli.net/2024/11/27/HLay32e96tOqjrU.png)

其中`x1`和`y1`表示起点，`x2`和`y2`表示终点，因此这里会绘制一个斜线。

我们还可以使用`polyline`来绘制一个多段线条，它用起来稍微有些麻烦：

```html
<svg>
  <polyline points="0,0 20,20 60,20" stroke="yellow"/>
</svg>
```

这里我们需要使用`points`属性来指定多段线条的各个点，接着浏览器会自动连接这些点形成一个多段直线：

![image-20241127185921258](https://s2.loli.net/2024/11/27/6C7j9wWfYplR4T2.png)

但是我们发现这里内容被填充了，我们可以使用`transparent`或是`none`取消掉它的默认填充色：

```xml
<svg>
  <polyline points="0,0 20,20 60,20" fill="none" stroke-width="2" stroke="yellow"/>
</svg>
```

![image-20241127190043617](https://s2.loli.net/2024/11/27/LXGg4b7QRUKVACJ.png)

同理，我们也可以使用`polygon`来绘制一个多边形：

```xml
<svg>
  <polygon points="0,0 20,50 200,10 150,0" stroke="blue" stroke-width="3" />
</svg>
```

![image-20241127190209608](https://s2.loli.net/2024/11/27/t7ZkIHMB2PmO5v8.png)

有关svg的更多用法和绘制图形，请参考：https://developer.mozilla.org/zh-CN/docs/Web/SVG

### map标签

前面基础部分中我们为大家介绍了`img`标签，其中有一个`usemap`属性，可以设置“图像映射”，图像映射允许你将一个图片分成多个区域，每个区域可以链接到不同的页面或位置。

我们先来创建一个简单的图片：

```html
<img src="assets/test.png" width="218" height="218" alt="测试图片">
```

接着我们可以为其编写一个`map`标签，来表示图像映射配置：

```html
<img src="assets/test.png" width="218" height="218" usemap="#test" alt="测试图片">
<map name="test">
  
</map>
```

我们需要使用`name`属性为`map`标签命名，接着在`img`标签中使用`usemap`来指定一个图像映射。接着就可以在`map`标签中使用`area`来设置图片的每个独立区域，比如现在我们想要把这个图片的三个圆形区域单独作为一个可点击交互的区域：

![image-20241127192747223](https://s2.loli.net/2024/11/27/8q3SKkEXDdetR6m.png)

我们可以在`map`中指定三个圆形区域，`area`标签具有多种形状可供选择，圆形我们可以使用`circle`：

```html
<img src="assets/test.png" width="218" height="218" usemap="#test" alt="测试图片">
<map name="test">
  <area shape="circle" coords="33,33,25" href="https://www.baidu.com" title="左上角区域">
</map>
```

这里`coords`是用于指定区域图形的相关属性，具体格式依赖于 `shape` 属性的值：

- 对于 `rect`，格式为 `x1,y1,x2,y2`（左上角和右下角的坐标）
- 对于 `circle`，格式为 `x,y,r`（圆心坐标和半径）
- 对于 `poly`，格式为 `x1,y1,x2,y2,...`（一系列点的坐标）

而`herf`则是点击之后跳转的链接，它与`a`标签一样，可以自由设置连接，以及对应的`target`属性。

使用 `<map>` 和 `<area>` 标签可以为图像添加交互性，使得用户可以通过点击不同区域访问不同的内容。这在创建图形导航、图形链接等场景中非常有用。

## HTML无障碍

HTML无障碍（Accessibility）是指网站和应用程序设计的方式，使其能够被所有人使用，包括那些有视觉、听觉、运动或认知障碍的用户。无障碍设计不仅符合道德标准，也是法律要求的一部分，尤其在某些国家和地区。

简单来说，我们学习无障碍，就是为了让一些残疾人也可以轻松使用网站，让他们也可以体验到互联网带来的便利。

![image-20241127194158493](https://s2.loli.net/2024/11/27/IdYCFGTaqr3BoJm.png)

在很多大型公司的网站上，都有明确标注支持残疾人使用，这不仅是大厂的一种能力的体现，也是一种社会责任和担当。

### 无障碍简单体验

这里我们开启系统中的一些残障人士辅助工具来简单体验一下无障碍功能。

在Windows系统下，我们可以开启系统的讲述人功能：

![image-20241128000350322](https://s2.loli.net/2024/11/28/O4Qwjosl9MXuUIq.png)

Mac系统下可以在设置中打开“旁白”：

![image-20241128001029273](https://s2.loli.net/2024/11/28/aLwrYOX6CKegVhx.png)

打开系统的辅助功能后，恭喜你们成功解锁了盲人模式，现在你只要聚焦在系统的任意一个区域，都会有实时的朗读声音，以及介绍。

比如这里我们打开百度，无需使用鼠标，只需使用键盘上的方向键和Tab键即可控制：

![image-20241128001300117](https://s2.loli.net/2024/11/28/3evW6CAJg7TUHY1.png)

讲述人会在旁边实时对内容进行讲解，这样盲人也可以知道现在聚焦的是什么元素，这对于一些残疾人来说提供了非常大的帮助，让更多的人也可以体验网上冲浪的感觉。

### WAI-AIRA相关属性

前面我们简单体验了一下无障碍功能，系统辅助工具之所以能够非常精准地识别页面上的元素，很大程度上都是因为我们使用了语义化标签导致的：

![image-20241128002228004](https://s2.loli.net/2024/11/28/ujfmeNZIyRlTE1X.png)

当讲述人在遇到`a`标签时，会自动将其朗读为链接、遇到`img`标签时，会自动将其朗读为图片。所以，从HTML整套课程的开头我们就一直在说到语义化的重要性，它在很多地方都起着非常重要的作用。

我们也可以在自己编写的页面中体验一下使用语义化标签和不使用语义化标签对于辅助工具的重要性：

```html
<div>1. 我是第一项</div>
<div>2. 我是第二项</div>
<ol type="a">
    <li>我是第一项</li>
    <li>我是第二项</li>
</ol>
```

当我们使用讲述人阅读上面不使用语义化标签实现的列表时，只会简单朗读其内容，而并未告诉我们这是一个列表。当我们阅读到下面的列表时，讲述人会明确告知我们，这里是有序列表的第一项。

不仅仅是标签，实际上一些属性也可以为辅助工具提供额外的信息，比如图片：

```html
<img src="assets/test.png">
<img src="assets/test.png" alt="我是自定义的描述文本，哈哈">
```

在朗读添加了`alt`属性和未添加的图片时，讲述人可以为我们更清楚地介绍我们为图片添加的自定义信息。

当然，如果我们的页面上存在一些不具备语义化的内容但是我们又希望它语义更明确，这时该怎么办呢？我们可以使用WAI-ARIA，它专门用于对页面上的一些语义不明确的标签提供额外的语义化。

>  [WAI-ARIA](https://www.w3.org/TR/wai-aria-1.1/) 是 W3C 编写的规范，定义了一组可用于其他元素的 HTML 特性，用于提供额外的语义化以及改善缺乏的无障碍。以下是规范中三个主要的特性：

这对于一些第三方使用JavaScript实现的组件库来说，非常重要，因为这些组件库往往都不会使用HTML提供的原生标签去实现某些功能，比如悬浮按钮或是其他的组件，此时使用WAI-ARIA就可以弥补因为使用非语义化标签导致的语义不明确问题。

这里我们还是以列表为例：

```html
<div>1. 我是第一项</div>
<div>2. 我是第二项</div>
```

我们可以使用`role`属性来为其明确指定一个HTML标签所代表的角色，比如列表就是`list`，列表中的每一项就是`listitem`，WebStorm会自动给我们提示：

```html
<div role="list">
    <div role="listitem">1. 我是第一项</div>
    <div role="listitem">2. 我是第二项</div>
</div>
```

通过我们为其自行赋予角色，讲述人就可以明确知道这里使用的是列表。

除了使用`role`来指定标签的角色外，我们还可以使用`aira-*`来指定此标签所具有的某个属性，比如我们之前在使用输入框时可以使用`readonly`来表示输入框只读。而我们也可以通过对应的`aira-readonly`属性为一个不支持此属性的标签添加只读的语义：

```html
<input value="我是只读内容" readonly>
<div role="textbox" aria-readonly="true">我是只读内容</div>
```

这两个标签虽然写法不同，但是在讲述人看来就是同样的一个只读属性的输入框。

有关更多`aria-*`的属性，可以参考：https://www.w3.org/TR/wai-aria-1.1/#state_prop_def

————————————————
版权声明：本文为柏码知识库版权所有，禁止一切未经授权的转载、发布、出售等行为，违者将被追究法律责任。
原文链接：https://www.itbaima.cn/zh-CN/document/njol93fs34gfwuzf