### 无类型参数

**Lambda**表达式的基本结构是：
```
f->{ }
```
!()[https://style.youkeda.com/img/ham/course/j5/j5-1-2-1.svg]
>作为参数的变量名，是可以自定义的，不是固定的。
**Lambda**表达式在功能上等同于一个匿名方法：
```
public void unknown(f){
    System.out.println(f.getName());
}
```
#### 类型识别

f变量的类型是系统根据上下文自动识别的。
```
List<Fruit>fruits = Arrays.asList(......);

fruits.forEach(f->{
    System.out.println(f.getName());
});
```
`forEach()`方法表示遍历`fruits`集合，每次循环时f变量指代当前遍历到的元素。

由于`fruits`变量的类型使用泛型语法定义为`List<Fruit>`，表示集合中的元素的类型是`Fruit`。

所以，`f`变量的类型自然是`Fruit`,就可以调用`Fruit`类的方法了。

演示一下Collections中sort()方法的排序功能。
```
List<Student> students = new ArrayList<Student>();
students.add(new Student(111, "bbbb", "london"));
students.add(new Student(131, "aaaa", "nyc"));
students.add(new Student(121, "cccc", "jaipur"));

// 实现升序排序
Collections.sort(students, (student1, student2) -> {
  // 第一个参数的学号 vs 第二个参数的学号
  return student1.getRollNo() - student2.getRollNo();
});

students.forEach(s -> System.out.println(s));
```
>Collections.sort()方法第二个参数是实现匿名类`new Comparator(){}`。
**Lambda**表达式又有了一些新写法：
**多参数**
箭头（->）前表示参数变量，有多个参数的时候，必须使用小括号包裹：`()`
```
(student1,student2)->{}
```
**无参数**
箭头（->）前表示参数变量，没有参数的时候，必须使用小括号
```
()->{}
```
**单条执行语句**
箭头(->)后执行语句只有一条时，可以不加大括号包裹
```
s->System.out.println(s);
```
推荐都加上大括号，这样代码的边界明确。
### 有类型参数
如果代码比较复杂，为了维护和容易阅读，也可以为参数变量指定类型。
```
fruits.forEach(Fruit f)->{
    System.out.println(f.getName());
};
```
即使只有一个参数，也必须使用小括号包裹，否则会出错。
##### Arrays.asList()
【1. 要点】

 该方法是将数组转化成List集合的方法。

 List<String> list = Arrays.asList("a","b","c");

注意：

（1）该方法适用于对象型数据的数组（String、Integer...）

（2）该方法不建议使用于基本数据类型的数组（byte,short,int,long,float,double,boolean）

（3）该方法将数组与List列表链接起来：当更新其一个时，另一个自动更新

（4）不支持add()、remove()、clear()等方法




总结：如果你的List只是用来遍历，就用Arrays.asList()。

           如果你的List还要添加或删除元素，还是乖乖地new一个java.util.ArrayList，然后一个一个的添加元素。

 



【3.示例代码】
```
package cn.wyc;
 
import java.util.Arrays;
import java.util.List;
 
public class Test {
    public static void main(String[] args){
 
       //1、对象类型(String型)的数组数组使用asList()，正常
        String[] strings = {"aa", "bb", "cc"};
        List<String> stringList = Arrays.asList(strings);
        System.out.print("1、String类型数组使用asList()，正常：  ");
        for(String str : stringList){
            System.out.print(str + " ");
        }
        System.out.println();
 
 
        //2、对象类型(Integer)的数组使用asList()，正常
        Integer[] integers = new Integer[] {1, 2, 3};
        List<Integer> integerList = Arrays.asList(integers);
        System.out.print("2、对象类型的数组使用asList()，正常：  ");
        for(int i : integerList){
            System.out.print(i + " ");
        }
//        for(Object o : integerList){
//            System.out.print(o + " ");
//        }
        System.out.println();
 
 
        //3、基本数据类型的数组使用asList()，出错
        int[] ints = new int[]{1, 2, 3};
        List intList = Arrays.asList(ints);
        System.out.print("3、基本数据类型的数组使用asList()，出错(输出的是一个引用，把ints当成一个元素了)：");
        for(Object o : intList){
            System.out.print(o.toString());
        }
        System.out.println();
 
        System.out.print("   " + "这样遍历才能正确输出：");
        int[] ints1 = (int[]) intList.get(0);
        for(int i : ints1){
            System.out.print(i + " ");
        }
        System.out.println();
 
        //4、当更新数组或者List,另一个将自动获得更新
        System.out.print("4、当更新数组或者List,另一个将自动获得更新：  ");
        integerList.set(0, 5);
        for(Object o : integerList){
            System.out.print(o + " ");
        }
        for(Object o : integers){
            System.out.print (o + " ");
        }
        System.out.println();
 
        //5、add()   remove() 报错
        System.out.print("5、add()   remove() 报错：  ");
//        integerList.remove(0);
//        integerList.add(3, 4);
//        integerList.clear(); 
    }
 
}
```
输出：
```
1、String类型数组使用asList()，正常：  aa bb cc 
2、对象类型的数组使用asList()，正常：  1 2 3 
3、基本数据类型的数组使用asList()，出错(输出的是一个引用，把ints当成一个元素了)：[I@1540e19d
   这样遍历才能正确输出：1 2 3 
4、当更新数组或者List,另一个将自动获得更新：  5 2 3 5 2 3 
5、add()、remove()、clear() 报错： 
```
### 引用外部变量

Lamdba表达式`{}`内的执行语句，出来能引用参数变量以外，还可以引用外部变量。
```
List<Fruit>fruits = Arrays.asList(.....);
String message = "水果名称；";

fruits.forEach(f->{
    System.out.println(message+f.getName());
});

message = " ";
```
上面的代码运行后会报错，我们借此来认识一些书写规范：
**规范一**
引用的局部变量不允许被修改。
>即使写在表达式后面也不行

下面代码修改了局部变量也是错误的：
```
String message;

fruit.forEack(f->{
    message = "水果名称:";
});
```
>Lambda表达式引用的局部变量即使不声明为`final`，也要具备`final`的特性，变量值初始化后不允许被修改。

在表达式外实际赋值一次的写法是允许的：
```
String message;
message = "水果名称";
```
**规范二**

参数不能与局部变量同名，下面的就是错误的：
```
String f = "水果名称";
fruits.forEach(f->{});
```
### 双冒号（::）操作符
前面的例子中，只有一条执行语句的Lambda表达式：
```
List<String>names = Arrays.asList("zhangSan","LiSi","WangWu");

names.forEach(n->{
    System.out.println(n);
});
```
可以进一步简化为：
```
names.forEach(System.out::println);
```
使用`::`时，系统每次遍历取得的元素（n），会自动作为参数传递给`System.out.println()`方法打印输出：`System.out::println`等同于`n->{System.out.println(n)}`

`::`语法省略了参数变量

语法含义
![](https://style.youkeda.com/img/ham/course/j5/j5-1-5-1.svg)
双冒号`::`语法，当然也是不能独立使用的，需要配合特定的方法

**不同用法**

静态方法调用：使用`LambdaTest::println`代替`f->LambdaTest.print(f)`简化了代码

调用非静态方法：方法print()不在标识为`static`，于是需要实例对象来调用
```
fruits.forEach(new LambdaTest()::print);
```
只是简写了：
```
LambdaTest lt = new LambdaTest();
fruits.forEach(lt::print);
```
效果是一样的。
开头的例子中，`System.out::println`语句就是调用非静态方法`println()`,因为`System.out`代指的是一个实例对象，只是这个实例对象比较复杂。

多参数：
```
Collection.sort(student,(student1,student2)->{
    return student1.getRollNo() - student2.getRollNo();
});
```
如果吧比较难的过程定义成一个方法：
```
private static int compute(Student s1,Student s2){
    .... ...
}
```
那么，排序过程就可以简写为：
```
Collections.sort(students,SortTest::compute);
```
注意，系统会自动获取上下文的参数，并按上下文定义的顺序传递给指定的方法，所谓顺序就是Lambda表达式（）中的顺序。

父类方法
`super`关键字的作用是在子类中引用父类的属性或者方法，那么同样，`::`语法也可以用super关键字调用父类的非静态方法
```
import java.util.Arrays;
import java.util.List;

public class LambdaTest extends LambdaExample {
  public static void main(String[] args) {
    List<Fruit> fruits = Arrays.asList(
            new Fruit("香蕉"),
            new Fruit("苹果"),
            new Fruit("梨子"),
            new Fruit("西瓜"),
            new Fruit("荔枝")
    );

    LambdaTest at = new LambdaTest();
    at.print(fruits);
  }

  public void print(List<Fruit> fruits){
    fruits.forEach(super::print);
    }
}

class LambdaExample {
    public void print(Fruit f){
        System.out.println(f.getName());
    }
}
```
### 流迭代 
先回顾Spring中内容
在Spring Resource中的classpath部分学过，在java内部中，我们一般吧文件路径称为classpath，所以读取内部文件就是从classpath内读取，classpath指定的文件不能解析成File对象，但是可以解析成InputStream，我们借助Java IO就可以读取出来。
>使用commons-io这个库
```
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.6</version>
</dependency>
```
代码如下
```
public class Test{

    public static void main(String[] args){
        //读取classpath的内容
        InputStream in = Test.class.getClassLoader().getResourceAsStream("data.json");
        //使用commons-io库读取文本
        try{
            String content = IOUtils.toString(in,"utf-8");
            System.out.println(content);
        }catch(IOException e){
            e.printStackTrace();
        }
    }
}
```
回到流迭代中
在Java中，`Stream`是一个接口，在接口中提供了操作数据的方法，通常我们叫做`API`。
**创建流**
1. 直接创建
```
import java.util.stream.Stream;

Stream<String> stream = Stream.of("苹果", "哈密瓜", "香蕉", "西瓜", "火龙果");
```
1. 由数组转化
```
String[] fruitArray = new String[] {"苹果", "哈密瓜", "香蕉", "西瓜", "火龙果"};
Stream<String> stream = Stream.of(fruitArray);
```
1.由集合转化
```
List<String>fruits = new ArrayList<>();
fruits.add("苹果");
fruits.add("哈密瓜");
fruits.add("香蕉");
fruits.add("西瓜");
fruits.add("火龙果");
Stream<String>stream = fruits.stream();
```
>由于原数据（集合或数组）是有序的，所以流中的数据也是有序排列的。

**流迭代**
```
Stream<String> stream = Stream.of("苹果", "哈密瓜", "香蕉", "西瓜", "火龙果");
stream.forEach(System.out::println);
```
>为了让系统自动识别Lambda表达式的参数类型，也必须使用泛型语法指定`Stream`中对象的类型。
>泛型,即**参数化类型**也就是把类型当做参数传递,在创建类的对象的时候,可以传入任意类型.

### 流数据过滤
小学生模型
```
public class Pupil {
    private String name;
    // 平均分
    private int averageScore;
    // 违规次数
    private int violationCount;
}
```
我们要筛选出分数不低于80，且没有违规记录的。
```
//存入小学生数据对象
List<Pupil>pupils = new ArrayList<>();

//符合条件的小学生集合
List<Pupil>qualified = new ArrayList<>();
for(Pupil pupil:pupils){
    if (pupil.getAverageScore() >= 80 && pupil.getViolationCount() < 1) {
        qualified.add(pupil);
    }
}
for(Pupil pupil:qualified){
    System.out.println(pupil.getName());
}
```
**另一种写法**
我们使用Java8新特性来完成。
```
List<Pupil>pupils = new ArrayList<>();

pupils.add(new Pupil());

pupils.stream()
    .filter(pupil -> pupil.getAverageScore() >= 80 && pupil.getViolationCount() < 1)
    .forEach(pupil->{System.out.println(pupil.getName());});

```
**filter()方法**
从方法名我们可以理解：对流中的数据对象进行过滤
![](https://style.youkeda.com/img/ham/course/j5/j5-2-3-2.svg)
方法参数是一个Lambda表达式，箭头后面式条件语句，判断数据需要符合的条件。

也就是说，使用Lambda表达式告诉过滤器，需要哪些符合条件的数据
>等同于`pupil ->((pupil.getAverageScore()>=80)&&(pupil.getViolationCount()<1))`

下面我们来对100个随机整数，使用`Stream API`和`Lambda`表达式判断哪些式偶数并输出这些偶数
```
public static void main(String[] args) {
        List<Integer> evenNumbers = initNumbers();
        evenNumbers.stream()
                .filter(evenNumber->evenNumber%2==0)
                .forEach(System.out::println);

        
    }

    private static List<Integer> initNumbers() {
        List<Integer> evenNumbers = new ArrayList<>();

        int i = 1;
        while (i <= 100) {
            evenNumbers.add((int)(Math.random() * 101));
            i++;
        }

        return evenNumbers;
    }
```

### 流的设计思想
数据流的操作过程，可以看作一个管道，管道由多个节点组成，每个节点完成一个操作。
![](https://style.youkeda.com/img/ham/course/j5/j5-2-4-1.svg)
>`.filter().forEach()`组成了一个管道，每个方法都是管道的一个节点。方法调用的顺序构成了管道节点顺序。
相比普通java代码，Stream的显著特点是：编程的重点，不再是对象的运用，而是数据的计算。
函数式风格，
![](https://style.youkeda.com/img/ham/course/j5/j5-2-8-1.svg)
### 数据流映射
小习题
```
List<Integer>numbers = Arrays.asList(3,2,2,7,63,2,4,5);
```
计算每个数字的平方并输出。
```
numbers.stream()
    .map(num->{
        return num*num;
    })
    .forEach(System.out::println);
```
这里用到的是`map()`方法
![](https://style.youkeda.com/img/ham/course/j5/j5-2-5-2.svg)
`map()`方法统称为映射，其作用就是用新的元素替换掉流中原来相同位置的元素，相当于每个对象都经历一次转换。

**映射到新的数据**
`map()`方法的参数是一个Lambda表达式，在语句块中对流中的每个数据对象进行计算，处理，最后用return语句返回的对象，就是转换后的对象。
![](https://style.youkeda.com/img/ham/course/j5/j5-2-5-1.svg)
映射后的对象类型，可以与流中原始的对象类型不一致。
在流中，可以用字符串替换原来的整数，这就极大的提供了灵活性、扩展性，让流的后继操作可以更加方便。

少数情况下，如果替换语句简单，系统能自动识别需要返回的值，代码可以简写为：
```
.map(num->num*num)
```

### 流数据排序
我们学会了使用Lambda表达式优化语句：
```
List<Student> students = new ArrayList<Student>();
students.add(new Student(111, "bbbb", "london"));
students.add(new Student(131, "aaaa", "nyc"));
students.add(new Student(121, "cccc", "jaipur"));

//实现升序排序
Collections.sort(students,(student1,student2)->{
    return student1.getRollNo()-student2.getRollNo();
});
students.forEach(s->System.out.println(s));
```
使用`Stream API`更简单
```
students.stream()
    .sort((student1,stduent2)->{
        return student1.getRollNo()-student2.getRollNo();
    })
    .forEach(System.out::println);
```
`sorted()`顾名思义，就是完成排序的方法。把排序规则写成一个Lambda表达式传给此方法即可。

参数顺序
student1代指后一个元素，student2代指前一个元素，
>第一次遍历时，student1指代第二个名为aaaa的学生，student2指代第一个名称为bbbb的学生。

如果计算语句简单，可以简写为
```
.sorted((student1,student2)->student1.getRollNo() - student2.getRollNo());
```
不论是集合排序`Collections.sort()`还是流排序`sorted()`都需要返回以个数值。
返回非正数就表示两个相比较的元素需要交换位置，返回正数就不需要。

### 流数据摘取
```
numbers.stream()
    .sorted((n1,n2)->n2-n1)
    .limit(3)
    .forEach(System.out::println);
```
`limit()`方法的作用是返回流的前n个元素，n不能为负。
>不能摘取任意位置，只能是流开头的。
### 流合并
`filter`、`map`、`sorted`都统称为聚合操作
聚合操作就是把集合中的对象做整体性的计算。

对1-10的十个正整数求和
```
List<Integer>numbers = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
```
普通的使用for循环完成计算的代码是：
```
int sum = 0;
for(int i:numbers){
    sum+=i;
}
System.out.println("sum:"+sum);
```
使用Stream API完成计算的代码：
```
import java.util.Arrays;
int sum = numbers.stream()
    .reduce((a,b)->a+b)
    .get();
System.out.println("1-10求和 : " + sum);
```
`reduce()`方法的作用,是合并了所有的元素，终止计算出一个结果，注意这里终止的意思，就是流已经达到终点结束了，不能再继续流动了。
>forEach()也是流的终点。
`reduce()`方法返回值需要调用get()方法返回最终的整数值。

>同理get()方法返回值的类型，也是系统自动根据流中元素的类型推定的。

`reduce()`的参数：
* a再第一次执行运算语句a+b时，指代流的第一个元素，然后充当缓存作用以存放本次计算结果，此后执行计算语句时，a的值就是上一次的计算结果并继续充当缓存存放本次计算结果。
* b参数第一次执行计算语句时指代流的第二个元素，此后一次指代流的每个元素。
>ab两个参数的作用时由位置决定的，变量名时任意的。

![](https://style.youkeda.com/img/ham/course/j5/j5-3-1-1.svg)


数据如下：
```
List<Student> students = new ArrayList<>();
students.add(new Student("赵祯", 92));
students.add(new Student("曹丹姝", 60));
... ...
... ...
```
计算学生分数。
```
Student result = students.stream()
    .reduce(new Student("", 0),
        (a, b) -> {
            a.setMidtermScore(a.getMidtermScore() + b.getMidtermScore());
            return a;
        }
    );

System.out.println(result.getName() + " - " + result.getMidtermScore());
```
* 第一个参数，是作为缓存角色的对象
* 第二个参数是lambda表达式，
    * 那么a变量不再指代流中的第一个元素了，专门指代缓存角色的对象，即方法第一个参数对象。
    * b变量依次指代流的每个元素，包括第一个元素。

![](https://style.youkeda.com/img/ham/course/j5/j5-3-2-1.svg)

reduce()方法返回作为缓存角色的对象，即第一个参数。

### 流收集
`forEach()`方法和`reduce()`方法都是流的终点。
```
List<Integer>numbers = Array.asList(3,2,2,7,63,2,3,5);
```
找出最大的前三个数字，放到一个新的集合中，用-组合成字符串打印

```
import java.util.stream.Collectors;
List<String>numberResult = numbers.stream()
    .sorted((n1,n2)->n2-n1)
    .limit(3)
    .map(a->""+a)
    .collect((Collectors.toList()));

String string = String.join("."+numResult);
System.out.println("字符串是: " + string);
```
`collect()`方法的作用就是收集元素，Collectors.toList()是一个静态方法，作为参数告诉`collect()`方法存入一个List集合，所以collect（）方法的返回值类型就是List
为了能够把最终结果转换为字符串打印，调用了map()方法把流中原来的整数映射为字符串(""+a)，所以collect（）方法的返回值类型就是List<String>,而不是List<Integer>


### 并行流
为了发挥多核CPU的优势，将串行计算模式改为并行计算模式（多线程，同时执行）。

使用并行流的代码很简单，不在调用`stream()`方法，改为调用`paralleStream()`方法即可。

![](https://style.youkeda.com/img/ham/course/j5/j5-3-4-1.svg)

### 设计模式

#### 单例模式
保证一个类仅有一个实例，要做到这一点，核心办法就是把构造函数设置为私有的。
```
public class ClassMaster {
  private String id;
  // 班主任名称
  private String name;
  private String gender;
  private ClassMaster() {
  }
}
```
`ClassMaster`可以自己实例化自己
```
public class ClassMaster {
  private String id;
  // 班主任名称
  private String name;
  private String gender;

  // 唯一实例
  private static ClassMaster instance = new ClassMaster();

  private ClassMaster() {
  }
}
```
>必须使用`static`修饰符，否则会造成死递归

也就是说，不允许其他类实例化`ClassMaster`（私有构造方法）、只有自己能实例化一个唯一的自己（private static）,所以可以保证`ClassMaster`的实例是全局唯一的。

这种可以保证只有一个实例对象的方式，就是**单例设计模式**

**访问实例**
类new出一个实例的目的就是要给其他的类使用。所以还需要增加一个方法，允许其他类访问这个单例的实例。
```
public class ClassMaster {
  private String id;
  // 班主任名称
  private String name;
  private String gender;

  // 唯一实例
  private static ClassMaster instance = new ClassMaster();

  private ClassMaster() {
  }

  // 外部类可以通过这个方法访问唯一的实例
  public static ClassMaster getInstance() {
    return instance;
  }
}
```
![](https://style.youkeda.com/img/ham/course/j5/j5-4-2-1.svg)

从Console可以看出，虽然访问了多次班主任，但构造函数只执行了一次。
**Spring中的单例**
类变量使用`@Autowired`注解，能够自动注入实例对象。
实际上，任何自动注入实例对象，默认只有一个实例对象，是单例的，例如：可能多个`Service`和`Control`等都需要用到用户服务，那么这些类中都会定义：
```
@Autowired
private UserService userService;
```
Spring会保证只生成一个`UserServiceImpl`实例，注入到多个`Service`和`Control`中，不会为每个`Service`和`Control`分别new出多个userserviceimpl实现类的实例


#### 简单工厂模式
这里的工厂就是生产**实例对象**的地方。

这种模式可以解决代码重复和耦合紧密（当西瓜过季了，甜水果店需要由西瓜改为凤梨时，餐厅，水果超市，甜品点等都需要改。如果由几百个店铺卖水果，那就是灾难）
>所谓耦合，指的是某个类的代码，包含了其他类的逻辑细节。包含的越多，耦合越紧密，也越容易相互影响。甜水果需要由西瓜改为凤梨时，影响了所有的水果店，就是耦合紧密，耦合度高。

简单工厂模式，需要两个步骤：
1. 从具体的产品类抽象出接口，意味着工厂应该生产书出一种产品，不应该生产某一个产品。
![](https://style.youkeda.com/img/ham/course/j5/j5-4-3-2.svg)

```
public class FruitFactory {
    public static Fruit getFruit(Customer customer) {
        Fruit fruit = null;
        if ("sweet".equals(customer.getFlavor())) {
            fruit = new Watermelon();
        } else if ("acid".equals(customer.getFlavor())) {
            fruit = new Lemon();
        } else if ("smelly".equals(customer.getFlavor())) {
            fruit = new Durian();
        }

        return fruit;
    }
}
```
工厂仍然需要实现功能，完成“根据不同的条件创建不同对象”需求。

### 抽象工厂模式
需要创建一个系列，多种产品的时候，简单工厂模式就不太适用了。

对于一批、多种类型的对象需要创建的场景，使用抽象工厂模式会更好。
简单工厂模式的主要作用就是把多个产品抽象，使用一个工厂统一创建，那么抽象工厂模式就是把多个工厂也进一步抽象。
![](https://style.youkeda.com/img/ham/course/j5/j5-4-4-1.svg)
咋一看很复杂，但仔细分析，实际上就是进一步抽象出来工厂接口(SnacksFactory),然后多了一个`SnackSFactoryBuilder`
1. 工厂接口
工厂接口`SnacksFactory`即规定工厂应该提供什么样的产品，所以包含了所有工厂的方法。
```
public interface SnacksFactory {
    // 取得水果
    public Fruit getFruit(Customer customer);
    // 取得饮料
    public Drink getDrink(Customer customer);
}

```
但有一个问题，水果工厂是不提供饮料的，但是水果工厂实现工厂接口后，又必须实现`getDrink()`方法，这时候直接返回null即可。
```
public class FruitFactory implements SnacksFactory {
    public Fruit getFruit(Customer customer) {
        Fruit fruit = null;
        if ("sweet".equals(customer.getFlavor())) {
            fruit = new Watermelon();
        } else if ("acid".equals(customer.getFlavor())) {
            fruit = new Lemon();
        } else if ("smelly".equals(customer.getFlavor())) {
            fruit = new Durian();
        }

        return fruit;
    }

    public Drink getDrink(Customer customer) {
        return null;
    }
}
```
>水果工厂不真正实现`getDrink()`方法，只是基于接口的需要，给一个没有实际作用的方法实现。
1. 工厂的厂
`SancksFactoryBuilder`称之为生产工厂的厂，工厂用来生成产品实例,`SnacksFactoryBuilder`用来生成工厂实例
```
public class SnacksFactoryBuilder {
    public SnacksFactory buildFactory(String choice) {
        if (choice.equalsIgnoreCase("fruit")) {
            return new FruitFactory();
        } else if (choice.equalsIgnoreCase("drink")) {
            return new DrinkFactory();
        }
        return null;
    }
}
```
注意：与简单工厂不同的是：
`SnacksFactoryBuilder`的`buildFactory()`方法并不是`static`的。
因为复杂场景下尽量不要使用类（static）方法，实例方法可以被继承，扩展性好，应该优先使用实例方法。

### 工厂模式结合Spring工程
不提倡在工厂中定义`static`方法的另一种原因是：
在使用Spring框架的时候，可以为`SnacksFactoryBuilder`加上`@Component`注解，可以让框架管理实例：
```
@Component
public class SnacksFactoryBuilder{
    public SnacksFactory buildFactory(String choice){

    }
}
```
>简单工厂模式的工厂类也可以像这样去掉static、加注解。

相应的，任何需要使用工厂的地方，只需要使用`@Autowired`注解让框架自动注入实例即可：
```
@Service
public class XxxxxServiceImpl implements XxxxxService {

    @Autowired
    private SnacksFactoryBuilder snacksFactoryBuilder;
}
```
这样可以让工厂模式的代码与Spring互为一体，扩展性更好、易于维护。
### 观察者模式
**基本思路**
提供一个天气服务器，谁想了解天气信息，从这个服务端订阅就好了：当天气发生变化，自动通知每个客户端。

这种**订阅/通知**的场景，很适合观察者模式来实现。

1. 观察什么

什么对象发生变化了需要发出通知。
天气对象发生变化了需要发送通知。
```
import java.util.Observable;

public class WeatherData extends Observable {
    // 城市
    private String cityName;
    // 时间
    private String time;
    // 温度
    private String temp;

    // 城市固定了就不变了
    public WeatherData(String cityName) {
        this.cityName = cityName;
    }

    // 打印天气信息
    public String toString() {
        return cityName + "，" + LocalDate.now().toString() + " " + time + "，气温：" + temp + "摄氏度。";
    }

    public String getCityName() {
        return cityName;
    }

    public String getTime() {
        return time;
    }

    public String getTemp() {
        return temp;
    }
}
```
天气信息类`WeatherData`继承了`Observable`类。`Observable`类是Java提供的，继承了就表示是核心的，需要被观察的类。
>记住这个写法：extends Observable

这个设计与以前模型设计不同的是，一个WeatherData代表一个城市的天气，初始化完毕以后就不能改了。所以去掉了所有熟悉的setter方法。
`WeatherData`是被观察者。
1. 数据变化后发起通知

时间和气温是监听的终点信息，所以增加一个新的方法。
```
import java.util.Observable;

public class WeatherData extends Observable {
    /**
     * 一个城市的气温在某个时刻发生了变化
     */
    public void changeTemp(String time, String temp) {
        if(time == null || temp == null) {
            // 输入数据为空是有问题的，不处理
            return;
        }

        // 与原数据不同，说明发生了变化
        if(!time.equals(this.time) || !temp.equals(this.temp)) {
            // 标记变化
            super.setChanged();
            this.time = time;
            this.temp = temp;
            // 发出通知，参数是额外的信息
            super.notifyObservers("温度变化已通知");
        }
    }
}
```
在`changeTemp()`中，如果天气数据与原来的不同，则会标记变化并发出通知。
父类`Observable`提供的方法`setChanged()`就是标记被观察者对象发送了变化。
父类`Observable`提供的方法`notifyObservers()`就是发出通知；如果需要发送额外（不在被观察者对象里面的）的信息，在参数中传入信息对象，可以是任意对象，需要自己根据具体的需求场景而定。
这里新增一个`changeTemp()`方法的主要原因是，天气信息有多个字段组成，所以最好有一个统一的变更数据的方法，防止混乱。如果只有一个数据属性，直接把发通知的这段逻辑写在`setter`方法里也可以。
1. 谁接收通知
需要了解天气的类，就是接收通知的类，通常叫做观察者。
观察者需要实现`Observer`接口，也是Java提供的，实现此接口表示作为观察者。
```
import java.util.Observable;
import java.util.Observer;

public class WeatherObserver implements Observer {
    private String name;

    @Override
    public void update(Observable o, Object arg) {
        System.out.print(this.name + "观察到天气变化为：");
        System.out.print(o.toString());
        System.out.println(" " + arg);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
作为观察者，实现`Observer`接口后，要自己实现`update()`方法，方法签名是接口中定义好的，属于固定写法。
* 第一个参数就是被观察者对象，被观察者对象都需要继承自Observable
* 第二个参数就是额外的信息，具体说就是调用`super.notifyObservers()`是传入的参数对象；传入什么对象，arg的值就是什么对象。
>如果这里不想发送额外信息，写为super.notifyObservers(null)，那么这里的arg值就是null，注意避免空指针异常。

`update()`方法的作用就是接收通知。实际上，系统在`super.notifyObservers()`发出通知后，及调用所有观察者的`update()`方法，完成通知的过程。
1. 运行
当天气发生变化时，只需要调用`changeTemp()`方法即可改变天气数据。
```
public class WeatherTest {
    public static void main(String[] args) {
        // 在天气变化后发邮件的观察者
        WeatherObserver w1 = new WeatherObserver();
        w1.setName("天气邮件观察者");

        // 在天气变化后发短信的观察者
        WeatherObserver w2 = new WeatherObserver();
        w2.setName("天气短信观察者");

        // 城市天气数据
        WeatherData weatherData = new WeatherData("余杭");
        // 添加观察者
        weatherData.addObserver(w1);
        weatherData.addObserver(w2);

        // 气温变化
        weatherData.changeTemp("11:08", "32.8");
        // 气温变化
        weatherData.changeTemp("14:46", "29.3");
    }
}
```
观察者可以有多个。观察者对象与被观察者对象谁先new出来都可以，但是必须先调用`addObserver()`方法把观察者对象实例添加到被观察者（天气数据）实例中，然后再调用自定义的`changeTemp()`方法变更天气，才能触发自动通知。
实现观察者模式，主要学会`Observable`父类和`Observer`接口提供的几个方法。
![](https://style.youkeda.com/img/ham/course/j5/j5-4-5-1.svg)


跟工厂模式不同的是，观察者模式主要描述的是类的行为，而不是如何创建。
跟工厂模式的思想相同的是，观察者模式让观察者和被观察者双方的耦合度降到最低（称之为**解耦**）
>观察者不需要知道数据变化后需要通知给谁，发出通知即可；而且不需要知道谁收到通知了谁没收到，由系统保证。


### 并发编程
#### 继承Thread类
1. 线程类
可以继承java的`Thread`类实现线程类
```
public class Person extends Thread{
    @Override
    public void run(){
        try{
            System.out.println(getName()+"开始取钱");
            Thread.sleep(200);
        }catch(InterruptedException e){
            e.printStackTrace();
        }
        System.out.println(getName()+"取钱完毕");
    }
}
```
继承`Thread`类之后需要重写父类的`run()`方法，注意必须修饰为`public void`，方法是没有参数的。
>加上`@Override`注解，会让系统自动检查`public void run()`方法定义有没有写错

线程类的作用就是完成一段相对独立的任务。
这里用`Thread.sleep(200)`模拟取钱的过程，sleep()方法（静态方法）的作用就是让线程睡眠200毫秒。

1. 运行线程

线程需要调用`start()`方法才可以启动
```
public class Bank {
    public static void main(String[] args) {
        Person thread1 = new Person();
        thread1.setName("张三");

        Person thread2 = new Person();
        thread2.setName("李四");

        thread1.start();
        thread2.start();
    }
}
```
![](https://style.youkeda.com/img/ham/course/j5/j5-5-2-1.svg)
Thread父类中由name属性，但是private的，所以可以调用setName()方法为线程设置名字，通过getName（）就知道是那个线程在运行。
>根据具体的要求，线程类也可以增加更多其他属性。只要不遗漏run（）方法就行。

我们需要了解的是，线程类的run（）方法是系统调用start（）后自动执行的，编程时不需要调用run（）方法。但永远无法知道系统在什么时刻调用，是立刻调用，还是延迟一小时调用，都由系统决定。







































































































































