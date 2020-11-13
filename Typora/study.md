# Java

## 堆栈

> 系统中的堆、栈和数据结构堆、栈不是一个概念。可以说系统中的堆、栈是真实的内存物理区，数据结构中的堆、栈是抽象的数据存储结构。

### 数据结构中的堆栈

> 栈就是一种满足先进后出的性质，是一种数据项按顺序排列的数据结构，只能在栈顶的一端进行插入和删除。可以理解为一个桶，先放入的数据位于桶的下方，且数据只能从桶的顶部出口拿出，也就是先进后出。

> 堆是一种完全二叉树或者近似完全二叉树，完全二叉树是效率很高的数据结构，像十分常用的排序算法、Dijkstra算法、Prim算法等都要用堆才能优化。

### 系统中的堆栈

> 堆，由编译器自动分配释放 ，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。

> 栈，是一个可动态申请的内存空间(其记录空闲内存空间的链表由操作系统维护)，在java中,所有使用new  xxx()构造出来的对象都在堆中存储一般由程序员分配释放， 若程序员不释放，程序结束时可能由OS回收  。注意它与数据结构中的堆是两回事，分配方式倒是类似于链表。
>
> 堆是全局的，堆栈是每个函数进入的时候分一小块，函数返回的时候就释放了，静态和全局变量，new得到的变量，都放在堆中，局部变量放在栈中，所以函数返回，局部变量就全没了。

#### Java中的堆、栈和常量池

```java
1.栈(stack)与堆(heap)都是Java用来在Ram中存放数据的地方。与C++不同，Java自动管理栈和堆，程序员不能直接地设置栈或堆。

2.栈的优势是，存取速度比堆要快，仅次于直接位于CPU中的寄存器。但缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。另外，栈数据可以共享，详见第3点。

堆的优势是可以动态地分配内存大小，所有使用new xxx()构造出来的对象都在堆中存储，生存期也不必事先告诉编译器，Java的垃圾收集器会自动收走这些不再使用的数据。但缺点是，由于要在运行时动态分配内存，存取速度较慢。

3.常量池：存放字符串常量和基本类型常量

常量池的好处是为了避免频繁的创建和销毁对象而影响系统性能，其实现了对象的共享。

	例如字符串常量池，在编译阶段就把所有的字符串文字放到一个常量池中。

	（1）节省内存空间：常量池中所有相同的字符串常量被合并，只占用一个空间。

	（2）节省运行时间：比较字符串时，==比equals()快。对于两个引用变量，只用==判断引用是否相等，也就可以判断实际值是否相等。

4. Java中的数据类型有两种。

一种是基本类型，共有8种，即int, short, long, byte, float, double, boolean, char(注意，不包含String)。

如int a = 3; 这里的a是一个指向int类型的引用，指向3这个字面值。这些字面值的数据，由于大小可知，生存期可知(这些字面值固定定义在某个程序块里面，程序块退出后，字段值就消失了)，出于追求速度的原因，就存在于栈中。
    
5.关于String str = "abc"的内部工作。Java内部将此语句转化为以下几个步骤：

(1) 先定义一个名为str的对String类的对象引用变量放入栈中。

(2) 在常量池中查找是否存在内容为"abc"字符串对象。
(3) 如果不存在则在常量池中创建"abc"，并让str引用该对象。
(4) 如果存在则直接让str引用该对象。

6. 我们再来看看 String str = new String("abc")创建过程

(1) 先定义一个名为str的对String类的对象引用变量放入栈中。

(2) 然后在堆中（不是常量池）创建一个指定的对象，并让str引用指向该对象。

(3) 在常量池中查找是否存在内容为"abc"字符串对象。
(4) 如果不存在，则在常量池中创建内容为"abc"的字符串对象，并将堆中的对象与之联系起来。
(5) 如果存在，则将new出来的字符串对象与字符串常量池中的对象联系起来（即让那个特殊的成员变量value的指针指向它）
    
这也就是有道面试题：String s = new String(“abc”);产生几个对象？答：一个或两个，如果常量池中原来没有”abc”,就是两个。
    
申请响应

栈：只要栈的剩余空间大于所申请空间，系统将为程序提供内存，否则将报异常提示栈溢出。

堆：首先应该知道操作系统有一个记录空闲内存地址的链表，当系统收到程序的申请时，会遍历该链表，寻找第一个空间大于所申请空间的堆结点，然后将该结点从空闲结点链表中删除，并将该结点的空间分配给程序，另外，对于大多数系统，会在这块内存空间中的首地址处记录本次分配的大小，这样，代码中的delete语句才能正确的释放本内存空间。另外，由于找到的堆结点的大小不一定正好等于申请的大小，系统会自动的将多余的那部分重新放入空闲链表中。

申请限制

栈：在Windows下,栈是向低地址扩展的数据结构，是一块连续的内存的区域。这句话的意思是栈顶的地址和栈的最大容量是系统预先规定好的，在 WINDOWS下，栈的大小是2M（也有的说是1M，总之是一个编译时就确定的常数），如果申请的空间超过栈的剩余空间时，将提示overflow。因此，能从栈获得的空间较小。

堆：堆是向高地址扩展的数据结构，是不连续的内存区域。这是由于系统是用链表来存储的空闲内存地址的，自然是不连续的，而链表的遍历方向是由低地址向高地址。堆的大小受限于计算机系统中有效的虚拟内存。由此可见，堆获得的空间比较灵活，也比较大。
```

## [JAVA SPI](https://www.cnblogs.com/jy107600/p/11464985.html)

> - 概念
>   - SPI 全程 Service Provider Interface，是Java提供的一套用来被第三方实现和扩展的接口，它可以用来对框架的组件进行扩展和替换，它的作用就是为被扩展的接口寻找实现。
> - SPI和API的使用场景
>   - API（Application Programming Interface），在大多数情况下，是由实现方提供接口并实现，调用者不能自由选择外部实现。从使用人员上来说，API 直接被应用开发人员使用。
>   - SPI（Service Provider Interface），是由调用方制定接口规范并提供外部实现，调用方在调用时自行选择外部实现。从使用人员上来说，SPI由框架扩展人员使用。

```java
//接口提供
public interface UploadCDN {
    void upload(String url);
}

//外部实现一
public class QiyiCDN implements UploadCDN {  //上传爱奇艺cdn
    @Override
    public void upload(String url) {
        System.out.println("upload to qiyi cdn");
    }
}
//外部实现二
public class ChinaNetCDN implements UploadCDN {//上传网宿cdn
    @Override
    public void upload(String url) {
        System.out.println("upload to chinaNet cdn");
    }
}

//需要在resources目录下新建META-INF/services目录，并且在这个目录下新建一个与上述接口的全限定名一致的文件，在这个文件中写入接口的实现类的全限定名

//调用
public static void main(String[] args) {
    ServiceLoader<UploadCDN> uploadCDN = ServiceLoader.load(UploadCDN.class);
    for (UploadCDN u : uploadCDN) {
        u.upload("filePath");
    }
}
```

### SPI总结

- 不需要改动源码就可以实现扩展，解耦。
- 实现扩展对原来的代码几乎没有侵入性。
- 只需要添加配置就可以实现扩展，符合开闭原则。

## Mybatis

### MyBatis标签详解

```xml
<!-- select 标签的属性信息 -->

<select id="selectUser">
　　<!-- 
　　　　1. id（必须配置） 
　　　　id是命名空间中的唯一标识符，可被用来代表这条语句
　　　　一个命名空间（namespace）对应一个dao接口
　　　　这个id也应该对应dao里面的某个方法（sql相当于方法的实现），因此id应该与方法名一致
	-->
　
　　<!-- 
　　　　2. parapeterType（可选配置，默认由mybatis自动选择处理）
　　　　将要传入语句的参数的完全限定名或别名，如果不配置，mybatis会通过ParamterHandler根据参数类型默认选择合适的typeHandler进行处理
　　　　paramterType 主要指定参数类型，可以是int, short, long, string等类型，也可以是复杂类型（如对象）
　　 -->
　　parapeterType="int"

　　<!-- 
　　　　3. resultType（resultType 与 resultMap 二选一配置）
　　　　用来指定返回类型，指定的类型可以是基本类型，也可以是java容器，也可以是javabean
　　 -->
　　resultType="hashmap"
　　
　　<!-- 
　　　　4. resultMap（resultType 与 resultMap 二选一配置）
　　　　用于引用我们通过 resultMap 标签定义的映射类型，这也是mybatis组件高级复杂映射的关键
　　 -->
　　resultMap="USER_RESULT_MAP"
　　
　　<!-- 
　　　　5. flushCache（可选配置）
　　　　将其设置为true，任何时候语句被调用，都会导致本地缓存和二级缓存被清空，默认值：false
　　 -->
　　flushCache="false"

　　<!-- 
　　　　6. useCache（可选配置）
　　　　将其设置为true，会导致本条语句的结果被二级缓存，默认值：对select元素为true
　　 -->
　　useCache="true"

　　<!-- 
　　　　7. timeout（可选配置）
　　　　这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数，默认值为：unset（依赖驱动）
　　 -->
　　timeout="10000"

　　<!-- 
　　　　8. fetchSize（可选配置）
　　　　这是尝试影响驱动程序每次批量返回的结果行数和这个设置值相等。默认值为：unset（依赖驱动）
　　 -->
　　fetchSize="256"

　　<!-- 
　　　　9. statementType（可选配置）
　　　　STATEMENT, PREPARED或CALLABLE的一种，这会让MyBatis使用选择Statement, PrearedStatement或CallableStatement，默认值：PREPARED
　　 -->
　　statementType="PREPARED"

　　<!-- 
　　　　10. resultSetType（可选配置）
　　　　FORWARD_ONLY，SCROLL_SENSITIVE 或 SCROLL_INSENSITIVE 中的一个，默认值为：unset（依赖驱动）
　　 -->
　　resultSetType="FORWORD_ONLY"
</select>
```

```xml
<!-- resultMap 标签的属性信息 -->

<!-- 
　　1. type 对应的返回类型，可以是javabean, 也可以是其它
　　2. id 必须唯一， 用于标示这个resultMap的唯一性，在使用resultMap的时候，就是通过id引用
　　3. extends 继承其他resultMap标签
 -->
<resultMap type="" id="" extends="">　　
　　<!-- 
　　　　1. id 唯一性，注意啦，这个id用于标示这个javabean对象的唯一性， 不一定会是数据库的主键（不要把它理解为数据库对应表的主键）
　　　　2. property 属性对应javabean的属性名
　　　　3. column 对应数据库表的列名
       （这样，当javabean的属性与数据库对应表的列名不一致的时候，就能通过指定这个保持正常映射了）
　　 -->
　　<id property="" column=""/>
        
　　<!-- 
　　　　result 与id相比，对应普通属性
　　 -->    
　　<result property="" column=""/>
        
　　<!-- 
　　　　constructor 对应javabean中的构造方法
　　 -->
　　<constructor>
　　　　<!-- idArg 对应构造方法中的id参数 -->
       <idArg column=""/>
       <!-- arg 对应构造方法中的普通参数 -->
       <arg column=""/>
   </constructor>
   
   <!-- 
　　　　collection 为关联关系，是实现一对多的关键 
　　　　1. property 为javabean中容器对应字段名
　　　　2. ofType 指定集合中元素的对象类型
　　　　3. select 使用另一个查询封装的结果
　　　　4. column 为数据库中的列名，与select配合使用
    -->
　　<collection property="" column="" ofType="" select="">
　　　　<!-- 
　　　　　　当使用select属性时，无需下面的配置
　　　　 -->
　　　　<id property="" column=""/>
　　　　<result property="" column=""/>
　　</collection>
        
　　<!-- 
　　　　association 为关联关系，是实现一对一的关键
　　　　1. property 为javabean中容器对应字段名
　　　　2. javaType 指定关联的类型，当使用select属性时，无需指定关联的类型
　　　　3. select 使用另一个select查询封装的结果
　　　　4. column 为数据库中的列名，与select配合使用
　　 -->
　　<association property="" column="" javaType="" select="">
　　　　<!-- 
　　　　　　使用select属性时，无需下面的配置
　　　　 -->
　　　　<id property="" column=""/>
　　　　<result property="" column=""/>
　　</association>
</resultMap>
```

```xml
<!-- insert 标签的属性信息 -->

<insert id="insertProject"
　　<!--
　　　　同 select 标签
　　 -->

　　<!-- 
　　　　同 select 标签
　　 -->
　　paramterType="projectInfo"
　　
　　<!-- 
　　　　1. useGeneratedKeys（可选配置，与 keyProperty 相配合）
　　　　设置为true，并将 keyProperty 属性设为数据库主键对应的实体对象的属性名称
　　 --> 
　　useGeneratedKeys="true"

　　<!-- 
　　　　2. keyProperty（可选配置，与 useGeneratedKeys 相配合）
　　　　用于获取数据库自动生成的主键
　　 -->
　　keyProperty="projectId"
>
```

```xml
<!-- 重用 sql 标签 -->
<sql id="userColumns">id,username,password</sql>

<!-- eg: -->
<select id="selectProjectList" paramertType="int" resultType="hashmap">
　　SELECT 
    　　<include refid="userColumns"/>
　　FROM 
    　　t_project_002_project_info
</select>
```

```xml
<!-- 完全限定名使用别名替代 -->
<typeAliases>
    <typeAlias type="com.enh.bean.ProjectInfo" alias="projectInfo"/>
</typeAliases>
```

### Mybatis调用储存过程

```xml
<!-- 
	statementType="CALLABLE" 表示调用的是存储过程
 	parameterType="java.util.Map" 传入的Map中包含所有的入参和出参，其中参数是按照顺序对应的，而不是名称，所以参数的名称可自定义
-->
<select id="getWebReportListWithLastMonth" parameterType="java.util.Map" statementType="CALLABLE">
    call platform.pro_itemMonthReport(
    #{type,mode=IN,jdbcType=INTEGER},
    <if test="type == 1">
        #{out_cursor,mode=OUT,jdbcType=CURSOR,resultMap=webReportItemMap}
    </if>
    <if test="type == 2">
        #{out_cursor,mode=OUT,jdbcType=CURSOR,resultMap=webFromCouponReportItemMap}
    </if>
    <if test="type == 3">
        #{out_cursor,mode=OUT,jdbcType=CURSOR,resultMap=webFromCouponReportItemMap}
    </if>
    <if test="type == 4">
        #{out_cursor,mode=OUT,jdbcType=CURSOR,resultMap=webRestItemMap}
    </if>
    )
</select>

<!--系统发放道具统计-->
<resultMap id="webReportItemMap" type="com.t2cn.propsWarning.ds.domain.WebReportItem">
    <result column="itemcode" property="itemCode" />
    <result column="itemname" property="itemName" />
    <result column="itemtimes" property="itemTimes" />
    <result column="itemcnt" property="itemCnt" />
    <result column="senduser" property="sendUser" />
    <result column="accepter" property="accepter" />
    <result column="dt" property="dt" />
    <result column="msg" property="msg" />
</resultMap>
```

```java
//系统发放道具统计
Map<String,Object> webMap1 = new HashMap<>();
webMap1.put("out_cursor",outCursorLst1);
webMap1.put("type",1);
List<WebReportItem> WebReportList = (List<WebReportItem>) webReportItemService.getWebReportListWithLastMonth(webMap1);

logger.info("out_cursor1>>>>>>>>>" + webMap1.get("out_cursor"));

//查询结果在传入的map中，key为出参的值
```

### 防止SQL注入

1. 在jdbc的PreparedStatement 用占位符 

```java
String sql= "SELECT * FORM emp where ENAME=?";
PreparedStatement ps = connect.preparedStatement(sql);
ps.setString(1,'KING');
ResultSet rs = ps.executeQuery();
```

2. 在mybatis中使用#{param}

```java
<select id="selectByName">
    select * from user WHERE name =  #{name}
</select>
```

3. 防止sql注入原理

```java
String sql = "select * from table where id = ";
String id = "123";
sql += id; // 拼接方式
execute(sql);

拼接方式如果 id 被(恶意)改成 id = "123 or 1=1" 则最终查询结果会完全不一样。

String sql = "select * from table where id = :id";

/*
而占位符的方式将语句与用户数据分开的。即使错写成 id = "123 or 1=1" 也会把
这个整体当作用户数据，而不会把 or 1=1 当成是查询语句。
*/
"select * from table where id = \'123 or 1=1\'"
//可以这样理解用占位符后的实际执行语句(但实际上并不是简单地增加了单引号
```

### 防止字符被转义

```
在使用mybatis 时我们sql是写在xml 映射文件中，如果写的sql中有一些特殊的字符的话，在解析xml文件的时候会被转义，但我们不希望他被转义，所以我们要使用<![CDATA[ ]]>来解决。

<![CDATA[   ]]> 是什么，这是XML语法。在CDATA内部的所有内容都会被解析器忽略。

如果文本包含了很多的"<"字符 <=和"&"字符——就象程序代码一样，那么最好把他们都放到CDATA部件中。

但是有个问题那就是 <if test="">   </if>   <where>   </where>  <choose>  </choose>  <trim>  </trim> 等这些标签都不会被解析，所以我们只把有特殊字符的语句放在 <![CDATA[   ]]>  尽量缩小 <![CDATA[  ]]> 的范围。

实例如下：

<select id="allUserInfo" parameterType="java.util.HashMap" resultMap="userInfo1">
<![CDATA[
SELECT newsEdit,newsId, newstitle FROM shoppingGuide WHERE 1=1 AND newsday > #{startTime} AND newsday <= #{endTime}
]]>
<if test="etidName!=''">
AND newsEdit=#{etidName}
</if>
</select>
因为这里有 ">"  "<=" 特殊字符所以要使用 <![CDATA[   ]]> 来注释，但是有<if> 标签，所以把<if>等 放外面
```

## Spring

### 注解

[@Import](https://www.jianshu.com/p/56d4cadbe5c9)

> 可以在类级别声明或作为元注释声明。
>
> 该注解只能在类上面使用,不能在方法上面 
>
> 元注释就是 作为 注解 的 注解

- 将 @Import 标记的类注册成 bean
- 导入@Configuration类”的具现化，将标有注解“@Configuration”的类和该类中所有的“@Bean”注册为Bean
- 导入ImportBeanDefinitionRegistrar的具体实现，将实现了ImportBeanDefinitionRegistrar接口的类中指定的类加载，不包含该类本身
- 导入ImportSelector的具体实现，略





# Linux

### 1. Linux概述

- Linux是一款开源、稳定、高效、免费的操作系统
- 我们常说的Linux一般指Linux的内核，而Linux有很多发行版，是各大厂商，在内核的基础上，添加了许多应用程序
  - CentOS
  - Redhad

### 2. Linux的安装与设置

#### 2.1 网络适配器设置

- 桥接模式：可以和其他机器通信，但是有可能造成IP冲突（只有253个可用IP）
- NAT模式（网络地址转换）：可以和其他机器通信，且不会有IP冲突
- 主机模式：仅主机，不能连接外网

#### 2.2 安装步骤（略）

- 自定义分区
  - /boot（引导文件）
  - swap（内存不够时，可以代替内存使用）
  - /（根分区）

#### 2.3 连接网络

> 右上角选择虚拟网卡，选择System demo 0

#### 2.4 安装 vm Tool

- 使window和Linux系统中的内容可以互相粘贴复制
- 可以设置一个两个系统共享的文件夹

### 3. Linux基础

#### 3.1 Linux目录结构

> Linux是以文件的形式，管理我们的设备，在Linux中，一切皆文件

|      目录      |                             作用                             | 目录  |                             作用                             |
| :------------: | :----------------------------------------------------------: | :---: | :----------------------------------------------------------: |
|      bin       |                      存放经常使用的指令                      | sbin  |                         高权限的指令                         |
|      home      | 存放普通用户的主目录，在Linux中每个用户都有自己的目录（以用户名命名） | root  |                 该目录为超级管理员的用户目录                 |
|      lib       |                            动态库                            |  etc  |   所有系统管理所需要的配置文件和子目录（如mysql中my.conf）   |
|      usr       |           用户的很多应用程序和文件都放在这个目录下           | boot  |            启动Linux时使用的核心文件（引导文件）             |
|   usr/local    |        安装软件后软件存放的目录（如/usr/local/mysql）        |  var  |    这个目录下存放着频繁变动和扩充的文件（如各种日志文件）    |
|    selinux     |        是一种安全子系统，可以控制程序只能访问特定文件        |  tmp  |                       临时文件存放目录                       |
|      dev       | 类似于windows的设备管理器，会将所有的硬件用文件的方式存储下来 | media | linux系统会自动识别一些设备，如U盘、光驱等，识别后，会将识别的设备挂载到这个目录下 |
|      mnt       |       可以让linux临时挂载别的文件系统（如共享文件夹）        |  opt  |          可以将安装软件存放在这个目录下（如安装包）          |
| proc、sys、srv |                     不要动（系统级目录）                     |       |                                                              |

#### 3.2 远程连接工具

> 需要注意的是，远程连接工具连接Linux远程服务器，需要Linux打开sshd服务（22端口），此服务默认打开
>
> 使用命令setup，选择系统配置即可查看，左侧*则代表此服务开启

- Xshell 5，远程控制Linux（22，SSH协议）
- Xftp 5，上传和下载文件（22，SFTP协议）

#### 3.3 vi和vim编辑器

> 所有Linux都有vi文本编辑器
>
> vim具有编辑程序的能力，可以看做是vi的增强版，可以主动以字体颜色区分正确性，方便程序设计
>
> vim 文件名，如果该文件不存在，将创建一个空文件

- 正常模式：vim 文件名，进入正常模式
  - 拷贝当前行（yy），拷贝当前向下的5行（5yy），粘贴（p）
  - 删除当前行（dd），删除当前向下的5行（5dd）
  - 回到文件的最上方（gg），回到文件的底部（G）
  - 撤销上一步操作（u）
  - 将光标移动到指定的行号，先显示行号，然后输入行号（如20），输入shift + g
- 插入模式：一般在正常模式下，按 i 即可进入插入模式
- 命令行模式：一般在正常模式下，按 Esc 键 + " : " 键即可输入命令
  - :wq，写入并保存
  - :q，退出
  - :q!，退出并强制修改（如修改文件却不想保存，需要使用此命令）
  - 在文件中查找某个单词（/关键字），输入（n）查找下一个
  - 设置文件的行号，:set nu 和 :set nonu

#### 3.4 开机、重启和登录注销

- shutdown
  - shutdown -h now：立即关机
  - shutdown -h 1：表示一分钟之后关机
  - shutdown -r now：表示立即重启
- halt：等价于关机
- reboot：重启
- syn：将内存的数据同步到磁盘
- logout：远程连接时，可以注销登录

#### 3.5 用户管理

> Linux的用户至少属于一个组，若创建用户时未指定对应的用户组，则会创建与用户同名的用户组

- 添加用户
  - useradd 用户名
  - useradd -d 目录（/home/gary/） 用户名，添加用户并制定该用户的home目录为gary
- 设置修改密码
  - passwd 用户名
- 删除用户，一般会保存home目录
  - userdel 用户名
  - userdel -r 用户名，删除该用户并删除用户home目录
- 查询用户信息
  - id 用户名（id root）
- 切换用户，当前用户权限不足时，可以切换到权限较高的用户
  - su - 用户名（su - root），当权限较高的用户切换到权限较低的用户时不需要密码，反之则需要。如果需要切换回原来的用户，使用exit即可
- 查询当前的用户
  - whoami / who am i
- 用户组，类似于角色，系统对有共性的角色进行统一的管理
  - 创建组
    - groupadd 组名
  - 删除组
    - groupdel 组名
  - 添加用户时直接加上组
    - useradd -g 用户组 用户名
  - 修改用户组
    - usermod -g 用户组 用户名
- /etc/passwd：用户的配置文件，记录用户的各种信息
  - 每行的含义：用户名：口令：用户标识号：组标识号：注释性描述：主目录：登录shell
  - <img src="D:\myTypora\image\image-20200623154650122.png" alt="image-20200623154650122" style="zoom: 50%;" />

- /etc/shadow：口令配置文件，加密保存了用户密码
- /etc/group：组配置文件，记录了linux包含的组信
  - 每行的含义：组名：口令：组标识号：组内用户列表（列表一般看不到）

#### 3.6 实用指令

##### 3.6.1 运行级别

>系统的运行级别的配置文件为/etc/inittab
>
>最常用的运行级别为3和5
>
>可以使用命令修改运行级别，也可以直接修改运行级别的配置文件来修改默认的运行级别

- init 0~6，切换用户级别
  - 0，关机
  - 1，单用户模式（可以用来找回密码，因为在单用户模式下，可以不需要密码登录root账号，但是此模式是不允许远程操控的，保证了系统的安全性）
  - 2，多用户无网络服务
  - 3，多用户有网络服务
  - 4，保留
  - 5，图形界面
  - 6，重启

##### 3.6.2 帮助指令

> 当对某个指令不熟悉时，可以使用Linux提供的帮助文档，按q退出帮助文档

- man 命令或者配置文件（可以获取帮助信息，如有哪些参数等等）

- help 命令（可以获取该命令的具体作用）

##### 3.6.3 文件目录

- pwd，显示当前工作目录的绝对路径
- ls [选项] [目录或是文件]，如  ls -al（以列表的形式显示当前目录下所有的文件）
  - -a，显示当前目录所有的文件和目录，包括隐藏的
  - -l，以列表的形式显示信息
  - -h，以人容易观看的形式显示信息
- cd，切换目录
  - cd 或者 cd ~ ，回到当前用户的主目录（home）
  - cd.. ，回到上一级目录
- mkdir [选项] 要创建的目录，创建目录
  - -p，创建多级目录
  - 如创建一个目录：mkdir /home/dog，如创建多级目录：mkdir /home/animal/dog
- rmdir，删除空目录
  - 只能删除空目录，如果目录下面有内容，则不能使用rmdir删除
  - 可以删除rm -rf 要删除的目录，进行删除
- touch 文件名称，创建一个空文件
  
  - 可以一次性的创建多个文件
- cp  [选项]  source  dest，拷贝文件到指定目录
  
  - 复制aaa.txt到home文件夹，cp aaa.txt /home
  - 复制test文件夹及其下所有的文件到/home目录，cp -r test /home
  - \cp，当复制到的目标文件有同名文件时，系统会询问是否覆盖，如果目标文件的数量过多，不想让系统提示可以使用\cp进行强制的覆盖，系统不会提示
- rm  [选项] 要删除的文件或目录

  - -r，递归删除整个文件夹，一般删除整个文件夹用，rm -rf  要删除的文件夹目录
  - -f，强制删除不提示，rm -f  文件，删除不提示
- mv，移动文件或者重命名

  - mv  当前目录/文件名  当前目录/new文件名，修改文件名
  - mv  当前目录/文件名  目标目录/文件名，移动文件
- cat [选项] 要查看的文件，查看文件内容

  - -n，显示行号
  - 通常会和more指令同时使用，如 cat /etc/profile | more
- more  要查看的文件，可以以全屏幕的方式按页显示文件文件的内容

  - 空格键（space），向下翻页
  - Enter，向下翻一行
  - q，退出more
  - Ctrl + F，向下滚动一屏
  - Crtl + B，向上滚动一屏
- less  要查看的文件，可以分屏查看文件内容，less指令在显示文件内容的时候，只先加载需要显示的内容，对于大型文件有较高的效率

  - 空格键（space），向下翻页
  - [pageup]，向上翻页
  - [pagedown]，向下翻页
  - /字符串，向下搜索，n：下一个，N：上一个
  - ?字符串，向上搜索，n：上一个，N：下一个
  - q，退出
- \> 和 >> ，输出重定向和追加

  - ls -l > 文件，可以将 ls -l 的内容写入到文件中，如果该文件不存在，则创建该文件。如果该文件已存在，则覆盖该文件
  - ls -l >> 文件，可以将ls -l 的内容追加到文件中，如果该文件不存在，则创建该文件。如果该文件已存在，则追加到文件末尾
  - cat  文件1 > 文件2，可以将文件1的内容覆盖到文件2
  - echo '内容' >> 文件，将内容追加至文件末尾
- echo [选项] [输入内容]，输出内容到控制台
  - echo	$PATH，显示Linux环境变量，区分大小写
  - echo	'Hello World !'，输出Hello World !
- head 文件，用于显示文件的开头部分内容，默认显示文件前10行内容
  - head -n 5 文件，显示文件前5行内容，此处可以指定任意行数
- tail 文件，用于输入文件中尾部的内容，默认显示文件的后10行的内容
  - tail -n 5 文件，查看文件尾部5行内容
  - tail -f 文件，实时查看该文件的所有更新
- ln -s [原文件或目录] [软连接名]，为原文件创建一个软连接
  - ln -s /root  linkToRoot，创建一个名为linkToRoot的软连接，连接到root目录。此时cd到linkToRoot，pwd显示仍然为原目录
  - rm -rf  linkToRoot，删除软链接，删除时不要带/
- history，查看或执行历史指令
  - history，查看所有的历史指令
  - history 10，查看最近10条历史指令
  - ! 10，执行标号为10的历史指令

##### 3.6.4 时间日期

- date，显示日期时间
  - date，显示当前时间——2020年 06月 30日 星期二 19:31:32 CST
  - date '+%Y-%m-%d %H:%M:%S'，显示年月日时分秒——2020-06-30 19:33:19，注意 ‘ + ’ 不能少
  - date -s 字符串时间，设置系统时间（date -s '2018-10-01 10:00:00'） 
- cal [选项]，显示日历信息
  - cal，显示本月的日历信息
  - cal 2020，显示2020年的日历信息

##### 3.6.5 搜索查找

- find [搜索范围] [选项]，将
  - find /home -name hello.txt，-name表示按照名字查找，/home为查找范围，hello.txt表示查找的文件，此处也可以用通配符进行查询，如 *.txt
  - find /opt -user root 。-user，表示按照文件的拥有者查找
  - find / -size +20M。-size，表示按照文件的大小查询，+20M（+20240k）表示查询大于20M的文件
  - 
- locate，可以快速定位文件的所在目录，该指令利用事先建立起来的系统中所有文件及其路径的locate数据库快速定位文件。locate指令无需遍历整个文件系统，并且查询速度较快。为了保证查询结果的准确，管理员必须定期更新locate时刻
  - 特别说明，由于locate指令基于数据库查询。所以第一次运行前，必须使用updatedb指令创建locate数据库
  - locate hello.txt，快速定位hello.txt的目录
- grep和 | 管道指令，grep：过滤查询，管道符  ' | ' 表示将前一个命令的处理结果输出给后面的命令处理
  - cat hello.txt | grep -ni  yes，表示查询hello.txt文件中是否包含yes，-n为显示行号，-i为忽略大小写

##### 3.6.6 压缩和解压缩

- gzip 文件/gunzip 文件，不能压缩目录
  - gzip 文件，将文件压缩为后缀为gz的压缩文件，不保留原文件
  - gunzip 压缩文件，将压缩文件解压，不保留原文件
  
- zip -r 压缩名 要压缩目录，unzip -d 解压到的目录 要解压的文件
  - zip -r mypackage.zip /home/，将/home下所有的内容压缩到名为mypackage.zip的压缩文件
  - unzip -d /opt/tmp/ mypackage.zip
  
- tar，打包指令，后缀为.tar.gz的文件
  
  > -c，产生.tar打包文件（compress）
  >
  > -v，显示详细信息
  >
  > -f，指定压缩后的文件名
  >
  > -z，打包同时压缩
  >
  > -x，解压文件（extract）
  
  - tar -zcvf ab a.txt b.txt，表示将a.txt和b.txt两个文件打包并压缩成ab.tar.gz
  - tar -zcvf myhome.tar.gz /home/，表示将home文件夹压缩成myhome.tar.gz
  - tar -zxvf a.tar.gz，表示将a.tar.gz解压到当前目录
  - tar -zxvf a.tar.gz  -C /opt/tmp，表示将a.tar.gz解压到/opt/tmp，注意该目录必须存在，-C是区分大小写的，C必须是大写
  

#### 3.7 组管理和权限管理

> 查看文件的所有者（a：all，h：human，l：list）

- ls -ahl
  - 会显示文件的包括文件所有者和文件所在组在内的详细信息

> 修改文件所有者（change own）

- chown	用户名	文件名
  - 将指定文件的所有者更换为指定用户
  - 注意：改变所有者不会改变文件的所在组
  - chown -R tom kkk/，表示将kkk目录下所有的子目录文件的所有者改为tom
  - chown 新所有者:新组 文件目录名，将指定文件的所有者和所在组同时改为指定所有者和所在组

>修改文件所在的组（change group）

- chgrp	组名	文件名
  - 将文件的所在组更换为指定的组
  - 注意：改变所在组不会改变文件的拥有者

> 其他组：除了文件的拥有者和所在组，系统中的其他用户都是其他组

> 文件的权限

- ls -l中显示内容如下，-rwxrw-r-- 1 root root 1213 Feb 2 09:39 ok.txt
  - 第0位确定文件类型（-）
    - ' - '，表示普通类型的文件
    - ‘ d ’，表示目录文件
    - ' l '，表示软链接文件
    - ' c ' ，表示字符设备（键盘）
    - ' b '，表示块文件（硬盘）
  - 第1-3位确定所有者（该文件的所有者）拥有的该文件权限（rwx）
    - ' r '，对于文件和目录，都表示可读和查看
    - ' w '，对于文件来说，表示可以修改，但是不一定可以删除。必须具有改文件所在目录的写权限才能删除。对于目录来说，可以在目录内，创建删除和重命名目录
    - ' x '，对于文件来说，表示可以被执行，对于目录来说，表示可以进入该目录
    - ' - '，表示没有相关权限
  - 第4-6位确定所属组拥有改文件的权限（rw-）
  - 第7-9位确定其他用户拥有改文件的权限（r--）
  - 第10位，如果是文件，则表示该文件的硬链接数。如果是目录，则表示该目录下的子目录数量
  - 第11个（root），表示文件的拥有者
  - 第12个（root），表示文件的所在组
  - 第13个（1213），表示改文件的大小，如果是目录，则该值固定为4096（字节）
  - 第14个（Feb 2 09:39），表示改文件的最后修改时间
  - 第15个（ok.txt），表示文件名

> 修改文件的权限

- chmod

  - 规则一，u：所有者，g：所在组，o：其他人，a：所有人（u+g+o的总和）
  - 规则二，r=4，w=2，x=1。chmod u=rwx,g=rw,o=x  文件目录名 ==》chmod 751 文件目录名

  ```shell
  案例说明
  
  一、
  chmod u=rwx,g=rw,o=x，——文件目录名，为所有者、所在组和其他人设置指定文件目录的相关权限
  chmod o+w 文件目录名，——为其他人添加写的权限
  chmod a-x 文件目录名，——为所有人移除执行的权限
  
  二、
  chmod 751 文件目录名 ==》 chmod u=rwx,g=rw,o=x
  ```

> 最佳实践

```shell
1、创建两个组，policeman和banbit
2、创建四个用户，其中jack和jerry属于policeman组，而xiaohong和xiaoming属于banbit组
3、使用jack用户创建文件，自己可以读写，本组人可以读，其他人没有任何权限
4、jack修改权限，其他组的人可以读，本组的人可以读写

注意：修改权限后，有时需要重新登录一下，才会生效
```

#### 3.8 任务调度

>任务调度，指系统在某个时间执行特定的命令或程序

- crontab [选项]
  - -e，编辑定时任务
  - -l，查询crontab任务
  - -r，删除当前用户所有的crontab任务

```shell
案例说明

一、快速入门
*/1 * * * * ls -l /etc >> /tmp/to.txt
此命令表示每分钟执行一次ls -l /etc >> /tmp/to.txt命令，将ls -l /etc的内容追加到to.txt文件中，如果文件不存在，将被创建

二、参数细节说明
	1.5个占位符的说明
		第一个 * ，表示一个小时中的第几分钟，范围：0-59
		第二个 * ，表示一天中的第几小时，范围：0-23
		第三个 * ，表示一个月中的第几天，范围：1-31
		第四个 * ，表示一年中的第几个月，范围：1-12
		第五个 * ，表示一周中的星期几，范围：0-7（0和7都表示周日）
		注意：星期几和几号最好不要同时出现，它们的单位时间都是天，容易让人混乱
    2.特殊符号的说明
    	* ，代表任何时间，如第一个 * 就表示一个小时中每分钟执行一次的意思
    	, ，代表不连续的时间，如 '* 2,5,16 * * *' 表示每天的2点，5点，16点期间每分钟执行一次
    	- ，代表连续的时间范围，如 '* 2-5 * * *' 表示每天的2至5点每分钟执行一次
    	*/n ，代表间隔时间，如 '*/10 * * * *' 表示每隔10分钟执行一次
三、特定时间执行任务案例
	45 22 * * * 命令		在每天22点45分执行
	0 17 * * 1 命令		每周一，17点整执行
	0 5 1,15 * * 命令		每个月的1号和15号的5点整执行
	40 4 * * 1-5 命令		周1到周5的4点40执行
	*/10 4 * * * 命令		每天4点，每隔10分钟执行
	0 0 1,15 * 1 命令		每个月1号和15号，每周1，0点0分执行
```

> 任务调度的应用实例

```shell
案例一、每隔一分钟，将当前的日期信息，追加到/tmp/mydate文件中
	1、先编写一个文件mytask.sh，路径为/root/mytask.sh，文件内容为date >> /tmp/mydate.txt
	2、给mytask.sh一个可执行权限
	3、crontab -e
	4、编写crontab文件   */1 * * * * /root/mytask.sh
案例二、每天凌晨两点将mysql数据库testdb，备份到文件中
	1、先编写一个文件mytask2.sh，路径为/root/mytask2.sh，文件内容为
	   /usr/local/mysql/bin/mysqldump -u root -proot testdb > /tmp/mydb.bak
	2、给mytask2.sh一个可执行权限，chmod 744
	3、crontab -e
	4、编写crontab文件   0 2 * * * /root/mytask2.sh
```

> crontab相关指令

- crontab -l，显示定时任务列表
- crontab -r，删除所有的任务调度
- service crond restart，重启任务调度
- 如果想要删除单个任务调度，使用crontab -e 删除指定任务调度指令

> crontab -e 指令打开的文件位置为：/var/spool/cron，如果是root用户，则可以看到该目录下有个root文件，该文件保存了定时任务的指令

#### 3.9 Linux磁盘分区、挂载

> 分区的方式

- mbr分区
  - 最多支持四个主分区
  - 系统只能安装在主分区
  - 扩展分区要占一个主分区
  - MBR最大只支持2TB，但拥有最好的兼容性
- gtp分区
  - 支持无限多个主分区（但操作系统可能限制，比如windows下最多128个分区）
  - 最大支持18EB的大容量（1EB=1024PB，1PB=1024TB）
  - windows764位以后支持gtp

> Winows下的磁盘分区

![image-20200702113246115](D:\myTypora\image\image-20200702113246115.png)

> Linux分区

- Linux来说无论有几个分区，分给哪一目录使用，它归根结底就只有一个根目录，一个独立且唯一的文件结构,Linux中每个分区都是用来组成整个文件系统的一部分
- Linux采用了一种叫“载入”的处理方法，它的整个文件系统中包含了一整套的文件和目录，且将一个分区和一个目录联系起来。这时要载入的一个分区将使它的存储空间在一个目录下获得（如下图所示）

![image-20200702113621417](D:\myTypora\image\image-20200702113621417.png)

> 磁盘说明

- Linux硬盘分IDE硬盘和SCSI硬盘，目前基本上是SCSI硬盘
- 对于IDE硬盘，驱动器标识符为“hdx~”,其中“hd”表明分区所在设备的类型，这里是指IDE硬盘了。“x”为盘号（a为基本盘，b为基本从属盘，c为辅助主盘，d为辅助从属盘）,“~”代表分区，前四个分区用数字1到4表示，它们是主分区或扩展分区，从5开始就是逻辑分区。例，hda3表示为第一个IDE硬盘上的第三个主分区或扩展分区,hdb2表示为第二个IDE硬盘上的第二个主分区或扩展分区。
- 对于SCSI硬盘则标识为“sdx~”，SCSI硬盘是用“sd”来表示分区所在设备的类型的，其余则和IDE硬盘的表示方法一样。

> 如下图所示，使用lsblk -f 或者 lsblk 可以查看磁盘的分区情况

![image-20200702140244099](D:\myTypora\image\image-20200702140244099.png)

> 给Linux添加新的硬盘
>
> ![image-20200702144539255](D:\myTypora\image\image-20200702144539255.png)

- 虚拟机添加硬盘
- 分区fdisk/dev/sdb
- 格式化mkfs-text4/dev/sdb1
- 挂载先创建一个/home/newdisk,挂载mount/dev/sdb1/home/newdisk
- 设置可以自动挂载(永久挂载，当你重启系统，仍然可以挂载到/home/newdisk)

```shell
案例说明

一、添加磁盘
在【虚拟机】菜单中，选择【设置】，然后设备列表里添加硬盘，然后一路【下一步】，中间只有选择磁盘大小的地方需要修改，至到完成。然后重启系统（才能识别）

二、磁盘分区
分区命令fdisk/dev/sdb开始对/sdb分区
m 显示命令列表
p 显示磁盘分区同fdisk–l
n 新增分区
d 删除分区
w 写入并退出
说明：开始分区输入n，新增分区，然后选择p，分区类型为主分区。两次回车默认剩余全部空间。最后输入w写入分区并退出，若不保存退出输入q

三、格式化磁盘
分区命令:mkfs-text4/dev/sdb1
其中ext4是分区类型

四、挂载
挂载:将一个分区与一个目录联系起来
mount设备名称挂载目录
例如：mount/dev/sdb1/newdisk
umount设备名称或者挂载目录
例如：umount/dev/sdb1或者umount/newdisk

五、虚拟机永久挂载
永久挂载:通过修改/etc/fstab实现挂载，
执行 vim/etc/fstab，修改文件
添加如下内容，/dev/sdb1	/home/newdisk	ext4	defaults	0	0
添加完成后执行 mount –a 即刻生效
```

#### 4.0 磁盘情况查询

> 查询系统整体磁盘使用情况

- df	- lh，-h可以让信息更容易查看

> 查询指定目录的磁盘占用情况

- du -h	目录

  - -s指定目录占用大小汇总

  - -h带计量单位

  - -a含文件

  - --max-depth=1子目录深度

  - -c列出明细的同时，增加汇总值

    ```shell
    案例
    
    查询/opt目录的磁盘占用情况，深度为1
    
    du -ach --max-depth=1 /opt
    ```

> 工作中常用指令演示

```shell
案例

1、统计/home文件夹下文件的个数
ls -l /home | grep 	"^-" | wc -l
2、统计/home文件夹下目录的个数
ls -l /home | grep 	"^d" | wc -l
3、统计/home文件夹下文件的个数，包括子文件夹里的
ls -lR /home | grep 	"^-" | wc -l
4、统计文件夹下目录的个数，包括子文件夹里的
ls -lR /home | grep 	"^d" | wc -l
5、以树状显示目录结构
tree
```

#### 4.1 网络配置

> 获取指定固定IP

```shell
直接修改配置文件来指定IP,并可以连接到外网(程序员推荐)，编辑vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

![image-20200702162039867](D:\myTypora\image\image-20200702162039867.png)

修改后要重启服务，service network restart 或者 reboot

#### 4.2 进程管理

>在LINUX中，每个执行的程序（代码）都称为一个进程。每一个进程都分配一个ID号。
>
>每一个进程，都会对应一个父进程，而这个父进程可以复制多个子进程。例如www服务器。
>
>每个进程都可能以两种方式存在的。前台与后台，所谓前台进程就是用户目前的屏幕上可以进行操作的。后台进程则是实际在操作，但由于屏幕上无法看到的进程，通常使用后台方式执行。
>
>一般系统的服务都是以后台进程的方式存在，而且都会常驻在系统中。直到关机才才结束。

- 查看进程使用的指令是ps,一般来说使用的参数是 ps	-aux
  - 



------



# JS

## ES6

### 序列化

> JSON.stringify会将js对象进行序列化，将对象转为方便传输的字符串
> JSON.parse则会执行相反的操作，进行反序列化，将JSON字符串转换为对象
>
> 序列化：是将对象转换为一种容易传输的格式的过程。如，可以序列化一个对象，然后使用HTTP通过InterNet，在客户端和服务端传输对象，在另一端，反序列化可以从该流中重新构造对象，是对象永久化的一种机制。

### 堆栈

### 循环

```js
let arr = [1,2,3,4];
arr.name = 5;

1、for...in，可遍历数组的所有属性
for(let i in arr){
	console.log(i);//1,2,3,4,name
}
//只索引或输出name，是因为for...in遍历的数组的所有属性名，而数组本身是一个对象，
//属性名就是它的下标，而我们为数组添加了一个新的属性名name，所以最终会输出name

2、for...of，可遍历数组元素值
for(let i of arr){
	console.log(i);//1,2,3,4
}
//看了for-in在数组中的表现，我们发现，必须找到一个可以直接遍历数组每一项的方法
//因此，for-of方法就应运而生，遍历时获得其中的每一项（属性值，同时，对于外界给其作为对象而添加的属性值则不会输出
//（这里name的5没有输出）

3、forEach，详见下方，数组方法
```

### 解构赋值

> 在解构中，有下面两部分参与：
>
> 解构的源，解构赋值表达式的右边部分。 解构的目标，解构赋值表达式的左边部分。

- 基本

  ```js
  //对象
  let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
  //foo为aaa，bar为bbb
  
  let { baz : foo } = { baz : 'ddd' };
  //foo为ddd，其中foo是baz的别名
  
  //数组
  let [a, b, c] = [1, 2, 3];
  // a = 1
  // b = 2
  // c = 3
  ```

- 可忽略可嵌套

  ```js
  //对象
  undefinedlet obj = {p: ['hello', {y: 'world'}] };
  let {p: [x, { y }] } = obj;
  // x = 'hello'
  // y = 'world'
  
  let obj = {p: ['hello', {y: 'world'}] };
  let {p: [x, {  }] } = obj;
  // x = 'hello'
  
  //数组
  let [a, [[b], c]] = [1, [[2], 3]];
  // a = 1
  // b = 2
  // c = 3
  
  let [a, , b] = [1, 2, 3];
  // a = 1
  // b = 3
  ```

- 不完全解构，如果左边的值在解构的源中没有匹配，则为undefined

  ```js
  //对象
  let obj = {p: [{y: 'world'}] };
  let {p: [{ y }, x ] } = obj;
  // x = undefined
  // y = 'world'
  
  //数组
  let [a = 1, b] = []; // a = 1, b = undefined
  ```

- 剩余运算符

  ```js
  //对象
  let {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40};
  // a = 10
  // b = 20
  // rest = {c: 30, d: 40}
  
  //数组
  let [a, ...b] = [1, 2, 3];
  //a = 1
  //b = [2, 3]
  ```

- 解构默认值，当解构目标在解构的源中没有匹配值时，会使用解构目标中设置的默认值

  ```js
  //对象
  let {a = 10, b = 5} = {a: 3};
  // a = 3; b = 5;
  let {a: aa = 10, b: bb = 5} = {a: 3};
  // aa = 3; bb = 5;
  
  //数组
  let [a = 3, b = a] = [];     // a = 3, b = 3
  let [a = 3, b = a] = [1];    // a = 1, b = 1
  let [a = 3, b = a] = [1, 2]; // a = 1, b = 2
  ```

- 在数组的解构中，解构的目标若为可遍历对象，皆可进行解构赋值。可遍历对象即实现 Iterator 接口的数据。

  ```js
  let [a, b, c, d, e] = 'hello';
  // a = 'h'
  // b = 'e'
  // c = 'l'
  // d = 'l'
  // e = 'o'
  ```

### 数组

#### 常用方法

```js
//注：如无特殊备注，以下方法回调参数有三个(element,index,arr)，分别为数组元素，数组下标，原数组

1、forEach，循环遍历数组，无return，forEach函数，写了return也是无效的

//注：此处forEach和map都是遍历数组，而区别在于，forEach没有return、map则有return
2、map，循环遍历数组，有return。每次循环返回一个值，循环结束所有的返回值，将成为新的数组

3、filter，过滤数组，有return，此函数将遍历数组，并返回一个bool值，如果返回值为true则当前元素通过测试，反之则当前元素被过滤

4、every，有return，遍历数组，并依据判断条件，判断数组的元素是否全满足，若满足则返回ture，反之返回false

5、some，有return，遍历数组，依据判断条件，数组的元素是否有一个满足，若有一个满足则返回ture，反之返回false	

6、reduce，有return，该方法有两个参数，第一个参数为回调函数，第二个参数为初始值，其中第二个参数可选。而该方法的回调函数有四个参数（init，next，index，arr），如果调用reduce函数时，其第二个初始值的参数被提供，则init等于被提供的初始值，next为数组中第一个值。如果初始值参数未被提供，则init为数组的第一个值，next为数组的第二个值。之后init则等于上一次循环中return的值

reduce的常见用法如下：
（1）、数组求和
[1, 2, 3, 4].reduce(function(a, b){
    return a + b;
})
（2）、合并两个数组作为一个对象的键值对
var columns = ["Date", "Number", "Size", "Location", "Age"];
var rows = ["2001", "5", "Big", "Sydney", "25"];
var result = rows.reduce(function(result, field, index) {
    result[columns[index]] = field;
    return result;
}, {})
{
  Date: "2001",
  Number: "5",
  Size: "Big",
  Location: "Sydney",
  Age: "25"
}
（3）、将对象数组合并为一个对象
var array = [{
     key: 'one',
     value: 1 
},{
     key: 'two',
     value: 2 
},{
     key: 'three',
     value: 3 
}];

方法1：
array.reduce(function(obj, current) { 
    obj[current.key] = current.value;
    return obj;
}, {});

方法2：
array.reduce((obj, current) => Object.assign(obj, {
  [current.key]: current.value
}), {});

（4）、找到最小或者最大的值
var arr = [4, 2, 1, -10, 9]
 
arr.reduce(function(a, b) {
    return a < b ? a : b
}, Infinity);

（5）、找到唯一的值
var arr = [1, 2, 1, 5, 9, 5];
 
arr.reduce((prev, number) => { 
    if(prev.indexOf(number) === -1) {
        prev.push(number);
    }
    return prev; 
}, []);

7、indexOf()，查找某个元素的索引值，若有重复的，则返回第一个查到的索引值，若不存在，则返回 -1

8、lastIndexOf和indexOf()的功能一样，不同的是从后往前查找

9、Array.from，将伪数组变成数组，就是只要有length的就可以转成数组。from有三个参数（arrayLike，[, mapFn[, thisArg]]）
arrayLike：伪数组、mapFn：map函数、thisArg：map函数中this的指向（可以用来解耦）如下

（1）、只有一个参数
Array.from('hello world');
结果：h,e,l,l,o, ,w,o,r,l,d

let arrayLike = {  
    '0': 'a',  
    '1': 'b',  
    '2': 'c',  
    length: 3  
};  
let arr = Array.from(arrayLike);
alert(Array.isArray(arr));//true
(2)、两个参数
Array.from([1, 2, 3, 4, 5], (n) => n + 1));
结果：2,3,4,5,6
（3）、三个参数
let diObj = {
  handle: function(n){
    return n + 2
  }
}
Array.from(
    [1, 2, 3, 4, 5],
    function (x){
    	return this.handle(x)
    },
    diObj
)

10、Array.of，将一组值转换成数组，类似于声明数组

注：ES6为Array增加了of函数用已一种明确的含义将一个或多个值转换成数组

因为，用new Array()构造数组的时候，是有二意性的。
构造时，传一个参数，表示生成多大的数组。
构造时，传多个参数，每个参数都是数组的一个元素。
ES6增加的Array.of()方法，只有一个含义，of的参数就是表示转换后数组的元素。

如：const arr6 = Array.of(1, 3, '白色', {p1: 'v1'})	//[1,3,"白色",{"p1":"v1"}]

11、find，有return，找到第一个符合条件的数组成员

12、findIndex，找到第一个符合条件的数组成员的索引值
        
13、include，判断数中是否包含给定的值

注：与indexOf()的区别
1 indexOf()返回的是数值，而includes()返回的是布尔值
2 indexOf() 不能判断NaN，返回为-1 ，includes()则可以判断

14、push，从后面添加元素，返回值为添加完后的数组的长度

15、pop，从后面删除元素，只能是一个，返回值是删除的元素

16、shift，从前面删除元素，只能删除一个 返回值是删除的元素

17、unshift，从前面添加元素, 返回值是添加完后的数组的长度

18、splice(i,n) 删除从i(索引值)开始之后的那个元素。返回值是删除的元素，其中 参数 i 为索引值，n为个数
  
19、concat，有return，连接两个数组 返回值为连接后的新数组

20、str.split，将字符串转化为数组

21、sort，将数组进行排序,返回值是排好的数组，默认是按照最左边的数字进行排序，不是按照数字大小排序的，见例子

22、reverse，将数组反转,返回值是反转后的数组

23、slice，有两个参数（startIndex，endIndex），切去索引值start到索引值end的数组，不包含end索引的值，返回值是切出来的数组
```

### 扩展运算符

#### 常见用法

1. 合并数组（如下）

   ```js
   var arr1 = ['a', 'b'];  
   var arr2 = ['c'];  
   var arr3 = ['d', 'e'];  
   
   // ES5 的合并数组  
   arr1.concat(arr2, arr3);  
   // [ 'a', 'b', 'c', 'd', 'e' ]  
   // ES6 的合并数组  
   [...arr1, ...arr2, ...arr3]  
   // [ 'a', 'b', 'c', 'd', 'e' ]  
   ```

2. 与解构赋值结合

   ```js
   // ES5  
   a = list[0], rest = list.slice(1)  
   // ES6  
   [a, ...rest] = list  
   下面是另外一些例子。  
   const [first, ...rest] = [1, 2, 3, 4, 5];  
   first // 1  
   rest // [2, 3, 4, 5]  
   const [first, ...rest] = [];  
   first // undefined  
   rest // []:  
   const [first, ...rest] = ["foo"];  
   first // "foo"  
   rest // []
   
   
   //如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
   const [...butLast, last] = [1, 2, 3, 4, 5];  
   //  报错  
   const [first, ...middle, last] = [1, 2, 3, 4, 5];  
   //  报错  
   const [first, , last,...middle] = [1, 2, 3, 4, 5];  
   //	正确
   ```

3. 字符串

   ```js
   [...'hello']  
   // [ "h", "e", "l", "l", "o" ]  
   ```

4. 实现了 Iterator 接口的对象

   凡是实现了Iterator 接口的对象通过扩展运算符都可以转为数组

### 浅拷贝和深拷贝

[深拷贝的终极探索](https://juejin.im/post/6844903692756336653#heading-1)

> 浅拷贝，在在拷贝过程中，遍历时那部分为对象/数组类型指向原来的地址
>
> 深拷贝，完全开辟新的内存地址
>
> 部分深拷贝，如果对象或者数组中包含子数组和子对象，那一部分为浅拷贝
>
> 浅拷贝，如直接创建一个新的变量并赋值
>
> 部分深拷贝，如扩展运算符(…)、Object.assign()
>
> 深拷贝常用方法：如JSON.parse(JSON.stringify(obj))
>
> 这种方法要求对象中的属性值不能是函数、undefined以及symbol值，第二无法拷贝对象原型链上的属性和方法

- 对象的拷贝

  ```js
  var obj = {a: 1, b: 2, c: { a: 3 },d: [4, 5]}
  var obj1 = obj
  var obj2 = JSON.parse(JSON.stringify(obj))//深拷贝常用方法
  var obj3 = {...obj}
  var obj4 = Object.assign({},obj)
  obj.a = 999
  obj.c.a = -999
  obj.d[0] = 123
  console.log(obj1) //{a: 999, b: 2, c: { a: -999 },d: [123, 5]}
  console.log(obj2) //{a: 1, b: 2, c: { a: 3 },d: [4, 5]}
  console.log(obj3) //{a: 1, b: 2, c: { a: -999 },d: [123, 5]}
  console.log(obj4) //{a: 1, b: 2, c: { a: -999 },d: [123, 5]}
  ```

- 数组的拷贝

  ```js
  var arr = [1, 2, 3, [4, 5], {a: 6, b: 7}]
  var arr1 = JSON.parse(JSON.stringify(arr))//深拷贝常用方法
  var arr2 = arr
  var arr3 = [...arr]
  var arr4 = Object.assign([],arr)
  console.log(arr === arr1) //false
  console.log(arr === arr2) //true
  console.log(arr === arr3) //false
  console.log(arr === arr4) //false
  arr[0]= 999
  arr[3][0]= -999
  arr[4].a = 123
  console.log(arr1) //[1, 2, 3, [4, 5], {a: 6, b: 7}]
  console.log(arr2) //[999, 2, 3, [-999, 5], {a: 123, b: 7}]
  console.log(arr3) //[1, 2, 3, [-999, 5], {a: 123, b: 7}]
  console.log(arr4) //[1, 2, 3, [-999, 5], {a: 123, b: 7}]
  ```

- 通过递归自定义函数实现深拷贝（此函数只能复制对象）

  ```js
  function deepCopy(obj) { 
      if(obj === null) return null 
      if(typeof obj !== 'object') return obj;
      if(obj.constructor===Date) return new Date(obj); 
      var newObj = new obj.constructor ();  //保持继承链
      for (var key in obj) {
          if (obj.hasOwnProperty(key)) {   //不遍历其原型链上的属性
              var val = obj[key];
              newObj[key] = typeof val === 'object' ? arguments.callee(val) : val; // 使用arguments.callee解除与函数名的耦合
          }
      }  
      return newObj;  
  };
  let obj1 = {
      x: {
          a: 1
      },
      y: undefined,
      z: function test (c, d) {
          return c+d
      },
      u: Symbol('test')
  }
  let obj2 = deepCopy(obj1)
  obj2.x.a = 3
  console.log(obj1)
  console.log(obj2)
  
  结果:
  
  { x: { a: 1 },
  y: undefined,
  z: [Function: test],
  u: Symbol(test) }
  
  { x: { a: 3 },
  y: undefined,
  z: [Function: test],
  u: Symbol(test) }
  ```

  

# vue

## 实用方法

```vue
1、this.$set()，如this.$set(this.student, 'age', 15)
	本方法三个参数
		（1）要添加属性的对象
		（2）属性名
		（3）属性值
	当需要为data中某一对象动态添加属性时，需要使用此方法。因为vue在初始化时会为所有的已知属性添加getter和setter方法，来进行视图的动态更新，而动态添加的对象属性则需要使用本方法才会拥有getter和setter方法，才能够实现视图的动态更新

2、v-clock
	本方法使用时需要已有一下样式
		[v-clock] {
			display: none;
		}
	如，<div class="mui-content" v-clock></div>
	可以使页面中的动态数据，在加载完毕后才显示，可以有效解决页面数据显示卡顿（或者数据先显示错误的动态代码而后才显示正确的数据）的问题
```

# Jenkins

## Gitlab

### Gitlab安装

1. 安装相关依赖
2. 启动SSH服务，并设置为开机自动启动
3. 设置postfix开机启动，postfix支持gitlab发信功能
4. 开放ssh以及http服务，然后重新加载防火墙列表（如果关闭防火墙，则不需要配置）
5. 下载并安装gitlab安装 
6. 可以修改端口并重载gitlab（默认80端口）
7. 将端口添加到防火墙中

### Gitlab权限分配

1. 添加组
2. 添加项目
3. 添加用户

推送项目到Gitlab

### Jenkins项目构建类型

> Jenkins中自动构建项目类型有很多，常用的有以下三种
>
> 每种类型的构建都可以完成一样的构建过程和结果，只是具体操作方式和灵活度不同。
>
> 而其中流水线类型，则是让开发者使用编码的方式自行编写构建过程，是灵活度最高的类型，更适合一些复杂项目的构建。

- 自由风格（FreeStyle Project）
- Maven项目（Maven Project）
- 流水线项目（Pipeline Project）

### Pipeline

> Pipeline，是一套运行在jenkins上的工作流框架，将原来独立运行与单个节点或多个节点的任务链接起来，实现单个任务难以完成的复杂流程安排和可视化工作。

- Pipeline脚本是由Groovy语言实现的。但是我们没有必要去单独学习该语言
- Pipeline支持两种语法，声名式和脚本式。jenkins推荐使用声名式的语法

#### 安装Pipeline插件

> Manage Jenkins->Manage Plugins->可选插件

![image-20200826102022325](D:\myTypora\image\image-20200826102022325.png)

安装插件后，创建项目的时候多了“流水线”类型

![image-20200826102048962](D:\myTypora\image\image-20200826102048962.png)

#### Pipeline基础模板

```groovy
pipeline{
    agentanystages{
        stage('Hello'){
            steps{
                echo'HelloWorld'
            }
        }
    }
}
```

- stages：代表整个流水线的所有执行阶段。通常stages只有1个，里面包含多个stage
- stage：代表流水线中的某个阶段，可能出现n个。一般分为拉取代码，编译构建，部署等阶段。

- steps：代表一个阶段内需要执行的逻辑。steps里面是shell脚本，git拉取代码，ssh远程发布等任意内容。

```groovy
//声名式
pipeline{
    agentanystages{
        stage('拉取代码'){
            steps{
                echo'拉取代码'
            }
        }
        stage('编译构建'){
            steps{
                echo'编译构建'
            }
        }
        stage('项目部署'){
            steps{
                echo'项目部署'}
        }
    }
}

//脚本式
node{
    def mvnHome
    	stage('Preparation'){//fordisplaypurposes
            
        }    
        stage('Build'){
            
        }
    	stage('Results'){
            
        }
    }
```

- 这次选择"Scripted Pipeline"Node：节点，一个 Node 就是一个 Jenkins 节点，Master 或者 Agent，是执行 Step 的具体运行环境，后续讲到Jenkins的Master-Slave架构的时候用到
- Stage：阶段，一个 Pipeline 可以划分为若干个 Stage，每个 Stage 代表一组操作，比如：Build、Test、Deploy，Stage 是一个逻辑分组的概念。
- Step：步骤，Step 是最基本的操作单元，可以是打印一句话，也可以是构建一个 Docker 镜像，由各类 Jenkins 插件提供，比如命令：sh ‘make’，就相当于我们平时 shell 终端中执行 make 命令一样

### 使用Pipeline

> 进入pipeline流水线语法
>
> 片段生成器中有很多脚本的模板，可以自行设置参数

1. 拉取代码

   ```groovy
   //拉取代码,checkout：Check out from version control
   
   checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'jenkins_gitlab', url: 'http://gitlab1.t2cn.com/hd/t2cnevent.git']]])
   ```

2. 编译打包

   ```groovy
   //执行shell脚本，sh:shell Script
   
   sh label:'', script:'mvncleanpackage'
   ```

3. 部署（需要安装相应插件）

   ```groovy
   //部署到容器,deploy:Deploy war to a container
   
   deployadapters:[tomcat8(credentialsId:'afc43e5e-4a4e-4de6-984f-b1d5a254e434',path:'',url:'http://192.168.66.102:8080')],contextPath:null,war:'target/*.war'
   ```

4. 整体代码

   ```groovy
   pipeline{
       agentanystages{
           stage('拉取代码'){
               steps{
                   echo'拉取代码'
                   checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'jenkins_gitlab', url: 'http://gitlab1.t2cn.com/hd/t2cnevent.git']]])
               }
           }
           stage('编译构建'){
               steps{
                   echo'编译构建'
                   sh label:'', script:'mvncleanpackage'
               }
           }
           stage('项目部署'){
               steps{
                   echo'项目部署'
                   deployadapters:[tomcat8(credentialsId:'afc43e5e-4a4e-4de6-984f-b1d5a254e434',path:'',url:'http://192.168.66.102:8080')],contextPath:null,war:'target/*.war'
               }
           }
       }
   }
   ```

#### Pipeline Script from SCM

> 刚才我们都是直接在Jenkins的UI界面编写Pipeline代码，这样不方便脚本维护，建议把Pipeline脚本放在项目中（一起进行版本控制）

1. 在项目根目录建立Jenkinsfile文件，把内容复制到该文件中,把Jenkinsfile上传到Gitlab

2. 在项目中引用该文件

   ![image-20200826110537192](D:\myTypora\image\image-20200826110537192.png)

### 常用的构建触发器

> Jenkins内置4种构建触发器

- 触发远程构建

  ![image-20200826112337218](D:\myTypora\image\image-20200826112337218.png)

- 其他工程构建后触发（Build after other projects are build）
  1. 创建pre_job流水线工程
  2. 配置需要触发的工程

- 定时构建（Build periodically）
  
- 定时字符串从左往右分别为：分时日月周
  
- 轮询SCM（Poll SCM）
  - 轮询SCM，是指定时扫描本地代码仓库的代码是否有变更，如果代码有变更就触发项目构建
  - 这次构建触发器，Jenkins会定时扫描本地整个项目的代码，增大系统的开销，不建议使用

### Git hook自动触发构建

![image-20200826155740236](D:\myTypora\image\image-20200826155740236.png)

#### 安装Gitlab Hook插件

- Gitlab Hook

- GitLab

![image-20200826155811359](D:\myTypora\image\image-20200826155811359.png)

#### 自动构建

1. 开启webhook功能
   - 使用root账户登录到后台，点击Admin Area -> Settings -> Network
   - 勾选"Allow requests to the local network from web hooks and services"
2. 在项目添加webhook
   - 点击项目->Settings->Integrations



# Docker

## 一、Docker介绍

### 1.1 引言

1. 运行环境和本地环境不一致

2. 在一个Linux系统中运行有多个项目，则这多个项目会互相影响

   如，其中一个项目中出现了死循环，就会占用大量的cpu资源，进而影响其他的项目运行

3. 如淘宝双11用户量激增

   运维成本过高

### 1.2 Docker的思想

1. 集装箱

   会将所有所需要的内容放在不同的集装箱中，谁需要这些环境就去拿这个集装箱

2. 标准化

   提供了一系列的命令，帮助我们去获取集装箱的操作

3. 隔离性

   Docker容器在运行时，会在Linux内核中单独开辟出一块空间，容器之间互不影响

## 二、Docker的基础操作

### 2.1 安装Docker

```sh
# 1.下载Docker依赖，就想Maven依赖jdk一样
yum -y install yum-utils device-mapper-persistent-data lvm2

# 2.修改Docker镜像源
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 3.安装Docker
yum makecache fast
yum -y install docker-ce

# 4.安装成功后，需要手动启动，设置为开机启动，并测试一下Docker
#启动docker服务
systemctl start docker
#设置开机自动启动
systemctl enable docker
#测试
docker run hello-world
```

### 2.2 Docker的中央仓库

> 1. Docker官方的中央仓库：这个仓库是镜像最全的，但是下载速度较慢。https://hub.docker.com/
> 2. 国内的镜像网站：网易蜂巢，daoCloud等，下载速度快，但是镜像相对不全。
>    https://c.163yun.com/hub#/home 
>    http://hub.daocloud.io/ （推荐使用）
> 3. 在公司内部会采用私服的方式拉取镜像（添加配置）
>    #需要创建 /etc/docker/daemon.json，并添加如下内容
>    {
>    	"registry-mirrors":["https://registry.docker-cn.com"],
>    	"insecure-registries":["ip:port"]
>    }
> 4. #重启两个服务
>    systemctl daemon-reload
>    systemctl restart docker

### 2.3 镜像的操作

```sh
# 拉取镜像
docker pull 镜像名称[:tag]，tag为镜像的版本，不写则拉取默认的版本
#举个栗子:docker pull daocloud.io/library/tomcat:8.5.15-jre8

# 查看全部本地镜像
docker images
	repository		镜像名
	tag				标签名（版本号）
	image ID		镜像ID（唯一标识）
	created			创建时间
	size			大小

# 删除本地镜像
docker rmi 镜像标识（不必写完整的标识）
# 栗子：docker rmi b8

# 镜像的导出
docker save -o 导出的路径 要导出的镜像id
# 栗子：docker save -o ./tomcat.image b8，其中tomcat.image是导出对象的名称
# 镜像的导入
docker load -i 要导入的镜像名称
# 栗子：docker load -i tomcat.image
# 通过导出导入的镜像并不规范，名称也需要进行更改，以方便后续的操作

# 修改镜像的名称
docker 镜像ID 镜像名称:镜像tag
# 栗子：docker b8 tomcat:8.5
```

### 2.4 容器的操作

```sh
# 运行容器

# 简单操作
docker run 镜像的标识|镜像的名称[:tag]
# 常用操作
docker run -d -p 宿主机端口:容器端口 --name 容器名称 镜像标识|镜像名称[:tag]
# -d 在后台运行容器
# -p 宿主机端口:容器端口，为了映射Linux和容器端口
# --name 指定容器的名称

# 查看容器
docker ps -qa
# docker ps 默认查看正在运行中的容器
# -q 只查看容器的标识
# -a 查看全部的容器，包括退出状态的容器

# 查看容器的日志
docker logs -f 容器id
# -f 查看容器日志的最后几行

# 进入容器内部（一般不在容器内部操作，常用于查看容器内部文件）
docker exec -it 容器id bash

# 复制文件到容器中
docker cp 文件名称 容器id:容器内部路径
# 栗子 docker cp 111.txt d4:/root/usr/local/tomcat

# 重启容器
docker restart 容器id

# 启动停止的容器
docker start 容器id

# 停止指定容器
docker stop 容器id

# 停止全部容器
docker stop $(docker ps -qa)

# 删除指定容器
docker rm 容器id

# 删除全部容器
docker rm $(docker ps -qa)

```

### 2.5 镜像和容器区别

> 1. 镜像
>
>    - 镜像层级：假设Linux的内核运行在第0层，那么无论怎样运行docker，它都是运行在内核之上的。
>
>    一个Docker的镜像是可以运行在另一个镜像之上的，且这种层级关系是一个多级叠加的。
>
>    我们称在第一层的镜像为基础镜像，其上的镜像为父级镜像（除了最顶层），这些镜像都继承了它们的父级镜像的属性和设置。
>
>    - 镜像标识：镜像通过镜像ID标识，镜像ID是一个64字符的十六进制的字符串。但是当我们运行镜像时，通常我们不会使用镜像ID来引用镜像，而是使用镜像名来引用。
>
>    - 镜像Tag：镜像可以发布为不同的版本，这种机制我们称之为标签（Tag）。
>
> 2. 容器
>    - 当我们使用命令创建一个docker容器，它会在所有镜像层面之前添加一个可写层。这个可写层有运行在cpu上的进程，而且有两个不同的状态，运行态和退出态。当我们使用docker run启动容器，容器就进入运行态，当我们停止容器，它就进入退出态
>    - 当我们有一个docker容器从运行态到退出态，那我们对该容器运行期间的更改都会保存在容器的文件系统中。注意，此处更改在容器的文件系统中，对于镜像本身并无影响
>    - 我们可以用同一个镜像启动多个的容器，这些容器启动后彼此是互相隔离的，我们对于一个容器的更改只局限于那一个容器本身
>    - 如果我们对容器的底层镜像进行更改，正在进行的容器不会发生动态更新。如果想更新容器到其镜。像的新版本，那么必须当心，确保我们是以正确的方式构建了数据结构，否则我们可能会导致损失容器中所有数据的后果
>    - 64字符的十六进制的字符串来定义容器ID，它是容器的唯一标识符。容器之间的交互是依靠容器ID识别的，由于容器ID的字符太长，我们通常只需键入容器ID的前4个字符即可。当然，我们还可以使用容器名，但显然用4字符的容器ID更为简便。

## 三、Docker应用

```sh
# 1、Docker安装tomcat
docker run -d -p 8080:8080 --name tomcat  daocloud.io/library/tomcat:8.5.15-jre8
#或者已经下载了tomcat镜像
docker run -d -p 8080:8080 --name tomcat 镜像的标识
```

----

```sh
# 安装运行mysql容器
docker run -d -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=root daocloud.io/library/mysql:5.7.4
```

-----

```
修改SSM工程环境，设置为Linux中Docker容器的信息
通过Maven的package重新打成war包
将Windows下的war包复制到Linux中
通过docker命令将宿主机的war包复制到容器内部
  
docker cp 文件名称 容器id:容器内部路径
     
测试访问SSM工程
```

## 四、数据卷

> 为了部署SSM的工程，需要使用到cp的命令将宿主机内的ssm.war文件复制到容器内部。
>
> 数据卷：将宿主机的一个目录映射到容器的一个目录中。
>
> 可以在宿主机中操作目录中的内容，那么容器内部映射的文件，也会跟着一起改变。

```sh
# 创建数据卷，此处数据卷还未映射。创建数据卷后，默认会存放在一个目录下/var/lib/docker/volumes/数据卷名称/_data
docker volume create 数据卷名称

# 查看全部数据卷
docker volume ls

# 查看数据卷详情
docker volume inspect 数据卷名称

# 删除数据卷
docker volume rm 数据卷名称

# 容器映射数据卷
# 此处有两个映射方式
# 方式一，使用docker默认的数据卷存放位置，数据卷中会有容器内部映射路径的文件，也就是说，会将容器中的指定路径中的文件带出来
docker run -d -p 8080:8080 --name tomcat -v 数据卷名称:容器内部的路径 镜像id
# 方式一，自定义数据卷的存放位置，则数据卷中不会存在任何文件，需要我们自行创建
docker run -d -p 8080:8080 --name tomcat -v 路径:容器内部的路径 镜像id
```

## 五、自定义镜像

```sh
# 根据dockerfile可以创建自定义镜像，以下为常用的dockerfile命令
from 指定当前自定义镜像的依赖
copy 将相对目录下的文件copy到自定义镜像中
workdir 声明镜像的默认工作目录
run 执行的命令，可以编写多个
cmd 需要执行的命令（在workdir下执行，cmd可以写多个，以最后一个为准）

#示例：
from daocloud.io/library/tomcat:8.5.15-jre8
copy ssm.war /usr/local/tomcat/webapps

#编写完Dockerfile后需要通过命令将其制作为镜像，并且要在Dockerfile的当前目录下，之后即可在镜像中查看到指定的镜像信息，注意最后的 .（当前目录）
docker build -t 镜像名称[:tag] ./
#如 docker build -t tomcat:1.0 ./
```

## 六、Docker-compose

### 6.1 下载安装Docker-Compose

```sh
## 下载Docker-Compose
# github官网搜索docker-compose，下载1.24.1版本的Docker-Compose
# 下载路径：https://github.com/docker/compose/releases/download/1.24.1/docker-compose-Linux-x86_64

## 设置权限
# 需要将DockerCompose文件的名称修改一下，给予DockerCompose文件一个可执行的权限
mv docker-compose-Linux-x86_64 docker-compose
chmod 777 docker-compose

## 配置环境变量
# 方便后期操作，配置一个环境变量
# 将docker-compose文件移动到了/usr/local/bin，修改了/etc/profile文件，给/usr/local/bin配置到了PATH中
mv docker-compose /usr/local/bin
vi /etc/profile
# 添加内容：export PATH=$JAVA_HOME:/usr/local/bin:$PATH
source /etc/profile

# 在任意目录下输入docker-compose进行测试
```

### 6.2 Docker-Compose管理Mysql和Tomcat容器

```sh
# docker-compose使用yml文件的方式管理配置信息
# 在docker-compose.yml文件中一定不能使用tab（制表符），无法识别
# 注：关于镜像的相关信息，如文件结构，文件存放位置等，可以进入容器内部进行查看，或者查看网站中关于镜像的相关介绍

version: '3.1'
services:
  mysql:              #服务的名称
    restart: always   #表示只要docker启动，容器就会启动
    image: daocloud.io/library/mysql:5.7.4  # 指定镜像路径
    container_name: mysql                   #指定容器名称
    ports:
      - 3306:3306      #指定端口号映射
    environment: 
      MYSQL_ROOT_PASSWORD: root             #指定mysql的密码
      TZ: Asia/Shanghai #指定时区
    volumes:
      - /opt/docker_mysql_tomcat/mysql_data:/var/lib/mysql  #指定数据卷映射
  tomcat:
    restart: always
    image: daocloud.io/library/tomcat:8.5.15-jre8
    container_name: tomcat
    ports:
      - 8080:8080
    environment:
      TZ: Asia/Shanghai
    volumes:
      - /opt/docker_mysql_tomcat/tomcat_webapps:/usr/local/tomcat/webapps
      - /opt/docker_mysql_tomcat/tomcat_logs:/usr/local/tomcat/logs
```

----

### 6.3 使用docker-compose命令管理容器

```sh
在使用docker-compose的命令时，默认会在当前目录下找docker-compose.yml文件
     
#1.基于docker-compose.yml启动管理的容器
docker-compose up -d
     
#2.关闭并删除容器
docker-compose down
     
#3.开启|关闭|重启已经存在的由docker-compose维护的容器
docker-compose start|stop|restart
     
#4.查看由docker-compose管理的容器
docker-compose ps
     
#5.查看日志
docker-compose logs -f
```

### 6.4 docker-compose配置dockerfile使用

> 使用docker-compose.yml文件以及Dockerfile文件在生成自定义镜像的同时启动当前镜像，并且由docker-compose去管理容器

```sh
# 编写docker-compose文件
     
# yml文件
version: '3.1'
services:
  ssm:
    restart: always
    build:            # 构建自定义镜像
      context: ../      # 指定dockerfile文件的所在路径
      dockerfile: Dockerfile   # 指定Dockerfile文件名称
    image: ssm:1.0.1   #指定自定义镜像的名称和版本号
    container_name: ssm
    ports:
      - 8081:8080
    environment:
      TZ: Asia/Shanghai
```

----

```sh
# 编写Dockerfile文件
     
from daocloud.io/library/tomcat:8.5.15-jre8
copy ssm.war /usr/local/tomcat/webapps
```

---

```sh
#可以直接基于docker-compose.yml以及Dockerfile文件构建的自定义镜像
docker-compose up -d
# 如果自定义镜像不存在，会帮助我们构建出自定义镜像，如果自定义镜像已经存在，会直接运行这个自定义镜像
#重新构建自定义镜像
docker-compose build
#运行当前内容，并重新构建
docker-compose up -d --build
```



docker 分层

dockerfile命令



------



# SQL

## 事务

### MyISAM和InnoDB的区别

1. Mysql默认采用的是MyISAM
2. MyISAM不支持事务，InnoDB支持事务。InnoDB的autocommit是打开的，所以会把 每条sql封装成一个事务，并自动提交
3. InnoDB支持行锁，而MyISAM只支持表锁。也就是说MyISAM的读锁和写锁是互斥的，MyISAM读写并发时，如果等待队列中既有 
   读请求，也有写请求，默认写请求优先，即是读请求先到，也会优先执行写请求。所以MyISAM不太适合大量查询和修改并存的情
   况，那样查询请求会被长时间堵塞。因为MyISAM是锁表，所以某项读操作比较耗时会使其他写进程堵死
4. InnoDB支持外键，MyISAM不支持

### 事务的特性

- 原子性

  >

- 一致性

- 隔离性

- 持久性

### 事务的隔离级别 

1. **Read Uncommitted (读未提交)**

   > 所有事务可以看到其他未提交事务的结果，而不同事务可以读取到其未提交的数据，叫作脏读

2. **Read Committed (读已提交)**

   > 

3. **Repeattable Read  (可重复读)**

   >

4. **Serializable (可串行读)**

   >

## 存储过程

> - 优点
>   1. 运行速度：对于很简单的sql，存储过程没有什么优势。对于复杂的业务逻辑，因为在存储过程创建的时候，数据库已经对其进行了一次解析和优化。存储过程一旦执行，在内存中就会保留一份这个存储过程，这样下次再执行同样的存储过程时，可以从内存中直接调用，所以执行速度会比普通sql快
>   2. 减少网络传输：存储过程直接就在数据库服务器上跑，所有的数据访问都在数据库服务器内部进行，不需要传输数据到其它服务器，所以会减少一定的网络传输。但是在存储过程中没有多次数据交互，那么实际上网络传输量和直接sql是一样的。而且我们的应用服务器通常与数据库是在同一内网，大数据的访问的瓶颈会是硬盘的速度，而不是网速
> - 缺点
>   1. SQL本身是一种结构化查询语言，但不是面向对象的的，本质上还是过程化的语言，面对复杂的业务逻辑，过程化的处理会很吃力。同时SQL擅长的是数据查询而非业务逻辑的处理，如果把业务逻辑全放在存储过程里面，违背了这一原则
>   2.  如果需要对输入存储过程的参数进行更改，或者要更改由其返回的数据，则您仍需要更新程序集中的代码以添加参数、更新调用，等等，这时候估计会比较繁琐了。
>   3. 开发调试复杂，由于IDE的问题，存储过程的开发调试要比一般程序困难。 
>   4. 没办法应用缓存。虽然有全局临时表之类的方法可以做缓存，但同样加重了数据库的负担。如果缓存并发严重，经常要加锁，那效率实在堪忧。
>   5. 不支持群集，数据库服务器无法水平扩展，或者数据库的切割（水平或垂直切割）。数据库切割之后，存储过程并不清楚数据存储在哪个数据库中。

### 游标

> 概括来讲，SQL的游标是一种临时的[数据库](http://lib.csdn.net/base/mysql)对象，即可以用来存放在数据库表中的数据行副本，也可以指向存储在数据库中的数据行的指针。游标提供了在逐行的基础上操作表中数据的方法。
>
> 因为我们做的数据量大，而且系统上跑的不只我们一个业务。所以，我们都要求尽量避免使用游标，游标使用时会对行加锁，可能会影响其他业务的正常进行。而且，数据量大时其效率也较低效。另外，内存也是其中一个限制。
>  因为游标其实是相当于把磁盘数据整体放入了内存中，如果游标数据量大则会造成内存不足，内存不足带来的影响大家都知道了。
>  所以，在数据量小时才使用游标

#### 什么时候使用游标

> 游标则是处理结果集的一种机制，它可以定位到结果集中的某一行，多数据进行读写，也可以移动游标定位到你所需要的行中进行操作数据。
>
> 可以将其理解为在存储过程中的循环，可以在内存中对查询结果进行精确读写

## Mysql

### 1. 变量

#### 1.1 分类

按照变量的类型可以分为

- 系统变量
  - 全局变量
  - 会话变量
- 自定义变量
  - 用户变量
  - 局部变量

> 全局变量为全局级别的变量。对整个mysql服务器生效，可以跨连接生效，但是不能跨重启。一旦mysql服务器重启，全局变量的更改失效，恢复为默认值。要想永久改变全局变量的值，需要更改mysql的相关配置文件。
>
> 会话变量、用户变量和局部变量为会话级别的变量。所谓会话级别，如在mysql客户端工具创建了两个查询分析器，就是两个会话。可以认为一个会话就是一个线程连接，线程之间不受影响。

#### 1.2 如何使用

##### 1.2.1 系统变量的使用

- 查询所有的系统变量

  ~~~mysql
  show global | session variables;
  ~~~

- 查询满足条件的系统变量

  ~~~mysql
  show global | session variables like '%autocommit%';
  ~~~

- 查询指定的系统变量

  ~~~mysql
  select @@global | @@session.系统变量名;	默认为session
  ~~~

- 为某个系统变量赋值

  ~~~mysql
  # 方式一
  set @@global.系统变量名 = 值;
  # 方式二
  set global | session 系统变量名 = 值;
  ~~~


##### 1.2.2 自定义变量的使用

- 用户变量的声明

  > 可以在任何地方声明和使用

  ~~~mysql
  # 方式一
  set @用户变量名 = 值;
  # 方式二
  set @用户变量名 := 值;
  # 方式三
  select @用户变量名 := 值;
  ~~~

- 局部变量的声明

  > 只能在begin、end中使用

  ~~~mysql
  declare 变量名 类型 [default 值];
  ~~~

- 局部变量的赋值

  ~~~mysql
  # 同上用户变量的三种方式也可在局部变量中使用
  # 方式四
  select 字段 into 局部变量名 from 表名;
  ~~~





# Git

## Git简介

> Git使用的是分布式的版本控制，区别于SVN的集中式的版本控制。
>
> Git使我们每个人的电脑上都有完整的版本库，即使有一台电脑出现问题，也不会影响到其他库的正常操作和版本记录。
>
> Git拥有强大的分支管理。
>
> Git完全兼容Linux命令

## Git安装

> Git安装后，需要进行最后一项设置
>
> 在命令行输入：
>
> git config --global user.name "Your Name"
>
> git config --global user.email "email@example.com"
>
> 因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。

## git常用命令

### 初始化版本库

```shell
#初始化仓库
git init
#将工作区文件提交到暂存区
git add
#将暂存区文件提交到master主分支（git初始化时为我们初始化的分支）
git commit
```

### 本地仓库管理

#### 仓库状态

```shell
#检查git仓库的状态
git status
    
#查看指定文件和当前文件暂存区中的差别
git diff <fileName>

1. 新创建文件没有添加和提交，Untracked files
2. 文件被修改没有添加和提交，Changes not staged for commit
3. 文件被添加到暂存区但未提交，Changes to be committed
4. 所有修改都已添加和提交，nothing to commit, working tree clean
```

#### 版本回退

- git reset --hard commit_id，可以帮助我们在不同的历史版本中切换
  - HEAD（不区分大小写），就是指当前版本，其中HEAD^指上一个版本，HEAD^^是上上个版本，HEAD~100则是指前100个版本
- 可以使用git log来查看历史，已确定你要回退的版本
  - git log --pretty=oneline，则是优化输出内容，在一行内显示出来
- 使用git reflog 来查看命令历史，可以查看我们所有的已提交版本的记录
- 注意：当使用git reset进行版本回退后，使用git log命令就无法看到回退前的版本记录。如果需要查看或者返回回退前的版本需要使用git reflog查看版本的历史版本变更记录

##### 版本历史查询

> git log 命令，可以使版本记录以某种格式展示出来，也可以在其后加一些限制条件，进行版本历史记录的筛查
>
> [具体命令](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)

#### 工作区和暂存区

- 工作区
  - 就是你在电脑里能看到的目录，比如我的myGit文件夹就是一个工作区：
- 暂存区
  - 工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。
  - Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

#### 管理修改

> 每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。

#### 撤销修改

- 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`
- 当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。
- 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

#### 删除文件

- 当我们手动删除了一个文件（没有使用git命令删除），未将删除操作添加或提交，则使用git checkout -- <fileName>帮助我们还原文件
- 如果使用git rm命令删除（相当于将删除操作添加到暂存区），或者将删除操作添加或提交，则要使用git reset 命令放弃暂存区的修改
- 若已经将删除操作提交，则需要进行版本回退

### 远程仓库管理

#### 远程仓库准备

> 以使用Github为例

1. 检查用户主目录下是否存在.ssh目录，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果有则进行下一步，若没有，则使用
   ssh-keygen -t rsa -C "youremail@example.com"创建私匙和公匙，然后一路回车，可以不设置密码
2. 登陆GitHub，打开“Account settings”，将`id_rsa.pub`文件的内容添加到Keys中，填上任意Title

#### 查看远程主机

```shell
#查看本地上所有远程主机名
git remote

#查看远程主机名，和链接信息
$ git remote -v
origin  git@github.com:jquery/jquery.git (fetch)
origin  git@github.com:jquery/jquery.git (push)

如果没有推送权限，则看不到（push）信息
```

#### 添加远程库

> 更多命令参考下方——Git远程操作详解

1. 在Github上创建一个远程仓库
2. 在本地库下运行命令， git remote add origin git@github.com:michaelliao/learngit.git 添加远程库
3. 使用git push -u origin master，将本地库的内容推送到远程库中。其中-u是指为当前分支和要提交的远程库分支创建追踪关系，下次本地库的当前分支提交时，可以直接使用git push命令即可，省略了远程主机名、本地分支名和远程分支名

#### 从远程库克隆

```shell
git clone git@github.com:michaelliao/gitskills.git
```

### 分支管理

#### 创建与合并

```shell
#查看分支
git branch
#创建分支
git branch <name>
#切换分支
git switch <name> 或 git checkout <name>
#创建并切换分支
git switch -c <name> 或 git checkout -b <name>
#合并某分支到当前分支
git merge <name>
#删除分支
git branch -d <name>
```

#### 解决冲突

> 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
>
> 解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
>
> 用 `git log --graph --pretty=oneline --abbrev-commit` 命令可以看到分支合并图。
>
> 也可以直接使用 `git log --graph`查看

#### 分支管理策略

```shell
#通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。
#如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

#准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward
#而且本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去
git merge --no-ff -m "merge with no-ff" dev

#合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
```

#### Bug分支

```shell
#修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

#当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场；

#存储当前分支的修改
git stash
#查看之前存储过的修改
git stash list
#恢复修改，保留存储内容
git stash apply
#删除存储内容
git stash drop
#恢复修改，并删除之前存储内容
git stash pop
#你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
git stash apply stash@{0}

#在master分支上修复的bug，想要合并到当前dev分支，可以用git cherry-pick <commit>命令，把bug提交的修改“复制”到当前分支，避免重复劳动。
git cherry-pick <commit>
```

#### 强制删除分支

```shell
#如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。
git branch -D <name>
```

#### 多人协作

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

#### rebase

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

### 远程操作详解

#### git clone

> 远程操作的第一步，通常是从远程主机克隆一个版本库，这时就要用到git clone命令。
>

#### git remote

> 为了便于管理，Git要求每个远程主机都必须指定一个主机名。`git remote`命令就用于管理主机名。

```shell
#使用-v选项，可以参看远程主机的网址。
$ git remote -v
origin  git@github.com:jquery/jquery.git (fetch)
origin  git@github.com:jquery/jquery.git (push)

#上面命令表示，当前只有一台远程主机，叫做origin，以及它的网址。
#克隆版本库的时候，所使用的远程主机自动被Git命名为origin。如果想用其他的主机名，需要用git clone命令的-o选项指定。

$ git remote show <主机名>
git remote add命令用于添加远程主机。

$ git remote add <主机名> <网址>
git remote rm命令用于删除远程主机。

$ git remote rm <主机名>
git remote rename命令用于远程主机的改名。

$ git remote rename <原主机名> <新主机名>
```

#### git fetch

> 一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到`git fetch`命令。

```shell
$ git fetch <远程主机名>
#上面命令将某个远程主机的更新，全部取回本地。
#git fetch命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响。
#默认情况下，git fetch取回所有分支（branch）的更新。如果只想取回特定分支的更新，可以指定分支名。

$ git fetch <远程主机名> <分支名>
#比如，取回origin主机的master分支。

$ git fetch origin master
#所取回的更新，在本地主机上要用”远程主机名/分支名”的形式读取。比如origin主机的master，就要用origin/master读取。
#git branch命令的-r选项，可以用来查看远程分支，-a选项查看所有分支。

$ git branch -r
origin/master

$ git branch -a
* master
  remotes/origin/master
  
#上面命令表示，本地主机的当前分支是master，远程分支是origin/master。
#取回远程主机的更新以后，可以在它的基础上，使用git checkout命令创建一个新的分支。

$ git checkout -b newBrach origin/master
#上面命令表示，在origin/master的基础上，创建一个新分支。
#此外，也可以使用git merge命令或者git rebase命令，在本地分支上合并远程分支。

$ git merge origin/master
#或者
$ git rebase origin/master
#上面命令表示在当前分支上，合并origin/master。
```

#### git pull

> `git pull`命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。它的完整格式稍稍有点复杂。

```shell
$ git pull <远程主机名> <远程分支名>:<本地分支名>
#比如，取回origin主机的next分支，与本地的master分支合并，需要写成下面这样。

$ git pull origin next:master
#如果远程分支是与当前分支合并，则冒号后面的部分可以省略。

$ git pull origin next
#上面命令表示，取回origin/next分支，再与当前分支合并。实质上，这等同于先做git fetch，再做git merge。

$ git fetch origin
$ git merge origin/next

#在某些场合，Git会自动在本地分支与远程分支之间，建立一种追踪关系（tracking）。比如，在git clone的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的master分支自动”追踪”origin/master分支。
Git也允许手动建立追踪关系。

git branch --set-upstream master origin/next
#上面命令指定master分支追踪origin/next分支。
#如果当前分支与远程分支存在追踪关系，git pull就可以省略远程分支名。

$ git pull origin
#上面命令表示，本地的当前分支自动与对应的origin主机”追踪分支”（remote-tracking branch）进行合并。
#如果当前分支只有一个追踪分支，连远程主机名都可以省略。

$ git pull
#上面命令表示，当前分支自动与唯一一个追踪分支进行合并。
#如果合并需要采用rebase模式，可以使用–rebase选项。

$ git pull --rebase <远程主机名> <远程分支名>:<本地分支名>
#如果远程主机删除了某个分支，默认情况下，git pull 不会在拉取远程分支的时候，删除对应的本地分支。这是为了防止，由于其他人操作了远程主机，导致git pull不知不觉删除了本地分支。
#但是，你可以改变这个行为，加上参数 -p 就会在本地删除远程已经删除的分支。

$ git pull -p
#等同于下面的命令
$ git fetch --prune origin
$ git fetch -p
```

#### git push

> `git push`命令用于将本地分支的更新，推送到远程主机。它的格式与`git pull`命令相仿。

```shell
$ git push <远程主机名> <本地分支名>:<远程分支名>
#注意，分支推送顺序的写法是<来源地>:<目的地>，所以git pull是<远程分支>:<本地分支>，而git push是<本地分支>:<远程分支>。
如果省略远程分支名，则表示将本地分支推送与之存在”追踪关系”的远程分支（通常两者同名），如果该远程分支不存在，则会被新建。

$ git push origin master
#上面命令表示，将本地的master分支推送到origin主机的master分支。如果后者不存在，则会被新建。
#如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。

$ git push origin :master
#等同于
$ git push origin --delete master
#上面命令表示删除origin主机的master分支。
#如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。

$ git push origin
#上面命令表示，将当前分支推送到origin主机的对应分支。
#如果当前分支只有一个追踪分支，那么主机名都可以省略。

$ git push
#如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。

$ git push -u origin master
#上面命令将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用git push了。
#不带任何参数的git push，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采#用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用git config命令。

$ git config --global push.default matching
#或者
$ git config --global push.default simple
#还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用–all选项。

$ git push --all origin
#上面命令表示，将所有本地分支都推送到origin主机。
#如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做git pull合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用–force选项。

$ git push --force origin
#上面命令使用–force选项，结果导致远程主机上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用–force选项。
#最后，git push不会推送标签（tag），除非使用–tags选项。
$ git push origin --tags
```

### 标签管理

> 发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。
>
> Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。
>
> Git有commit，为什么还要引入tag？
>
> “请把上周一的那个版本打包发布，commit号是6a5819e...”
>
> “一串乱七八糟的数字不好找！”
>
> 如果换一个办法：
>
> “请把上周一的那个版本打包发布，版本号是v1.2”
>
> “好的，按照tag v1.2查找commit就行！”
>
> 所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

#### 创建标签

```shell
#创建标签
$ git tag v1.0

#查看所有的标签
$ git tag
v1.0

#查看历史提交记录
$ git log --pretty=oneline --abbrev-commit

#对指定的提交打标签
$ git tag v0.9 f52c633

#注意，标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：
$ git show v0.9
commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:56:54 2018 +0800

    add merge

diff --git a/readme.txt b/readme.txt

#还可以打有说明的标签
$ git tag -a v0.1 -m "version 0.1 released" 1094adb


#用命令git show <tagname>可以看到说明文字：
$ git show v0.1
tag v0.1
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 22:48:43 2018 +0800

version 0.1 released

commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800
```

#### 操作标签

- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。





# SpringBoot

## 启动的初始化

![image-20201030110813493](F:\Other\Typora\Image\image-20201030110813493.png)

```java
如上图所示，run方法中主要分为两个步骤

- new SpringApplication：环境的初始化和其他的准备工作
- run：启动容器，以及自动配置类的服务
```

![image-20201030111335058](F:\Other\Typora\Image\image-20201030111335058.png)

```java
如上图，new SpringApplication 中的初始化工作

1.设置初始化器
2.设置容器的监听器
```

![image-20201030112209936](F:\Other\Typora\Image\image-20201030112209936.png)

```java
如上图，此处是对如何进行SpringBoot初始化容器的配置的调用
```

![image-20201030112828975](F:\Other\Typora\Image\image-20201030112828975.png)

```java
如上图，SpringBoot容器的初始化工作
    
关键逻辑为：利用SpringFactortiesLoader类对所有Jar包资源进行扫描，找出位于Jar包下，META-INF/spring.factorties的文件
		  并扫描每个文件中的各项配置（如：自动配置类的名字、Springboot初始化器等），其中关键参数type，则是将扫描结果进行
    	  筛选，此处传入的应该为ApplicationContextInitializer（Springboot的初始化器）
    	  而且在扫描的过程中，也会对所有扫描到的资源进行缓存，在之后的运行中，则会在缓存中进行查找，而不需要再次进行扫描。
```

![image-20201030113127039](F:\Other\Typora\Image\image-20201030113127039.png)

```
如上图，展示的是springboot的初始化器
```

![image-20201030150355668](F:\Other\Typora\Image\image-20201030150355668.png)

```java
如上图，为SpringFactortiesLoader类对Jar包资源进行扫描的实现调用
```

![image-20201030153014671](F:\Other\Typora\Image\image-20201030153014671.png)

```xml
如上图，为SpringFactortiesLoader类对Jar包资源进行扫描的实现逻辑

其中包含了三重循环
第一重，所有的Jar资源进行循环

第二重，对扫描到的Jar资源下的每个类进行循环，具体如下

# Initializers
org.springframework.context.ApplicationContextInitializer=\
org.springframework.boot.autoconfigure.SharedMetadataReaderFactoryContextInitializer,\
org.springframework.boot.autoconfigure.logging.ConditionEvaluationReportLoggingListener

# Application Listeners
org.springframework.context.ApplicationListener=\
org.springframework.boot.autoconfigure.BackgroundPreinitializer

# Auto Configuration Import Listeners
org.springframework.boot.autoconfigure.AutoConfigurationImportListener=\
org.springframework.boot.autoconfigure.condition.ConditionEvaluationReportAutoConfigurationImportListener

# Auto Configuration Import Filters
org.springframework.boot.autoconfigure.AutoConfigurationImportFilter=\
org.springframework.boot.autoconfigure.condition.OnBeanCondition,\
org.springframework.boot.autoconfigure.condition.OnClassCondition,\
org.springframework.boot.autoconfigure.condition.OnWebApplicationCondition

第三重，将对每个类下的实现进行循环

最后封装到result（MultiValueMap）中返回
```

## 热部署

```xml
<!-- 热部署依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>

<!-- 
实现热部署 
1.引入热部署依赖
2.idea不会自动编译，需要通过ctrl+F9来触发

如果想要自动生效得修改idea设置，需要做以下设置
（1）File-Settings-Compiler-Build Project automatically（设置IDEA自动编译）
（2）ctrl + shift + alt + / ,选择Registry,勾上 Compiler autoMake allow when app running（运行IDEA在运行时编译）
以上设置完成后，我们每次将浏览器切换到IDEA前方时，都会触发热部署

热部署的排除
1.默认情况下，/META-INF/maven，/META-INF/resources，/resources，/static，/templates，/public这些文件夹下的文件修改不会使应用重启
但是devtools内嵌了一个LiveReload server，当资源发生改变时，浏览器刷新，所以我们也会得到重新刷新的效果

2.同时我们可以根据自己的意愿来设置想要排除的资源，如下
spring.devtools.restart.exclude=static/**,public/**

-->
<!--

热部署的实现原理，是使用了两个classLoader（类加载器），其中一个classLoader只加载那些不会轻易改变的类（如第三方Jar包等），而另一个
classLoader则会加载会被修改的类，这个类加载器也被称为restartClassLoader，这样在代码被改变的时候，restartClassLoader则会被丢
弃，而重新创建的restartClassLoader则会加载到被改变的内容，需要重新加载的内容较少，所以可以较为快速的及逆行重启

-->
```

## 配置文件的位置

> springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件
>
> 优先级由高到底，高优先级的配置会覆盖低优先级的配置
>
> SpringBoot会从这四个位置全部加载主配置文件；互补配置

- file:./config/  跟路径下config文件夹
- file:./  项目的跟路径，如果当前的项目有父工程，配置文件要放在父工程 的根路径
- classpath:/config/
- classpath:/

## yaml

> yml是YAML（YAML Ain't Markup Language）语言的文件，以数据为中心，比properties、xml等更适合做配置文件
>
> yml和xml相比，少了一些结构化的代码，使数据更直接，一目了然。
>
> 相比properties文件更简洁

### 语法

> 以空格的缩进程度来控制层级关系。空格的个数并不重要，只要左边空格对齐则视为同一个层级。且大小写敏感。支持字面值，对象，数组三种数据结构，也支持复合结构

- 字面值：字符串，布尔类型，数值，日期。字符串默认不加引号，单引号会转义特殊字符。日期格式支持yyyy/MM/dd HH:mm:ss
- 对象：由键值对组成，形如 key:(空格)value 的数据组成。冒号后面的空格是必须要有的，每组键值对占用一行，且缩进的程度要一致，也可以使用行内写法：{k1: v1, ....kn: vn}
- 数组：由形如 -(空格)value 的数据组成。短横线后面的空格是必须要有的，每组数据占用一行，且缩进的程度要一致，也可以使用行内写法： [1,2,...n]
- 复合结构：上面三种数据结构任意组合

```yaml
yaml:
  str: 字符串可以不加引号
  specialStr: "双引号直接输出\n特殊字符"
  specialStr2: '单引号可以转义\n特殊字符'
  flag: false
  num: 666
  Dnum: 88.88
  list:
    - one
    - two
    - three
  #set: [1,2,2,3]
  set:
   - 1
   - 2
   - 1
  map: {k1: v1, k2: v2}
  users:
    - name: txjava
      salary: 15000.00
    - name: liangge
      salary: 18888.88
```

## 属性绑定

> @ConfigurationProperties，将配置文件中的属性注入到指定类中
>
> @Component，将指定类注入到IOC容器中

```java
@Component
@ConfigurationProperties("acme")
@Data
@NoArgsConstructor
@AllArgsConstructor
public class AcmeProperties {

    private boolean enabled;

    private InetAddress remoteAddress;

    private final Security security = new Security();

    public boolean isEnabled() {
        return enabled;
    }
}
```

> 为了让当前的实体类能在配置文件中有对应的提示，我们需要引入如下的依赖
>
> 加完依赖后通过Ctrl+F9来使之生效

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

### 构造器绑定

> 要使用构造函数绑定，必须使用@EnableConfigurationProperties或配置属性扫描启用类。
>
> 不能对由常规Spring机制创建的Bean使用构造函数绑定
>
> （例如@Component Bean、通过@Bean方法创建的Bean或使用@Import加载的Bean

```java
@ConfigurationProperties("acme")
@ConstructorBinding
public class AcmeProperties {

    private boolean enabled;

    private InetAddress remoteAddress;

    private final Security security ;

    public AcmeProperties(boolean enabled, InetAddress remoteAddress, Security security) {
        this.enabled = enabled;
        this.remoteAddress = remoteAddress;
    }
}

@RestController
@EnableConfigurationProperties(AcmeProperties.class)
public class YamlController {

    @Autowired
    private AcmeProperties acmeProperties;

    @RequestMapping("yaml")
    public AcmeProperties yaml(){
        System.out.println(acmeProperties);
        return acmeProperties;
    }
}
```



有待验证的想法







