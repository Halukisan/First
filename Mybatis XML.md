# MyBatis XML

使用mybatis XML，首先要在配置文件中添加配置

```
mybatis.mapper-locations=classpath:com/youkeda/comment/dao/*.xml
#扫描指定路径下的所有XML文件的加载（*会匹配所有）
```

amespace是命名空间的含义，一般就是我们这个mapper所对应的DAO接口的全称，比如这里的UserDao

```
<?xml version="1.0" encoding="UTF-8"?><!--头信息-->
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.youkeda.dewu.dao.UserDAO">
</mapper>
```

resultMap用于处理表和DO对象的属性映射，确保表中的每个字段都有属性可以匹配

```
<resultMap id="userResultMap" type="com.youkeda.dewu.dataobject.UserDO"><!--resultMap用于处理表和DO对象的属性映射，确保表中的每个字段都有属性可以匹配-->
    <id column="id" property="id"/>
    <result column="user_name" property="userName"/>
    <result column="pwd" property="pwd"/>
    <result column="nick_name" property="nickName"/>
    <result column="avatar" property="avatar"/>
    <result column="gmt_created" property="gmtCreated"/>
    <result column="gmt_modified" property="gmtModified"/>
</resultMap>
```

* id

  > 唯一标识，一般我们的命名规则是`` xxxResultMap``,比如这里的``userResultMap``，基于这样的规则就能保证唯一了。

* type

  > 对应的DO类完整路径，比如这里的``com.youkeda.comment.dataobject.UserDO``

  resultMap子节点

  最主要的是``id、result``这两个子节点

  * id

    > 设置数据库主键字段信息，column属性对应的是表的字段名称，property对应的是DO属性名称

  * result

    > 设置数据库的其他字段信息，column属性对应的是表的字段名称，property对应的是DO属性名称

  下面来写一个完整的resultMap

  ```
  
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="com.youkeda.comment.dao.UserDAO">
  
    <resultMap id="userResultMap" type="com.youkeda.comment.dataobject.UserDO">
      <id column="id" property="id"/>
      <result column="user_name" property="userName"/>
      <result column="pwd" property="pwd"/>
      <result column="nick_name" property="nickName"/>
      <result column="avatar" property="avatar"/>
      <result column="gmt_created" property="gmtCreated"/>
      <result column="gmt_modified" property="gmtModified"/>
    </resultMap>
  
  </mapper>
  
  
  ```

  下面是关于Mybatis XML 的各种语句

  ```
   <insert id="batchAdd" parameterType="java.util.List" useGeneratedKeys="true" keyProperty="id">
          INSERT INTO user (user_name, pwd, nick_name,avatar,gmt_created,gmt_modified)
          VALUES
          <foreach collection="list" item="it" index="index" separator=",">
              (#{it.userName}, #{it.pwd}, #{it.nickName}, #{it.avatar},now(),now())
          </foreach>
      </insert>
  
  
      <insert id="add" parameterType="com.youkeda.dewu.dataobject.UserDO" useGeneratedKeys="true" keyProperty="id">
          INSERT INTO user (user_name, pwd, nick_name,avatar,gmt_created,gmt_modified)
          VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar},now(),now())
      </insert>
  
      <update id="update" parameterType="com.youkeda.dewu.dataobject.UserDO">
          update user
          <set>
              <if test="nickName != null">
                  nick_name=#{nickName},
              </if>
          </set>
          gmt_modified=now()
          where id=#{id}
      </update>
  
      <delete id="delete">
          delete from user where id=#{id}
      </delete>
  
      <select id="findAll" resultMap="userResultMap">
          select * from user
      </select>
  
      <select id="findByUserName" resultMap="userResultMap">
          select * from user where user_name=#{userName} limit 1
      </select>
  
      <select id="query" resultMap="userResultMap">
          select * from user where user_name like CONCAT('%',#{keyWord},'%')
          or nick_name like CONCAT('%',#{keyWord},'%')
      </select>
  
      <select id="search" resultMap="userResultMap">
          select * from user
          <where>
              <if test="keyWord != null">
                  user_name like CONCAT('%',#{keyWord},'%')
                  or nick_name like CONCAT('%',#{keyWord},'%')
              </if>
              <if test="startTime != null">
                  and gmt_created <![CDATA[ >= ]]> #{startTime}
              </if>
              <if test="endTime != null">
                  and gmt_created <![CDATA[ <= ]]> #{endTime}
              </if>
          </where>
      </select>
  
      <select id="findByIds" resultMap="userResultMap">
          select * from user
          <where>
              id in
              <foreach item="item" index="index" collection="ids"
                       open="(" separator="," close=")">
                  #{item}
              </foreach>
          </where>
      </select>
  
  ```

  比如里面的insert节点，它有两个属性

  * id

    > 通DAO类的方法名，同一个XML内是要唯一的，比如这里的``id="add"``是和``UserDAO.add``一致的

  * paramterType

    > 这个用于传递参数类型，一般是和``UserDAO.add``方法的参数类型一致的

  现在我们改一下API的方法调用

  ```
  @PostMapping("/user")
  @ResponseBody
  public UserDO save(@RequestBody UserDO userDO) {
      userDAO.add(userDO);
      return userDO;
  }
  ```

  如果我们想同时获得插入的主键id值，你还需要配置``useGeneratedKeys、keyProperty``，这和之前注解``@Options``介绍的一样

  ```
  <insert id="add" parameterType="com.youkeda.comment.dataobject.UserDO" useGeneratedKeys="true" keyProperty="id">
      INSERT INTO user (user_name, pwd, nick_name,avatar,gmt_created,gmt_modified)
      VALUES(#{userName}, #{pwd}, #{nickName}, #{avatar},now(),now())
  </insert>
  ```

  ## MyBatis XML条件语句

  **if语句**

  ```
  <update id="update" parameterType="com.youkeda.comment.dataobject.UserDO">
    update user set
     <if test="nickName != null">
      nick_name=#{nickName},gmt_modified=now()
     </if>
     where id=#{id}
  </update><update id="update" parameterType="com.youkeda.comment.dataobject.UserDO">
    update user set nick_name=#{nickName},gmt_modified=now() where id=#{id}
  </update>
  ```

  updata语句里面，为了防止字段出现nul，我们一般配合着条件语句来执行

  通过这个if语句，我们就可以提前判断，从而保证数据不会因为错误而丢失

  

  同时，在实际开发中，我们一般会结合set语句来编写update语句

  ```
  <update id="update" parameterType="com.youkeda.comment.dataobject.UserDO">
    update user
    <set>
      <if test="nickName != null">
        nick_name=#{nickName},
      </if>
      <if test="avatar != null">
        avatar=#{avatar},
      </if>
      gmt_modified=now()
    </set>
     where id=#{id}
  </update>
  ```

  

  **if+select**

  很多时候我们的查询条件都是动态的，比如下面的例子：我们想模糊查找某个时间后注册的用户

  ```
  List<UserDO> search(@Param("keyWord")String keyWord,
        @Param("time")LocalDateTime time);
  ```

  SQL XML文件如下

  ```
  <select id="search" resultMap="userResultMap">
    select * from user where
      <if test="keyWord != null">
        user_name like CONCAT('%',#{keyWord},'%')
          or nick_name like CONCAT('%',#{keyWord},'%')
      </if>
      <if test="time != null">
        and  gmt_created <![CDATA[ >= ]]> #{time}
      </if>
  </select>
  ```

  > \``>=、<、<=、>、>=、& `` 这类的表达式会导致MyBatis解析失败，所以我们需要使用``<![CDATA[ key ]]>``来包围住。

  由于这里的参数是LocalDateTime，我们学习过Http的知识应该知道，URL传递过来的参数都是字符串类型的，所以这里的API如下：

  ```
  @Controller
  public class UserController {
  
      @Autowired
      private UserDAO userDAO;
  
      @GetMapping("/user/search")
      @ResponseBody
      public List<UserDO> search(@RequestParam("keyWord") String keyWord,
      @RequestParam("time")
      @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
      LocalDateTime time) {
          return userDAO.search(keyWord, time);
      }
  }
  ```

  如上面代码所示，我们在``LocalDateTime``这个参数里面还额外增加了``@DateTimeFormat``注解，这个注解是由Spring提供的，用于把字符串转化为日期类型

  **where**

  Mybatis XML是有纠错能力的，这个时候我们建议把SQL中where改成MyBatis XML的where子句

  ```
  <select id = "search" resultMap="userResultMap">
  select * from user
  	<where>
  		<if test="keyWord != null">
  			user_name like CONCAT('%',#{keyWord},'%')
  			or nick_name like CONCAT('%',#{keyWord},'%')
  		</if>
  		<if test="time != null">
  			and gmt_created<![CDATA[ >= ]]>#{time}
  		</if>
  	</where>
  </select>
  ```

  ## Redis

  Redis作为支持高并发的缓存系统，能有效减少mysql数据库的访问。

  ![](https://style.youkeda.com/img/course/d2/redisregistered.svg)

  

  优化的第一个重点操作就是把用户数据缓存在Redis里，每次校验先从Redis里查询用户数据。

  当用户数据更新时，也需要实时更新Redis数据

  

  ### 步骤

  1. 注入RedisTemplate实例

     RedisTemplate是Spring Data Redis提供给用户的最高级的抽象客户端，用户可以直接通过RedisTemplate进行多种操作

     ```
     @Component
     public class UserServiceImpl implements UserService {
     
         @Autowired
         private UserDAO userDAO;
     
         @Autowired
         private RedisTemplate redisTemplate;
     }
     ```

  2. 用户模型实现序列化接口

     ```
     import java.io.Serializable;
     
     public class UserDO implements Serializable {
     
     }
     ```

     用户模型实例在网络上传输，必须能够序列化。对象序列化必须实现Serializable接口。

     > java系统提供的Serializable接口中没有定义任何方法，相当于一个空接口：
     >
     > ![](https://style.youkeda.com/img/ham/course/d2/d4-2-3-1.png?x-oss-process=image/resize,w_1440/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

  那么类实现了Serializable接口，不用再多写方法实现代码，意义主要是标识此对象能够被序列化。

  

  3. 优化重名判断逻辑

     修改UserServiceImpl.register()代码，优先从Redis查询用户名有没有对应的用户对象，如果没有，再从数据库查询一次。

     ```
     UserDO userDO = (UserDO)redisTemplate.opsForValue().get(userName);
     
     if (userDO == null) {
         userDO = userDAO.findByUserName(userName);
     }
     
     if (userDO!=null){
         result.setCode("602");
         result.setMessage("用户名已经存在");
         return result;
     }
     ```

     要读写缓存Value，就要调用``RedisTemplate.opsForValue()``,然后再调用get（）方法根据参数Key取得缓存值。这里以用户名作为缓存Key

  4. 用户实例放入缓存

     UserServiceImpl.register()方法结尾，调用DAO方法向数据库增加一条数据后，把新用户实例放入Redis:

     ```
     userDAO.add(userDO1);
     
     result.setSuccess(true);
     User user = userDO1.toModel();
     result.setData(user);
     
     //新用户注册成功后存入缓存
     redisTemplate.opsForValue().set(userName,userDO1);
     ```

     向Redis存数据，同样需要调用``redisTemplate.opsForValue()``,再调用set（）方法：

     * 第一个参数是Key
     * 第二个参数是Value

     本示例以用户名作为缓存，以用户实例作为Value

     > 存数据的Key和Value，和前面读数据的Key和Value是要对应起来的。

  

  **修改缓存**

  ```
  redisTemplate.opsForValue().set(userName, userDO);
  ```

  

  **删除缓存**

  ```
  redisTemplate.delete(userName);
  ```

  注意：删除操作不再是删除Value，而是通过删除Key来删除整条K-V数据，所以没有调用``opsForValue()``方法，直接调用delete()方法。

  

  对于UserController调用DAO进行数据操作：

  1. ``save()``保存数据库后，再存入Redis

     ```
     @PostMapping("/user")
         @ResponseBody
         public UserDO save(@RequestBody UserDO userDO) {
             if(userDO == null){
                 return null;
             }
             userDAO.add(userDO);
             redisTemplate.opsForValue().set(userDO.getUserName(),userDO);
             return userDO;
         }
     ```

  2. ``batchAdd()``在for循环中依次存入Redis中

     ```
     @PostMapping("/user/batchAdd")
     @ResponseBody
     public List<UserDO>batchAdd(@RequestBody List<UserDO> userDOs){
     	if(CollectionUtils.isEmpty(userDOs)){
     		return userDOs;
     	}
     	userDAO.batchAdd(userDOs);
     	userDOs.forEach(userDO->{
     		redisTemplate.opsForValue().set(userDO.getUserName,userDO);
     	});
     	return userDOs;
     }
     ```

  3. ``update()``重新存入Redis即可

     ```
     @PostMapping("/user/update")
         @ResponseBody
         public UserDO update(@RequestBody UserDO userDO) {
             if(userDO == null){
                 return null;
             }
     
             userDAO.update(userDO);
             redisTemplate.opsForValue().set(userDO.getUserName(),userDO);
             return userDO;
         }
     ```

  4. ``delete()``要先查询出来用户对象实例，才能获取到用户名，再调用缓存删除方法

     ```
     @GetMapping("/user/del")
         @ResponseBody
         public boolean delete(@RequestParam("id") Long id) {
         	if(id == null || id<0){
         		return false;
         	}
         	List<Long>ids = new ArrayList<>();
         	ids.add(id);
         	List<UserDO>userDOS = findByIds(ids);
         	
         	if(!CollectionUtils.isEmpty(userDOS)){
         		UserDO userDO = userDOS.get(0);
         		redisTemplate.delete(userDO.getUserName());
         	}
         	return userDAO.delete(id)>0;
         }
     ```

  5. ``findByUserName()``现根据用户名查询缓存，查不到再查数据库

     ```
     @GetMapping("/user/findByUserName")
     @ResponseBody
     public UserDO findByUserName(@RequestParam("userName") String userName) {
         if(StringUtils.isEmpty(userName)){
     		return null;
     	}
         UserDO userDO = (UserDO)redisTemplate.opsForValue().get(userName);
         
         if(userDO == null){
         	userDO = userDAO.findByUserName(userName);
         }
         return userDO;
     }
     ```

  # <a  id = "List">List</a>

  **插入类目数据**

  ![](https://style.youkeda.com/img/ham/course/d2/d4-3-4-1.jpeg?x-oss-process=image/resize,w_600/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_ne,x_10,y_10)

  * 头元素是指列表左端第一个元素。
  * 尾元素指的是列表右端第一个元素。

  

  添加List数据

  ```
  //第一个对象，省略了set各属性值
  Catgory category = new Category();
  redisTemplate.opsForList().leftPush("categoryList",category);
  
  category = new Category();
  redisTemplate.opsForList().leftPush("categpryList",category);
  ```

  从尾部添加元素

  ```
  // 第一个对象，演示代码省略 set 各属性值
  Category category = new Category();
  redisTemplate.opsForList().rightPush("categoryList", category);
  
  // 第二个对象，演示代码省略 set 各属性值
  category = new Category();
  redisTemplate.opsForList().rightPush("categoryList", category);
  ```

  `leftPush()`和`rightPush()`方法第一个参数都是Key，第二个参数是数据对象。注意：整个列表是Value，列表中的数据元素不能称为Value。

  Java系统会把对象进行序列化，然后存入Redis。Redis会把相应同Key的数据组织到一个数据结构List里面。

  > Category类需要实现`java.io.Serializable`接口，标识为可序列化对象。

  `leftPush()`和`rightPush()`的返回值类型是long，表示执行完数据添加操作后，列表的长度。

  ```
  Long listLength = redisTemplate.opsForList().leftPush("categoryList",category);
  ```

  **查询类目数据**

  查询列表长度

  ```
  Long size = redisTemplate.opsForList().size("categoryList");
  ```

  `size()`方法返回列表的数据量大小，参数是数据Key返回值是Long类型的数字。

  **根据索引查询**

  ```
  Category category = (Category)redisTemplate.opsForList().index("categroyList",index);
  ```

  索引从头部开始计算，第一个元素的索引是0。

  **范围查询**

  ```
  List<Category> categoryDatas = redisTemplate.opsForList().range("categoryList", 0, 1);
  ```

  `range()`方法用于根据索引查询一批数据

  * 第一个参数是数据Key
  * 第二个参数是起始索引
  * 第三个参数是结束索引

  >> - `range("categoryList", 0, 0)` 表示只查询第一条数据
  >> - `range("categoryList", 0, 1)` 就表示查询第一和第二条数据了
  >
  >特别的，如果想查询整个列表的所有数据，第三个参数填 ***-1*** ：`range("categoryList", 0, -1)`

  

  **修改类目数据**

  ```
  // 演示代码省略 set 各属性值
  Category category = new Category();
  redisTemplate.opsForList().set("categoryList", 0, category);
  ```

  必须根据索引重新放入数据，所以第二个参数是索引，第三个参数是新的对象。

  第一个参数是Key。

  **如何知道索引**

  ```
  List<Category>categoryDatas = reidsTemplate.opsForList().range("categoryList",0,-1);
  long index = 0;
  for(Category cat : categoryDatas){
  	if(StringUtils.equals(cat.getId(),"gcl_001")){
  		break;
  	}
  	index++;
  }
  ```

  **删除类目数据**

  从头部删除

  ```
  Category cat = (Category)redisTemplate.opsForList().leftPop("categoryList");
  ```

  从尾部删除

  ```
  Category cat = (Category)redisTemplate.opsForList().rightPop("categoryList");
  ```

  

  > 永远只能删除第一个或最后一个数据，不能从中间位置删除

  

  ![](https://style.youkeda.com/img/ham/course/d2/d4-4-4-1.jpeg?x-oss-process=image/resize,w_922/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

  

  ```
  //用户抢购商品
      @Override
      public Result<Boolean> snappedUp(Long id) {
      	Result result = new Result();
      	if(id == null || id<=0){
      		result.setMessage("请输入不小于1的商品id");
              result.setSuccess(false);
              return result;
      	}
      	//通过id找到key，再从缓存里面根据key找到商品库存信息
      	Integer stock = (Integer)redisTemplate.opsForValue().get(getKey(id));
      	//在缓存中没有找到库存信息
      	if(stock == null){
      		ProductDO productDO = productDAO.selectById(id);
      		if(productDO == null){
      			result.setMessage("商品不存在");
                  result.setSuccess(false);
                  return result;	
      		}
      		//获得库存信息
      	stock = productDO.getStock();
      	
      	//存入缓存
      	redisTemplate.opsForValue().set(getKey(id),productDO);	
      	}
      	
      	if(stock < 1){
                  result.setMessage("商品已经销售空");
                  result.setSuccess(false);
                  return result;
              }
              //缓存中的库存数量-1
              redisTemplate.opsForValue().set(getKey(id),stock-1);
  
              //数据库里面库存数量-1
              productDAO.reduceStock(id,1);
  
          result.data(true);
          result.setSuccess(true);
          return result;
      }
      
      
  ```

  

  采用并发编程来模拟抢购商品，比如并发抢购10次。

  ```
  @GetMapping("/snappedUp")
      @ResponseBody
      public boolean snappedUp(@RequestParam("id") Long id) {
          List<CompletableFuture> cfs = new ArrayList<>();
          for(int i = 0;i<10;i++){
              CompletableFuture<Void> cf  = CompletableFuture.supplyAsync(
                      ()->snappedUpService.snappedUp(id)
              ).thenAccept(r->{
                  if(r.getSuccess()){
                      System.out.println("抢购成功");
                  }else {
                      System.out.println("抢购失败"+r.getMessage());
                  }
              });
              cfs.add(cf);
          }
          try {
              CompletableFuture.allOf(cfs.toArray(new CompletableFuture[]{})).get();
          } catch (Exception e) {
              e.printStackTrace();
          }
  
          return true;
      }
  ```

  **对于CompletableFuture**

  `CompletableFuture`是一个异步任务编排、调度框架，以更优雅的方式实现组合式异步编程。

  > 在方法调用的时候（就是所谓任务），需要等待返回去的返回值就是同步，不等待而继续执行代码的就是异步。采用异步方式，能够支持多个任务并行执行，这中机制称为并发。

  1. Register类

  ```
  import java.util.concurrent.atomic.AtomicInteger;
  
  public class Register {
    private static AtomicInteger count = new AtomicInteger(0);
  
    // 注册学号
    public Student regId(Student student) {
      student.setId(count.incrementAndGet());
      return student;
    }
  }
  ```

  2. 并行注册

     ```
     import java.util.ArrayList;
     import java.util.List;
     import java.util.concurrent.CompletableFuture;
     
     public class StudentIDTest {
       public static void main(String[] args) {
         // 构建学生集合
         List<Student> studentList = new ArrayList<>();
         for (int i = 1; i <= 10; i++) {
           Student s = new Student();
           s.setName("学生" + i);
           studentList.add(s);
         }
     
         Register reg = new Register();
     
         studentList.forEach(s -> {
           CompletableFuture.supplyAsync(
               // 每个学生都注册学号
               () -> reg.regId(s)
             )
             // 学号注册完毕后，打印欢迎消息
             .thenAccept(student -> {
               System.out.println("你好 " + student.getName() + ", 欢迎来到春蕾中学大家庭");
             });
         });
     
         System.out.println("mission complate");
       }
     }
     ```

     

  `CompletableFuture.supplyAsync()`方法运行一个异步任务并且返回结构，所以regId（）方法必须有返回值。

  `supplyAsnc()`方法的作用是：在一个单独的线程执行`reg.regId(s)`语句，本质上就是多线程编程。

  注册完成后，使用`thenAccept()`方法完成后继的任务步骤，`thenAccept()`方法的参数（student）就是前置任务的返回结果，系统会在前一个任务完成后，自动执行`student->{}`后继任务。所以本质上，后继任务也是多线程方式执行的。`thenAccept()`方法通常用于任务链的末尾。

  ![](https://style.youkeda.com/img/ham/course/j5/j5-5-6-1.svg)

  

  **任务执行**

  ```
  CompletableFuture.supplyAsync(()->reg.regId(s))
  .thenApply(student -> {
  	return dis.assignClasses(student);
  })
  .thenAccept(student->{
  	System.out.println("姓名： "+student.getName());
  });
  ```

  `supplyAsync()`用于开头，`thenAccept()`用于末尾，各自调用一次即可。中间有多个步骤，可以调用多次`thenApply()`。由于末尾也要用到`stuent`实例对象，所以位于中间的`thenApply()`方法，总是要`return`学生实例对象，否则下一个步骤就获取不到了。

  ![](https://style.youkeda.com/img/ham/course/j5/j5-5-7-1.svg)

  

  **扩展知识:返回值**

  `supplyAsync()`是静态方法，返回值是`CompletableFuture`实例对象，再调用`thenApply()`或`thenAccept()`实例方法，返回的也是`CompletableFuture`实例对象。

  所以，虽然整条语句是连着写的，其实也可以定义返回值。

  ```
  CompletableFuture<Void> cf = CompletableFuture.supplyAsync(() -> reg.regId(s))
    .thenApply(student -> {
      return dis.assignClasses(student);
    })
    .thenAccept(student -> {
       System.out.println("姓名：" + student.getName() + "，学号：" + student.getId() + "，班级号：" + student.getClassId());
    });
  ```

  返回的仍然是`CompletableFuture`实例对象，所以定义变量的类型就是`CompletableFuture`。但可以用泛型`CompletableFuture<>`表示其中包含的数据类型具体是什么类型。

  因为上面的案例末尾调用了`thenAccept()`，其Lambda表达式没有return语句，表示`CompletableFuture`实例对象**不包含数据**，所以泛型写`CompletableFuture<Void>`.

  

  **返回CompletableFuture类型**

  如果没有调用`thenAccept()`方法，以`thenApply()`或`supplyAsync()`结尾的话，例如代码：

  ```
  CompletableFuture.supplyAsync(()->reg.regId(s))
  	.thenApply(student->{
  		return dis.assignClasses(stduent);
  	});
  ```

  因为`thenApply()`的Lambda表达式返回的是`Sturent`对象，所以`CompletableFuture`实例对象包含的是`Student`数据，于是泛型写为`CompletableFuture<Student>`

  ```
  CompletableFuture<Student> cf = CompletableFuture.supplyAsync(() -> reg.regId(s))
    .thenApply(student -> {
      return dis.assignClasses(student);
    });
  ```

  **main方法的问题**

  如果学生人数比较多，所有注册线程的运行就没有那么快完毕了。

  问题是，可能线程任务没有执行完毕，main（）方法就执行完毕了，导致程序运行结束退出了。

  要解决这个问题，返回值就有用了，我们先把每个学生的入学任务实例对象（`CompletableFuture<Void>`）收集起来（装入集合）然后等待所有的线程执行完毕。

  ```
  List<CompletableFuture> cfs = new ArrayList<>();
  studentList.forEach(s -> {
    CompletableFuture<Void> cf = CompletableFuture.supplyAsync(() -> reg.regId(s))
      .thenApply(student -> {
          return dis.assignClasses(student);
      }).thenAccept(student -> {
          System.out.println("姓名：" + student.getName() + "，学号：" + student.getId() + "，班级号：" + student.getClassId());
      });
  
    cfs.add(cf);
  });
  
  try {
    // 等待所有的线程执行完毕
    CompletableFuture.allOf(cfs.toArray(new CompletableFuture[] {})).get();
  } catch (Exception e) {
    e.printStackTrace();
  }
  ```

  

  `CompletableFuture.allOf()` 是静态方法，的作用就是收集所有的任务实例对象。因为 `allOf()`方法只支持数组不支持集合，所以需要把集合转换成数组`cfs.toArray(new CompletableFuture[] {})`）。当然，你可以一开始就定义数组来收集任务实例对象，因为学生的个数可以通过 `studentList.size()` 取得。`allOf()` 方法的返回值也是 `CompletableFuture` 实例对象。

  

  再调用类方法 `get()`，其作用就是等待所有的任务线程（`allOf()` 收集的）都执行完毕，再继续执行（本案例 `main()` 方法后面没代码了，就退出程序）。

  运行一下：

  代码演示

  

  ***需要强调的是***：

  在 *SpringBoot* 等服务端运行 `supplyAsync()` 异步任务编排的时候，就没有必要可以使用 `get()` 方法等待所有线程任务执行完毕了。因为服务端往往是常驻程序，不像 `main()` 方法执行完毕就退出程序了。

  **再次理解同步与异步**

  `get()` 方法造成了 `main()` 方法等待，所以是同步的；通过 `CompletableFuture` 编排的任务，不会造成 `main()` 方法等待，是异步的。

  

  

  ## Redis事务

  在高并发的场景下，数据的读写没有做控制，速度较快的线程，在库存值被修改之前已经通过了校验库存的步骤，而本线程修改库存数量时，又不知道库存值已经被其他线程修改了，其实不是最新值减一的。

  **什么是事务**

  事务，就是将一个业务逻辑作为一个整体一起执行，四和五其实就是打包一组操作（或命令）作为一个整体，在事务处理时将顺序执行这些操作，并返回结果。，如果任何一个环节出错，所有的操作将被取消。

  **Redis事务基本概念**

  redis事务四大指令：MULTI  EXEC  DISCARD  WATCH

  

  * WATCH 用于客户端并发情况下，为事务提供一个锁，可以用watch命令来监控一个或多个变量，如果在执行事务之前，某个监控项被修改了，那么整个事务会终止执行。
  * MULTI开启一个事务
  * EXEC执行一个事务
  * DISCARD取消一个事务

  **事务基础框架代码**

  ```
  try {
      redisTemplate.execute(new SessionCallback<List<Object>>() {
          @Override
          public List<Object> execute(RedisOperations operations) throws DataAccessException {
  
          }
      });
  } catch (Exception e) {
      LOG.error("redisTemplate.execute() error. ", e);
  }
  ```

  **监听对象**

  在开启事务前，先选择一个需要监听的对象，即redis中的某个Key

  ```
  try{
  redisTemplate.execute(new SessionCallback<List<Object>>(){
  	@Override
  	public List<Object> execute(RedisOperations operations)throws DataAccessException{
  	//监听商品的ID
  	operations.watch(id);
  	}
  });
  }catch(Exception e){
  	LOG.error("redisTemplate.execute() error . ",e);
  }
  ```

  `operations.watch()`方法传入在redis中存在的Key

  1. 开启事务

  ```
  redisTemplate.execute(new SessionCallback<List<Objectt>>(){
  	@Override
  	public List<Object>execute(RedisOperations operations)throws
  DataAccessException{
  	//监听商品的ID
  	operations.watch(id);
  	
  	//开启事务
  	operations.multi();
  }
  })
  ```

  2. 命令入列

  ```
  redisTemplate.execute(new SessionCallback<List<Object>>(){
  	@Override
  	public List<Object> execute(RedisOperations operations) throws
  DataAccessException{
  	//监听商品的ID
  	operations.watch(id);
  	
  	//开启事务
  	operations.multi();
  	
  	//插入一条订单数据
  	//缓存库的库存减一
  	operations.opsForValue().set(idKey,(stock - 1));
  	productDAO.raduceStock(id,1);
  }
  });
  ```

  在execute()方法中，Redis的操作不在使用`redisTemplate.opsForValue()`，而是使用 `operations.opsForValue()` ，这样系统才知道是同一个事务中的操作。

  3.  执行事务

  在完成业务逻辑后，就可以执行事务了：

  ```
  redisTemplate.execute(new SessionCallback<List<Object>>() {
      @Override
      public List<Object> execute(RedisOperations operations) throws DataAccessException {
          //监听商品的ID
          operations.watch(id);
  
          //开启事务
          operations.multi();
  
          // 插入一条订单数据。
          // 缓存库的存减 1
          // 数据库的库存减 1
  
          // 执行事务
          List exec = operations.exec();
      }
  });
  ```

  `operations.exec()`用于执行事务，返回值是List列表，存放了每个事务执行结果的标记。事务开始后执行的每个操作，如果成功了则放入true值作为标记，操作失败则不放入结果标记。

  因为事务要么是每个操作都成功，要么都失败，所以一般来说可以简单处理，不用判断operations.exec()方法返回值列表中的每个元素是否都为true，只要判断返回值列表长度大于0则表示执行成功。

  

  4. 取消事务

  在`execute(RedisOperations operations)` 方法抛异常时，会自动取消事务，本案例不必写取消事务的代码。

  如果有的项目需求，需要手动取消事务的话，代码也非常简单，只需要用：

  ```java
  operations.discard();
  ```

  即可取消事务。

  当然，取消事务和执行事务是互斥的，要注意 `exec()` 和 `discard()` 代码的顺序逻辑。

  ![](https://style.youkeda.com/img/ham/course/d2/d4-4-5-1.jpeg?x-oss-process=image/resize,w_1024/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

  

  从Redis中查询数据：

  ```
  Integer stock = (Integer)redisTemplate.opsForValue().get(getKey(id));
  ```

  判断是否有数据:根据stock是否为空判断

  没有数据的话从数据库里面找

  ```
  ProductDO product = productDAO.selectById(id);
  //判断商品是否存在
  if(product == null){
  	result.setMessage("商品不存在，id = "+id);
  	result.setSuccess(false);
  	return result;
  }
  ```

  将查询到的信息存入redis中

  ```
  stock = product.getStock();
  redisTemplate.opsForValue().set(getKey(id),stick);
  ```

  

  

  开启事务

  事务开始要独立查询，前面的操作是为了保证Redis中存在商品Key

  > 事务开始有基本框架~~~

  

  ```
  try {
      redisTemplate.execute(new SessionCallback<List<Object>>() {
          @Override
          public List<Object> execute(RedisOperations operations) throws DataAccessException {
              Integer stock = (Integer) redisTemplate.opsForValue().get(getKey(id));
              if(stock <1){
                  LOG.error("商品已售完");
                  result.setSuccess(false);
              }
              //监听商品的ID
              operations.watch(getKey(id));
  
              //开启事务
              operations.multi();
  
              //下单
  
              //缓存的库存减一
              operations.opsForValue().set(getKey(id),(stock-1));
  
              //数据库的库存减一
              productDAO.reduceStock(id,1);
  
              //执行事务
              List exec = operations.exec();
  
              if(exec!=null){
                  LOG.error("redisTemplate.execute() error. ");
              }
              return exec;
          }
      });
  } catch (Exception e) {
      LOG.error("redisTemplate.execute() error. ", e);
  }
  ```

  

  执行成功了，就返回成功提示

  ```
  result.data(true);
  result.setSuccess(true);
  return result;
  ```

  

  ## 分布式锁

  什么是分布式

  ![](https://style.youkeda.com/newcoursep4/d2/servier.svg)

  

  分布式结构就是将一个完整的系统，按照业务功能，拆分成一个个独立的子系统，在分布式结构中，每个子系统就被称为服务。

  

  ### 什么是分布式锁

  `synchronized` 关键字在我们的代码中是常见的，这些都是 **本地锁**，只能解决一台服务器并发问题。

  但是随着业务量不断增大，单机结构不满足那么大的访问量，需要变成集群或者分布式结构，因此**我们无法保证某个数据的改变是同一台服务器操作的**。我们需要的是一个能锁所有服务器的锁，这就是本节所要说的分布式锁

  ## Redis 客户端

  要使用 Redis 分布式锁，就需要用到 **Reddissin** 客户端，它提供的功能远远超出了一个 **Redis** 客户端的范畴，在可以使用基本 Redis 功能的同时，也能使用它提供的一些高级服务：

  - 远程调用
  - 分布式锁
  - 分布式对象、容器

  ## 集成

  那么如何在 SpringBoot 中集成 Redission 呢？首先需要在 *pom* 文件中引入下面的依赖：

  ```xml
  <dependencies>
      <dependency>
          <groupId>org.redisson</groupId>
          <artifactId>redisson-spring-boot-starter</artifactId>
          <version>3.13.0</version>
      </dependency>
  </dependencies>
  ```

  > spring-boot-starter-data-redis 仍然保留哦

  别忘了 *application.properties* 中配置 *Redis* 的IP、端口、密码。

  ## Redis 分布式锁实现

  先注入 redissonClient 实例：

  ```java
  import org.redisson.api.RedissonClient;
  @Autowired
  private RedissonClient redissonClient;
  ```

  要实现 *Redis* 分布式锁其实大致只需要三步：

  ### 1. 取得锁

  ```java
  RLock rLock = redissonClient.getLock("CUSTOM_NAME");
  ```

  用 `redissonClient.getLock()` 得到一个 Redis 锁对象，方法参数是 *字符串* 类型的自定义锁名称，不一定要用数据 Key ，一般推荐用容易理解的、业务相关的名称。

  > CUSTOM_NAME 只是代码举例哦

  为了减少冲突、明确含义、易于理解和维护，不要以简单的数字 id 值作为 Redis 中的数据 Key，推荐的格式是：

  ```txt
  productId-1-stock
  ```

  这样就知道 Redis 缓存中这个 Key 的作用和含义。同样，锁的名称也不要太简单，推荐的格式是：

  ```txt
  productId-1-lock
  ```

  这样也比较明确、容易理解。

  当然，这种命名格式并不是强制规定，只是经验之谈，大家可以按照自己的喜好指定格式。

  ### 2. 上锁

  ```java
  rLock.lock();
  ```

  并发情况下，每个线程都会竞争锁：

  - 竞争成功（获取锁）的线程会继续运行
  - 竞争失败的线程会被禁用，并且重新获取锁之前，该线程将一直处于休眠状态。

  简单说，抢不到锁的线程会持续等待，所以使用锁要特别小心。

  ### 3. 解锁

  ```java
  rLock.unlock();
  ```

  有上锁就必须解锁，否则会导致线程持续等待而产生死锁，系统也就卡死了。

  所以，推荐把业务操作放在 *try...catch...finally* 中：

  ```java
  try {
    rLock.lock();
    // 抢购业务逻辑
  } catch (Exception e) {
      LOG.error("some error. ", e);
  } finally {
      rLock.unlock();
  }
  ```

  `finally{}` 代码块无论业务逻辑是否产生异常，一定会执行，这样保证正确与否都能解锁。

  > `catch(){}` 代码块在产生异常才执行，这是异常的基础知识了

  

  

  ## 过期处理

  很多数据都是临时存储，可能用了一次就不用了。例如短信验证码。

  **设置过期时间**

  ```
  redisTemplate.opsForValue().set("code","78987");
  ```

  但是想要在1000ms后过期

  ```
  redisTemplate.opsForValue().set("code","78987",1000,TimeUnit.MILLISECONDS);
  ```

  ![](https://style.youkeda.com/img/course/d2/expire.png?x-oss-process=image/resize,w_500/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

  

  常用时间单位：

  * TimeUnit.MILLISECONDS:毫秒

  * TimeUnit.SECONDS:秒
  * TimeUnit.MINUTES:分钟
  * TimeUnit.HOURS:小时
  * TimeUnit.DAYS:天

  **设置Redis List的过期时间**

  列表结构的操作方法：

  ```
  redisTemplate.opsForList().leftPush()
  ```

  是无法设置过期时间的，这时候可以用：

  ```
  redisTemplate.expire("category",60,TimeUnit.MINUTES);
  ```

  设置过期时间，第一个参数是Key，第二个和第三个参数组合确定具体的时间。

  其实`redisTemplate.expire()`是一个通用方法，可以为任何数据设置过期时间。

  `redisTemplate.expire()` 是一个通用方法，可以为任何数据类型设置过期时间

  ## 删除策略

  还有一个问题，如果一个键已经过期了，那么他什么时候会被清理掉呢，不然会占用存储空间

  Redis 服务器清理过期数据有多种策略：

  ### 1. 惰性删除

  每次查询或写键时，都会检查取得的键是否过期。如果过期就删除该键，否则就返回该键

  这样做对 CPU 最友好。只有在操作的时候进行过期检查，删除的目标仅限于当前需要处理的键，不会在删除其他无关本次操作的过期键上花费任何 CPU 时间

  但是对内存不友好，键过期了，但因为一直没有被访问到，所以一直保留着一直占着存储空间

  ### 2. 定期删除

  每隔一段时间，程序就对数据库进行检查，删除里面的过期键。至于要删除多少过期键，以及检查多少数据库，则由算法决定

  ### 3. 定时删除

  在设置键的过期时间的同时，创建一个定时器，让定时器在键的过期时间来临时，立即执行对键的删除操作。

  这样的做法对于内存是最好的，可以即使释放存储空，但是对于 cpu 却不好，在过期键比较多的情况下，删除过期键会占用相当一部分 CPU 时间。

  这样对服务期的性能会有影响

  > 在企业中，采用什么样的策略，需要经验丰富的架构师确定。

  

  ## 分布式ID

  ID是数据的唯一标识，常见的做法是用数据库的`AUTO_INCREMENT`自增主键机制，自动生成ID。

  **解决的问题**

  随着业务的发展，数据量越来越大，一张表，一个库可能无法容纳太多的数据的时候，就需要对数据进行分表，甚至分库。分表后每个表的数据会按自己的节奏来自增，这样会造成ID冲突，这时就需要一个单独的机制来负责生成唯一ID。

  > 分表、分库的目的都是让每张表都不太大，数据能够均衡分布，提升读写效率

  演示如何生成分布式ID

  引入关键类：

  ```
  import org.redisson.api.RAtomicLong;
  import org.redisson.api.RedissonClient;
  ```

  过程代码：

  ```
  //格式化格式为年月日
  DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyyMMdd");
  //获取当前时间
  String now = LocalDate.now().format(dateTimeFormatter);
  //通过redis的自增获取序号
  RAtomicLong atomicLong = redissonClient.getAtomicLong(now);
  atomicLong.expire(1, TimeUnit.DAYS);
  long number = atomicLong.incrementAndGet();
  //拼装订单号
  String orderId = now + "" + number;
  ```

  通过redis获取一个自增ID，也需要一个Key

  实例代码中，格式化当前日期的作用也是如此，用yyyyMMdd格式的日期作为Key，获取自增ID值。

  

  先获取一个自增的实例对象：

  ```
  RAtomicLong atomicLong = redissonClient.getAtomicLong(now);
  ```

  再取得实际自增值：

  ```
  long number = atomicLong.incrementAndGet();
  ```

  方法返回值类型是长整数型数字`long`。

  # <a id = "ZSet">ZSet</a>

  Zset是一个没有重复元素的字符串集合。不同之处是有序集合的每个成员都关联了一个评分( score) ,这个评分( score)被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但是评分可以是重复了。

  因为元素是有序的,所以你也可以很快的根据评分( score )或者次序( position )来获取一个范围的元素。

  访问有序集合的中间元素也是非常快的,因此你能够使用有序集合作为一个没有重复成员的智能列表。

  **查询方式**

  1. 根据排序值score升序查询

     ```
     Set tv = redisTemplate.opsForZSet.rangeWithScores("TV",0,-1);
     ```

  2. 根据排序值score降序排序

     ```
     Set tv = redisTemplate.opsForZSet().reverseRangeWithScores("TV",0,-1);
     ```

     `rangeWithScores()` 是升序查询，`reverseRangeWithScores()` 是降序查询。参数和返回值的用法都是一样的。

     - 第一个参数是 Key

       > 通用的

     - 第二个参数是起始索引（包含），从 0 开始计

     - 第三个参数是结束索引（包含）

       > 特别的，第三个参数值 -1 查询所有的数据记录。比较常用

     实际上两个方法都是取某个范围的数据：start <= index <= end

     ### 如果想取得前 2 条数据，应该怎么呢？

     前两条数据的起始索引是 0，那么结束索引是 (2-1)，即查询索引为 0、1 的两条数据。对于不习惯从 0 开始计数的同学来说，下列写法可能更容易理解一些：

     ```java
     //0代表数据的第0条数据，2-1代表查询至索引为 1 的数据，即 2 条数据
     redisTemplate.opsForZSet().rangeWithScores("TV", 0, 2-1);
     ```

     ## 返回值 Set

     Set 是 Java 中使用频率一般的集合，不如 List、Map 用的多，其特点是*不允许出现重复元素*，不强调顺序。所以，即使是 null ，也只能存放一个，不能存放多个 null 元素。

     跟 List 一样，Set 也是接口，都继承于 `Collection` 接口。

     最常用的实现类是 HashSet ，是无序的。所以，很多资料上直接说 Set 是无序的。

     > 有兴趣的同学可以自学。网上资料很多，推荐[Java中的Set总结](https://www.jianshu.com/p/d6cff3517688)

     而本节讲的查询是有序的，所以返回值类型实际上不是 HashSet ，而是 LinkedHashSet ，是有序的。

     Java 强调的是“面向接口编程”的思想，那么：对于程序代码来说，返回值不用管具体实现类，而是 Set 接口；对于程序员来说，需要知道返回值是有序的。

     > 同学们可以自己研究原理和数据结构，网上资料很多，推荐[JAVA集合Set之LinkedHashSet详解](https://blog.csdn.net/zhaojie181711/article/details/80510129/)

     ### 遍历返回值 Set 集合

     引入新包：

     ```java
     import org.springframework.data.redis.core.ZSetOperations.TypedTuple;
     import java.util.Set;
     ```

     查询及遍历：

     ```java
     Set<TypedTuple<PersonalRecord>> datas = redisTemplate.opsForZSet().rangeWithScores("integralRank", 0, -1);
     
     // 遍历
     datas.forEach(data -> {
         // 存入的对象
         PersonalRecord pr = data.getValue();
         // 对应的分数
         Double score = data.getScore();
         System.out.println(pr.getId() + " - " + score);
     });
     ```

     查询返回 Set 集合中的元素类型是 `TypedTuple` ，用于整合查询结果对象（通过 `add()` 方法放入 ZSet 中的对象）及其对应的分数，封装在一起，便于读取。

     > TypedTuple 是 ZSetOperations 接口中定义的内部接口

     于是，通过 `getValue()` 可以取得放入 `ZSet` 的元素对象，通过 `getScore()` 可以取得元素的分数。

     

  用户缓存时以用户名作为Key的，万一有用户以integralRank作为用户名。那么就跟个人战绩的数据集合冲突了，可以用Redis的Hash数据结构解决这个问题

  # <a id = "Hash"> 什么是Hash</a>

  Redis Hash是一个字符串类型的field（字段）和value（值）的映射表，Hash特别适合用于存储对象

  ![](https://style.youkeda.com/img/ham/course/d2/hash.jpeg?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

  

  简单的说，Redis的整个Value就是键值对映射结构，通过Key和field取得所需的值。

  **新增和修改**

  把所有的用户数据都存储在integralRankUser为Key的缓存中，以用户名作为fiedl，那么无论用户名是什么，都不会跟其他领域的数据冲突了。

  ```
  redisTemplate.opsForHash().put("integralRankUser",userDO.getUserName(),userDO);
  ```

  `put()` 方法第一个参数是 Key；第二个参数是 field ；第三个参数就是具体的值了。

  > 相当于把 Redis 缓存数据做个归类

  ### 修改

  当 field 相同时，再次 `put()` 会覆盖原来 field 的值。

  ## 读取

  可以根据 Key 和 field 精确查询：

  ```java
  UserDO userDO = (UserDO)redisTemplate.opsForHash().get("integralRankUser", userName);
  ```

  也可以根据 Key 和 一批 field 批量查询：

  ```java
  List<String> userNames = new ArrayList<>();
  userNames.add(userName);
  List<UserDO> users = redisTemplate.opsForHash().multiGet("integralRankUser", userNames);
  ```

  ## 删除

  根据 Key 和 field 进行参数：

  ```java
  redisTemplate.opsForHash().delete("integralRankUser", userName);
  ```

  由于 `delete()` 方法支持变长参数，所以想删除多个 field 时，只需要传入多个 field 即可：

  ```java
  redisTemplate.opsForHash().delete("integralRankUser", userName, "zhangsan", "lisi");
  ```

  ## 小知识点：变长参数

  从 Java5 开始支持变长参数机制，允许在调用方法时传入不确定数量的参数。变长参数是 Java 的一个语法糖，本质上还是基于数组的实现。

  > 语法糖是计算机编程领域场景的术语，表示其实并没有提供全新的功能机制，实现功能与原功能一致，但让代码书写简单、简洁流畅、通俗易懂

  Redis 中定义 `delete()` 方法的源码是：

  ```java
  Long delete(H key, Object... hashKeys)
  ```

  在方法的参数类型后面加三个点（`...`）就表示变长参数，调用时可以传入一个或多个值。而变量 `hashKeys` 的类型实际上就是数组，可以用 `hashKeys.length` 获取输入参数的个数。

  

  ## <a id = "Set">Set</a>

  这里所说的Set，是Redis的数据类型，不要和Java的混淆了。

  ZSet用中文描述是"无序集合"，其特点是：

  1. 集合中的元素是无序的
  2. 集合中的元素不能重复，是唯一的

  ![](https://style.youkeda.com/img/ham/course/d2/set.png?x-oss-process=image/resize,w_848/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

  

  与ZSet相比，由于缺少了分数，所以无法排序

  **新增数据操作**

  可以调用add()方法批量增加数据

  ```
  redisTemplate.opsForSet().add("ranks",personalRecord1,personalRecord2);
  ```

  add方法第一个参数是Key，后面就是java实例对象。

  `add()` 方法第一个参数是 Key；后面就是 Java 实例对象。

  ### 删除数据操作

  使用 `remove()` 方法删除一个数据元素：

  ```java
  redisTemplate.opsForSet().remove("ranks", personalRecord);
  ```

  `remove()` 方法第一个参数是 Key；第二个参数是待删除的数据对象。删除与添加是对应的，添加的是自定义对象，删除的时候也要传入相同的自定义对象。

  > 千万不要理解为，添加的是 personalRecord 实例对象，然后用 personalRecordId 删除。不能跟 MySQL 混淆了。

  ### 修改数据操作

  不存在修改这个说法。或者说，修改等同于：先删除旧数据，再加入新数据

  ## 基本查询

  查询方法不是常见的 getXX 而是 `members()` ：

  ```java
  Set<PersonalRecord> datas = redisTemplate.opsForSet().members("ranks");
  ```

  通过 Key 查询集合中的所有数据元素。返回值的泛型，就是新增数据的类型，往 Set 缓存里放了什么数据，拿出来就是什么数据。

  ## 多集合操作

  使用 Set 一般来说并不是用于数据对象的缓存，因为无序，实际上操作很不方便，不能像列表一样精确查询。

  使用 Set 多用于集合间的操作。所以，推荐 Set 存储简单的数据，比如 Java 的字符串或数字，而不要在 Set 中存入复杂的 Java 自定义对象。

  比如只存入个人战绩的 id 值而不是整个对象。

  多集合的操作主要有：

  ### 求并集

  给定两个集合A，B，把他们所有的元素合并在一起组成的集合，叫做集合A与集合B的并集。大家应该学习过并集的概念。

  `union()` 方法用于求多个集合的并集：

  ```java
  List<String> keys = new ArrayList<>();
  keys.add("ranks1");
  keys.add("ranks2");
  keys.add("ranks3");
  Set<Long> unionDatas = redisTemplate.opsForSet().union(keys);
  ```

  ### 求交集

  集合论中，设A，B是两个集合，由所有属于集合A且属于集合B的元素所组成的集合，叫做集合A与集合B的交集

  `intersect()` 方法用于求多个集合的交集：

  ```java
  List<String> keys = new ArrayList<>();
  keys.add("ranks1");
  keys.add("ranks2");
  keys.add("ranks3");
  Set<Long> interDatas = redisTemplate.opsForSet().intersect(keys);
  ```

  `intersect()` 方法返回给定所有给定集合的交集。

  不存在的集合 key 被视为空集。当给定集合当中有一个空集时，结果也为空集(根据集合运算定律)。

  ### 求差集

  设A，B是两个集合，以属于A而不属于B的元素为元素的集合成为A与B的差集

  `difference()` 方法用于求一个集合与其它集合的差集：

  ```java
  List<String> otherkeys = new ArrayList<>();
  otherkeys.add("ranks2");
  otherkeys.add("ranks3");
  Set<Long> diffDatas = redisTemplate.opsForSet().difference("ranks1", otherkeys);
  ```

  `difference()` 方法返回第一个集合与其他集合之间的差异，也可以认为说第一个集合中独有的元素。不存在的集合 key 将视为空集。

  **Set 由于不常用，有兴趣的同学可以自己写程序实验一下。**

  # 面试

  

  ### Redis为什么这么快

  1. 完全基于内存，绝大部分请求是存粹的内存操作，非常快速。数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O（1）。

  2. 数据结构简单，对数据操作也简单，Redis中的数据结构是专门进行设计的。

     1. String：String 是最基本的 key-value 结构，key 是唯一标识，value 是具体的值，value其实不仅是字符串， 也可以是数字（整数或浮点数），value 最多可以容纳的数据长度是 `512M`。
  
        ![](https://style.youkeda.com/img/ham/course/d2/d4-5-1-1.jpeg?x-oss-process=image/resize,w_848/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
  
        Key也是字符串

     2. <a href = "#List">List</a>：比如说评论的类目关系，虽然类目具备上下级的关系，但只是关联关系，就存储来说，每一条类目都是一条独立的数据，多条类目组成一组类目数据，这就适合用List。

     3. <a href = "#ZSet">ZSet</a>：

        * 字符串集合
  
        * 不允许重复
  
        * 按score（double浮点数）排序
  
          ![](https://style.youkeda.com/img/ham/course/d2/zset.png?x-oss-process=image/resize,w_848/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
  
          适用于排行榜之类的根据指定数值排序的数据。

     4. <a href = "#Hash">Hash</a>：redis hash是一个字符串类型的field（字段）和value（值）的映射表，Hash特别适合用于存储对象。

        ![](https://style.youkeda.com/img/ham/course/d2/hash.jpeg?x-oss-process=image/resize,w_848/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

        同一类数据中，需要根据一个关键字查询的场景。

     5. <a href = "#Set">Set</a>:![](https://style.youkeda.com/img/ham/course/d2/set.png?x-oss-process=image/resize,w_848/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)
  
        适合于多集合运算。
  
  3. 采用单线程。优点包括：避 免了不必要的上下文切换和竞争条件，也不存在多线程切换消耗CPU资源；不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗。
  
  4. 使用多路I/O复用模型，非阻塞IO。I/O多路复用时操作系统支持的，不是Redis独立的功能。
  
     >>问题：
     >>
     >>一个服务器进程和一个客户端进程通信，服务端需要阻塞以等待客户发送数据，那么多个客户与服务器通信时，其他客户端就啊哟持续阻塞（排队）等待处理，并发场景时效率低。
     >
     >>解决：
     >>
     >>有多个客户端连接时，监听每个连接，当其中有一个发来消息时就从阻塞中返回，然后就读取收到的消息，然后又循环阻塞；这样就不会因为阻塞在其中一个而不能处理另一个客户的信息。
     >>
     >>
  
  5. 使用底层模型不同，它们之间底层实现方式以及客户端之间的通信的应用协议不一样，Redis直接自己构建了VM机制，因为一般的系统调用系统函数的话，需要耗费一定的时间去移动和请求；
  

# 缓存穿透、雪崩与击穿

一瞬间大量数据的访问，单一数据库来保存数据的系统会因为面向磁盘，磁盘读写速度比较慢的问题而存在严重的性能弊端，一瞬间成千上万的请求到来，需要系统在极短的时间内完成成千上万次的读写操作，这个时候往往不是数据库能够承受的住的，极其容易造成数据库系统瘫痪，最终导致服务器宕机的严重问题产生。

为了克服上述问题，项目通常引入NoSQL技术，这是一种基于内存的数据库，并提供一定的持久化功能。

redis技术就是NoSQL技术中的一种，但是引入redis又可能出现缓存穿透，缓存击穿，缓存雪崩等问题。

## 缓存穿透

Key对应的数据在数据源并不存在，每次针对此Key的请求从缓存获取不到，请求都会到数据源，从而可能压垮数据源。

比如用一个不存在的用户id获取用户信息，不论缓存还是数据库都没有，若黑客利用此漏洞进行攻击，可能会压垮数据库。

**缓存穿透的解决方案**

1. 简单解决法：缓存空对象结果，但注意超时时间不能过长。
2. 系统解决方法：采用布隆过滤器，将所有可能存在的数据都缓存，那么一个一定不存在的数据会被拦截。

## 缓存雪崩

当缓存服务器重启或者大量缓存集中在某一个时间段失效，这样在失效的时候，也会给后端系统带来很大压力。

雪崩前缓存正常从Redis中获取，示意图如下：

![](https://style.youkeda.com/img/ham/course/d2/xuebeng1.png?x-oss-process=image/resize,w_1314/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_ne,x_10,y_10)

缓存失效瞬间示意图如下：

![](https://style.youkeda.com/img/ham/course/d2/xuebeng2.png?x-oss-process=image/resize,w_1314/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_ne,x_10,y_10)

**缓存雪崩的解决方案**

1. 简单解决：缓存失效时间分散开，比如我们可以在原有的失效时间基础上增加一个随机值，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引起集体失效的事件。
2. 严谨解决：用加锁或者队列的方式保证来保证不会有大量的线程对数据库一次性进行读写，从而避免失效时大量的并发请求落到底层存储系统上。

## 缓存击穿

Key对应的数据存在，但在Redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据，并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。

雪崩强调的时大量的数据缓存过期出现的问题。而击穿侧重于描述少量Key（热点数据)过期时遇到大量并发请求时出现的问题。

**缓存击穿的解决方案**

使用锁机制，在缓存失效的时候（判断拿出来的值为空），不是立即去数据库中查询，而是先加锁，在锁中查询数据库并回设缓存。目的就是防止大并发请求在短时间内请求数据库。

# Key过大要如何处理

*！Key代表的数据量过大！*

1. 单个Key存储的value很大
2. hash、set、zset、list中存储过多的元素（万级或以上）

由于Redis是单线程运行的，如果一次操作的数据量很大会对整个redis的响应时间造成负面影响。

有效的解决方法是拆分！

**单个Key存储的Value很大**

可能是一个大对象，或者Java的大List、Map等容器对象直接存入Redis了。如果是对象，可以把每个属性作为以恶搞File存入hash结构的缓存中，而不是整个对象序列化成字符串存入Redis，这些读写对象，相当于操作Redsi的field，可以有效环节操作大Value带来的性能损耗。

如果是Java的大容器对象，建议不要直接缓存大容器对象，而是使用对应的Redis结构，避免每次查询完整的数据。

**hash、set、zset、list中存储过多的元素**

也是拆分，把大容器拆分成多个小容器。

以Hash为例子，大Hash拆分为多个小Hash，每个小Hash称为桶。

固定一个桶的数量，比如10000，每个小桶的Key带一个数字后缀。

每次存取的时候们现在Java程序中计算field的hash值，抹除1000，确定了该field落在哪个Key上。











































