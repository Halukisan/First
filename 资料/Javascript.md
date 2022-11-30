### **JavaScript**

**1.** 核心（ECMAScript）

**1.** 文档对象模型（DOM）

**1.** 浏览器对象模型（BOM）

**JavaScript书写**

**1.** 在HTML内部

    <!DOCTYPE html>

    <html lang="en">

     <head>

        <meta charset="UTF-8" />

      <title>Document</title>

     </head>

     <body>

      <!-- 正常的html标签一定要写在script标签的前面 -->

        <div></div>

      <!-- 在body标签的内部并在末尾 -->

        <script></script>

     </body>

    </html>

**1.** 在HTML外部

需要在script标签多一个src参数`<script src='index.js'></script>`

**console 访问控制台**

`console`表示访问控制台

`log()`表示在控制台输出信息

中间用（.）连接

    console.log("要输出的内容");

或者

    <script>console.log("Hello World");</script>

### 模板字符串

    let firstName = "胡";

    let lastName = "雪岩";



    let say = `大家好，我姓${firstName}，名${lastName}`;



    console.log(say);

在模板字符串中使用三元表达式

    let str = `这里是${true ? "江苏" : "浙江"}-${true ? "南京" : "常州"}`;



    console.log(str); // 这里是江苏-南京

### 变量

`let`定义的变量可以被多次重新赋值。

`const`定义的变量只能被赋值一次。

    let name1 = "李天宇";

    let age1 = 12;

    let sex1 = "男";

    let class1 = "六年级三班";



    let name2 = "刘芳";

    let age2 = 13;

    let sex2 = "女";

    let class2 = "六年级三班";



    let name3 = "李锐";

    let age3 = 13;

    let sex3 = "女";

    let class3 = "六年级四班";



    const school = "杭州一中";



    console.log("姓名\t年龄\t\t性别\t\t班级\t\t\t学校");

    console.log(`${name1}\t${age1}\t\t${sex1}\t\t${class1}\t\t${school}`);

    console.log(`${name2}\t${age2}\t\t${sex2}\t\t${class2}\t\t${school}`);

    console.log(`${name3}\t${age3}\t\t${sex3}\t\t${class3}\t\t${school}`);

**终端(terminal)输入:**`node ./index.js`

### 数组元素的操作（增/删/改/查）

**数组元素操作--增**

`push`方法可以在数组的末尾添加值

    变量名.push('要添加的值');

在后面添一个值

    let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];



    // 在末尾添加“河海大学”

    schools.push('河海大学');

    console.log(schools); // 清华大学','北京大学','浙江大学','同济大学','河海大学'

添加多个值

    schools.push('河海大学');

    schools.push('大连理工大学');

    schools.push('哈尔滨工业大学');



    // 上述三步操作可以一次性完成

    schools.push('河海大学', '大连理工大学', '哈尔滨工业大学');

`unshift`方法可以从数组的前面添加值

**数组元素操作--删**

`pop`方法对应的是`push`方法，表示从后往前删除

    let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];



    // 在末尾添加“河海大学”

    schools.push('河海大学');

    console.log(schools); // 清华大学','北京大学','浙江大学','同济大学','河海大学'



    // 从末尾删除一个元素

    schools.pop();

    console.log(schools); // 清华大学','北京大学','浙江大学','同济大学'

`shift`方法从开始位置删除一个元素

`splice`方法

**一个参数**

表示从指定位置开始到结束位置的所有元素，并返回被删除的元素

    let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];



    // 删除从下标为1的位置到结束位置的值

    let deleteSchools = schools.splice(1);

    // 删除之后，原数组中的剩余内容

    console.log(schools); // ["清华大学"]

    // 删除的内容

    console.log(deleteSchools); // ["北京大学", "浙江大学", "同济大学"]

**两个参数**

    let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];



    // 从下标为0开始,往后数两个元素,删除

    let deleteSchools = schools.splice(0, 2);

    console.log(schools); // ['浙江大学', '同济大学']

    console.log(deleteSchools); // ['清华大学', '北京大学']

**数组元素操作--改**

splice方法（需要指定位置的元素）

方法里需要添加三个值

**1.** 第一个值，整数类型，表示起始位置

**1.** 第二个值，整数类型，表示步长（往后选几个元素，1代表往后选1个元素）

**1.** 第三个参数，要替换的数组的值

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-4-HTML-CSS/4/splice.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

**数组的下标在左下角！！！**

案例一

    let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];



    schools.splice(2, 0, '江西理工大学');

    console.log(schools); // ["清华大学", "北京大学", "江西理工大学", "浙江大学", "同济大学"]

案例二

    let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];



    schools.splice(2, 1, '江西理工大学');

    console.log(schools); // ["清华大学", "北京大学", "江西理工大学", "同济大学"]

案例三

    let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];



    schools.splice(2, 2, '江西理工大学');

    console.log(schools); // ["清华大学", "北京大学", "江西理工大学"]

**数组元素操作--查**

`indexOf()`括号里可以写两个参数

*   一个参数



    let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];



    let result = schools.indexOf('大连理工');

    console.log(result); // -1

*   两个参数



    let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];



    let result = schools.indexOf('浙江大学');

    console.log(result); // 2 表示“浙江大学”所在的下标为2



    let schools = ['清华大学', '北京大学', '浙江大学', '同济大学'];



    let result = schools.indexOf('浙江大学', 3);

    console.log(result); // -1

因为外面是从下标为3的位置（同济大学）开始寻找的，所以找不到。

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-4-HTML-CSS/4/splice.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

### For 循环

**for...in和for...of的写法**

1.  for...in



    let peppaFamily = ["佩奇", "乔治", "猪妈妈", "猪爸爸"];



    for (let i in peppaFamily) {

     console.log(peppaFamily[i]);

    }

    // 输出:

    // 佩奇

    // 乔治

    // 猪妈妈

    // 猪爸爸

**1.** for...of

    let peppaFamily = ["佩奇", "乔治", "猪妈妈", "猪爸爸"];



    for (let item of peppaFamily) {

     console.log(item);

    }

    // 输出:

    // 佩奇

    // 乔治

    // 猪妈妈

    // 猪爸爸

> 两者的区别：for...in循环遍历的结果是数组元素的下标，而for...of遍历的结果是元素的值。

### 函数

*   获取随机数

**1.** 先获取\[0,100)之间的随机数

    const num = Math.random() * 1000;

**1.** 再获取\[100,1000)之间的随机数

要获取\[100,1000)的数，那么Math.radom()应该属于\[0.1,1).

    // num1 的范围是 [0.1, 1)

    const num1 = Math.random() * 0.9 + 0.1;

    // num2 的范围是 [100, 1000)

    const num2 = Math.floor(num1 * 1000);



    console.log(num2);

> Math.floor(x)是js的内置方法，返回小于x的最大整数。比如Math.floor(2.3)返回2，Math.floor(4.9)返回4.

### 自定义函数

> 小驼峰命名法：第一个单词首字母小写，后面的其他单词首字母大写。比如：firstName,checkFirstName等。

### typeof

    typeof "John"        // 返回 string 

    typeof 3.14         // 返回 number

    typeof false         // 返回 boolean

    typeof [1,2,3,4]       // 返回 object

    typeof {name:'John', age:34} // 返回 object

*   null

主动释放一个变量引用的对象，表示一个变量不再指向任何对象地址

*   undefined



    typeof``一个没有值的变量会返回``undefined



    let person;

    person = undefined;

    //值是undefined,类型是undefined

### 内置函数————计时器

延时执行 setTimeout()

setTimeout函数用来指定某个函数或某段代码，在多少毫秒之后执行，他返回一个整数，表示定时器的编号，**这个编号以后可以用来取消这个定时器**

![](https://document.youkeda.com/P3-4-HTML-CSS/6/3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

第一个参数func|code是将要推迟执行的函数名或者一段代码，第二个参数delay是推迟执行的毫秒数。

    console.log(1);
    /**

     \* 第一个参数是代码，注意代码需用引号包裹，否则会立即执行代码

     \* 第二个参数是 1000，即 1000ms 后执行 console.log(2)

     */

    setTimeout('console.log(2)', 1000);



    /**

     \* 第一个参数是匿名函数

     \* 第二个参数是 2000，即 2s 后执行 console.log(3)

     */

    setTimeout(function () {

     console.log(3);

    }, 2000);



    // 第一个参数是函数名，注意函数名后不要加小括号“()”，否则会立即执行 print4

    setTimeout(print4, 3000);



    console.log(5);



    function print4() {

     console.log(4);

    }

运行结果

    1;

    5;

    2;

    3;

    4;

**60秒倒计时**

    //首先定义计时总秒数
    let i = 60;

    //定义变量用来存储定时器的编号
    let timerId;

    fuction count(){

     console.log(i);

     i--;

     if(i>0){

      timerId = setTimeout(count,1000);

     }else{

      clearTimeout(timerId);

     }

    }


    count();

无限调用 setInterval

![](https://document.youkeda.com/P3-4-HTML-CSS/6/4.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

    let i = 60;

    print();

    let timer = setInterval(print,1000);



    fuction print(){

     console.log(i);

     i--;

     if(i<1){

      clearInterval(timer);

     }

    }

### 对象的构建

    // 第一步：创建构造函数

    function People(name, age) {

     this.name = name;

     this.age = age;

     run: function() {

      console.log('running');

     }

    }



    // 第二步：通过 new 创建对象实例

    let person = new People('henry', 18);

    console.log(person);



    person.run();

### 对象中属性的读取

**1.**

    let person = {

     name: 'henry',

     age: 18

    }



    console.log(person.name);

    console.log(person['name']);

**2.**

    let person = {

     name: 'henry',

     age: 18

    }



    let variable = 'name';

    console.log(person[variable]);



    variable = 'age';

    console.log(person[variable]);

**3.**

    let person = {

     name: 'henry',

     age: 18,

     parents: {

      papa: 'jack',

      mama: 'mary'

     }

    }



    console.log(person.parents.papa);

    console.log(person['parents']['mama']);

### 对象属性的赋值

    let person = {

     name: 'henry',

     age: 18

    }



    person.name = 'tom';

    person['age'] = 10



    console.log(person.name);

    console.log(person.age);

### 对象属性的查看

查看一个对象本身的所有属性，可以用Object.keys方法：

    let person = {

     name: 'henry',

     age: 18

    }



    console.log(Object.keys(person));

`Object.keys`返回的是一个数组，这个数组由`person`对象的所有属性名构成。

### 属性的删除和增加

    let person = {

     name: 'henry',

     age: 18

    }



    delete person.name;



    console.log(person);

输出：

    { age: 18 }

增加一个属性：

    let person = {

     name: 'henry',

     age: 18

    }



    person.gender = 'male';

输出：

    { name: 'henry', age: 18, gender: 'male' }

### 遍历对象属性

**用`for...in`遍历属性**

    let person = {

     name: 'henry',

     age: 18,

    }



    for (let key in person) {

     console.log('键名：' + key + '；键值：' + person[key]);

    }

**借助`object.keys`遍历属性**

    let person = {

     name: 'henry',

     age: 18,

    }



    let keys = Object.keys(person);



    for (let i = 0; i < keys.length; i++) {

     console.log('键名：' + keys[i] + '；键值：' + person[keys[i]]);

    }

### 对象的继承

> Object是Javascript提供的基本对象，JavaScript的所有其他对象都继承自Object对象，即那些对象都是Object的实例。keys是Object对象的一个静态方法。

    // 字面量

    let o1 = {

     name: 'alice',

    };



    // 构造函数

    let o2 = new Object();

    let o3 = new Object();



    // 继承

    fuction O4() {

    }; 

    O4.prototype = o1; //Property 财产、属性

    let o4 = new O4();

o4对象继承自o1，o1就o4的原型.

### 原型

o1继承自Object，Object就是o1的原型。

### 属性是否存在：in

用in运算符来判断对象是否拥有某个属性：

    let person = {

     name: 'henry',

     age: 18,

    };



    'name' in person;

    'gender' in person;

    'toString' in person;

输出

    true;

    false;

    true; 

`toString`是Object对象的属性。

用Object对象提供的hasOwnProperty方法进行判断

> 查看一个对象本身的所有属性，可以用Object.keys方法

### 自身属性是否存在：hasOwnProperty

    let person = {

     name: 'henry',

     age: 18,

    };



    person.hasOwnProperty('name');

    person.hasOwnProperty('gender');

    person.hasOwnProperty('toString');

输出：

    true;

    false;

    false;

### Object与JSON、Map的区别

**JSON**

JSON是一种轻量级的文本数据交换格式，编程语言间用于传递数据而约定的数据格式。

JSON格式和Javascript对象的转换

**1.** JSON.parse()	:	JSON格式 => JavaScript对象

    // 一个 JSON 字符串

    const jsonStr =

     '{"sites":[{"name":"Runoob", "url":"www.runoob.com"},{"name":"Google", "url":"www.google.com"},{"name":"Taobao", "url":"www.taobao.com"}]}';



    // 转成 JavaScript 对象

    const obj = JSON.parse(jsonStr);

**2.** JSON.stringify()	:	JavaScript对象 => JSON格式

    const jsonStr2 = JSON.stringify(obj)；

**Map**

Map和Object很相似，都可以保存键值对，但是他们仍有些重要的区别：

**1.** 一个Object的键通常是字符串，但一个Map的键可以是任意值，包括函数、对象、基本类型，因此Map会方便很多。

**2.** Map中的键值是有序的，而添加到对象中的键则不是；

**3.** Map的键值对个数可以直接获取，Object则要借助Object.keys()等计算得到

> 查看一个对象本身的所有属性，可以用Object.keys方法

**4.** Map可以直接进行迭代，Object则要借助Object.keys()等；

**5.** Map不存在键名和原型键名冲突问题，可以直接覆盖，Object则不行；

在控制台输出myObj对象的所有非继承属性

    let myObj = new Object();

    let rand = 'random';



    myObj.type = 'Dot syntax';

    myObj['str'] = 'String value';

    myObj[rand] = 'Random Number';



    for (const key in myObj) {

     if (myObj.hasOwnProperty(key)) {

      console.log(key + ': ' + myObj[key]);

     }

    }

输出：

    type: Dot syntax

    str: String value

    random: Random Number

### 内置对象--Math、Storage

**Math**对象

该对象不是构造函数，不能生成实例，所有的属性和方法都必须在Math对象上调用。

常量

Math对象的静态属性

    Math.E // 常数e。

    Math.LN2 // 2 的自然对数。

    Math.LN10 // 10 的自然对数。

    Math.LOG2E // 以 2 为底的e的对数。

    Math.LOG10E // 以 10 为底的e的对数。

    Math.PI // 常数π。

    Math.SQRT1_2 // 0.5 的平方根。

    Math.SQRT2 // 2 的平方根。

静态方法

```
Math.abs() // 绝对值

Math.ceil() // 向上取整

Math.floor() // 向下取整

Math.round() // 四舍五入取整

Math.max() // 最大值

Math.min() // 最小值

Math.pow() // 指数运算

Math.sqrt() // 平方根

Math.log() // 自然对数

Math.exp() // e的指数

Math.random() // 随机数

```

> 注意，以上方法除了Math.random()之外，都需要传入合适的参数，即需要处理的数字。

    Math.ceil(4.6) // 向上取整，取大于等于 x，并且与它最接近的整数。

    Math.floor(4.6) // 向下取整，取小于等于 x，并且与它最接近的整数。

    Math.round(4.6) // 四舍五入取整，取与 x 最接近的整数。

输出：

    5

    4

    5

**Storage对象**

Storage接口用于脚本在浏览器保存数据。两个对象部署了这个接口：`window.sessionStorage`和`window.localStorage`。

`sessionStorage`保存的数据用于浏览器的一次会话（session），当会话结束（通常是窗口关闭），数据被清空；

`localStorage`保存的数据长期存在，下一次访问该网址的时候，网页可以直接读取之前保存的数据。

**数据的存入**`setltem`

    window.localStorage.setItem('myLocalStorage', 'storage Value');

`window.localStorage.setItem('key', 'value');`方法接受两个参数：

**1.** key：键名；

**2.** value：键值

两个参数都是字符串，不是字符串的参数会被转成字符串后再存入浏览器。

> 打开网页的开发者工具（右键=>检查=>application=>Local Storage）,查看存储情况：

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-4-HTML-CSS/7/4.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

注意，如果要存入的数据不是字符串类型的数据，最好先转换成字符串类型，比如要存入一个对象，可以这么写

    const obj = {

     name: 'henry',

     age: 18

    }

    const value = JSON.stringify(obj);

    window.localStorage.setItem('myLocalStorage', value);

> JSON.stringify()方法可以将Javascript值（对象或者数组）转换为一个JSON字符串。

**读取数据：getltem**

    window.localStorage.getItem('myLocalStorage');

`window.localStorage.getItem('key');`接受一个参数，即键名。

**清除缓存：clear**

    window.localStorage.clear();

该方法可以清除所有保存的数据。

### 内置对象——Stirng

Javascript原生提供的三个包装对象之一就是`String`(另外两个是Number、Boolean)。

> 包装对象：原生对象可以把原始类型的值变成（包装成）对象。

    let v2 = new Stirng('abc');

> 包装对象的最大目的：1. 使得JavaScript的对象涵盖所有的值；2. 使得原始类型的值可以方便地调用某些方法（比如下面的）。

**字符串长度：length**

    let len = 'here is an apple'.length；

    字符串中的空格也计算在内。

**查找字符：indexOf（）**

从字符串中查找某个字符串是否存在：

    let str = 'here is an apple';

    const index = str.indexOf('an');

    console.log(index);



    结果：8
    //从0开始数，包括空格在内。

**去掉两端空格：trim()**

trim（）可以把字符串开头和结束位置的空格去掉。

    // 'here' 之前有一个空格，'apple' 之后有三个空格

    let str = ' here is an apple  ';

    const trimedStr = str.trim();

    console.log(str.length);

    console.log(trimedStr.length);



    结果：
    16
    16

> 注意，`trim（）`是去掉字符串前后的空格，不论前后有多少空格都会去掉，但不会去掉中间的空格。另外，`trim（）`不会改变原字符串`str`，而是复制一份原字符串，修改后返回给`trimedStr`。

**截取字符串：substring/substr**

可以截取一个字符串的一部分

    let url = 'https://www.youkeda.com/userhome#collect';



    //首先找到 # 后第一个字母的下标

    const index = url.indexOf('#')+1;



    //有hash才能进行截取，没有就直接提示不存在

    if(index){

     //用substring截取字符串

     const hash1 = url.substring(index,url.length);

     

     //计算hash的长度

     const lenHash = url.length - index;

     //用substr截取字符串

     const hash2 = url.substr(index,lenHash);

     

     console.log(hash1);

     console.log(hash2);

    }else{

     console.log('不存在 hash');

    }

*   substring(start,end)\:start————要截取的字符串的开始下标。end————要截取的字符串的结束下标。

*   substr（start,len）\:start————要截取的字符串的开始下标。

len————要截取的字符串的长度。

> substring和substr的第二个参数不写的时候，会一直截取到字符串结束为止。

它们和`trim`一样都不会改变原字符串，而是返回处理后的字符串。

**分隔字符串：split**

split方法按照给定规则分割字符串，返回一个由分割出来的子字符串组成的数组(不改变原字符串)：

    const splitedStr = 'a|b|c'.split('|');

    console.log(splitedStr);

### 内置对象————Array

**连接数组：join()**

join()方法以指定参数作为分隔符，将所有数组成员链接为一个字符串返回。如果不提供参数，默认用逗号分隔：

    let arr = [1,2,3,4];



    arr.join(" ");//'1 2 3 4'

    arr.join("|");//"1|2|3|4"

    arr.join();"1,2,3,4"

这个方法和字符串里的split方法正好是一对作用相反的方法：

    let str = "a|b|c";



    const splited = str.split("|");

    console.log(splited);



    const joined = splited.join("|");

    console.log(joined);

`join()`方法不会改变数组。

**倒序排列：reverse()**

reverse方法用于颠倒排列数组元素，返回改变后的数组。：

    let arr = ["a","b","c"];



    arr.reverse();//["c","b","a"]

    arr;//["c","b","a"]

> 注意，该方法将改变原数组。

**排序：sort()**

sort方法对数组成员进行排序，默认是按照字典顺序排序。如果想让sort方法按照自定义方式排序，可以传入一个函数作为参数。

    let arr = [

     { name: "jenny", age: 18 },

     { name: "tom", age: 10 },

     { name: "mary", age: 40 },

    ];



    arr.sort(function (a, b) {

     return a.age - b.age;

    });



    console.log(arr);

> 注意，该方法将改变原数组。

**遍历：map/forEach**

有返回值的遍历：map

它接受一个函数，然后将数组的所有成员依次传入这个参数函数，最后把每一次的执行结果组成一个新数组返回：

    let arr = [

     { name: "jenny", age: 18 },

     { name: "tom", age: 10 },

     { name: "mary", age: 40 },

    ];



    //elem:数组成员

    //index：成员下标

    //a:整个数组



    const handledArr = arr.map(fuction(elem,index,a){

     elem.age+=1;

     console.log(elem,index,a);

     return elem.name;

    });



    console.log(arr);

    console.log(handledArr);

map的三个参数：

*   elem: 表示依次传入的数组成员

*   index: 表示依次传入的数组成员所对应的下标

*   a: 表示整个数组

在上面的代码中，map方法的返回值是一个由return后的内容elem。name组成的数组。

无返回值的遍历：forEach

    const handledArr = arr.forEach(fuction (elem,index,a){

     elem.age+=1;

     console.log(elem,index,a);

     retrun elem.name;

    });



    console.log(handledArr);

输出

```
undefined


```

> 注意，当你在map和forEach之间难以选择时，可以想一下是否需要返回值，map会返回操作后的数组，forEach则没有返回值

### 内置对象————Date

**获取当前时间\:new Date()**

我们看把Date作为一个构造函数，用new命令生成一个时间对象的实例，在不加参数的情况下，返回的是当前时间：

    let now = new Date();

    console.log(now);

那如果给构造函数传入一些参数的话，就能够生成特定的时间对象

    // 传入表示“年月日时分秒”的数字

    let dt1 = new Date(2020, 0, 6, 0, 0, 0);

    console.log(dt1);



    // 传入日期字符串

    let dt2 = new Date("2020-1-6");

    console.log(dt2);



    // 传入距离国际标准时间的毫秒数

    let dt3 = new Date(1578240000000);

    console.log(dt3);

> 注意，传入表示“年月日时分秒”的数字时，1.如果只传入一个数字，会被认为传入的是毫秒数；2.月份的范围是0-11，而不是1-12。

**日期运算**

时间差：毫秒数

两个时间对象是可以直接相减的，返回值为两者的毫秒数差：

    let dt1 = new Date(2020, 2, 1);

    let dt2 = new Date(2020, 3, 1);



    // 求差值

    let diff = dt2 - dt1;



    // 一天的毫秒数

    let ms = 24 * 60 * 60 * 1000;



    console.log(diff / ms); // 31

早晚比较：大小于符号

    let dt1 = new Date(2020, 2, 1);

    let dt2 = new Date(2020, 3, 1);



    console.log(dt1 > dt2); // false

    console.log(dt1 < dt2); // true

**解析日期字符串：Date.parse()**

`Date.parse`方法用来解析日期字符串，返回该时间距离时间零点（1970年1月1日 00：00：00）的毫秒数：

    let dt = Date.prase("2020-1-6");

    console.log(dt);1578240000000

### 时间对象转时间字符串：to方法

`toJSON()`方法：

    let dt = new Date();

    let dtStr = dt.toJSON();



    console.log(dtStr); // 2020-01-03T09:44:18.220Z

打印的时间和当前的时间差8个小时，这是因为打印的时间是现在UTC时区的时间，而我们的时间是东八区时间，比国际标准时间快8个小时。

### 获取时间对象的年/月/日：get方法

    let dt = new Date();

    dt.getTime(); // 返回实例距离1970年1月1日00:00:00的毫秒数。

    dt.getDate(); // 返回实例对象对应每个月的几号（从1开始）。

    dt.getDay(); // 返回星期几，星期日为0，星期一为1，以此类推。

    dt.getFullYear(); // 返回四位的年份。

    dt.getMonth(); // 返回月份（0表示1月，11表示12月）。

    dt.getHours(); // 返回小时（0-23）。

    dt.getMilliseconds(); // 返回毫秒（0-999）。

    dt.getMinutes(); // 返回分钟（0-59）。

    dt.getSeconds(); // 返回秒（0-59）。

> 注意，所有的这些get\*方法返回的都是整数，不同方法返回值的范围不一样：分钟和秒：0到59；小时：0到23；星期：0到6；日期：1到31；月份：0到11.

*   除日期外，其他的时间范围都是从0开始的。

`dt,getFullYear()`来获取当前的完整年份：

    let dt = new Date();

    let year = dt.getFullYear();



    console.log(year);

### 设置时间对象的年/月/日\:set方法

    let dt = new Date();

    dt.setTime(ms); // 设置实例距离1970年1月1日00:00:00的毫秒数。

    dt.setDate(date); // 设置实例对象对应每个月的几号（从1开始）。

    dt.setFullYear(year); // 设置四位的年份。

    dt.setMonth(month); // 设置月份（0表示1月，11表示12月）。

    dt.setHours(hour); // 设置小时（0-23）。

    dt.setMilliseconds(ms); // 设置毫秒（0-999）。

    dt.setMinutes(min); // 设置分钟（0-59）。

    dt.setSeconds(sec); // 设置秒（0-59）。

> set方法没有setDay方法，因为星期几是计算得到的。

    let dt = new Date();

    dt.setFullYear(2030);



    console.log(dt);

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-4-HTML-CSS/7/3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

### BOM

浏览器对象模型--BOM

**BOM对象**

*   window（窗口）：window是整个网页的框架，每个网页的内容都是装载在window里面

*   navigator（浏览器）：navigator里面存储浏览器相关信息

*   history（历史）：我们知道每个网页可以前进后退，history便拿来存储争个网页栈的

*   screen（显示屏幕）：screen包含我们显示的屏幕的信息，这个是硬件信息

*   location（地址）：location包含当前访问的地址（网址）信息

![](https://style.youkeda.com/img/course/f4/8/1.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

**1.** screen是整个电脑唯一的

**2.** navigator是整个浏览器唯一的，如果有多个浏览器就会有多个navigator

**3.** window是每个网页唯一的，每个网页都有一个独立的window

**4.** history，location是每个网页的信息，当让也是网页唯一的

详细内容点击->![window](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)

    console.log('优课达');

    window.console.log('优课达');



    console.log(navigator);

    console.log(window.navigator);



    function hello() {}

    console.log(hello);

    console.log(window.hello);

> window是默认对象，如果是调用window上面的方法，可以省略，也可以称为隐式调用window上面的属性和方法。

### Location/History

`Location`对象，用来保存当前网页位置的信息。![Location](https://developer.mozilla.org/zh-CN/docs/Web/API/Location)

![](https://style.youkeda.com/img/course/f4/8/3.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

Location方法

重点掌握--\*\*reload()\*\*刷新当前界面

为了防止无线快速循环，设置一个定时器延时调用reload（）。

    setTimeout(function () {

     window.location.reload();

    }, 3000);

跳转到新的地址

    window.location = 'https://www.youkeda.com';

History

允许操作浏览器的曾经在标签页或者框架里访问的会话历史记录，History会存储该窗口的历史记录。![History](https://developer.mozilla.org/zh-CN/docs/Web/API/History)

如果原始网页为`https://www.youkeda.com`，那history中存储为

    // 会话记录

    ['https://www.youkeda.com'];

如果我们在网页中点击某个链接，或者用`window.location = xxx`跳转到`https://www.baidu.com`,那history中存储为

    // 会话记录

    ['https://www.youkeda.com', 'https://www.baidu.com'];

后续访问，以此类推，在实际存储中用到的数据结构和数组特别类似，叫做**栈**

在history中需要掌握两个方法，**back()和**forward()，分别对应到浏览器左上角的返回和前进按钮。

### Navigator/Screen

Navigator

表示用户代理的状态和表示，也就是浏览器基本信息，了解一个属性--**userAgent**,代表当前浏览器的用户代理

### DOM

文档对象模型 可以将web页面与脚本或者编程语言连接起来

**DOM映射**

像HTML和XML这种形式的文档都是树状结构，也对应数据结构中的树。

    <html>

     <head>

      <title>youkeda</title>

     </head>

     <body>

        <div>

       <h1>优课达</h1>

          <p>学的比别人好一点</p>

      </div>

     </body>

    </html>

![](https://style.youkeda.com/img/course/f4/9/1.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

DOM树特性：

**1.** 树根是DOCUMENT，也称为整个页面文档

**2.** 每个HTML标签我们称之为**DOM节点**，英文为**Node**或者**Element**

**3.** 每个HTML标签包裹的子标签，在树上体现为分支，称为**儿子节点**。

比如上图，`p`和`H1`都是`DIV`的儿子节点。`DIV`同样是`BODY`的儿子节点。

**4.** 儿子节点类推可以得知`p`,`H1`是`BODY`的**孙子节点**。

**5.** 所有`p`,`H1`的长辈，我们称为`p`和`H1`的**祖先节点**。

**6.** `p`,`H1`是亲兄弟，我们称之为**兄弟节点**

### 访问DOM

**获取DOCUMENT**

获取DOM树的根部元素`window.document;`

**选择器查询**

     <h2>每一天，乐在沟通。</h2>

      <a class="free-bright">免费靓号</a>

    </div>
    <div class="subtitle">

如何获取到`subtitle`这个节点

这就需要用到**选择器查询方法---querySelector()**

    document.querySelector('main .core .subtitle');

**迭代查询**

当我们得到subtitle元素后，我们还可以利用这个元素，继续筛选器内部元素，比如获取内部的a标签

    let subtitle = document.querySelector('main .core .subtitle');

    console.log(subtitle.querySelector('a'));

最终可以得到一个`HTMLAnchorElement`节点。

**选择器全量查询**

查询所有满足条件的节点。

比如，查询某个HTML中的所有**input**节点。

    document.querySelectorAll('input');

![](https://style.youkeda.com/img/course/f4/9/9.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

可以发现，我们找到了4个`input`节点，并且查询放回的是一个**类数组**，我们可以直接通过索引访问。

> 类数组，顾名思义类似数组形式，（可以通过索引访问的对象我们都称之为类数组），从JSConsole中我们实际得到的是NodeList对象。

其他筛选方法

`querySelector`和`querySelectorAll`是最新的方法，在此之前还有原生的DOM查询函数

**getElementByld()**：根据id查询某个节点

**getElementByClassName()**:根据**class**查询多个节点

**getElementByTagName()**:根据**标签名**查询多个节点

querySelector（All）和getElementXXX最主要的区别在于---**动态性**

> querySelector查询出来的元素是拷贝的原始数据，不会在随着页面DOM节点的改变而变化。get系列方法查询出来的元素就是原始数据，所以会随着界面的DOM节点的改变而变化。

### DOM属性

**DOM种类**

    <!-- HTMLDocument 根文档 -->

    <html>

     ……

    </html>



    <!-- HTMLDivElement DIV类型 -->

    <div class="subtitle">

     ……

    </div>



    <!-- HTMLAnchorElement 超链接类型 -->

    <a class="free-bright">免费靓号</a>



    <!-- HTMLInputElement Input类型 -->

    <input class="password" type="pasworkd" placeholder="请输入密码" />

DOM属性

DOM类别

**1.** 元素节点

**2.** 特性节点

**3.** 文本节点

```
<!DOCTYPE html>

<head>

  <meta charset="UTF-8" />

 <title>优课达</title>

</head>

<body>

  <div id="test">优课达</div>

  <script src="./index.js"></script>

</body>


```

    let divDom = document.querySelector('dic#test');

    console.log(divDom.nodeType,divDom.nodeName,diDom.nodeValue);



    //获取DIV节点的第一个儿子节点，代表‘优课达’这个字符串

    let txtDom = divDom.firstChild;

    console.log(txt.nodeType,txtDom.nodeName,txtDom.nodeValue);



    //获取DIV节点的id属性

    let attDom = divDom.attributes.id;

    console.log(attDom.nodeType,attDom.nodeName,attDom.nodeValue);

总结特性如下：

**1.** 整个HTML中，无论是标签，标签属性，还是纯文本字符串都是`Element`,不同的地方在于`nodeType`分别为`1,2,3`。

**2.** HTML标签都是**元素节点**，可以用`nodeName`获取标签名称

**3.** 纯文本都是**文本节点**,可以用`nodeValue`获取文本内容

**4.** 标签的每个属性都是特性节点，可以用`nodeName`获取属性Key，

用`nodeValue`取得属性Value

**5.** `attributes`可以获取元素节点的所有属性，得到的结果是一个字典，通过属性key获取对应的特性节点

    <!DOCTYPE html>

    <head>

      <meta charset="UTF-8" />

     <title>优课达</title>

    </head>

    <body>

      <div id="test">

      优课达

        <p>youkeda</p>

        <p>学的比别人好一点</p>

     </div>

      <script src="./index.js"></script>

    </body>



    let divDom = document.querySelector('div#test');

    console.log(divDom.outerHTML, divDom.innerHTML, divDom.innerText);

outerHTML

    <div id="test">优课达<p>youkeda</p><p>学的比别人好一点</p></div> 

**整个DOM的HTML代码**

innerHTML

    优课达<p>youkeda</p><p>学的比别人好一点</p>

**DOM内部HTML代码**

innerText

    优课达

**DOM内部纯文本内容**

DOM亲属

    let divDom = document.querySelector('div#test');

    console.log(divDom.firstChild, divDom.lastChild);

    console.log('-----');

    console.log(divDom.childNodes);

    console.log('-----');

    console.log(divDom.parentNode);

firstChild  `优课达` 指定节点的第一个子节点

lastChild  `<p>学的比别人好一点</p>` 指定节点的最后一个子节点

childNodes `优课达<p>youkeda</p><p>学的比别人好一点</p>` 指定节点的字节带你的集合

parentNode `<body><div id="test">优课达<p>youkeda</p><p>学的比别人好一点</p></div><script src="./index.js"></script></body>` 指定节点在DOM树中的父节点

### DOM样式

```
<!DOCTYPE html>

<head>

  <meta charset="UTF-8" />

 <title>优课达</title>

</head>

<body>

 <h1 class="test youkeda" style="color: #FF3300; font-size: 24px;">优课达</h1>

  <script src="./index.js"></script>

</body>


```

    const h1Dom = document.querySelector('h1');

    console.log(h1Dom.classList);

    console.log(h1Dom.style);

    console.log(h1Dom.style.color);

classList `DOMTokenList`类数组 `['test', 'youkeda']` classList数组方式存储所有的class名称

style `CSSStyleDeclaration` `color`属性为`rgb(255, 51, 0)` 对象或字典的方法存储CSSStyle

**DOM数据属性**

HTML提供一种数据属性的标准，利用`data-*`允许我们在标准内于HTML元素中存储额外的信息。

    <!DOCTYPE html>

    <head>

      <meta charset="UTF-8" />

     <title>优课达</title>

    </head>

    <body>

     <article data-parts="3" data-words="1314" data-category="python">

      ...

     </article>

      <script src="./index.js"></script>

    </body>

语义化标签，`article`一般用于放置文章区域。

对文章而言，除了文章内容，我们还有其他额外数据，例如：段落，字数，分类等。

我们可以用`data-*`来存储

    <article data-parts="3" data-words="1314" data-category="python"></article>



    const article = document.querySelector('article');

    console.log(article.dataset);

`dataset`是个Map对象，它是`data-*`这个`*`的**Key-Value**集合

### DOM操作

DOM样式修改

![](https://style.youkeda.com/img/course/f4/9/17.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

![](https://style.youkeda.com/img/course/f4/9/18.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

> 默认未选中的状态如左，点击选中的状态如右。

代码中`<div class="select"></div>`是那个圆圈选择框。

\*>\*在点击的时候，向`select`插入一个`img`节点，渲染选中的打钩图片。然后再次点击的时候清除`select`内部节点。

**1.** 在JavaScript创建节点（在这里创建`img`节点）

**2.** 设置节点样式，属性（`img`节点设置`src`属性）

**3.** 在已存在节点内部添加子节点（`img`节点需要添加到`select`中）

**4.** 清空节点内部子节点（再次点击时清空`select`的子节点）

    // 保存当前是否选中的状态

    let isSelected = false;



    // 获取整个元素的节点

    const box = document.querySelector('.box');



    // 获取select框节点

    const select = document.querySelector('.select');



    // 给整个元素添加点击事件【大家可以先忽略这部分】

    box.addEventListener('click', function () {

     // 点击以后触发这个函数



     // 修改当前选中状态，取反即可

     isSelected = !isSelected;



     // 如果当前是选中状态、则添加img到select中

     if (isSelected) {

      // 创建一个img标签节点

      const img = document.createElement('img');



      // 设置img的src属性和样式，让其撑满select框

      img.src = 'https://style.youkeda.com/img/sandwich/check.png';

      img.setAttribute('style', 'width: 100%; height: 100%;');



      // 将这个节点添加到select框中

      select.appendChild(img);

     } else {

      // 如果不是选择状态，则清空内部子元素

      select.innerHTML = '';

     }

    });

**1.** 创建标签节点

**document.createElement(tagName)**

> 此方法用于创建一个由标签名称tagName指定的HTML元素，也就是元素（标签）节点。

如果想创建一个`div`标签：

    const div = document.createElement('div');

**document.createTextNode(string)**

可以用此方法在div标签内部添加纯文本内容

    const div = document.createElement('div');

    const txt = document.createTextNode('hello你好');

继续把txt添加到div中，把div添加到body中

    const div = document.createElement('div');

    const txt = document.createTextNode('hello 你好');

    div.appendChild(txt);

    document.body.appendChild(div);

**2.** 添加新节点

**appendChild(newNode)**

此方法可以往该节点中插入儿子节点

**inserBefore(newNode,referenceNode)**

此方法与`appendChild()`刚好相反，`appendChild`是在所有儿子节点之后添加，`inserBefore`是在某个目标儿子节点之前添加。

`inserBefore(newNode,referenceNode)`,需要两个参数，`newNode`表示新节点，`referenceNode`表示目标节点，也就是新节点插入到目标节点之前。

    <!DOCTYPE html>

    <head>

      <meta charset="utf-8" />

     <title>优课达</title>

    </head>

    <body>

     <ul class="root">

      <li class="sars">SARS</li>

     </ul>

      <script src="./index.js"></script>

    </body>

根据病毒严重性，最终的顺序应为：

    <ul class="root">

     <li>新型冠状病毒</li>

     <li>SARS</li>

     <li>H1N1</li>

    </ul>



    const root = document.querySelector('ul.root');

    const sars = document.querySelector('li.sars');



    // 创建 H1N1

    const H1N1 = document.createElement('li');

    const H1N1Txt = document.createTextNode('H1N1');

    H1N1.appendChild(H1N1Txt);

    root.appendChild(H1N1);



    // 创建 新型冠状病毒

    const nCoV = document.createElement('li');

    const nCoVTxt = document.createTextNode('新型冠状病毒');

    nCoV.appendChild(nCoVTxt);

    root.insertBefore(nCoV, sars);

或者

    function createDisease(txt) {

     const dom = document.createElement('li');

     const domTxt = document.createTextNode(txt);

     dom.appendChild(domTxt);

     return dom;

    }



    const root = document.querySelector('ul.root');

    const sars = document.querySelector('li.sars');



    // 创建 H1N1

    const H1N1 = createDisease('H1N1');

    root.appendChild(H1N1);



    // 创建 新型冠状病毒

    const nCoV = createDisease('新型冠状病毒');

    root.insertBefore(nCoV, sars);

**3.** 设置样式、属性

    img.setAttribute('style','width:100%;height:100%;');

`dom.style`是一个Map对象，如果不想全量替换样式，我们可以单独设置某些属性：

    dom.style.color = 'xxxx';

**setAttribute()** 不仅仅可以设置`style`之外，所有HTML属性都能用他设置，比如`id`,`src`,`type`,`disabled`,etc...

**classList**

能获得到DOM上所有的类，把样式写成CSS，然后在此处只用添加或者删除class。

![classList](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/classList)

**4.** innerHTML

在上面的代码中，我们使用`innerHTML = ''`清空`select`节点所有的后代内容

可以用`innerHTML`,给某个元素添加内容：

    fuction createDisease(txt){

     const dom = document.createElement('li');



     //我们可以直接用innerHTML设置其纯文本

     dom.innerHTML = txt;

     return dom;

    }

### 时钟

    <!DOCTYPE html>

    <html lang="en">

    <head>

        <meta charset="UTF-8">

        <meta http-equiv="X-UA-Compatible" content="IE=edge">

        <meta name="viewport" content="width=device-width, initial-scale=1.0">

      <title>Document</title>

    </head>

    <body>

        <style id="style">

    ​    ul{

    ​      list-style: none;

    ​    }

    ​    \#circle{

    ​      width: 200px;

    ​      height: 200px;

    ​      border-radius: 100px;

    ​      border: 1px solid black;

    ​    }

    ​    \#kedu li{

    ​      width: 1px;

    ​      height: 6px;

    ​      border-radius: 10px;

    ​      background-color: black;

    ​      transform-origin: center 101px;/*设置li标签的旋转中心和旋转半径。*/

    ​      position: absolute;

    ​      left: 109px;

    ​      top: 9px;

    ​    }

    ​    \#kedu li:nth-of-type(5n+1){

    ​      height: 12px;

    ​      width: 2px;

    ​    }



    ​    /* 秒针的绘制，用transform把div绘制成线条，后面的指针都是在这样。 */

    ​    \#second{

    ​      width: 2px;

    ​      height: 80px;

    ​      background-color: red;

    ​      transform: scaleY(1);

    ​      position: absolute;

    ​      left: 108px;

    ​      top: 30px;

    ​      transform-origin: bottom; /*设置它们的旋转中心，transform-origin: bottom；意思是以它们的底部为中心旋转。*/

    ​    }

    ​    \#min{

    ​      width: 2px;

    ​      height: 65px;

    ​      background-color: gray;

    ​      transform: scaleY(1);

    ​      position: absolute;

    ​      left: 108px;

    ​      top: 45px;

    ​      transform-origin: bottom;

    ​    }

    ​    \#hour{

    ​      width: 2px;

    ​      height: 50px;

    ​      background-color: black;

    ​      transform: scaleY(1);

    ​      position: absolute;

    ​      left: 108px;

    ​      top: 60px;

    ​      transform-origin: bottom;

    ​    }

    ​    \#p12{

    ​      position: absolute;

    ​      left: 100px;

    ​      top: 0px;

    ​    }

    ​    \#p3{

    ​      position: absolute;

    ​      left: 190px;

    ​      top: 84px;

    ​    }

    ​    \#p6{

    ​      position: absolute;

    ​      left: 105px;

    ​      top: 165px;

    ​    }

    ​    \#p9{

    ​      position: absolute;

    ​      left: 20px;

    ​      top: 82px;

    ​    }

      </style>

        <div id="circle">

    ​    <ul id="kedu"></ul>

      </div>

        <div id="second"></div><!--绘制秒针-->

        <div id="min"></div><!--绘制分针-->

        <div id="hour"></div><!--绘制时针-->

        <p id="p12">12</p>

        <p id="p3">3</p>

        <p id="p6">6</p>

        <p id="p9">9</p>

        <script>

    ​    //绘制时钟的刻度 动态创建60个li标签。

    ​    function li(){

    ​      let ul=document.getElementById("kedu");//先获取到ul，因为要在ul下创建li。

    ​      let css;//用来存li的style样式中的CSS设置。

    ​      for(let i=0;i<60;i++){

    ​        css+=`#kedu li:nth-of-type(${i+1}){transform:rotate(${i*6}deg)}`//循环设置ul下的第i+1个li的旋转角度，要在css中设置了li的旋转中心

    ​        ul.innerHTML+=`<li></li>`;//这里要用+=，如果直接用=，只会创建一个li，因为会覆盖前面的li，为了不出现覆盖就用+=。

    ​      }

    ​      let sty=document.getElementById("style")//这里获取到style标签。

    ​      sty.innerHTML+=css //把ul下的li标签的css样式写入到style里。

    ​    }

    ​    li();//这里结束就把刻度画好了。



    ​    function time(){

    ​      let s=document.getElementById("second");//获取到时分秒的三个指针，后面用来动态让它们旋转起来。

    ​      let m=document.getElementById("min");

    ​      let h=document.getElementById("hour");



    ​      //获取时间。

    ​      let date=new Date();

    ​      let snum=date.getSeconds();//获取现在是多少秒。

    ​      let mnum=date.getMinutes()+snum/60;//获取现在是多少分，不能忘记加上 秒数/60。

    ​      let hnum=date.getHours()+mnum/60; //获取现在是多少时，不能忘记加上 分钟数/60。

    ​      

    ​      s.style.transform=`rotate(${snum*6}deg)`;//设置的trasnform就可以让它们旋转起来，秒针时一秒旋转6度。

    ​      m.style.transform=`rotate(${mnum*6}deg)`//分针也是一分钟旋转6度。

    ​      h.style.transform=`rotate(${hnum*30}deg)`//这里时小时，一小时旋转30度，所以*30.

    ​    }

    ​    setInterval(time,100)//用计时器每100ms运行这个time函数。

    ​    

      </script>

    </body>

    </html>

### DOM事件

DOM绑定事件

    //监听input输入事件

    dom.addEventListener("input",fuction(){});



    //监听鼠标放置，移动事件

    dom.addEventListener("mouseover",fuction(){});

我们可以看到DOM可以通过`addEventListener(eventName,callback)`绑定`eventName`事件。

**DOM事件**

属性          值             解释

target  `<h1>优课达-学的比别人好一点</h1>`  点击事件触发的DOM节点

type        `click`         事件名称

pageX/pageY     `92/47`         鼠标事件触发的页面坐标

![事件](https://developer.mozilla.org/zh-CN/docs/Web/Events)

### 焦点事件

**focus**:表单组件（input，Textarea，etc...）获取焦点事件**blur**：表单组件（input，Textarea，etc...）失去焦点事件

#### 鼠标事件

**click**点击事件

**dblclick**双击事件

**mousedown**在元素上按下任意鼠标按钮

**mouseenter**指针移到有事件监听的元素内。

**mouseleave**指针移出元素范围外（不冒泡）。

**mousemove**指针在元素内移动时持续触发。

**mouseover**指针移到有事件监听的元素或者它的子元素内。

**mouseout**指针移出元素，或者移到它的子元素上。

**mouseup**在元素上释放任意鼠标按键。

#### 键盘事件

**keydown**键盘按下事件

**keyup**键盘释放事件

#### 视图事件

**scroll**文档滚动事件

**resize**窗口放缩事件

#### 资源

**load**资源加载成功的事件

### 点击事件

> 监听DOM事件，使用DOM操作，修改ODM属性

    // 默认是未点击喜欢

    let hasLike = false;



    const likeBtn = document.querySelector(".like-btn");

    const likeBtnRight = likeBtn.querySelector(".right");

    likeBtn.addEventListener("click", function () {

     // 点击事件

     hasLike = !hasLike;

     if (hasLike) {

      likeBtn.classList.add("has-like");

      likeBtnRight.innerHTML = parseInt(likeBtnRight.innerText.trim()) + 1;

     } else {

      likeBtn.classList.remove("has-like");

      likeBtnRight.innerHTML = parseInt(likeBtnRight.innerText.trim()) - 1;

     }

    });

> `parseInt()` 函数解析字符串并返回整数。

> `likeBtnRight.innerText`选中了点赞按钮里面的text内容，即点赞数量.

> `trim()` 方法用于删除字符串的头尾空白符，空白符包括：空格、制表符 tab、换行符等其他空白符等。

`trim()` 方法不会改变原始字符串。

`trim()` 方法不适用于 null, undefined, Number 类型。

### 冒泡、捕获、委托

在JS中添加点击事件

    const workspace = document.querySelector('.workspace');

    workspace.addEvenListener('click',fuction(){

     window.location.href = 'https://www.youkeda.com';

    });

![](https://style.youkeda.com/img/course/f4/10/7.gif)

**冒泡**

**1.** 点击事件触发`button.like-btn`监听事件

**2.** 冒泡找到其父亲节点，触发父亲节点的监听事件

**3.** 依次冒泡直到html根元素为止。

**阻止冒泡-e.stopPropagation();**

    likeBtn.addEventListener('click',fuction(e){

     e.stopPropagation()

    });

**捕获**

捕获和冒泡是完全相反的，冒泡是从当前元素沿着祖先节点往上冒泡，而捕获是从根HTML节点开始依次移动到当前元素。

上面使用的`addEventListener`是在冒泡阶段监听事件，如果想在捕获阶段监听事件，需要传递第三个参数为`true`：

    dom.addEventListener('click',fuction(){},true);

**委托**

委托是冒泡事件的一种应用，如果想在大量子元素中单击任何一个都可以运行一段代码，可以将事件监听器设置在其父节点上，并让子节点上发生的事件冒泡到父节点上，而不是每个子节点单独设置事件监听器。

上一节代码：

    const box = document.querySelector('.box');

    const imgArr = box.children;



    for(let i = 0;i<imgArr.length;i++){

     imgArr[i].addEventListener('click',fuction(){

      document.body.style.backgroundImage = `url(${imgArr[i].src})`;

     });

    }

用委托：

    const box = document.querySelector('.box');



    box.addEventListener('click',fuction(e){

     if(e.target.nodeName === 'IMG'){

      documen.body.style.backgroundImage = `url(${e.target.src})`;

     }

    })

### 移动事件

**鼠标移动事件**

（上面也有提及）

**1.** mousemove

> 这个是鼠标移动事件

**2.** mouseenter/mouseleave

> 这个是鼠标进入和离开事件，但是仅仅只作用于当前DOM节点，不会作用于其后代节点

**3.** mouseover/mouseout

> 这个也是鼠标进入和离开事件，但和`enter/leave`不同的是：此事件除了作用于当前DOM节点，也会同时作用于其后代节点

### 表单元素事件

焦点事件

获取焦点和失去焦点两个事件---**focus**和**blur**

我们给QQ注册页的昵称输入框加入焦点事件：

    const nick = document.querySelector('input.nick');

    nick.addEventListener('focus',fuction(){

     console.log('获取焦点');

    });

    nick.addEventListener('blur',fuction(){

     console.log('失去焦点');

    });

对于表单元素，我们有两种事件可以监听元素内容变化---**input**和**change**。

    const nick = document.querySelector('input.nick');

    nick.addEventListener('input',fuction(){

     console.log('-----input');

     console.log(nick.value);

    });

我们发现，只要在输入框里面输入值，就会触发**input**事件。而要等到输入框失去焦点，才会触发**change**事件。

*   change ：当用户提交对元素值的更改时触发；change事件不一定会对元素值的每次更改触发

案例：

**1.** checkbox值修改了以后

**2.** selec选择后

**3.** input内容修改并失去焦点

*   input：只要value值修改就会触发

![](https://style.youkeda.com/img/course/f4/10/13.gif)

**1.** 监听`mobile-input`的`input`事件

**2.** 在监听回调里面判断`input.value`值，如果不是空或者11位，则提示错误信息。

**3.** 错误信息放置在`<p class="error" style="color: red"></p>`中，提示信息为`手机号码格式不正确`

    const inputs = document.querySelector('.mobile-input');

    const errors = document.querySelector('p.error');



    inputs.addEventListener('input',fuction(){

     if(inputs.value.length !== 11 && inputs.value !== ''){

      errors.innerHTML = '手机号码格式不正确';

     }

     else{

      errors.innerHTML = '';

     }

    });

### 滚动事件

**1.** 无滚动

在很多网站看到过，网页每次滚动到底部，会加载新内容，再次滚动到底部，又会加载新内容。

**2.** 动态效果

滚动一般用于展示一些动态效果，比如**知乎头部**，效果图如下：

![](https://style.youkeda.com/img/course/f4/10/14.gif)

当我们发现当页面滚动时，知乎头部会切换成新的头部。

### 事件描述

滚动事件，事件名称————**scroll**。

    window.addEventListener('scroll',fuction(){

     console.log(window.scrollY);

    });

当我们滚动页面，我们发现JSConsole里会陆续打印一些数字，这些数字就是scrollY（Y轴滚动距离）。

### 无尽滚动

当页面快滚动到底部时，添加新的文章内容到body中。

     window.addEventListener('scroll',fuction(){

     //可以通过clientHeight获取内容高度

     const height = document.body.clientHeight;



     //通过screen.height获取浏览器的高度

     const screenHeight = window.screen.height;



     //当距离底部的距离小于500时，触发页面新增内容

     if(height - window.scrollY - screenHeight < 500>){

      console.log('加载新文章内容');

      //在底部添加10张图片

      const div = document.createElement('div');

      let str = '';

      for(let i = 0;i < 10;i++>){

       str += `

    ​    <img

    ​    class = "first"

    ​    alt = ""

    ​    src = "https://document.youkeda.com/P3-1-HTML-CSS/1.8/1.jpg?x-oss-process=image/resize,h_300"

    ​    />

    ​    `;

      }

      div.innerHTML = str;

      document.body.appendChild(div);

     }

    });

上面代码主要使用到的技术

**1.** 内容高度`document.body.clientHeight`

**2.** 浏览器高度`window.screen.height`

**3.** 滚动距离`window.scrollY`

**4.** 滚动距离底部距离`内容高度 - 浏览器高度 - 滚动距离`

### 键盘事件

**1.** **键盘事件**![](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent)

### 知乎头部模拟

    const core = document.querySelector('.core');



    window.addEventListener('scroll',fuction(){

     const scrollY = window.scrollY;

     if(scrollY < 100){

      core.style.transform = 'translateY(0)';

     }

     else{

      core.style.transform = 'translateY(-52px)';

     }

    });

### 协议

协议，网络协议的简称，网络协议是通信计算机双方必须共同遵守的一组约定。如怎么样建立连接，怎么样互相识别。

### HTTP/HTTPS协定

![文档](https://ham.youkeda.com/articles/detail/5f3756865e205f30b2c2b0f5)

### URL

英文全称是： `Uniform Resource Locator`(统一资源定位符)。

![](https://style.youkeda.com/img/course/f4/11/py2-1-2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

格式说明:

*   协议类型与域名之间以`://`(固定写法)分隔。

*   路径（英文常称为`path`）以单斜杠`/`开头，中间每层的分隔符也是单斜杠`/`。

*   路径相当于一层一层的文件夹，但要注意与windows的文件夹分隔符`\`不要混淆了。

*   参数：

*   路径与参数之间用`？`分隔。看到问号`？`就知道后面的内容就是参数了。

*   多个参数之间用`&`分隔

*   参数用"参数名 = 参数值"（`key=value`）的格式表示

### 端口号

域名后的`：443`表示网站的端口号。HTTP协议默认的端口号是`80`，HTTPS协议默认的端口号是`443`.默认的端口号在URL中是可以省略的，其它的端口号就必须要写明了。

**路径的两种情况**

**1.** 相对路径

不是以斜杠`/`开头的路径，表示相对路径。

建议书写URL时一定要写以斜杠/开头的绝对路径。

**2.** 默认路径

我们在上网时，仅仅输入了<https://www.douban.com或https://www.douban.com/都能打开豆瓣首页。在没有输入路径时，表示请求网站的默认页面，那么服务器就会返回一个默认页面给浏览器。>

### API + GET请求

API （Application Programming Interface），应用程序接口，

API一般是指一些预先定义的函数，目的是可以为开发人员快速访问某一程序，而无需了解和访问源码，或理解它内部工作机制的细节

简单的讲，**API可以快速调用某个程序**

![文档](https://ham.youkeda.com/articles/detail/5f3756bd5e205f30b2c2b125)

### fetch调用API

我们看一个显示优课达公司信息的`API`:

<https://www.fastmock.site/mock/b73a1b9229212a9a3749e046b1e70285/f4/f4-11-1-1>

把这个URL贴到浏览器，我们可以看到查询返回结果为：

![](https://style.youkeda.com/img/course/f4/11/1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

所谓API，本质上就是一个URL。开头也是http（或https），只是返回的内容有明显的区别，没有大量多余的字符。

API返回的内容统称为数据，我们可以用`fetch`这个方法获取这部分数据：

    fetch(

     'https://www.fastmock.site/mock/b73a1b9229212a9a3749e046b1e70285/f4/f4-11-1-1'

     ).then(fuction(response){

      return response.json();

     }).then(fuction(myJson){

      console.log(myJson);

     });

`fetch()`返回结果是一个`Promise`对象

**Promise**是异步编程的一种解决方案，之前异步编程是通过回调方法来实现的。

Promise对象可以通过`.then`触发回调函数，

在上面`response.json()`返回的也是一个Promise对象，所有后续可以继续使用`.then`触发后续回调。

**一个完整的POST请求和响应的过程**

```
        var url = "/fetch";
          fetch(url,{
              method:"post",
              headers:{
                  "Content-type":"application/x-www-form-urlencoded"
              },
              body:"name=luwenjing&age=22"
          })
            .then(function (response){
                if (response.status == 200){
                    return response;
                }
            })
            .then(function (data) {
              return data.text();
            })
            .then(function(text){
                console.log("请求成功，响应数据为:",text);
            })
            .catch(function(err){
                console.log("Fetch错误:"+err);
            });

```

（1）fetch的参数有两个，第一个是URL即请求的处理路径

第二个是初始化信息，包括：

*   method：请求方法，常用的有get和post
*   headers：请求头信息，最常用的就是表格式的数据：“Content-type”:"

    application/x-www-form-urlencoded”；
*   mode:控制是否允许跨域。

    same-origin（同源请求）、no-cors（默认）和cros（允许跨域请求）；
*   cache：关于缓存的一些设置
*   body：要发送到后台的参数，可以为ArrayBuffer,String,FormData等类型。

（2）从上面的代码我们可以知道fetch() 方法返回的是一个promise对象；

（3）响应信息为传入then方法成功时的参数，相应包含很多HTTP的响应信息。

（4）可以判断响应的状态码，返回请求成功的对应信息；

（5）通过状态转换，转换为指定的格式，如果是文本信息，直接text（）方法就可以；若为json格式，则json()就可以转换为json格式信息，其实也就是自己请求数据格式时所设置的格式

### GET请求

像上面这种请求数据的接口，我们一般称为`GET接口`，而fetch在不指定类型是，默认发起GET请求。

### POST请求

![文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)

    // 把JSON数据序列化成字符串

    const data = JSON.stringify({

     username: 'admin',

     password: '123456'

    });



    fetch(

     'https://www.fastmock.site/mock/b73a1b9229212a9a3749e046b1e70285/f4/f4-11-4-1',

     {

      method: 'POST',

      body: data,

      headers: {

       'content-type': 'application/json'

      }

     }

    )

     .then(function(response) {

      return response.json();

     })

     .then(function(myJson) {

      console.log(myJson);

     });

例子：

```
```

fetch(

'<https://www.fastmock.site/mock/b73a1b9229212a9a3749e046b1e70285/f4/f4-11-5-10>'

).then(function(response){

return response.json();

}).then(function(myJson){

console.log(myJson);

});

const data = JSON.stringify(myJson);

const firstMenuBox = document.querySelector('.first');

const secondMenuBox = document.querySelector('.second');

firstMenuBox.innerHTML = '';

secondMenuBox.innerHTML = '';

function createItem(item) {

const li = document.createElement('li');

li.innerHTML = \`

      \<div class="left">

​    \<img

​     class="icon"

​     src="\${item.icon}"

​    />

​    <span>\${item.title}</span>

   </div>

​    \${

​     item.children && item.children.length > 0

​      ? \`

​    \<img

​     class="more"

​     src="<https://style.youkeda.com/img/pizza/context-menu/more.png>"

​    />\`

​      : ''

​    }\`;

return li;

}

for (let i = 0; i < data.length; i++) {

const li = createItem(data\[i]);

firstMenuBox.appendChild(li);

li.addEventListener('mouseover', function() {

​    if (data\[i].children && data\[i].children.length > 0) {

​     secondMenuBox.innerHTML = '';//innerHTML：整个secondMenuBox的HTML代码

​     secondMenuBox.style.display = 'block';

​     for (let j = 0; j < data\[i].children.length; j++) {

​      secondMenuBox.appendChild(createItem(data\[i].children\[j]));

​      //appendChild(newNode)往该节点中插入儿子节点

​     }

​    } else {

​     secondMenuBox.style.display = 'none';

​    }

});

}
