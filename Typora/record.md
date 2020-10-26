# Java

## session和cookie详解

https://www.cnblogs.com/fnng/archive/2012/08/14/2637279.html

https://www.cnblogs.com/sharpxiajun/p/3395607.html

## Servlet

> 作用：是Java编写的服务器端程序，主要功能可以让用户对网站进行交互式的浏览和修改，生成动态的Web内容
>
> 定义：所有实现了servlet接口的类

### 相关接口

>   Servlet编程需要使用到javax.servlet和javax.servlet.http两个包下面的类和接口，在所有的类和接口中，javax.servlet.servlet接口最为重要。所有的servlet程序都必须实现该接口和继承实现了该接口的类。以下是Servlet的主要类和接口：

```java
javax.servlet.ServletConfig;

javax.servlet.ServletException;

javax.servlet.HttpServlet;

javax.servlet.HttpServletRequest;

javax.servlet.HttpServletResponse;

javax.servlet.HttpSession;

javax.servlet.Cookie;

javax.servlet.ServletContext;

javax.servlet.GenericServlet;


public void init（ServletConfig parma1） throws ServletException

//该函数用于初始化Servlet，该函数只会被调用一次，当用户第一次访问该Servlet的时候被调用

public  ServletConfig getServletConfig()   

// 用于得到servlet配置文件 与生命周期无关。

public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException

// 用于处理业务逻辑，程序员应该吧业务逻辑代码写作这里。用户在每次访问servlet的时候都会被调用，ServletRequest对象用于获得客户端信息，ServletResponse对象用于向客户端（浏览器）返回信息

public void destroy()  // 销毁Servlet实例并释放内存，关闭Tomcat都会调用这个函数。
```



### 转发和重定向

> 在serlvet中实现页面跳转的方式有两种：转发和重定向

#### 转发

> 由服务器端进行的页面跳转
>
> 原理如下图所示

![image-20201023113437922](F:\Other\Typora\Image\image-20201023113437922.png)

> 转发的方法如下：request.getRequestDispatcher("/two").forward(request, response);

![image-20201023114314642](F:\Other\Typora\Image\image-20201023114314642.png)

#### 重定向

> 由浏览器端进行的页面跳转
>
> 原理如下图所示

![image-20201023114524514](F:\Other\Typora\Image\image-20201023114524514.png)

> 重定向方法如下：response.sendRedirect("two");

![image-20201023114554681](F:\Other\Typora\Image\image-20201023114554681.png)

#### 疑问

1. 问：什么时候使用转发，什么时候使用重定向？

   如果要保留请求域中的数据，使用转发，否则使用重定向。

   以后访问数据库，增删改使用重定向，查询使用转发。

2. 问：转发或重定向后续的代码是否还会运行？

   无论转发或重定向后续的代码都会执行

#### 重定向和转发的区别

| **区别**         | **转发forward()**  | **重定向sendRedirect()** |
| ---------------- | ------------------ | ------------------------ |
| **根目录**       | 包含项目访问地址   | 没有项目访问地址         |
| **地址栏**       | 不会发生变化       | 会发生变化               |
| **哪里跳转**     | 服务器端进行的跳转 | 浏览器端进行的跳转       |
| **请求域中数据** | 不会丢失           | 会丢失                   |