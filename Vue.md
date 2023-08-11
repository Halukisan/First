# Vue

### Vue配置

Vue是用于构建用户界面的渐进式框架
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/video/vue%20%E5%AE%98%E6%96%B9%E4%BB%8B%E7%BB%8D.mp4)

**安装node.js**
我们需要node.js里的npm。
去官网![node.js](https://nodejs.org/en/)下载安装包
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/nodejs%E6%88%AA%E5%9B%BE.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

* 12.14.1LTS稳定版，版本较低
* 13.7.0Current最新版

建议下载稳定版，之后一路“下一步”就可以了

打开Windows终端
输入
```
node -v
```
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/node%E6%A3%80%E6%B5%8B.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```
npm -v
```
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/npm%E6%A3%80%E6%B5%8B.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)


**安装Vue CLI（脚手架）**
Vue CLI简单来说就是Vue工程的升级版
```
npm install -g @vue/cli
// 如果是mac电脑，需要在命令前面加sudo
sudo npm install -g @vue/cli
```
如果执行上面的代码，很久都安装不好，那可以先切换一下镜像：
>因为npm下载的插件都是从国外的服务器中下载，速度慢，下面这段代码就是将镜像切换成淘宝镜像，这样可以从国内的服务器下载插件等。
```
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
```
切换后再执行安装命令。
```
// 注意这里要用的是cnpm
cnpm install -g @vue/cli
```
安装完成后要验证一下脚手架是否安装成功：
```
vue --version
```
当你看见显示vue版本的时候，说明已经安装完成。
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/vue%E6%A3%80%E6%B5%8B.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
**创建Vue工程**
执行完上述所有操作后，我们就会得到一个以vue开头的命令。
执行如下命令创建一个vue工程：
```
// vue 创建 工程名
vue create vue_first
```
此时出现几个选项，这里选择最后一个---自定义配置（键盘上下选择，回车确认）
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue1.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
选中后，进入自定义配置选项，勾选Babel、Router即可（空格选中，回车确认）：
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue2.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
下一步是询问你是否使用历史模式的路由器，可以根据自己的需要选择N或Y，然后回车确认
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

下一步是询问你要将Babel等配置文件放在那里，我们选择放在package.json文件里：
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue4.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
最后一步是询问你是否要保存这次配置，以后如果遇到相同配置的项目会比较方便，根据自己需求选择
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue5.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
然后经过一系列的编译，最后创建成功一个原始的Vue项目
* cd vue_first: 进入到工程根目录，
* npm run serve: 启动vue工程
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue6.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
下面我们就执行这两个命令
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue7.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
接下来我们可以去浏览器打开项目的页面，脚手架给我们了两个地址
* ``http://localhost:8081/``
>localhost表示你自己的电脑
* ``http://192.168.0.102:8081/``
> ``192.168.0.102``是你电脑的ip地址，每个人的电脑ip不一定相同

我们打开其中一个即可,打开后可以看到
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/%E5%88%9B%E5%BB%BAvue8.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)


### Vue 工程项目介绍
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/1/img/src-explain.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
1. assets:存放项目中需要用到的资源文件，css、js、images等。
1. componets：存放vue开发中的一些公共组件：例如项目初始的header.vue、footer.vue就是公共组件。
1. router：vue路由的配置文件。
1. views：存放页面文件。
1. app.vue：根组件
1. main.js:项目的入口文件，定义了vue实例，并引入根组件app.vue，将其挂载到index.html中id为‘app’的节点上。
### 声明式渲染
```
// template即模版的意思，每一个vue文件里必须要有一个，在这里写HTML代码
<template>
  <div id="app"></div>
</template>

// 在这里写js逻辑相关的代码
<script>
  export default {
    name: "app"
  };
</script>

// 这里写样式代码
<style></style>
```
每一个Vue文件都由三部分组成``template``、``script``、``style``,它们分别对应``HTML``、``JavaScript``、``CSS``。
另外需要注意，在template里面只允许一个块状元素，通常情况下在template里面写的都是div
像下面这几种写法是错误的：
1. 错误示范
```
<template>
    <div></div>
    <div></div>
</template>
```
1. 错误师范：
```
<template>
    <div></div>
    <ul>
    </ul>
</template>
```
1. 错误示范：
```
<template>
    <div></div>
    <span></span>
</template>
```
template标签内只能有一个块标签，其他标签都要在这个块标签里面

**声明式渲染**
在学习JavaScript时，我们使用的是模板字符串来将数据渲染在页面中的，那时候我们用的以恶搞符号是``${}``,具体代码如下：
```
let element = `<div>${data.name}</div>`;
```
**差值表达式**，就是一个两层的大括号``{{}}``
差值表达式渲染--字符串
如何在页面中渲染一串文字
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/2/1.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
我们在学习js的时候，会这样写：
```
let str = "优课达--学的比别人好一点";
let template = `<h2>${str}</h2>`;
```
在Vue中，首先在template部分确定插值表达式的位置，并填充变量名：
```
<template>
    <h2>{{title}}</h2>
</template>
```
在script部分定义字符串变量：
```
<script>
  // export default是固定格式，不需要纠结
  export default {
    // 模块的名字
    name: "app",
    // 页面中数据存放的地方
    data() {
      return {
        title: "优课达--学的比别人好一点"
      };
    }
  };
</script>
```
给字体添加样式：
```
<!-- scope的意思表示这段样式只在本xxx.vue文件中生效，其他xxx.vue文件中不会生效，有锁定的意思 -->
<style scope>
  h2 {
    color: deeppink;
    border: 1px solid #cccccc;
  }
</style>
```
差值表达式渲染--数组
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/2/data-list.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
HTML代码和之前的写法是一样的，填充数据的时候可以采用数组下标的形式去取数据，代码如下
```
<h2 class="title">{{title}}</h2>
<ul class="list">
  <li>{{todoList[0]}}</li>
  <li>{{todoList[1]}}</li>
  <li>{{todoList[2]}}</li>
  <li>{{todoList[3]}}</li>
  <li>{{todoList[4]}}</li>
</ul>
```
>学习了v-for后，写一行li标签即可

然后再script里面定义数组：
```
<script>
export default {
  name: "app",
  data() {
    return {
      title: "今日待完成事项",
      todoList: [
        "完成HTML标签学习",
        "完成CSS文字样式学习",
        "完成CSS盒模型学习",
        "完成Flex布局学习",
        "完成JavaScript入门学习"
      ]
    };
  }
};
</script>
```
>data中，变量和变量之间用逗号（，）隔开

最后需要修改一点样式，让文字和布局显得更美观一点，使用sass语法，需要在style标签上添加``lang = "scss"``:
```
<style lang="scss" scope>
  .title {
    box-sizing: border-box;
    width: 300px;
    height: 30px;
    margin: 0;
    padding: 0 20px;
    font-size: 18px;
    font-weight: 700;
    line-height: 30px;
    color: white;
    background: #fd6821;
    border-radius: 6px;
  }
  .list {
    list-style: none;
    margin: 0;
    padding: 0;
    margin-top: 15px;
    li {
      box-sizing: border-box;
      width: 300px;
      height: 30px;
      padding: 0 20px;
      margin-bottom: 8px;
      font-size: 14px;
      line-height: 30px;
      background: #8d999d;
      color: white;
      border-radius: 5px;
    }
  }
</style>
```
**data、scope**
* data方法里面存放数据或者全局变量，具体格式如上面的代码所示。你可以将``title:'今日待完成事项'``理解成``let title = '今日待完成事项'``
* scope你可以理解成锁，将css代码锁在本文件内，只在本文件内有用

差值表达式--对象
如果我们要渲染以恶搞班级名单列表
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/2/3.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
根据要求，我们需要定义一个这样的变量：
```
<script>
export default {
  name: "app",
  data(){
      return {
          list:[
              {
                  name:"张三",
                  grade:"三年级二班",
                  mark:290
              },
              {
                  name:"李四",
                  grade:"三年级二班",
                  mark:270
              },
              {
                  name:"王五",
                  grade:"三年级二班",
                  mark:270
              }
          ]
      }
  }
};
</script>
```
**作业**
完成以下图：
![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/2/2-2%E9%A2%98%E7%9B%AE%E8%A6%81%E6%B1%82.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
```
<template>
  <div id="app">
    <div class="top">
      <div class="left">{{checked}}</div>
      <div class="right">{{word}}</div>
    </div>
    <div class="bottom">
      <div class="left">
          <input type="checkbox" id = "paobu" value="跑步" v-model = "checked"/>
          <label for="paobu">跑步</label>
          <input type="checkbox" id="yangwo" value="仰卧起坐" v-model = "checked"/>
          <label for="yangwo">仰卧起坐</label>
          <input type="checkbox" id="yujia" value="瑜伽" v-model = "checked"/>
          <label for="yujia">瑜伽</label>
      </div>
      <div class="right">  
          <textarea placeholder="请输入你的计划" v-model = "word"></textarea>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
        checked:[],
        word:""
    };
  }
};
</script>

<style scoped>
.top {
  display: flex;
  width: 500px;
  min-height: 100px;
  border: 1px solid black;
}

.top .left {
  flex: none;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 150px;
  background: #c8c8a9;
}

.top .right {
  flex: 1;
  background: #83af9b;
}

.bottom {
  margin-top: 10px;
  width: 500px;
  height: 100px;
  display: flex;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.11);
}

.bottom .right {
  flex: 1;
  margin-left: 10px;
}

.bottom .right textarea {
  box-sizing: border-box;
  overflow: hidden;
  width: 100%;
  height: 100%;
  border: none;
  resize: none;
  outline: none;
  padding: 5px;
}


#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

```
### 处理用户事件

事件绑定

```html
<button v-on:click="add">按钮</button>
```

当我们点击按钮的时候就会触发这个add事件。

可以使用@来替代v-on，即为

```html
<button @click="add">
    按钮
</button>
```

然后，写add方法的实现逻辑

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/2/5.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

```vue
<script>
	export default{
        name:"app",
        data(){
            return {
                counter:0
            };
        },
        methods:{
            add(){
                this.counter++;
            }
        }
    };
</script>
<template>
	<div>
        <p>
            你点击按钮的次数为：{{counter}}
    </p>
        <button @click="add">
            点击
    </button>
    </div>
</template>
```

在这里面需要注意：

1. this指向的是当前vue的实例，如果你需要用在data里面定义的变量，就必须在变量前面添加this
2. `@click="add"`的完整写法是`@click="add()"`在括号里面可以传递参数

### 事件修饰符

1. 阻止冒泡事件

   ```vue
   <div @click.stop="fn2">
       
   </div>
   ```

2. 捕获事件

   ```vue
   <div class="div2" @click.capture="fn2">
       
   </div>
   ```

   

3. 阻止默认事件

   ```vue
   <div class="div2" @click.prevent="fn2">
       
   </div>
   ```

### 监听数据变化

监听器watch的书写位置

```vue
<script>
  export default {
    name: "app",
      // 侦听器 key---watch value---{}
    watch: {}
    // 数据 key---data value---Function
    data: function () {
      return {};
    },
    // 方法 key---methods value---{}
    methods: {}
  };
</script>
```

监听器的监听对象是data里面的变量，当变量发生变化，监听器开始运行

```vue
<script>
	export default{
        name:"app",
        data(){
            return {
                count:1
            };
        },
        watch:{
            count(){
                console.log("count发生了变化");
            }
        }
    }
</script>
```

**注意**监听器里面的方法count一定要和被监听的变量名一样

监听器的运行时机

```vue
<script>
	export default{
        name:"app",
        data(){
            return {
                count:1
            };
        },
        methods:{
            add(){
                this.count++;
            }
        },
        watch:{
            count(){
                console.log("count发生了变化");
            }
        }
    }
</script>
```

### 监听器的进阶用法

获取前一次的值

```vue
watch:{
	inputValue(value,oldValue){
	//第一个为新值，第二个为旧值，不能调换顺序
	console.log('新值:${value}');
	console.log('旧值:${oldValue}');
}
}
```

页面第一次渲染是不会触发监听器的

但是，当我们想让页面第一次渲染的时候就触发监听器就需要用到监听器的`immediate`属性

在之前用的监听器都是简写模式，实际上监听器就是一个对象，里面包含一个handler方法和其他属性，immediate属性就在里面

```vue
<script>
	export default{
        name:"app",
        watch:{
            firstName:{
                handler(newName,oldName){
                    this.fullName = newName+" "+this.lastName;
                },
                immediate:true
            }
        }
    }
</script>
```

当immediate属性的值为true时，不论数据如何变化，页面刷新以后，handler方法就会运行

### HTML属性渲染语法

#### 动态绑定v-bind

在学习img标签的时候，alt属性就是在图片加载不出来的时候，有一个默认的文字去说明一下这个图是什么内容

```html
<img src="#" alt="nihao"/>
```

实际网站，加载这些文字说明的时候不能写死，这个时候需要用动态绑定

```vue
<script>
	export default{
        name:"app",
        data(){
            return {
                imgText:'周杰伦演唱会图片'
            };
        }
    };
</script>
<template>
	<div>
        <img src="#" :alt="imgText"/>
    </div>
</template>
```

动态图片渲染：

![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/3/%E7%9B%B8%E5%86%8C%E6%A1%88%E4%BE%8B.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)



```vue
<template>
  <div id="app">
    <div class="album">
      <img :src="imgList[0]" />
      <img :src="imgList[1]" />
      <img :src="imgList[2]" />
      <img :src="imgList[3]" />
    </div>
  </div>
</template>

<script>
  export default {
      name: "app",
      data() {
          return {
              imgList:[
                  'http://pic2.zhimg.com/50/v2-4a06728efc99ba874a5d7422fd55aaed_hd.jpg',
                  'http://img2.imgtn.bdimg.com/it/u=372764256,3394765004&fm=26&gp=0.jpg',
                  'http://img1.imgtn.bdimg.com/it/u=1898582417,1582081952&fm=26&gp=0.jpg',
                  'http://b-ssl.duitang.com/uploads/item/201707/10/20170710141316_vVFNh.thumb.700_0.jpeg'
              ]
          };
      };
  }
</script>
```

### 模板中使用表达式

#### 模板中进行计算

渲染商品的时候，序号从1开始，但是获取的数据的序号从0开始，需要加一

```vue
<template>
  <div id="app">
    <ul>
      <li>{{ goods[0].index + 1 }}---{{ goods[0].name }}</li>
      <li>{{ goods[1].index + 1 }}---{{ goods[1].name }}</li>
      <li>{{ goods[2].index + 1 }}---{{ goods[2].name }}</li>
    </ul>
  </div>
</template>
<script>
export default {
    name: "app",
    // 数据
    data() {
        return {
            goods:[
                {
                    index:0,
                    name:"扫地机器人"
                },
                {
                    index:1,
                    name:"华为手机"
                },
                {
                    index:2,
                    name:"戴尔笔记本"
                }
            ]
        };
    }
};
</script>
```

#### 模板中使用三元表达式

```vue
<template>
	<div>
        <p>
            {{flag?'你已经通过考试':'你还没有通过考试'}}
    </p>
        <button @click="exchange">
            转换
    </button>
    </div>
</template>
```

```vue
<script>
	export default{
        name:"app",
        data(){
            return {
                flag:true
            };
        },
        methods:{
            exchange(){
                this.flag = !this.flag;
            }
        }
    };
</script>
```

#### 在模板中写方法

```vue
<script>
	export default{
        name:"app",
        data(){
            return {
                message:"谢谢佳文的奶茶"
            };
        }
    };
</script>
<template>
	<div id = "app">
        <p>
            {{ message.split('').reverse().join('') }}
    </p>
    </div>
</template>
```



```html
<p v-if="isShow">{{ message }}</p>
<script>
export default {
    name: "app",
    // 数据
    data() {
        return {
            message:"当条件满足的时候，显示这里的内容"
        };
    },
    methods:{
        isShow(){
            if(!this.message) 
                return false;
            return true;
        }
    }
};
</script>
```

### 条件渲染

#### v-else

与js的条件判断语句相应的，学了if，接下来就是else，其意义和js中的else一样，即不符合if的时候，就显示else中的内容。

代码如下：

```html
<p v-if="isShow">{{ message }}</p>
<p v-else>{{ defaultMessage }}</p>
<script>
export default {
    name: "app",
    // 数据
    data() {
        return {
            message:"当条件满足的时候，显示这里的内容",
            defaultMessage:"优课达"
        };
    },
    methods:{
        isShow(){
            if(!this.message) 
                return false;
            return true;
        }
    }
};
</script>
```

#### v-else-if

v-else-if是v-if的一个补充，当条件不止一个的时候，就可以用到v-else-if。

比如下面这个案例，仅靠v-if是不够的：

```html
<p v-if="questions[0].type==='PICK'">{{ questions[0].content }}</p>
<p v-else-if="questions[1].type==='MULT'">{{ questions[1].content }}</p>
<p v-else>题目还没有加载进来...</p>
<script>
export default {
    name: "app",
    // 数据
    data() {
        return {
            questions:[
                {
                    type:"PICK",
                    content:"这是一道选择题"
                },
                {
                    type:"MULT",
                    content:"这是一道多选题"
                },
                {
                    type:"ESSAY",
                    content:"这是一道论述题"
                }
            ]
        };
    }
};
</script>
```

因为我们还没有学习循环语句，所以可以通过更改html代码的条件来看显示的结果：

```html
<p v-if="questions[0].type==='PICK'">{{ questions[0].content }}</p>
<!-- 改为下面的代码，就会发现显示的是--这是一道多选题 -->
<p v-if="questions[1].type==='PICK'">{{ questions[0].content }}</p>
```

#### v-show

v-show的用法和v-if是一样的，即当条件满足，就会显示标签中的内容，区别就是

- v-show指令只是将标签的display属性设置为none
- v-if指令，如果不满足条件，则此标签在dom中根本不存在。

那么如何选择呢？

当标签出现以后就不会再次消失，或者消失/出现的频率不高，就用v-if。 当标签会被频繁的切换，从消失到显示，那么就用v-show。

不过大家不用太纠结这个，因为开发中大多数情况下都会用v-if。只不过有些人比较严谨，会考虑切换开销和渲染开销。

### 列表渲染语句

```html
<div id="app">
    <ul>
        <li v-for="item in 5" :key="item">{{ item }}</li>
    </ul>
</div>
```

循环渲染数字的时候，不需要写js代码，Vue的内核会从1开始循环遍历数字，直到5结束。最终得到5个li标签，标签内的item就是每次循环出来的数字。

#### 循环渲染数组

与js遍历数组一样，Vue中的v-for指令也可以遍历数组对象。代码如下：

```html
<ul>
    <li v-for="(item,index) in nameList" :key="index">{{ item }}</li>
</ul>
<script>
export default {
    name: "app",
    // 数据
    data() {
        return {
            nameList:["张淮森","周逸依","梁澄静","宁古薄","丘约靖"]
        };
    }
};
</script>
```

这样我们就得到了一个姓名列表，但是如果我们希望在名字前面添加一个序号，这时候就要用到v-for中的index，在上面代码的基础上添加一点东西即可：

```html
<ul>
    <li v-for="(item,index) in nameList" :key="index">{{ index }}---{{ item }}</li>
</ul>
```

这时候你会发现序号是从0开始的，那么我们就可以利用之前学习的**在模版中使用表达式**来给index加1，修改后的代码如下：

```html
<ul>
    <li v-for="(item,index) in nameList" :key="index">{{ index + 1 }}---{{ item }}</li>
</ul>
```

#### 循环渲染对象

循环渲染对象，会从对象里面循环取出里面的值。

这里要讲一下v-for循环遍历对象时候，括号内的三个参数：

```html
<!-- 
    value：对象中每一项的值
    key：对象中每一项的键
    index：索引
 -->
<li v-for="(value,key,index)" :key="index"></li>
```

接下来我们遍历一个对象来看看这三个参数分别对应的是什么？

```html
<ul>
    <li v-for="(value,key,index) in book" :key="index">值：{{ value }}--键：{{ key }}--索引：{{ index }}</li>
</ul>
<script>
export default {
    name: "app",
    // 数据
    data() {
        return {
            book:{
                bookName:'指环王',
                author:'JK 罗琳'
            }
        };
    }
};
</script>
```

观察结果你会很明确的直到，这三个参数分别代表对象中的那些字段。

不过我们大多数情况下只会用到value。

#### 遍历数组中的对象

绝大多数情况下，我们遍历的都是数组中的对象。

```html
<ul>
    <li v-for="(item,index) in books" :key="index">
        {{ index+1 }}----{{ item.title }}----{{ item.author }}----{{ item.publishedTime }}
    </li>
</ul>
<script>
export default {
    name: "app",
    // 数据
    data() {
        return {
            books: [
                {
                    title: '《魔戒》',
                    author: '约翰·罗纳德·瑞尔·托尔金',
                    publishedTime: '1954'
                },{
                    title:'《哈利·波特》',
                    author:'J·K·罗琳',
                    publishedTime:'1997'
                },{
                    title:'《人性的弱点》',
                    author:'戴尔•卡内基',
                    publishedTime:'2008'
                }
            ]
        };
    }
};
</script>
```

#### :key

在Vue CLI工程中为了保证每一个item都是唯一的，所以需要一个唯一的key，否则你的项目会报错。

key的添加位置：

```html
<li v-for="(item,index) in books" :key="">
```

key的值一般都是从后台取到的数据的id，主要是为了保证每一个节点都有唯一key值。我们在学习的时候，可以使用索引。代码如下：

```html
<li v-for="(item,index) in books" :key="index">
```

到这里，Vue中的列表渲染就已经讲完了～

### 计算属性

下面的一段代码功能上没有问题，但不方便阅读

```html
<p>
    {{  message.split('').reverse().join('') }}
</p>
```

计算属性的书写位置：

```vue
<script>
  export default {
    name: 'app',
    // 计算属性
    computed: {},
  };
</script>
```

计算属性的写法

计算属性内部是由一系列方法组成的键值对，键是方法名，值是方法体，以文章开头的代码示例，计算属性可以这么写：

```vue
<script>
	export default{
        name:"app",
        data(){
            return {
                message:"奶茶好喝"
            }
        },
       	computed:{
            reverseMessage(){
                return this.message.split('').reverse().join('');
            }
        }
    };
</script>
```

计算属性和方法的区别

上面的案例，用methods也可以实现，如下

```html
<div>
    <p>
        原字符串：{{  message }}
    </p>
    <p>
        反转后的字符串：{{  reverseMessage() }}
    </p>
</div>
```

```vue
<script>
  export default {
    name: 'app',
    data: function () {
      return {
        message: '优课达--学的比别人好一点～',
      };
    },
    methods: {
      reverseMessage: function () {
        return this.message.split('').reverse().join('');
      },
    },
  };
</script>
```

看起来没有什么区别，但为什么要用计算书写，这是因为计算属性的两种性质：**依赖**和**缓存**

* 依赖：案例中的message就是计算属性的依赖
* 缓存：上一次计算得到的值

结合上面的案例解释一下：

计算属性：

* 当message发生变化（依赖变化），reverseMessage计算书写会重新计算，然后返回计算结果
* message不发生变化（依赖不变化），reverseMessage计算书写会返回缓存的值，而不会重新计算

方法：

* 每次访问的时候，都会执行方法体里面的逻辑，然后返回结果

### 动态class

**动态样式绑定就是根据条件给某个标签添加样式**

#### 动态样式绑定

```vue
<div class="base" :class="{active:isActive}">
    
</div>
```

```css
.active{
    border:1px solid green;
}
```

* active是类名，对应css中的类名
* isActive是Boolean类型的值，决定是否应用该类名

1. 类名的书写：如果是单个单词的类名，如active，就跟上面的写法一样，如果是代连字符的类名`base-active`，就需要用引号：双引号，单引号， 引起来

   ```html
   <div :class="{'base-active':isActive}">
       
   </div>
   ```

2. 引号规则：

   ```vue
   <!-- 外双内单 -->
   <div v-bind:class="{ 'base-active': isActive }"></div>
   <!-- 外单内双 -->
   <div v-bind:class='{ "base-active": isActive }'></div>
   ```

3. 多类名写法

   ```vue
   <div :class='{"base-active":isActive,"base":true}'>
       
   </div>
   ```

#### 动态样式绑定的条件类型

现在我们来说一下，动态样式绑定的条件，也就是类名后面的boolean值，如何去获取这个布尔值

1. 变量形式

   ```html
   <div :class="{hover:isActive}">巴拉巴拉
       
   </div>
   ```

   在data中定义布尔类型的变量

   ```vue
   data(){
   	return{
   	isActive:false
   }
   }
   ```

   最后通过用户事件来改变布尔值，从而改变动态绑定的样式

2. 方法形式

   方法形式在循环中用的比较多，可以用下面的案例说明

   ![](https://qgt-document.oss-cn-beijing.aliyuncs.com/P3-5-Vue/4/2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)



```vue
data(){
	return {
		listData:[
			{
				imgUrl:data:function(){
    return {
        liListData: [
            {
            imgUrl:
                'https://m.360buyimg.com/babel/jfs/t1/30057/19/14881/720/5cbf064aE187b8361/eed6f6cbf1de3aaa.png',
            text: '话费',
            type: 'NEW',
            },
        ]
    }
}
}
]
}
}
```

数据中type字段决定是否显示“新”这个字样。但是它并不是布尔值，所以需要用一个方法来转换一下，这时候就用到了方法形式

```vue
<span :class="{'new-appear':isActive(item.type)}"></span>
```

定义方法

```vue
isActive(type){
	if(tyoe === 'NEW'){
		return true;
}
	return false;
}
```

3. 表达式形式

   其实在上面的例子中，是没有必要使用方法的，因为逻辑过于简单，完全可以在HTML标签上使用表达式来解决

   ```vue
   <span :class="{'new-paper':item.type === 'NEW'}">新</span>
   ```

4. 计算属性形式

   ```vue
   computed: {
     hoverObj() {
       return {
         hover: this.index === 1,
       };
     },
   },
   ```

5. 动态样式绑定的写法

   前面我们用的都是对象写法，它可以写单个属性

   ```vue
   <div :class="{active:isActive}">
       
   </div>
   ```

   也可以写多个属性

   ```vue
   <div :class="{active:isActive,hover:true,'after-hover':false}">
       
   </div>
   ```

   数组写法

   注意，这里的类名都要带引号

   ```vue
   <div :class="['red-style','font-style']">
       
   </div>
   ```

   数组中使用三元表达式

   有时候某个样式类可能会根据条件决定要不要应用，所以需要三元表达式去帮助

   ```vue
   <div :class="['red-style','font-style',isChoosed?'redbg':'']">
       
   </div>
   ```

   你也可以使用三元表达式实现两个类名的二选一

   ```vue
   <div :class="['red-style','font-style',isChoosed?'redbg':'bluebg']">
       
   </div>
   ```

   在数组中使用对象

   上面的第一个写法，其实是有点多余的，既然isChoosed为false的时候，没有类名被应用到标签上，那么完全没有必要去写，

   所以结合对象的写法，可以将三元表达式改写成数组中使用对象的写法

   ```vue
   <div :class="['red-style','font-style',{'redbg':isChoosed}]">
       
   </div>
   ```

   而第二种是不能改为对象的形式。仅限于第二个结果是`’‘`的情况

### 动态syle

#### 对象语法

对象语法跟:class的对象语法不同的是，它的键值对是css样式的**属性：值**组成的一对键值对。写法如下

```vue
<div :style="{background:'red','font-weight':700,'font_size':'20px'}">
    
</div>
```

##### 引号的使用

* 属性：属性如果是单个单词，如background，可以不用引号，如果是带连字符的单词，如`font-weight`就需要用引号
* 值：除了数字，如`'font-weight':700`不用引号，其他的都用引号，特殊情况，如`1px solid black`这种值，可以使用模板字符串--(//)

#### 避免使用引号

属性其实未必一定使用引号，比如font-weight如果写成`fontWeight`就可以不用引号

```vue
<div :style="{background:'red',fontWeight:700,fontSize:'20px'}">
    
</div>
```

#### 改变动态样式的书写位置

只是简单的几个样式，加在标签上是可以的，但是如果要加的样式很多，比如

```vue
<div
  :style="{display:'flex',flexDirection:'column',justifyContent:'space-between',alignItems:'center',flexWrap:'no-wrap'}"
></div>
```

`HTML`看起来不那么整洁了，那么就需要将动态样式提取出来。

首先根据动态样式的功能，给它取个名字---`flexStyle`，然后以`flexStyle`为属性，样式为值，定义在`data`中

```js
data:function(){
    return {
        flexStyle: {
            display: 'flex',
            flexDirection: 'column',
            justifyContent: 'space-between',
            alignItems: 'center',
            flexWrap: 'no-wrap',
        },
    }
}
```

最后在标签的动态样式中引入变量即可

```vue
<div :style="flexStyle">
    
</div>
```

```html
<div :style="flexStyle"></div>
```

这样一来，`HTML`代码又显得干净整洁了

#### 位置不可变情况

如果要做下面这个案例，那么动态样式就不能抽离到`data`中了



这个案例很简单，就是点击“更换”按钮，随机出一个头像，作为背景图，如果将动态样式写在`data`中，结果是会报错的：

```js
data:function(){
    return {
        bg: {
            background: `url(${imgUrls[currentIndex]}) no-repeat center / contain`,
        },
    }
}
```

运行后悔告诉你`imgUrls`未定义，这会因为`bg`跟`imgUrls`都属于全局变量，这里不能互相引用。

所以只能在标签上写动态样式去实现，代码演示如下：

代码演示

#### 数组语法

数组语法也跟`:class`大同小异，不同的是，`:class`数组里面是类名，所以要用引号(`""`)，`:style`数组里面是变量，所以不用引号(`""`)

**1. 常规写法**

在`data`定义一个样式对象：

```js
data() {
  return {
    fontStyle: { color: 'red', fontSize: '33px' },
    boxStyle:{width: '200px', height: '200px', border: `1px solid black`}
  };
},
```

在标签的动态样式中引入：

```html
<div :style="[fontStyle,boxStyle]">这里是文字</div>
```

**2. 在数组中使用三元表达式**

有了`:class`的基础以后，这个知识点应该是信手拈来的

```html
<div :style="[fontStyle, isActive ? boxStyle : colorBox]">这里是文字</div>
```

### Vue组件

#### 自定义组件

组件是可以复用的实例

组件的注册：分为局部注册和全局注册

1. 全局注册：用`vue.component`来创建组件，注册后可以在任何新创建的vue根实例中使用
2. 局部注册：在单个Vue格式的文件中创建组件，在需要用到的地方进行注册

但通常我们都是基于Vue工程进行开发的，会用到webpack这样的构建系统，而通过全局注册的组件在构建系统中即使没有使用依然会构建于构建结果中，所以我们通常选择局部注册

组件的创建

![](https://document.youkeda.com/P3-5-Vue/5/1.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

![](https://document.youkeda.com/P3-5-Vue/5/2.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)



![](https://document.youkeda.com/P3-5-Vue/5/3.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

组件中的数据：

自定义组件内的数据data必须是一个函数

```vue
data: function () {
  return {
    count: 0
  }
}
```

重复使用的组件间的data是相互独立的。这一点是很重要的。

#### 组件单向数据流

在实际开发中，复用的组件里面显示的内容往往是不同的，比如下面的图片中的显示的每个文章的介绍其实是用的一个组件，但是每个介绍的内容是不同的，这就需要从父组件中传递不同的内容给子组件

![](https://document.youkeda.com/P3-5-Vue/5/6.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

那么如何从父组件中把内容传给子组件呢？

#### prop的使用方法

基础使用

当父组件给子组件的prop传递一个值的时候，这个值就变成了子组件实例的一个属性。

现在我们给HelloVue组件传递一个标题

1. 父组件中，传一个title给子组件：

   ```vue
   <template>
     <div id="app">
       <!-- 注意！title1 和 title2 是父组件的 data 中定义的数据，title 则是子组件中接收数据时的变量名 -->
       <HelloVue :title="title1"></HelloVue>
       <HelloVue :title="title2"></HelloVue>
     </div>
   </template>
   ```

   * title1和title2是父组件的data中定义的数据，而title则是子组件中接收数据时的变量名
   * 因为title1和title2是变量，所以title前需要加`:`

2. 子组件中用prop接收title：

   ```vue
   <template>
   	<div class="hello">
           <!--第二步：在页面上显示title的值，写法和显示data里定义的数据一样-->
           <h1>
               {{  title }}
       </h1>
       </div>
   </template>
   <script>
   	export default{
           name:'HelloVue',
           //在prop书写中接收title
           props:['title']
       }
   </script>
   ```

   注意，在子组件中prop属性的写法，因为传过来的可能不止一个，因此这里是个数组，其中的每一项均为传过来的值的名称

#### prop的进阶用法

进阶使用：附带类型声明

我们可以用下面的写法给值声明类型，类型首字母要大写哦：

```vue
<script>
	export default{
        name:'HelloVue',
        //在prop属性中接收title，其类型为String
        props:{
            title:String
        }
    };
</script>
```

这里prop是一个对象，当传入的值有多个的时候，可以用逗号隔开，我们还可以用下面的写法给值设置一些要求

```vue
props: {
  title: String,
  // 多类型
  likes: [String, Number],
  // 带有默认值
  isPublished: {
    type: Boolean,
    default: true
  },
  // 必填
  commentIds: {
    type: Array,
    required: true
  },
  author: Object,
  callback: Function,
  contactsPromise: Promise
}
```

从上面的示例中，我们知道了怎么通过prop实现父到子的数据传递，那么能不能用prop实现子到父的数据传递呢？

答案是不能。我们来解释一下上面是“单向数据流”

#### 单向数据流

单向数据流指的是父子prop之间形成了一个**单向下行**绑定：父级prop的更新会向下流动到子组件中，但是反过来不行。

这样可以防止从子组件意外改变父组件的状态，从而导致你的应用的数据流向难以理解

简单来说：

1. prop可以实现父到子的数据传递
2. 父组件中的数据变化，会通过prop传递到子组件
3. 子组件不能直接修改父组件通过prop传递过来的数据。

但实际开发中常会遇到传到子组件中的数据需要处理后才能使用的情况，比如排序、格式化等，这时候怎么办捏？

1. prop传入的数据需要处理

   ```vue
   props: ['initialTitle'],
   computed: {
     normalizedTitle: function () {
       // 对传入的 initialTitle 进行去空格、字母小写化处理
       return this.initialTitle.trim().toLowerCase()
     }
   }
   ```

   

2. #### prop 传入的数据作为本地数据使用

   如果想将 prop 传递的初始值作为一个本地数据使用，可以定义一个本地的 data 属性并将这个 prop 用作其初始值：

   ```js
   props: ['initialTitle'],
   data: function () {
     return {
       // 要读取 prop 里的 initialTitle，要在前面加 “this.”
       // 将传入的 initialTitle 作为本地属性 title 的初始值
       title: this.initialTitle
     }
   }
   ```

   接下来可以直接修改 title，而不必改变父组件的 initialTitle。

   > 在某些情况下，我们还可以通过自定义事件，调用父组件中定义的方法，在父组件中修改数据，然后让数据的更新自动向下流动到子组件中。“自定义事件”这个概念我们会在后面的章节进行讲解。

   ### 文章列表的实现

   现在我们来尝试做上面的文章列表，完成后是这样的：

   ![img](https://document.youkeda.com/P3-5-Vue/5/7.jpg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

   注意，这里我们从父组件 App.vue 传到子组件 Article.vue 里的是一个对象 article：

   ```html
   <Article
     v-for="article in articleList"
     :key="article.title"
     :article="article"
   ></Article>
   ```

   article 是 App.vue 中的 data 数据 articleList 中的一项。

   在子组件 Article.vue 中使用 article 数据时，通过计算属性对时间和文章内容做了处理，注意，这种做法并不是直接改变 article 的内容：

   ```js
   computed: {
     formatTime: function() {
       if (this.article) {
         const dt = new Date(this.article.time)
         const month = dt.getMonth()
         const date = dt.getDate()
         return `${month}月${date}日`
       }
       return '';
     },
     brief: function() {
       return this.article && (this.article.content.substr(0, 35) + '...') || ''
     }
   }
   ```

### 自定义事件

我们不能在子组件中直接修改父组件传来的prop数据

如果我们想要修改父组件中的原数据怎么办呢

我们可以用自定义事件来完成

现在我们用自定义事件`upVote`来实现点赞功能

1. 给子组件Article.vue绑定自定义事件

   在App.vue中用`v-on:upVote="handleLikes"给Article.vue绑定自定义事件`

   ```vue
   <article v-for="article in artilceList"
            :key="article.title"
            :article="artilce"
            v-on:upVote="handleLikes">
   </article>
   ```

   > “upVote”是我们给自定义事件取的事件名，就像点击事件叫"click"一样。比较一下：点击事件`v-on:click`；自定义事件`v-on:upVote`

2. 在Article.vue中调用自定义事件"upVote"

   ```vue
   <!--在template中直接调用自定义事件 upVote-->
   <button @click="$emit('upVote')">
       点赞
   </button>
   ```

   如果在点赞的同时还有其他要执行的代码可以这样写：

   ```vue
   <button @click="childEvent">
       点赞
   </button>
   ```

   ```vue
   <script>
   	export default{
           method:{
               childEvent(){
                   //调用自定义事件 upVote
                   this.$emit('upVpte');
                   //do other things
               }
           }
       }
   </script>
   ```

   特别注意，这里的三个名称：

   1. handleLikes：父组件中修改点赞数的方法；
   2. upVote：自定义事件名
   3. childEvent：子组件中的按钮点击时调用的方法

   自定义事件的参数

   prop可以完成父组件到子组件的数据传递，自定义事件则可以帮我们完成子组件到父组件的数据传递

   下面我们就通过自定义事件的参数把数据从子组件传到父组件：

   父组件App.vue中：

   ```vue
   <!--自定义事件upVote，调用该事件时会执行handleLikes方法-->
   <!--注意：我们接下来会在子组件里面给handleLikes传参数-->
   <article v-for="article in articleList"
            :key="article.title"
            :article="artilce"
            v-on:upVote="handleLikes">
   </article>
   ```

   ```vue
   <script>
   	export default{
           methods:{
               handleLikes(artilce){
                   article.likes++
               }
           }
       }
   </script>
   ```

   注意，虽然这里的handleLikes方法需要传入参数artile，但`v-on:upVote="handleLikes"`没有传入参数article。

   在子组件Article.vue中调用自定义事件”upVote“时会把参数传入：

   ```vue
   <button @click="childEvent">
       点赞
   </button>
   ```

   ```vue
   <script>
       export default{
           methods{
           childEvent(){
               //调用自定义事件upVote，这里的第二个参数最后会传到父组件中的handleLikes方法里
               this.$emit('upVote',this.article)
               //do other things
           }
       }
       }
   </script>
   ```

   `$emit`的第一个参数是自定义事件的名称，它还可以有第二个，第三个参数，甚至有更多的参数，这些参数最终会成为自定义事件对应的那个方法的参数

   

   小结一下，自定义事件可以：

   1. 在子组件中调用父组件的方法
   2. 把子组件的数据通过自定义事件参数的形式传给父组件

   自定义事件中的双向绑定

   之前讲过v-model可以实现双向绑定，如果是自定义组件，如何实现双向绑定

   这里我们需要用到修饰符`.sync`.

   父组件App.vue中，用修饰符`.sync`完成count的双向绑定

   ```vue
   <MyCount class="count" :count.sync="count"></MyCount>
   ```

   ```vue
   <script>
       export default{
           data(){
               return:{
                   count:0
               }
           }
       }
   </script>
   ```

   子组件MyCount.vue中用`update:count`的模式触发事件，把count+1的值赋给count：

   ```vue
   <div class="my-count">
       <button @click="$emit('update:count',count+1)">
           加一
       </button>
   </div>
   ```

   ```vue
   props:['count'],
   ```

   虽然count是定义在App.vue里的，但是通过双向绑定，我们在子组件中修改count的值，App.vue里面的count的值也会有相同的变化。

   

### 组件函数调用

除了用自定义组件双向绑定的方法完成弹窗的显示和隐藏外，我们还可以把visible直接写在子组件中，通过在父组件里面调用子组件方法的形式，修改子组件中的visible的值，完成弹窗的显示和隐藏。

想要调用子组件中的方法，其实就是访问子组件实例，调用实例中的方法

下面我们利用vue提供的ref属性来访问子组件实例，并调用子组件中的方法

#### 改写子组件Modal.vue

调用子组件中的方法

接下来我们使用ref属性来访问子组件实例，并调用子组件中的方法：

1. 给要访问的子组件添加ref属性

   ```vue
   <template>
   	<Modal ref="modal"></Modal>
   </template>
   ```

   现在，我们可以通过`this.$refs.modal`来访问自定义组件Modal.vue

2. 调用子组件中的方法

   现在我们调用子组件中的show方法来改变子组件中的visible的值，使弹框出现：

   ```vue
   <script>
       export default{
           methods:{
               showModal(){
                   //调用子组件中的show方法
                   this.$refs.modal.show();
               }
           }
       }
   </script>
   ```

#### ref访问子元素

`ref`除了可以访问组件实例，还可以访问子元素

```vue
<template>
	<div>
        <input ref="input" type="text" />
        <button @click="focusInput">
            点击使输入框获取焦点
    </button>
    </div>
</template>
<script>
export default{
    name:'app',
    methods:{
        focusInput(){
            //this.$refs.input访问输入框元素，并调用focus()方法使其获取焦点
            this.$refs.input.focus();
        }
    }
}
</script>
```

实现效果：

![](https://document.youkeda.com/P3-5-Vue/5/13.gif)





### 组件solt

组件的 slot 是一种强大的方式，用于在父组件中插入子组件的内容。通过使用 slot，您可以在子组件的特定位置插入任意内容，从而实现更灵活的组件复用和定制。

让我们通过一个简单的示例来学习如何使用 Vue 组件的 slot。

假设您有一个名为 `ParentComponent` 的父组件，以及一个名为 `ChildComponent` 的子组件。

**ChildComponent.vue**:
```vue
<template>
  <div class="child-component">
    <slot></slot>
  </div>
</template>
```

在上面的代码中，我们定义了一个名为 `slot` 的插槽，它表示了子组件中的一个可插入位置。

**ParentComponent.vue**:
```vue
<template>
  <div class="parent-component">
    <ChildComponent>
      <p>This content will be placed inside the slot of ChildComponent.</p>
    </ChildComponent>
  </div>
</template>

<script>
import ChildComponent from './ChildComponent.vue';

export default {
  components: {
    ChildComponent
  }
}
</script>
```

在上述代码中，我们在 `ChildComponent` 的标签内部插入了一段内容。这段内容将会被插入到 `ChildComponent` 中定义的 slot 中。

当 `ParentComponent` 被渲染时，`ChildComponent` 中的 slot 就会被填充上面提供的内容。

可以根据需要在父组件中插入不同的内容，从而实现更多的灵活性和复用性。








































