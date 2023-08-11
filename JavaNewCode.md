### URL
网址，全称Uniform Resource Locator
![](https://style.youkeda.com/img/ham/course/py2/py2-1-2.png?x-oss-process=image/resize,w_1024/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
格式说明：
* 协议类型与域名之间以`` :// ``(固定写法)分隔。
* 路径（英文常称为path）以单斜杠`` / ``开头，中间每层的分隔符也是单斜杠`` / ``。路径相当于一层一层的文件夹。但要注意与windows的文件夹分隔符`` \ ``不要混淆了。
* 参数：
 * 路径与参数之间用`` ? ``分隔。看到问号就知道后面的内容就是参数了。
 * 多个参数之间用`` & ``分隔
 * 参数用“参数名 = 参数值”（key=value）的格式表示。
 
### API
API全称Application Programming Interface,API可以快速调用某个程序
！[API知识](https://ham.youkeda.com/articles/detail/5f3756bd5e205f30b2c2b125)
所谓的API，本质上就是一个URL。开头也是http（或https），只是返回的内容有明显的区别，没有大量多余的字符。

###### 抓取网站内容
首先，安装Okhttp3，这是一个非常流行的HTTP库，可以简单快速的实现HTTP调用。
安装Okhhttp3的方法是在pom。xml文件中添加依赖：
```
<!-- https://mvnrepository.com/artifact/com.squareup.okhttp3/okhttp -->
<dependency>
 <groupId>com.squareup.okhttp3</groupId>
 <artifactId>okhttp</artifactId>
 <version>4.1.0</version>
</dependency>
```
###### 使用Okhttp3完成页面请求，需要三大步骤
1. 实例化OkHttpClient。使用`` OkHttpClient okHttpClient = new OkHttpClient(); ``代码。
1. 执行调用。
 1. 在执行调用前，需要实例化一个Request对象，作用时定义请求的各种参数，`` Request request = new Request.Builder().url(url).build(); ``
 1. 然后构建调用对象`` Call call = okHttpClient.newCall(request); ``
 1. 最后执行调用，如果调用失败可能抛异常，所以必须抓取异常。`` call.execute() ``就是执行调用的代码。
1. `` call.execute() `` 返回的其实时一个执行的结果对象，调用对象的方法即可获取返回的字符串内容：``call.execute().body().string(); ``
这些代码基本上属于**固定写法**
>任何时候都不要忘记在``pom.xml``文件添加依赖，以及代码中使用``import``语句引用使用的类哦。



```
 <!-- https://mvnrepository.com/artifact/com.squareup.okhttp3/okhttp -->
```
        <dependency>
            <groupId>com.squareup.okhttp3</groupId>
            <artifactId>okhttp</artifactId>
            <version>4.1.0</version>
        </dependency>

* 记得加入这些在pom.xml里的两个dependency中。
* 然后再点击右上角的标志，跑一下。

```
package com.youkeda.test.http;

import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;

import java.io.IOException;




public class GetPage {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public String getContent(String url) {
    OkHttpClient okHttpClient = new OkHttpClient();
    Request request = new Request.Builder().url(url).build();

    Call call = okHttpClient.newCall(request);

    // 返回结果字符串
    String result = null;

    try {
      result = call.execute().body().string();
    } catch (IOException e) {
      System.out.println("request"+url+"error.");
      e.printStackTrace();
    }

    return result;
  }

  public static void main(String[] args) {
    GetPage getPage = new GetPage();
    String url ="https://ipservice.3g.163.com/ip";
    String content = getPage.getContent(url);
  
    System.out.println("API调用结果");
    System.out.println(content);
  }
}

```
``call.execute().body().string();``可以取得服务器返回的具体内容。

### 调用API
我们看一个查询杭州市区天气``API``:
把这个URL粘贴到浏览器，可以看到查询返回的结果为：
```
{
  "weatherinfo": {
    "city": "浣欐澀",
    "cityid": "101210106",
    "temp": "24",
    "WD": "鍖楅",
    "WS": "灏忎簬3绾�",
    "SD": "77%",
    "AP": "1000.7hPa",
    "njd": "鏆傛棤瀹炲喌",
    "WSE": "<3",
    "time": "17:50",
    "sm": "3.6",
    "isRadar": "0",
    "Radar": ""
  }
}
```
所谓API，本质上就是一个URL。开头也是http(或者https),只是返回的内容有明显的区别，没有大量多余字符。

我们看到一个新的**网址**，必须能够想到使用Okhttp3获取其内容

### Post 表单数据
**post()**方法
``Okhttp3``库也支持``POST``操作。我们前面学习的调用``API``属于``GET``操作。不同的是，``POST``操作时，数据不是放在``URL``中的，而是放在表单中提交的。
所以程序实现需要构建一个``FormBody``表单对象，用于放置表单数据
```
Builder builder = new FormBody.Builder();
// 设置数据，第一个参数是数据名，第二个参数是数据值
builder.add("", "");
FormBody formBody = builder.build();

Request request = new Request.Builder().url(url).post(formBody).build();
```
整体步骤和请求``API``相似，不同的时，在构建``Request``对象时，使用``.post(formBody)``语句放入表单对象，就可以表示执行``POST``操作了。
>这段代码仍然属于固定写法。

我们仍然把发送表单数据的过程，封装为``postContent()``方法。


### POST JSON 数据
跟表单数据提交方法一样，也是调用``post()``方法，只是参数不再使用``FormBody``对象，而是使用``RequestBody``


* 实现JSON方式提交数据的步骤
1. 将数据转化为JSON格式的字符串，调用JSON.toJSONString()方法即可。
1. 创建RequestBody实例，注意需要指定提交的类型是
``application/json; charset=utf-8``。这里的utf-8是API规定的编码格式。
1. 构建Request实例对象时。调用
``.post(requestBody)``即表示使用JSON的方式提交数据。
1. 固定写法，必须掌握！
```
//引入依赖
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.62</version>
</dependency>
```
![《JSON与FastJSON》](https://ham.youkeda.com/articles/detail/5f3757fd5e205f30b2c2b1f9)

### 请求状态码Response文本文件
上一章中，使用``Okhttp3``库请求网页或者调用``API``的知识。
使用一条语句执行调用请求，并取得返回结果字符串
```
call.execute().body().string()
```
实际上，除了具体的返回结果，我们通常还关心的数据是：``http``状态码。
状态码是用一个数字直观反映了本次请求的状况，例如常见的``200``表示请求成功了，``404``表示出错了，服务端没有被请求的内容。![文档](https://ham.youkeda.com/articles/detail/5f3758675e205f30b2c2b2a4)
为了取得响应状态码，可以使用语句：
```
call.execute().code();
```
但是通常还要读取响应内容，但是又不能执行两次请求，所以代码应该优化为：
```
import okhttp3.Response;

// 执行请求
Response rep = call.execute();
// 获取响应状态码
int code = rep.code();
// 获取响应内容
String content = rep.body().string();
```

### Response非文本文件
实际上，``Okhttp3``库不仅可以请求网页、``API``、也能请求图片、``excel``等各种文件。
但我们知道，图片文件的明显不同之处在于，图片的内容不是可以直接阅读的字符文本，而是二进制编码，所以，请求图片，excel等各种非字符文本文件时，不能使用response.body().string();获取返回的内容。
而是要使用``response.body().bytes();``
```
byte[] bytes = response.body().bytes();
```
获取返回的二进制编码内容
### Response-JSON
在工作中，经常会调用API，而很多API返回的文本内容是JSON格式的。
JSON是一段文本，也就是Java的字符串，必须转换成Java的对象。

可以把JSON结果转换为对象。
```
Map contentObj = JSON.parseObject(content,Map.class);
```
![JSON与FastJSON](https://ham.youkeda.com/articles/detail/5f3757fd5e205f30b2c2b1f9)
### 解析JSON对象
*[JSON格式化工具](http://www.ab173.com/json/jsonviewernew.php)
```
{
  "code": 0,
  "data": {
    "keyword": "西海情歌",
    "priority": 0,
    "qc": [
      
    ],
    "semantic": {
      "curnum": 0,
      "curpage": 1,
      "list": [],
      "totalnum": 0
    },
    "song": {
      "curnum": 50,
      "curpage": 1,
      "list": [
        {
          "albumid": 14880,
          "albummid": "0024RXz94OSKCa",
          "albumname": "刀郎Ⅲ",
```
```
    Map contentObj = JSON.parseObject(content, Map.class);
    // 解析嵌套的对象，获取数据
    Map dataobj = (Map)contentObj.get("data");
    Map songobj = (Map)dataobj.get("song");
    List listobj = (List) songobj.get("list");
    Map numObj = (Map) listobj.get(0);

    
    String albumname = (String) numObj.get("albumname") ;
    System.out.println("albumname = " + albumname);
```

```
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Connection: keep-alive
Content-Length: 179
Content-Type: application/json; charset=utf-8
Date: Thu, 30 Jun 2022 11:16:54 GMT
Etag: W/"b3-PpPst2uxF1Y4VNnEycfFncSWbqk"
Server: nginx/1.17.8
Vary: Accept, Origin, Accept-Encoding
X-Powered-By: Express

{
  "code": 0,
  "data": {
    "song": [
      {
        "songname": "刺心",
        "artistname": "常艾非"
      },
      {
        "songname": "刺心",
        "artistname": "大壮"
      },
      {
        "songname": "刺心的痛",
        "artistname": "方妍/安梓雪"
      }
    ]
  }
}
```
```
package com.youkeda.test.http;

import com.alibaba.fastjson.JSON;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import java.util.List;
import okhttp3.Call;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;

public class ApiAsker {

  /**
   * 根据输入的url，读取页面内容并返回
   */
  public String getContent(String url) {
    // okHttpClient 实例
    OkHttpClient okHttpClient = new OkHttpClient();
    // 定义一个request
    Request request = new Request.Builder()
        .url(url)
        .addHeader("User-Agent","Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.163 Safari/535.1")
        .addHeader("Referer","https://www.fastmock.site/")
        .addHeader("Host","www.fastmock.site")
        .build();
    // 返回结果字符串
    String result = null;
    try {
      // 执行请求
      Response response = okHttpClient.newCall(request).execute();
      // 获取响应内容
      result = response.body().string();
    } catch (IOException e) {
      System.out.println("request " + url + " error . ");
      e.printStackTrace();
    }
    return result;
  }

  public static void main(String[] args) {
    String url = "https://www.fastmock.site/mock/3d95acf3f26358ef032d8a23bfdead99/api/info/suggestion?from=web&format=json&word=刺心&third_type=0&client_type=0&version=2";
    ApiAsker asker = new ApiAsker();
    String content = asker.getContent(url);

    // 解析结果

    System.out.println("歌曲");
    Map resultObj = JSON.parseObject(content,Map.class);
    Map DateObj = (Map) resultObj.get("data");
    List songObj = (List) DateObj.get("song");
   // String result = (String) songObj.get("song");
    for (int i=0;i< songObj.size();i++)
    {
      Map songnameObj = (Map) songObj.get(i);
      System.out.println(songnameObj.get("songname"));
    }


  }
}
```
### User-Agent
**HTTP header**
HTTP消息头Headers是HTTP协议的一项重要内容，作用是在发起请求的时候，除了请求参数之外，可以附加更多的信息。
![HTTP Headers文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers)
Headers信息不是写在URL中的，属于隐藏的数据，不能直观看到。
**User-Agent**
User-Agent是存放在Headers中的一种数据信息。作用是，在指定URL发送请求的时候，告诉服务端当前用户的浏览器类型、版本，甚至操作系统、CPU等非隐私的技术信息。
![User-Agent文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/User-Agent)

所以，我们只要在程序代码中，附加上User-Agent信息，就能允许成功了，当然，这里的User-Agent是模拟的。我们来模拟一个win7+chrome的环境，User-Agent的写法如下：
```
Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.163 Safari/535.1
```
怎么用User-Agent，实际上Okhttp3库已经支持Headers了，只需要在构建Request对象的时候，调用addHeader()方法即可:
```
Request request = new Request.Builder()
    .url(url)
    .addHeader("User-Agent","")
    .build();
```
addHeader()方法第一个参数是名称，第二个参数是值。
### Referer
上一节设置了User-Agent解决程序请求HTTP不成功的情况。实际情况中，还有一类更严格的检查：图片防盗链（图片无法正常显示）。
**如何防盗链呢？**
因为浏览器在请求网页中的图片（或其他任何文件）时，会自动在HTTP消息头Headers中，加一个Referer信息，表示请求的来源
>浏览器自动加”Referer“是业界规范，是默认的行为，不容易干预
**程序中的Referer**
为了模拟浏览器自动加Referer信息的行为，可以调用语句：
```
Request request = new Request.Builder()
    .url(url)
    .addHeader("Referer","https://ham.youkeda.com/course/j14/0")
    .build();
```
同样，程序访问此图片也是失败的，http响应状态码是403，不成功。

**解决方法**

需要把Referer信息设置为图片原始使用的网站
### Host
Host表示当前请求的域名，虽然这个域名已经存在于URL中，但遇到复杂的场景，例如使用代理服务器，或者URL中不写域名而是写IP地址进行请求等，设置Host就非常有用了
![Host文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Host)
```
Request request = new Request.Builder()
    .url(url)
    .addHeader("Host","www.douban.com")
    .build();
```
注意，Host的值一定是一个域名且**不带协议头**。

### 下载文件
在java中写文件必须经历三个步骤：
```
创建文件对象
   ↓
写入内容
   ↓
关闭写入操作
```

**写入文本文件**

```
import java.io.File;
import java.io.FileWriter;

// 文件对象
File file = new File("foo.txt");

// 写入内容
FileWriter fileWritter = new FileWriter(file.getName());
fileWritter.write(content);

// 关闭
fileWritter.close();
```
>File是文件类，FileWriter是用来给文件写入内容的类。再次强调，写入文件类必须执行关闭操作。


**写入二进制文件类**
假设文件数据的变量是``byte[] data``，那么写入本地文件的代码如下：
```
import java.io.File;
import java.io.FileOutputStream;

// 文件对象
File file = new File("china-city-list.xlsx");

// 写文件
FileOutputStream fos = new FileOutputStream(file);
fos.write(data);

// 必须刷新并关闭
fos.flush();
fos.close();
```
>与写入文本内容不同，写入二进制内容，需要用``FileOutputStream``。再次强调，必须执行刷新、关闭操作。

### 解析excel

```
import com.alibaba.excel.EasyExcel;
import java.util.Map;
import java.util.List;

// 读取第一个sheet
List<Map<Integer, String>> sheetDatas = EasyExcel.read("xzq_201907.xlsx").sheet(0).doReadSync();
// List 中每个元素表示一行
for (Map<Integer, String> rowData : sheetDatas) {
  // Map 中用序号指代每一列
  for (Integer index : rowData.keySet()) {
    // 列值
    String columnValue = rowData.get(index);
  }
}
```
>解析文件的第一个步骤是读取文件内容 ，调用EasyExcel.read()方法，传入文件名称。然后这里解析的第一个工作表  ，所以调用sheet()方法，传入参数0
。最后的doReadSync()表示同步方式读取文件内容，返回一个读取到的内容集合List。
```
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>easyexcel</artifactId>
  <version>2.1.6</version>
</dependency>
```

### Cookie
![谷歌开发者工具讲解](https://www.cnblogs.com/yaoyaojing/p/9530728.html)
```
addHeader("Cookie",ReadFileTool.readContent("Cookie.txt"))
```

### session


### SMTP
* JavaMail 调用发送和接收邮件
* 依赖
```
<dependency>
 <groupId>javax.mail</groupId>
 <artifactId>mail</artifactId>
 <version>1.4</version>
</dependency>
```
* 以下为固定写法的代码
```
import java.security.Security;
import java.util.Properties;
import javax.mail.Authenticator;
import javax.mail.Message;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

public class MailClient {
  public static void main(String[] args) {
    try {
      final String SSL_FACTORY = "javax.net.ssl.SSLSocketFactory";

      //配置邮箱信息
      Properties props = System.getProperties();
      //邮件服务器
      props.setProperty("mail.smtp.host", "smtp.qq.com");
      props.setProperty("mail.smtp.socketFactory.class", SSL_FACTORY);
      props.setProperty("mail.smtp.socketFactory.fallback", "false");
      //邮件服务器端口
      props.setProperty("mail.smtp.port", "465");
      props.setProperty("mail.smtp.socketFactory.port", "465");
      //鉴权信息
      props.setProperty("mail.smtp.auth", "true");
      //建立邮件会话
      Session session = Session.getDefaultInstance(props, new Authenticator() {
        //身份认证
        protected PasswordAuthentication getPasswordAuthentication() {
          //1.账户 授权码
          return new PasswordAuthentication("xxxxxxx@qq.com", "xxxx");
        }
      });
      //建立邮件对象
      MimeMessage message = new MimeMessage(session);
      //设置邮件的发件人
      message.setFrom(new InternetAddress("xxxxxxx@qq.com"));
      //2.设置邮件的收件人
      message.setRecipients(Message.RecipientType.TO, "xxxxxxx@qq.com");
      //设置邮件的主题
      message.setSubject("通过javamail发出！！！");
      //文本部分
      message.setContent("文本邮件测试", "text/html;charset=UTF-8");
      message.saveChanges();
      //发送邮件
      Transport.send(message);
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}
```
1. 注释1中账户和授权码改为自己的QQ号和刚才申请保存的授权码
1. 注释2中message.setRecipients可以设置多个收件人信息
>用逗号分隔多个收件人``xxxx@qq.com,xxxx@126.com``
1. 所有xxxx都改为自己的邮箱地址，否则看不到邮件内容


### 碎片点
1. 反序列化
```
// 构建歌单url 构建可以动态修改的url
    String aUrl = ARTIEST_API_PREFIX + artistId;
    // 调用 okhttp3 获取返回数据
    String content = getPageContentSync(aUrl);
    // 反序列化成 Map 对象
    Map returnData = JSON.parseObject(content, Map.class);
```
```
// 从 Map 对象中取得 歌单 数据。歌单也是一个子 Map 对象。
    Map artistData = (Map) returnData.get("artist");
    Artist artist = new Artist();
    artist.setId(artistData.get("id").toString());
    if (artistData.get("picUrl") != null) {
      artist.setPicUrl(artistData.get("picUrl").toString());
    }
    artist.setBriefDesc(artistData.get("briefDesc").toString());
    artist.setImg1v1Url(artistData.get("img1v1Url").toString());
    artist.setName(artistData.get("name").toString());
    artist.setAlias((List) artistData.get("alias"));
    return artist;
```

### Kumo
在java中，最常用的词云库为Kumo。github地址为https://github.com/kennycason/kumo

使用方式，首先在pom.xml文件中引入相关的依赖。
```
<dependency>
  <groupId>com.kennycason</groupId>
  <artifactId>kumo-core</artifactId>
  <version>1.17</version>
</dependency>
<!-- 下面tokenizers是为了中文分词引入 -->
<dependency>
  <groupId>com.kennycason</groupId>
  <artifactId>kumo-tokenizers</artifactId>
  <version>1.17</version>
</dependency>




```



































