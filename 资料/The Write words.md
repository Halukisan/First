
###### 注册栏放到中间
```
steJustifyContentMode(JustifyContent.CENTER);
setAlignItems(Aligenment.CENTER);
```
###### 删除忘记密码
```
LoginForm loginForm = new LoginForm();
ps：前面需要有这个

loginForm.setForgotPasswordButtonVisble(false);
add(loginForm);
```
###### 建立一个大的中间的标题
```
 H1 h1 = new H1("TODO");
```
>形成一个中间的一个大的居中的TODO标题

###### 创建一个输入框和按钮
```
TextField todoField = new TextField();
Button addButton= newButton(text:"Add");
```
>创建了一个Add的按钮

###### java数组定义方法
```
public static void main(String[] args) {
  String[] tables = new String[]{"张三","李四", "王五"};

>除此之外在一个函数中 return name//返回数组名
>这样做可以直接把整个数组全部返回到主函数中

```
######## 数组扩展
```
for (String name : tables) {

}
等同于
for(int i = 0; i < tables.length; i++){
  String name = tables[i];
}
```
![增强for循环](https://ham.youkeda.com/articles/detail/5f3757005e205f30b2c2b140)


###### 取出字符串中的一个字 charAt
```
public static void main(String[] args) {

  String message = "Hello Java";
  // 取出第一个字
  char str = message.charAt(0);
  System.out.println(str);
  // 取出第二个字
  str = message.charAt(1);
  System.out.println(str);
}
```
###### 去掉左右多余的空格 trim
```
public class Test721 {

  public static void main(String[] args) {

    String str = " 优课达 ";
    // 打印一下现在的length
    System.out.println(str+"的长度是:"+str.length());

    // 处理一下去空格
    String newStr = str.trim();
    // 打印一下新字符串的length
    System.out.println(newStr+"的长度是:"+newStr.length());
    // 对比再打印一下老的字符串的length
    System.out.println(str+"的长度是:"+str.length());

  }
```
###### 查找字符串 indexOf("字符串")
* 如果返回值是-1说明不匹配，如果返回值不是-1，那说明匹配到了  
* indexOf("字符串"，"开始索引值")第二个参数是一个数字类型，用于设定从什么位置开始查找。所以我们找到第一个匹配到的索引+匹配字符串长度就是开始值了，这个时候查找的就是第二个匹配内容了。
```
 public static void main(String[] args) {
    String str = "Java是一种广泛使用的计算机编程语言，拥有跨平台、面向对象、泛型编程的特性，广泛应用于企业级Web应用开发和移动应用开发。任职于太阳微系统的詹姆斯·高斯林等人于1990年代初开发Java语言的雏形，最初被命名为Oak，目标设置在家用电器等小型系统的编程语言，应用在电视机、电话、闹钟、烤面包机等家用电器的控制和通信。由于这些智能化家电的市场需求没有预期的高，Sun公司放弃了该项计划。随着1990年代互联网的发展，Sun公司看见Oak在互联网上应用的前景，于是改造了Oak，于1995年5月以Java的名称正式发布。Java伴随着互联网的迅猛发展而发展，逐渐成为重要的网络编程语言。";

    int index = str.indexOf("Java");

    if(index!=-1){
      System.out.println("匹配到了Java，索引位置是"+index);
    }else{
      System.out.println("没有匹配到了Java");
    }
  }
```
* 如果想找到第二个出现Java的位置 
```
 int index = str.indexOf("Java");

    index = str.indexOf("Java", index + 4);

    System.out.println("第二次匹配到了Java，索引位置是" +index);

  }
```
>indexOf("字符串","开始索引值")
>开始索引值用于设定从什么位置开始查找。所以我们找到的第一个匹配到的索引+匹配字符长度就是开始值了。


###### 字符串拼接substring
* 第一种为substring（开始索引，结束索引）；
>拼接新的一个字符串从开始索引（含这个值）到结束索引结束（不含）
```
int index = str.indexOf("Java");

    if (index != -1) {
      System.out.println("匹配到了Java，索引位置是" + index);

      String newStr = str.substring(index+4);
      System.out.println(newStr);

    } else {
      System.out.println("没有匹配到了Java");
    }
  }
```
>输出结果为：匹配到了Java，索引位置是0
是一种广泛使用的计算机编程语言，拥有跨平台、面向对象、泛型编程的特性，广泛应用于企业级Web应用开发和移动应用开发。任职于太阳微系统的詹姆斯·高斯林等人于1990年代初开发Java语言的雏形，最初被命名为Oak，目标设置在家用电器等小型系统的编程语言，应用在电视机、电话、闹钟、烤面包机等家用电器的控制和通信。由于这些智能化家电的市场需求没有预期的高，Sun公司放弃了该项计划。随着1990年代互联网的发展，Sun公司看见Oak在互联网上应用的前景，于是改造了Oak，于1995年5月以Java的名称正式发布。Java伴随着互联网的迅猛发展而发展，逐渐成为重要的网络编程语言。
* 第二种为substring（开始索引）
>从索引开始（含这个值）一直到结束
* 只想取出Java后的3个字符
```
// 结束索引应该是 index+4+3
  String newStr = str.substring(index+4,index+7);
```
>输出结果为：是一种

###### 字符串开始和结束内容判断 startsWith/endsWith
```
  public static void main(String[] args) {
    String fileName = "报告.doc";

    if(fileName.endsWith(".doc")){
      System.out.println("是word文档");
    }

  }
```
```
 public static void main(String[] args) {
    String url = "https://www.youkeda.com";

    if(url.startsWith("https")){
      System.out.println("网址是安全的");
    }

  }

```
###### 字符串替换 replaceAll
* replaceAll("要替换的值","新值")
```
public static void main(String[] args) {
    String str = "Java是一种广泛使用的计算机编程语言，拥有跨平台、面向对象、泛型编程的特性，广泛应用于企业级Web应用开发和移动应用开发。于1995年5月以Java的名称正式发布。Java伴随着互联网的迅猛发展而发展，逐渐成为重要的网络编程语言。";

    String newStr = str.replaceAll("Java","Python");

    System.out.println(newStr);
    System.out.println(str);
  }
```
>newStr为新值，str为原始值；原始值不会变化，str里放的还是原数据。

* 除了文本替换，还有一种作用是删除内容，我们只需要把新值换成空格就好了。
* 比如我们想去掉文件的后缀名
```
 public static void main(String[] args) {
    String fileName = "报告.doc";

    String name = fileName.replaceAll(".doc","");

    System.out.println(name);
  }
```
###### 字符串分割split
```
String text = "姓名|英文名称|年龄|性别\n张三|Zhang San|20|男\n李四|Li Si|18|男\n小花|xiaohua|18|女";
 String[] data = text.split("\n");
 //以\n为分界点，把这组数据以数组的形式存入data数组中，数组大小为4
```
* 当我们想要去找这组数据中的某个值时，利用上面已经转换好的数组，使用for循环，用"\\|"为分割点，
* 首先，在data[1]中进行分割，存入一个新数组中 

`String[] lines = data[i].split("\\|");`
* 判断这个新数组中是否有出现过
```
 if (lines[3].equals("女")) {
                return lines;
```
>返回的是数组！


###### 大小写转化 toUpperCase/toLowerCase
```
public static void main(String[] args) {
    String text = "ZhanSan";
    // 把拼音全部转化为大写字母
    String enName = text.toUpperCase();
    System.out.println(enName);
  }
```
###### 字符串比较 equals
```
 public static void main(String[] args) {

    String text = "字符串";
    // 使用 equals 方法判断是否相同
    if (text.equals("字符串")) {
      System.out.println("equals 方法字符串相等");
    }
    // 前后顺序无所谓,下面代码是一样的
    if ("字符串".equals(text)) {
      System.out.println("equals 方法字符串相等");
    }
  }
```
>在有些时候用==运行结果也是成功的

###### 数字和字符串转化 Integer.parselnt
```
 public static void main(String[] args) {
    String text = "123";
    // 转化字符串为数字
    int a = Integer.parseInt(text);
    System.out.println(a);

    // 转化字符串为数字
    a = Integer.parseInt("100");
    System.out.println(a);
  }
```
* 之前也有两种方法
1. 运用+号
 ```
 public static void main(String[] args) {

    int a = 100;
    //使用空字符串相加数字，会自动变成字符串类型
    String str = ""+a;
    System.out.println(str);

  }
```
1. 运用String.valueOf()
```
public static void main(String[] args) {

    int a = 100;
    //使用valueOf强制把数字转化为字符串
    String str = String.valueOf(a);
    System.out.println(str);
  }
```

#### Java包管理器
* 得到当前服务器的时间LocalDate
* 包路径为java.time.LocalDate
```
 // 得到当前的日期
    LocalDate now = LocalDate.now();
    // 打印出日期
    System.out.println(now.toString());
```

* 日期时间和字符串的转化DateTimeFormatter
* 包路径为java.time.format.DateTimeFormatter
```
// 创建一个格式化方式
    DateTimeFormatter df = DateTimeFormatter.ofPattern("yyyy/MM/dd");
    // 执行时间的格式化处理，得到期望格式的时间字符串
    String timeStr = df.format(time);
    // 打印时间
    System.out.println(timeStr);
```

* 获取日期时间具体的值
>getMonth()和getDayOfMonth()方法的返回值不是具体的数字，而是一个对象，必须用getValue()得到具体的数字
```
import java.time.LocalDate;

public class DateTest7 {

  public static void main(String[] args) {

    LocalDate time = LocalDate.now();

    // 得到当前时间所在年
    int year = time.getYear();
    System.out.println("当前年份 " + year);

    // 得到当前时间所在月
    int month = time.getMonth().getValue();
    System.out.println("当前月份 " + month);

    // 得到当前时间在这个月中的天数
    int day = time.getDayOfMonth();
    System.out.println("当前日 " + day);

    // 得到当前时间所在星期数
    int dayOfWeek = time.getDayOfWeek().getValue();
    System.out.println("当前星期 " + dayOfWeek);

  }
}
```

###### 字符串转化为日期时间
>如果日期字符串的格式不是yyyy-MM-dd，那么就要借助DateTimeFormatter
```
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;

public class DateTest81 {

  public static void main(String[] args) {
    // 定义一个时间字符串，日期是2019年1月1日
    String date = "2019/01/01";

    DateTimeFormatter df = DateTimeFormatter.ofPattern("yyyy/MM/dd");

    // 把字符串转化位 LocalDate 对象，并得到字符串匹配的日期
    LocalDate date2 = LocalDate.parse(date,df);
    // 打印出日期
    System.out.println(date2.toString());
  }
}
```

###### 日期时间的计算

```
 public static LocalDate getLeaveTime(String checkInTime, int days) {
    
    // 把字符串转化为 LocalDate 类型
    LocalDate time = LocalDate.parse(checkInTime);
    
    // 使用 plusDays 添加天数，得到新的时间
    LocalDate leaveTime = time.plusDays(days);
    
    return leaveTime;
  }


```
###### 日期的判断
>isBefore
>isAfter
>isequal

###### 4.5 ArrayList
```
    // 创建一个 ArrayList 存储字符串集合
    ArrayList<String> strs = new ArrayList<>();

    // 添加数据到 ArrayList 实例里
    strs.add("张三");
    strs.add("李四");
```
>创建一个字符串集合，用于存储多个字符串

```
List<User> users = new ArrayList<>();
```
>创建一个用户集合，用于存储多个用户信息



















