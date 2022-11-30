### 什么是HTML

超文本标记语言（Hyper Text Markup Language）,HTML描述了一个网站的结构，是一种标记语言而非编程语言。一个HTML元素是HTML文件的一个基本组成单元。

### 什么是CSS

层叠样式表（Cascading Style Sheets）它的主要职能就是确定布局和元素的表现。

CSS不能单独使用，必须和HTML或XML一起协同工作，为HTML或XML齐装饰作用。改变HTML元素的样式。

### 什么是JavaScript

缩写为JS，是一种高级的，解释型的编程语言，用来给网页添加各式各样的动态功能。

Javascript支持面向对象编程，命令式编程，以及函数式编程

### 完整的html文档结构

![](https://document.youkeda.com/P3-1-HTML-CSS/1.2/4.jpg)

**1.** \<!DOCTYPE html>

     **1..** 作用：告知浏览器该页面文件的文档类型，指示web浏览器使用哪个HTML版本编写页面。

     **2.** 位置：`` <!DOCTYPE> ``声明必须是HTML文档的第一行，位于`` <html> ``标签之前

     **3.** `` <!DOCTYPE> ``声明对大小写不敏感。

​	  **4.** `<!DOCTYPE> `声明没有结束标签。

**2.** <html>...</html>

     **1.** 此元素可告知浏览器其自身是一个HTML文档。

​	  **2.** `<html>`与`</html>`标签限定了文档的开始点和结束点，再他们之间是文档的头部和主体。文档的头部由`<head> `标签定义，而主体由`<body>`标签定义。

     **3.** lang属性（语言属性）：当搜索引擎或者浏览器拿到语言属性后，有可能做一些针对指定语言的辅助操作，'en'表示英文。

**3.** 标签属性

​	  **1.** 标签可以拥有零个或多个标签属性，注意：标签属性与标签名称，标签属性与标签属性之间需要用一个空格隔开。

​	  **2.** 标签属性可以赋予标签更多的信息，标签属性通常是以\*\*key='value'\*\*即名称='值'的形式出现。

​	  **3.** 常见的标签属性有：class，id，style，lang。src等。

**4.** 文档的头部`<head>...</head> `

​	  **1.** head元素定义文档的头部，我们通常再这里引用样式表，提供元信息等。

​	  **2.** 文档的头部中的`<title>...</title> `定义文档点多标题，再网页上体现为网页标签的标题。

​	  **3.** 一个`<head>`元素必须包含且只能包含一个`<title>`元素。

> 元信息：又叫元数据，就是描述数据的数据，这里主要指文档的概要信息。

**5.** 文档的主体`<body>...</body>`

     **1.** body元素定义文档的主体，包含文档的所有内容（比如文本，超链接，图像，表格和列表等等）。

### 注释

### **插入音频**

```html
<audio

​    controls

​    scr="音频地址">

​    your bowser dose not support the<code>audio</code>element

</audio>



<!--加入进度条-->

<label for="file>File progress:</label>

<progress id="file" max="100" value="70"> 70% </progress>


```

### 插入图片

```html
<img src="图片地址"alt="图片地址不存在的时候，显示这个alt的地址下的图片">
```

### 插入链接

```html
<a herf="链接地址">标题（点这个标题就等于点这个地址）</a>
```

### 列表

`ul`无序列表标签

`ol`有序列表标签

列表中的每一项都用标签`<li>`表示

### form 标签

**1.** `action`一个处理此表单信息的程序所在的URL，所述表格信息将在表单提交时被发送到定义的地址。

**2.** `method`它的值可以时GET或POST，用来规定如何发送表单信息。

```html
<form action="">

 <input type="text" />

</form>
```

*   占位文本`placeholder`

```html
<input type="text" placeholder="昵称" />
```

*   输入框名字`name`

```html
<input type="text" placeholder="昵称" name="nick" />
```

*   输入框的值`value`

希望在输入框中预先填写用户的名称

```html
<input type="text" placeholder="昵称" name="nick" value="小明" />
```

*   不可修改的输入框`readnoly`和`disabled`

把输入框变成只读输入框，这样用户就不能自己修改预填写的内容。

    <input type="text" placeholder="昵称" name="nick" value="小明" readonly />

    <input type="text" placeholder="昵称" name="nick" value="小明" disabled />

disabled

​    对象：所有表单元素

​    作用：使文本框不能输入，当表单以POST或GET的方式提交时，使用了disabled的元素的值不会被传递出去。

​    外观：使文本框变灰。

readonly

​    对象：input和textarea

​    作用：仅使文本框不能输入。

​

​    外观：外观没有变化。

### 多行文本输入框和密码输入框

*   `textarea` 多行输入框

```html
<!-- name属性表示表单元素的名称，placeholder属性表示表单元素的占位文本 -->

<textarea

 name="sign"

 rows="5"

 cols="30"

 placeholder="请输入个性签名"

\></textarea>
```

> 其中`rows`和`cols`分别表示行数（高度）和文本域的可视宽度。

*   密码输入框

用户输入的内容将以黑圆点表示

输入内容不显示

将`type="text"`改为`type=password`

```html
<!-- type属性表示表单元素的类型，name属性表示表单元素的名称，placeholder属性表示表单元素的占位文本 -->

<input type="password" name="password" placeholder="密码" />
```

### 单选框和复选框

*   单选框

```html
<!-- type属性表示表单元素的类型，name属性表示表单元素的名称，value属性表示表单元素的值 -->

<input type="radio" name="gender" value="male" />

<input type="radio" name="gender" value="female" />
```

> 属于用一道单选题的每个单选按钮，应该拥有相同的`name`属性值。

想让选择点后有字，可以直接在`/>`后写。如果想让用户在点击字的时候也可以选择，则需要在字后面添加`</label>`当然，在`<input>`前面

加上`<label>`

*   复选框

    type = "checkbox"

```html
<label> <input type="checkbox" name="interest" value="coding" />编程 </label>

<label> <input type="checkbox" name="interest" value="other" />其他 </label>
```

属于同一道多选题的每个复选框元素，应该拥有相同的`name`属性值。

### 选择菜单

每个选项用`option`标签表示,一组选项用`slect`包裹。

```html
<select name="career">

 <option value="default">请选择职业</option>

 <option value="staff">公司职员</option>

 <option value="freelancer">自由职业者</option>

 <option value="student">学生</option>

 <option value="other">其他</option>

</select>
```

想让它变为多选菜单`<select name="career" multiple>`加上multiple属性。

### input 标签小结

![](https://document.youkeda.com/P3-1-HTML-CSS/1.4/9.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

### HTML内部添加样式

    <p style="font-size:14px;color:white"></p> 

> 设置p标签中的字体大小为14px，颜色为白色。

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.5/key_value.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

字体大小/字体粗细

###### 字体大小

设置格式：font-size:36px;

> font-size-->字体的大小 36px-->字体大小的尺寸

*   字体加粗

设置格式：font-weigth:100;

> 设置文字粗细的时候，其值可以是100，200，300...900中的任何一个，或者可以用英文代替，normal（正常粗细），light（细），bold（粗），bolder（更粗）

### 文字对齐方式

*   text-align\:center;文字居中对齐

*   ext-align\:left;文字左对齐

*   text-align\:right;文字右对齐

### 文字行高/字间距/字体

*   行高

行高的设置格式 `line-height:30px`

```html
We understand every aspect of project and we put a great amount of time in

 understanding the project.

</p>



<p style="line-height:32px;">

 We understand every aspect of project and we put a great amount of time in

 understanding the project.

</p>
```

当行高与矩形的高度一样的时候，文字在矩形中上下居中。

> 例如：文字外面的矩形高度为80px，要想让文字在矩形中上下居中，就要设置文字的行高为80px

*   字间距

**1.** 英文的字间距是每个字母之间的距离，单词和单词之间的距离不属于字间距

**1.** 中文是每个汉字之间的距离

设置字间距的格式为`letter-spacing:30px`

*   字体

```html
font-family: 'Goudy Bookletter 1911', sans-serif, 'Gill Sans Extrabold';
```

这段代码，页面加载后会先去找代码中设置的字体，这里有三个备选的字体。浏览器会按顺序加载：首先看“Goudy Bookletter 1911 ”字体是否在电脑上安装，如果没有安装，就去看第二个字体，如果到最后了还没有安装，那么就只能用微软自带的默认字体。

*   多个字体之间用英文逗号隔开

*   字体名称中间有空格的时候，要加引号，单引号和双引号都行：`"Times News Roman"`

*   中文字体名称要用空格：“宋体”

### **html中th与thead**

HTML中的table可以大致分为三个部分

thread--表头

tbody--表躯干

tfoot--表注

一般来说：**数据的标题放在thead里面，数据放在tbody里面，表格的注释放在tfoot里面**

thead、tbody、tfoot三个标签的使用目的是把一个表格分为三个大的部分，每个部分存放不同的东西，这样这个表格就会比较有结构。

\<tr>\</tr>这个标签就是放在上面三个标签里面的东西，**每一个\<tr>\</tr>就是表格一行**，你如果打开一张Excel表格就会发现每张表格都是一行一行的。

在HTML中，为了把数据名称和真正的数据区分开来，**用\<th>\</th>表示数据的名称，也就是标题。\<td>\</td>表示真正的数据内容。**

接下来详细的看一下html中的tr和td标签

首先看一下简介：

1：th标签是table表格中的标题部分，就是每一列的表头用th包裹
2：th标签内文字带自动居中，加粗效果
3：td标签内的文字为左对齐的普通文本
ps:在 HTML 4.01 中，th元素的 “bgcolor”、”height”、”width” 以及 “nowrap” 属性是不被赞成使用的

**tr标签----代表html表格中的一行**

1.  tr标签是成对出现的，以\</tr>结束
2.  属性

    ——Common -- 一般属性

    1》align ：代表行的水平对齐方式(left(左对齐) | center(居中对齐) |right(右对齐) | justify)(此属性应该使用CSS实现)

    2》valign ：代表行的垂直对齐方式(top(顶部对齐) | middle(中部对齐) | bottom(下部对齐) | baseline(基线对齐))(此属性应该使用CSS实现)&#x20;
3.  tr 是 table row的缩写

**td标签--代表HTML中的每一个单元格**

1.  td标签也是成对存在的，以\</td>结束

2.  属性
    1》Common -- 一般属性
    2》abbr -- 代表表头的简写
    　　3》axis -- 对单元格在概念上分类
    4》colspan -- 一行跨越多列
    5》headers -- 连接表格的数据与表头
    6》rowspan -- 一列跨越多行
    7》scope -- 定义行或列的表头
    8》align -- 代表水平对齐方式(left(左对齐) | center(居中对齐) | right(右对齐) | justify)(此属性应该使用CSS实现)
    9》valign -- 代表垂直对齐方式(top(顶部对齐) | middle(中部对齐) | bottom(下部对齐) | baseline(基线对齐))(此属性应该使用CSS实现)

3.  td是table data cell的缩写

<table>代表表格</table>
       <tr>代表表格中的一行</tr>
       <td>代表表格中的一列</td>

      tr 与 td 交成一个单元格
    <table>...</table>之间有多少个<tr>，就有多少行
     <tr>...</tr>之间有多少个<td>，就有多少列

例子：

```html
<table width="80%" border="1">

<tr>

<th>www.dreamdu.com</th>

<th>.com域名的数量</th>

<th>.cn域名的数量</th>

<th>.net域名的数量</th>

</tr>

<tr>

<td>2010</td>

<td>01</td>

<td>23</td>

</tr>

<tr>

<td>shen</td>

<td>xiao</td>

<td>jun</td>

</tr>

</table>

```

<table width="80%" border="1">

<tr>

<tr>

</tr>

<tr>

### CSS

内部样式

​	1.首先外面将每一个标签里的CSS样式抽取出来

​	2.然后在head标签里声明了一个\<styrl>\</style>标签

​	3.接下来将样式都放在了style标签里

​	4.将相同标签的样式写在相同的大括号里，大括号前面加上标签	名

```html
p {

 font-size: 16px;

 color: #ffffff;

}
```

书写规则

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/%E6%A0%B7%E5%BC%8F%E9%87%8D%E7%82%B9%E8%A7%A3%E9%87%8A.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

*   不要忘记写声明标签\<style>\</style>

*   样式要用花括号括起来

*   每个样式后面用分号结尾

```html
<link rel="stylesheet" type="text/css" href="./index.css" />
```

​	1.rel属性规定了当前文档与被链接文档之间的关系，但是rel属性的stylesheet值被所有浏览器支持，也就是说，只记住这一个值即可

1.  stylesheet的意思就是文档的外部样式表

2.  type属性规定了被链接文档的MIME（多用途互联网邮件扩展类型）

类型，type属性对应的最常见的值就是text/css，该类型描述样式表。

​	1.href属性后跟的是要引入的链接地址

### 相对路径/绝对路径

绝对路径

**指的是文件在硬盘上真正存在的路径**`<img src="E:\book\网页布局\bg.jpg" /> `

相对路径

为了避免绝对路径造成的页面资源丢失现象，在引入外部资源的时候，都会选择使用相对路径。

**所谓相对路径就是相对于文件自身位置，去寻找要引入的资源文件**

*   ./：当前文件夹目录，比如在下面这个目录结构下（test文件夹下有index.html和index.css两个文件），要在index.html中引入index.css就需要用到./

```html
<link rel="stylesheet" href="./index.css">

<!-- 或者./去掉也可以，效果是一样的 -->

<link rel="stylesheet" href="index.css">
```

*   ../：回到上一级文件夹目录，我们要在index.html中引入index.css文件，首先哟啊从index.html文件所在目录即test文件夹里出来，才能找到index.css，从某个文件夹里面出来，就要用到../

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.6/fanhuishangyijimulu.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

具体写法如下：

```html
<link rel="stylesheet" href="../index.css">
```

> 用相对路径引入文件记住三点：

*   找到引用资源的文件所在位置，以引用资源的文件为基准，寻找资源

*   ../返回一层，如果有多层，就用多个../，比如返回两层就用../../

*   文件夹名代表进入该文件夹，例如css/,表示进入css文件夹，比如从test文件夹里出来进入css文件夹找到index.css文件，可以这样写

    `../css/index.css`

### 常用选择器

```html
<!DOCTYPE html>

<html lang="en">

 <head>

    <meta charset="UTF-8" />

  <title>标签选择器</title>

    <style>

   h3 {

​    font-size: 25px;

​    color: #330867;

   }

   h4 {

​    font-size: 18px;

​    color: #30cfd0;

   }

   p {

​    font-size: 14px;

​    line-height: 28px;

​    color: #4a5252;

   }

  </style>

 </head>



 <body>

  <h3>孟航沛</h3>

  <h4>平面设计师</h4>

    <p>

   专业综合性强，具有较强的综合能力，学习认真刻苦，虽然我的专业是思想政治教育，

   但是我们所学的除了马克思主义的相关理论之外，同时还学习了管理学、心理学和法学的相关理论知识，

   专业具有较强的综合性，使我具备较强的综合能力，基本能够胜任行政设计师助力这个岗位。

  </p>

 </body>

</html>
```

*   类选择器

````html
class是定义类的关键字，article是类名，类名可以任意，但是要符合规范

</p>

\```

\```

.article {

 color: red;

 font-size: 14px;

}
````

\<p class="article">

```html
<p class="common color font-size">
 common设置通用样式，color设置特殊颜色，font-size设置特殊字体大小

</p>
```

```html
.common {

 font-size: 22px;

 color: #333333;

 letter-spacing: 8px;

}



.current {

 color: #ff6973;

}
```

*   id选择器

```html
<p id="p-item">这是一段文字</p>
```

```html
\#p-item {

 font-size: 24px;

 font-weight: 400;

}
```

1.  id选择器在文档中只会出现一次，像下面的使用方式是不对的

```html
<a href="#" id="link">点击进入详情</a>
<!-- link这个id名已经被使用，就不可以再次定义 -->
<a href="#" id="link">点击进入主页</a>
```

1.  不能像类选择器一样，一个标签上定义多个id名，像下面的写法是错误的

```html
<a href="#" id="link linkto">点击进入详情页</a>
```

### 高级选择器

```html
<!DOCTYPE html>

<html lang="en">



<head>

    <meta charset="UTF-8">

  <title>练习</title>

  <link rel="stylesheet" href="./index.css">

</head>



<body>

  <h3>热点排行</h3>

  <ul>

​    <!-- 因为序号、热度的样式和li中其他文本的样式不同，所以我们用span包裹序号和热度，方便我们给它们添加特殊样式 -->

​    <!-- 为了区分序号和热度两个span，我们给它们添加不同的类名做区分 -->

​    <!-- 取类名的时候注意考虑语义化，我们给序号取类名为rank，给热度取类名为point-number，类名尽量不要用拼音，应该选用合适的英文 -->

​    <!-- 由于前三个序号的样式和后几个序号的样式不同，我们给前三个序号再增加一个类名“top-three”以做区分 -->

​    <li><span class="rank top-three">1</span>校招主持称山大女生漂亮留学生幸福 涉事企业回应<span class="point-number">96844</span></li>

​    <li><span class="rank top-three">2</span>合肥"女黑老大"获刑25年:有受害人借900万还71套房<span class="point-number">17556</span></li>

​    <li><span class="rank top-three">3</span>女子体检发现"包块" 尚未确诊跳楼身亡<span class="point-number">15283</span></li>

​    <li><span class="rank">4</span>到银行没领到20多个鸡蛋 7旬老人被气得脑出血<span class="point-number">7471</span></li>

​    <li><span class="rank">5</span>潘石屹要清空在中国的所有核心资产？SOHO中国回应<span class="point-number">6581</span></li>

​    <li><span class="rank">6</span>40名大学生因旷课太多被学校退学 教育部回了3个字<span class="point-number">6463</span></li>

​    <li><span class="rank">7</span>云南亿万富豪被杀案:家人怀疑买凶杀人弃百万赔偿<span class="point-number">6416</span></li>

​    <li><span class="rank">8</span>与父亲争吵怄气 22岁小伙喝百草枯抢救无效死亡<span class="point-number">5723</span></li>

​    <li><span class="rank">9</span>司机被黄牛收钱后万元罚款减半 山西祁县:正在调查<span class="point-number">5433</span></li>

​    <li><span class="rank">10</span>蒙古国逮捕800名中国人：涉嫌从事电信诈骗活动<span class="point-number">5425</span></li>

  </ul>

</body>



</html>


```

```Css
ul,

li {

  list-style: none;

  padding: 0;

}

.h3{

  font-weight: 100;

  font-size: 16px;

  color: #404040;

}

ul {

  font-size: 14px;

  color: #404040;

 }

 

 /* 给序号添加样式 */

 ul li .rank {

  font-size: 16px;

  color: #888888;

 }

 

 /* 给前三个序号添加样式 */

 /* 因为样式层叠性的存在，前三个序号会采用这里的样式 */

 ul li .top-three {

  font-size: 26px;

  color: #f34540;

 }

 

 /* 给热点添加样式 */

 ul li .point-number {

  color: #f34540;

 }
```

### 盒模型

**content**

div标签

```html
<div class="box"></div>
.box{
	wieth:200px;
	height:100px;
	background-color:purple;
}
```

**padding**

padding默认的是给矩形的四周添加相同的内边框。

`.box{padding:20px;}`

上面的代码等价于：

```css
.box{
	padding-top:20px;/*上内边距*/
	padding-right:20px;/*右内边距*/
	padding-bottom:20px;
	padding-left:20px;
}
```

> padding简写`div{padding:20px 20px 20px 20px}`这四个值分别代表上右下左，**如果第三个值没有，就和第一个值保持一致，第四个值没有就和第二个值保持一致**

**border**

为一个矩形设置边线框 	简写方式为：

```css
.box{
	border:2px solid blue;
}
```

> solid是实线
>
> dashed是虚线

```css
.box {
  /* 添加顶部border */
  border-top: 1px solid black;
  /*添加右侧border*/
  border-right: 3px solid orange;
  /*添加底部border*/
  border-bottom: 5px dashed pink;
  /*添加左侧border*/
  border-left: 10px dashed purple;
}

```

**无边框**

比如，让下边框不显示

```css
.box{
	border-bottom:none;
}
```

**圆角边框**`border-radius`

```css
div{
	width:200px;
	height:200px;
	border-radius:18px;
	border:1px solid black;
}
```

圆角分开设置 只能设置左上角或右上角等等

```css
.box{
  width200px;
  heigth200px;
  background-color:violet;
  border-top-left-radius:5px;
  border-top-right-radius:10px;
  border-bottom-right-left-radius:20px;
  border-bottom-right-right-radius:15px;
}

```

**阴影**

```css
<div class="box"></div>
.box{
	width:200px;
	heigt:200px;
	border:1px solid #c4c4c4;
	/* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
	box-shadow:2px 2px 2px 1px rgba(0，0，0，0.2)；
	border-radius:15px;
}
```

阴影的实现原理可以看作是矩形下面有一个重叠的，同样大小的矩形，如果它在x轴，y轴上移动，就会有阴影的效果

*   x偏移量：在x轴上移动，向右为正
*   y偏移量：在y轴上移动，向下为正
*   阴影模糊半径：就是边线的清晰度
*   阴影扩散半径：就是向外伸展
*   阴影颜色：就是矩形下面那个矩形的背景色

**margin**

外边距--矩形和矩形之间的距离

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/margin1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>案例</title>
	<style>
		div{
			width:300px;
			height:100px;
			background-color:#D5E8D4;
			border:1px solid #82B366;
		}
		
		.box{
			background-color:#F5F5F5;
			border:1px solid #FF0818;
			margin-top:20px;
			margin-bottom:20px;
		}
	</style>
</head>

<body>
	<div></div>
	<div class="box"></div>
</body>

</html>
```

margin(外边距)的书写方式和padding（内边距）是一样的。

*   关于margin的垂直距离的计算如下图所示，给其中的一个盒子设置margin-bottom，两个盒子的垂直距离就i是第一个盒子的margin-bottom；然后，给第二个盒子设置一个margin-top，但是值小于第一个盒子的margin-bottom的值，**得到两个盒子之间的垂直距离，是取两个margin的最大值**；最后，将下方盒子的margin-top设置成50px，大于第一个盒子的margin-bottom，两个盒子之间的垂直距离变成了50px。

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-1-HTML-CSS/1.7/max_margin.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

**设置两个盒子之间的距离，一般选择一个去设置即可**

盒子左右居中

必须有宽度才能左右居中！

```css
<div class="father"> 
    <div class="son"></div>
</div>
.father{
    width:400px;
    height:200px;
    border: 1px solid #ccc;
}

.son{
    width:200px;
    height:100px;
    margin:0 auto;
    border: 1px solid #ccc;
}
```

如果让son中的width去掉，你就会发现居中效果失效了。

**display\:block/none**

行内元素可以通过display属性转换成块元素，从而达到设置宽高的目的，

```html
<span class="demo"> 这是一个span标签 </span>

.demo {

 /*将span标签转换成块元素*/

 display: block;

 width: 300px;

 height: 100px;

 background-color: #fff2cc;

}
```

**display\:none**

使标签消失

**display：inline/inline-block**

1.  行内元素可以设置padding

```CSS
<a href="#">超链接</a>

a {

 background-color: #fff2cc;

 padding: 20px;

}
```

1.  行内元素可以设置左右margin，但不能设置上下margin

**inline-block**

是一个可以在同一行显示的元素

**空白折叠现象**

在使用inline-block时，会发现两个盒子之间多了一点空白，这是因为回车被当成了一个文字，使两个<div>之间加了一个字母。

**1.** 去除回车

```CSS
<!-- 将div标签写在一行 -->

<div class="box1"></div><div class="box2"></div>

div {

 width: 200px;

 height: 50px;

 display: inline-block;

}



.box1 {

 background-color: #fff2cc;

}



.box2 {

 background-color: #b0e3e6;

}

```

1.  给父元素添加word-spacing属性

`word-spacing`就是单词和单词之间的距离，这里将这个距离写成负值就可以，这个值要尽量小，我们一般写小于-20px的值。

具体做法如下

```CSS
<div class="father"></div>

.father {

 word-spacing: -50px;

}



.box1 {

 width: 200px;

 height: 50px;

 display: inline-block;

 background-color: #fff2cc;

}



.box2 {

 width: 200px;

 height: 50px;

 display: inline-block;

 background-color: #b0e3e6;

}
  <div class="box2"></div>
```

1.  给父元素设置font-size：0px；

从第二点可以知道，回车可以当作一个文字，那么如果将文字的大小设置为0，空隙自然会消失。

具体做法如下

    <div class="father">
     <div class="box1"></div>
       <div class="box2"></div>
       </div>

    .father {

     font-size: 0px;

    }



    .box1 {

     width: 200px;

     height: 50px;

     display: inline-block;

     background-color: #fff2cc;

    }



    .box2 {

     width: 200px;

     height: 50px;

     display: inline-block;

     background-color: #b0e3e6;

    }

### CSS定位（一）

*   position-static(默认定位)

不能使用`left, top, right, bottom`属性

*   position-relative(相对定位)

想进行位置偏移，不影响页面整体布局。

在不改变页面布局的前提下根据`left, top, right, bottom`调整此元素的位置

![](https://document.youkeda.com/P3-1-HTML-CSS/1.8/2-relative/1.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

*   position-absolute(绝对定位)

\*>\*absolute被称为绝对定位，绝对定位不为元素预留空间，通过指定元素相对于最近的非static定位祖先元素的偏移，来确定元素的位置

**1.** 首先获取到第一张图片元素`<img class="first" src="xxx"/>`我们发现它是absolute布局

**1.** 因此寻找它的父亲节点`<div class= "img-box">`我们发现此元素并未配置position属性，其遵循默认布局position=static，并不符合非static要求。

**1.** 因此继续找`<div class="img-box">`的父亲节点，找到`<body>`

**1.** `<body>`已经没有父亲节点了，所以按照`<body>`的位置为标准进行偏移

![](https://document.youkeda.com/P3-1-HTML-CSS/1.8/3-absolute/1.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

*   position-fixed(固定定位)

固定定位不为元素预留空间，而是通过指定元素相对与屏幕视口（viewport）的位置来指定元素位置，元素的位置在屏幕滚动时不会改变

**为防止固定的字被挡住**使用z-index，此元素决定谁覆盖谁，谁在谁的上面，谁离观察者最近

> 1.  默认非static元素的z-index都为0，

> 1.  z-index越大，则越在最上面，里观察者最近

> 1.  同样的z-index，在HTML中的元素越靠后，则越在最上面，离观察者越近。

*   position-sticky(粘性定位)

![](https://document.youkeda.com/P3-1-HTML-CSS/1.8/6-sticky/1.mp4)

### Float

可以让元素靠左或靠右排版，它使用起来很简单，但使用后引起的行为就是千变万化了，可能引起祖先，兄弟，后代元素布局的变化。

> **nav**一般用于表示此区块是导航区域main:一般用户表示此区块是网页的主体区域。

### 模态框

![](https://document.youkeda.com/P3-1-HTML-CSS/1.9/combat-1-modal/1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

> 1.  模态框总是在浏览器中心，浏览器随意的放大缩小，模态框还是在浏览器中心。

> 1.  模态框总有一个半透明的背景 。

### 常用的居中方法

**元素水平居中**

**1.** 如果是行内元素，我们可以在父容器上使用`text-align:center`。

**1.** 如果内部是块状元素，我们可以在子容器上使用`margin:0 auto`（如果此元素不是块状元素，需要设置`display:block`）。

**元素垂直居中**

之后会有flex完成元素的垂直居中。目前先用margin完成

> margin-top=(modal高度-img高度)/2 //因此算下来margin-top:39

### 背景颜色

常用方法

```css
background: red;

background: #ffffff;

background: rgba(200, 200, 200);

background: rgba(0, 0, 0, 0.5);


```

**渐变色**`linear-gradient`

![](http://document.youkeda.com/P3-1-HTML-CSS/1.10/1-gradient/3.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

渐变方向

*   to right/to left 向右/向左渐变

*   to top/to bottom 向上/向下渐变

*   to right bottom/to right top 向右下/向右上渐变

*   to left bottom/to left top 向左下/向左上渐变

*   xxxdeg xxx范围(0到360)  更精确的渐变方向

渐变位置

渐变不一定是从开始到结束，我们可以设置中间状态，我们可以再每个色值后面跟一个**百分比**，来约定渐变色起止位置

![](http://document.youkeda.com/P3-1-HTML-CSS/1.10/1-gradient/4.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

关于多颜色渐变完整文档见<https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Using_CSS_gradients>

### 背景图片

设置元素背景图片`background-image:url(远程地址或本地地址)`

防止背景图片重复`bbackground-repeat:no-repeat`

*   repeat 这是默认值如果背景图片比容器小，将在垂直和水平方向进行重复

*   repeat-x 背景图片只在水平方向重复

*   repeat-y   背景图片只在垂直方向重复

*   no-repeat   背景图片只显示一次，不重复

*   设置图片位置`background-position:`

*   top left

*   top center

*   top right

*   center left

*   center right

*   center center

*   bottom left

*   bottom right

*   bottom center

    > 左侧两个元素为一组一起出现，分别代表垂直和水平布局，比如`background-position:top left`效果等于`background-position-x:left` `background-position-y:top`如果只写一个关键词，那么另一个关键词默认为center。

    x%y%

    > 第一个值是水平位置，第二个值是垂直位置。左上角是0%0%。右下角是100%100%。如果只写一个值，另一个值将是50%。

    xpx ypx

    > 第一个值是水平位置，第二个值是垂直位置。左上角是0 0，单位是像素（0px\*0px）或任何其他的css单位。如果只写一个值，另一个值将是50%。

    图片撑满整个容器`background-size`

        cover

    > 把背景图像扩展足够大，以使背景图像完全覆盖背景区域。背景图像的某些部分也许无法显示再背景定位区域中

        contain

    > 把图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域

        xpx ypx

    > 手动设置宽度和高度

        x% y%

    > 手动设置宽度和高度相对于容器的百分比

    ### CSS伪类--清除浮动

    在父盒子上添加clearfix类名即可

    ```html
    <!-- 添加清除浮动类名 -->
    <div class="father-one clearfix">
      <div class="son-one">son-one</div>
        <div class="son-two">son-two</div>
          <div class="son-three">son-three</div>
          </div>
    ```

    ```css
    .clearfix::after{
      content: '';
      display: block;
      clear: both;
    }
    ```

    **哪个盒子的子元素有浮动，就在哪个盒子上添加清除浮动**

    ### CSS伪类--事件伪类

    ![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.1/courseware/hover%E4%BC%AA%E7%B1%BB.mp4)

    `hover`--鼠标移动上去

    ![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.1/courseware/%E4%BC%98%E8%AF%BE%E8%BE%BE-hover.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

    ![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.1/courseware/%E4%BC%AA%E7%B1%BB-hover2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

    观察之后我们发现，每一个小块的背景颜色从白色-->蓝色，文字从黑色-->白色，所以我们要做的是，在鼠标箭头移动上去后，完成这两种状态的转换，能完成这种转换过程的就是**伪类**。

    ```css
    li:hover{

      background-color: #47A0FC;

      color: white;

    }
    ```

    ![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.2/f2-2-3-demo1.gif)

    `active`鼠标点击时

    > 注意，这里的点击是点住鼠标不松开

    ![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.2/f2-2-3-demo2.gif)

    **注意**

    `hover`一定要在`active`之前，否则不会生效

    `focus`--获取焦点后

    一般用于具有焦点的元素，比如`input`，比如我们可以让input获取到焦点以后，改变input的边框颜色。

    ![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-2-HTML-CSS/1.2/f2-2-3-demo3.gif)

### CSS装饰--cursor

```css
cursor:pointer

cursor:default

cursor:text

cursor:move

cursor:grab

cursor:zoom-in

cursor:zoom-out
```

### CSS Flex布局

Flex是Flexible Box的缩写，意为“弹性布局”

它最显著的效果就是把原本从上到下排列的块状元素变成水平排列：

```html
<div>
	<div class="item">项目1</div>
	<div class="item">项目2</div>
 	<div class="item">项目3</div>
</div>
```

```css
.container {

 display: flex;

 background: #D5E8D4;

 border: 1px solid #5D9E5A;

}



.item {

 width: 50px;

 height: 50px;

 background: #FFF2CC;

 border: 1px solid #B7A570;

 margin: 10px;

}
```

**注意**：设为Flex布局以后，子元素的float，clear和vertical-align属性将失效。

采用Flex布局的元素，称为Flex容器（flex container），它的所有子元素称为Flex项目（flex item）

![](https://document.youkeda.com/P3-2-HTML-CSS/1.4/2.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

在上面的这个例子中，外层div是容器，内层的子元素是项目：

![](https://document.youkeda.com/P3-2-HTML-CSS/1.4/1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

### justify-content 和 align-items

justify-content**控制水平方向分布**

\*>\*注意，justify-content是容器的属性，设置在容器上。

![](https://document.youkeda.com/P3-2-HTML-CSS/1.4/9.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

align-items**调整垂直方向上的分布**

![](https://document.youkeda.com/P3-2-HTML-CSS/1.4/10.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

### flex-wrap

**定义项目是否换行，以及如何换行**

![](https://document.youkeda.com/P3-2-HTML-CSS/1.4/19.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

### flex\:none 和 flex:1

*   不允许项目压缩，放大 **flex\:none**

\*>\*之前我们接触到的属性，justify-content align-items flex-wrap，都是设置在flex容器上的，但是这个控制项目是否被压缩或放大的属性，是设置在flex项目上的。

*   项目自动充满剩余空间 **flex:1**

file:///C:/Users/asd61/Desktop/QQ%E6%88%AA%E5%9B%BE20220806105859.png

    <!DOCTYPE html>

    <html lang="en">

      <head>

            <meta charset="UTF-8">

    ​    <link rel="stylesheet" href="css13.css">

      </head>

      <body>

            <div class="flex-auto">

                <div class="static"></div>

                <div class="flexible"></div>

                <div class="static"></div>

    ​    </div>

      </body>



    </html>



    .flex-auto{

      padding: 4px;

      display: flex;

      background: black;

      height: 100px;

    }

    .flex-auto .static{

      background: #d5e8d4;

      width: 100px;

      flex: none;

    }

    .flex-auto .flexible{

      flex: 1;

      background: #fff2cc;

    }

#### 指定项目放大

如果一行有剩余的情况下，我们可以任意指定某个项目放大盛满剩余空间，只要给指定项目设置flex：1即可。

### 单行文本溢出省略

**1.** 强制不换行

`white-space:nowrap`

**1.** 元素内容溢出

`overflow: visible | hidden | inherit | scroll | auto;`

visible 默认值。内容不会被修剪，会呈现在元素之外。

hidden 内容会被修剪，而且超出的内容不可见。

inherit 规定应该从父元素继承overflow属性的值

scroll 内容会被修剪，浏览器会显示滚动条以便查看超出的内容

auto 由浏览器定夺，如果内容被修剪，就会显示滚动条

**1.** 文本溢出省略

`text-overflow: clip | ellipsis`

*   clip: 默认值，表示内容区域的极限处截断文本，可以简单的理解成超出部分被一刀切掉了。

*   ellipsis：表示用一个省略号（“...”）来表示被截断的文本

    /\* 强制不换行 \*/

    white-space: nowrap;

    /\* 隐藏超出部分 \*/

    overflow: hidden;

    /\* 用省略号代替剩余内容 \*/

    text-overflow: ellipsis;

### 多行文本超出省略

    /* 隐藏超出部分 */

    overflow : hidden;

    /* 文本超出就用省略号 */

    text-overflow: ellipsis;

    /* 把对象作为弹性伸缩盒子模型显示 */

    display: -webkit-box;

    /* WebKit内核的浏览器的私有属性，设置文本超出2行就用省略号 */

    -webkit-line-clamp: 2;

    /* WebKit内核的浏览器的私有属性，设置或检索伸缩盒对象的子元素的排列方式 */

    -webkit-box-orient: vertical;

### SCSS

**什么是SASS**

SASS是一款Css预编译器

*   完全兼容CSS3

*   在CSS基础上增加变量、嵌套（nesting）、混合（mixins）等功能

*   通过![函数](http://sass-lang.com/docs/yardoc/Sass/Script/Functions.html)进行颜色值与属性值的运算

*   提供![控制命令](https://www.sass.hk/docs/#t8)等高级功能

*   自定义输出格式

写好的Sass/Scss文件会在运行代码之后直接编译成同名的Css文件，因此在HTML文件中引入编译后的CSS文件即可。

**什么是SCSS**

SCss其实是SASS 3为了兼容CSS引入的新语法

CSS

    #sidebar {

     width: 30%;

     background-color: #faa;

    }

Sass

    #sidebar

     width: 30%

     background-color: #faa

任何标准的CSS3样式表都是具有相同语义的有效的Scss文件。

**Sass和Scss唯一的不同是，Scss需要使用分号和花括号而不是换行和缩进。**

### SassScript

**变量**

    $width: 10px;



    \#main{

     width: $width;

    }



    编译成CSS



    \#main{

     width: 10px;

    }

用引号的写法

    // 变量需要定义在使用之前

    $position: "absolute";

    $mainTxtColor: '#333';



    // 应用

    \#main {

     position: $position;

     color: $mainTxtColor;

    }

### 插值法

    $name: "mail";

    $top-or-bottom: "top";

    $left-or-right: "left";



    .icon-#{$name} {

     background-image: url("/icons/#{$name}.svg");

     position: absolute;

     \#{$top-or-bottom}: 0;

     \#{$left-or-right}: 0;

    }

**1.** `.icon-#{$name}`:"\$name为mail",编译后选择器为“.icon-mail”;

**1.** `url("/icons/#($name).svg")`:"\$name为mail",编译后图片的路径为"/icon/mail.svg";

**1.** `#{$top-or-bottom}:0;`:"\$top-or-bottom"为"top",编译后声明为“top:0;”

完整的编译结果：

    .icon-mail {

     background-image: url("/icons/mail.svg");

     position: absolute;

     top: 0;

     left: 0;

    }

### 嵌套

**嵌套规则**

![](https://document.youkeda.com/P3-2-HTML-CSS/1.6/3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

**父选择器 &**

![](https://document.youkeda.com/P3-2-HTML-CSS/1.6/4.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

或者特殊一点的用法

![](https://document.youkeda.com/P3-2-HTML-CSS/1.6/5.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

### 复用：mixin/include

```css
@mixin square {

 width: 100px;

 height: 100px;

}
// 应用：

.user-avatar {

 @include square;

}

.admin-avatar {

 @include square;

}

```

编译结果

```css
.user-avatar {

 width: 100px;

 height: 100px;

}



.admin-avatar {

 width: 100px;

 height: 100px;

}


```

**@mixin：定义可复用的样式,@include:应用可复用的样式**

*   带参数

```
@mixin square($size: 100px) {

 width: $size;

 height: $size;

}



// 不传参数就会使用默认的值 100px

.avatar {

 @include square;

}



// 传入参数就会使用传入的值 200px

.avatar-200 {

 @include square($size: 200px);

}


```

编译结果

    /* 不传参数就会使用默认的值 100px */

    .avatar {

     width: 100px;

     height: 100px;

    }

    /* 传入参数就会使用传入的值 200px */

    .avatar-200 {

     width: 200px;

     height: 200px;

    }

### 响应式布局

**什么是响应式网页设计**

就是一个网站能够兼容多个终端，而不是为每个终端做一个特定的版本

响应式布局主要通过规定特定的宽度范围使用特定的布局来实现在不同的设备上应用不同的布局

### 媒体查询

![](https://document.youkeda.com/P3-2-HTML-CSS/1.7/18.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

\*\*逻辑操作符 and \*\*

![](https://document.youkeda.com/P3-2-HTML-CSS/1.7/21.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

### 断点

![](https://document.youkeda.com/P3-2-HTML-CSS/1.7/16.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

**1.** 媒体查询用max-width 表示条件的时候，大的断点放上面。

**1.** 反过来，用min-width 表示条件的时候，小的断点放上面。

![](https://document.youkeda.com/P3-2-HTML-CSS/1.7/17.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

### 新的单位 rem/em

rem是一个相对长度单位，我们熟悉的px可以认为是一个绝对单位。

使用rem作为单位时，rem和px之间的转换比取决于页根元素`<html></html>`的字体大小

**1rem=根元素的字体大小**

\*>\*一般浏览器中根元素的字体默认大小为16px，有些情况下可能为其他大小。最小显示字体一般为10px，即设置10px以下的数值，比如6px，8px等是没有用的，显示的时候都会按照10px的大小进行显示，除非用到了css中的transform（另当别论）。

**单位rem的使用**

    // .title选中大标题“11 powerful examples of responsive web design”

    // .logo选中右上角logo

    @media screen and (max-width: 768px) {

     html {

      font-size: 16px;

     }

    }

    @media screen and (min-width: 768px) {

     html {

      font-size: 20px;

     }

    }



    .title {

     font-size: 2.5rem;

    }

    .logo {

     width: 10rem;

     height: 10rem;

    }

**1.** 浏览器宽度<=678px时，根元素font-size设置为16px，此时：

*   标题的字体大小为2.5\*16px=40px；

*   logo的宽度为10\*16px=160px；

**1.** 浏览器宽度>678px时，根元素font-size设置为20px，此时：

*   标题的字体大小为2.5\*20px=50px；

*   logo的宽高为10\*20ox=200px；

### em

rem是相对于根元素的字体大小转换的。em则是根据父元素的字体大小转换的。

比如

    父元素中的文字

      <div class="child">

      子元素中的文字

     </div>

    </div>
    <div class="parent">



    .parent {

     // 父元素的字体大小为 20px

     font-size: 20px;



     \> .child {

      // 子元素的字体大小为 0.7 * 20px = 14px

      font-size: 0.7em; 

      // 特别注意：子元素的宽度为 10 * 14px = 140px

      width: 10em;

     }

    }

子元素的em转换分为两种：

**1.** 子元素字体大小0.7em

这里的1em和父元素的字体大小相同，为20px，故而子元素的字体大小为0.7\*20px=14px；

**1.** 子元素的宽度10em

这里的1em和子元素的字体大小相同，为14px，故而子元素的宽度为10\*14px=140px；

**注意计算中子元素的宽度计算过程**

> 子元素中除了font-size属性外，其他属性计算的时候是根据子元素本身的字体大小计算的的，但由于子元素本身的字体大小是根据父元素的字体大小计算的，所以可以认为em是根据父元素的字体大小转换的。

### 新的单位 vw/vh

vw和vh也是相对长度单位。

*   1vw=1/100视口宽度；

*   1vh=1/100视口高度；

![](https://document.youkeda.com/P3-3-HTML-CSS/1/3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

**模态框**

![](https://document.youkeda.com/P3-3-HTML-CSS/1/4.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
