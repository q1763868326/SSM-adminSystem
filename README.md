# 项目技术难点分析及解决过程
## 一、SSM环境的搭建
 ![image.png](https://i.loli.net/2020/07/21/7GFiucM1BsIq6YP.png)
##### 首先导入SSM框架所需要的一系列jar包同时配置好Tomacat服务器，然后使用Mybatis逆向工程生成mapper和pojo来对数据库进行增删改查的操作，同时新建controller和service，分别用来接收前端信息控制数据流向和与数据库交互。同时前端使用jsp进行界面设计和用户交互。这样基本的SSM框架就搭建起来了。
## 二、	中文乱码问题的解决
#### 由于SSM框架默认使用的是编码格式对中文并不支持因此需要对编码类型进行指定使用UTF-8，主要从几方面下手
##### 1. 前端jsp界面保证字符编码为utf-8：<%@ page language="java" contentType="text/html; charset=UTF-8" >
##### 2. 在web.xml配置文件中配置一个过滤器，将请求参数在SpirngMVC中的编码方式指定为utf-8
 ![image.png](https://i.loli.net/2020/07/21/NYmpS8bOTglJQax.png)
##### 3.上述过滤器仅解决了post请求的乱码问题，为了解决get请求的乱码问题，我们需要在tomcat的配置文件server.xml中找到对应端口的定义处添加编码格式utf-8
![image.png](https://i.loli.net/2020/07/21/HOafoz1jJ6FXIYp.png)
 
## 三、	前后端数据的交互
### 交互根据需求分为3种：
#### 1.添加
 ![image.png](https://i.loli.net/2020/07/21/peB67EkgAOFfX9P.png)
#### 2.搜索
 ![image.png](https://i.loli.net/2020/07/21/3FvtKo1zjkHrdTm.png)
##### 使用Service对数据库信息进行查询，并将查询结果封装成json格式后发回给前端。
#### 3.删除
![image.png](https://i.loli.net/2020/07/21/bslapvVN3kxRogB.png)

##### 删除操作与添加类似
## 四、	模糊查询
 ![image.png](https://i.loli.net/2020/07/21/fF8HRSz3B4reX9Q.png)
##### 模糊查询主要用到了Example类下的createCriteria()生成一个Criteria类的一个对象，然后通过判断是否传进该字段，如果有则根据相应字段添加模糊查询的匹配条件，同时中间可以通过EqualTo进行等值条件的添加。最终将查询到的列表信息返回给控制器，由控制器处理为json格式后发回给前端。
## 五、	删除选中项
##### 删除选中项的难点在于前端数据的选择和传输，后端只需要根据传进来的数据的主键进行delete删除操作就好.
 ![image.png](https://i.loli.net/2020/07/21/auXFSmJCUbh3jPW.png)
##### 前端在Datagrid 开头一列加上选择框，并且设置为单选，选定要删除的项目后，通过删除方法进行数据的异步ajax传输。
 ![image.png](https://i.loli.net/2020/07/21/ZA598X6Jsc24UaS.png)
#### 大致步骤为
##### 1.获取选中的行列表，虽然只有一个但也是个列表，然后通过下标选择就好。
##### 2.使用jquery进行异步请求，将数据发送给后端，后端返回200 success后进行列表刷新。
