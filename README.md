**项目说明** 
- cloud-core是公安警务云资源运营服务平台，使用前后端分离模式进行开发
- 项目内使用Oracle数据库，配置在jdbc.properties
- 项目使用springmvc+mybatis开发

<br>
 
**项目结构** 
```
cloud-core
│
├─java 代码模块
│  ├─bean 实体类
│  ├─common 通用类\工具类
│  ├─controller 控制器
│  │  ├─backend 后台系统接口
│  │  ├─open 开放接口
│  │  ├─portal 网站平台接口
│  │  └─trash 支持接口
│  ├─dao 实体映射
│  ├─request 通用请求返回
│  └─service 接口实现 
│ 
│  
├──resources 配置
│  ├─mapper SQL对应的XML文件
│  ├─beta\dev\perPro\pro\test 多环境配置文件
│  │  ├─dcuc-client-config 单点登录配置
│  │  ├─env-coommon 通用组件配置
│  │  ├─es elasticsearch配置
│  │  ├─jdbc 数据库配置
│  │  ├─logback 日志
│  │  ├─redis redis配置
│  │  └─web.xml web配置文件
│  │
│  └─spring spring配置文件

```


<br> 


**技术选型：** 
- 核心框架：Spring
- 安全框架：Apache Shiro
- 视图框架：Spring MVC
- 持久层框架：MyBatis
- 定时器：Quartz
- 数据库连接池：Druid
- 日志管理：SLF4J
- 页面交互：Vue2.x 
- 文档生成：swagger
- 缓存服务：redis
- 消息队列：kakfa
- 搜索服务：elasticsearch
- 模板引擎：thymeleaf
<br> 


 **后端部署**
- 通过git下载源码
  (http://{username}@ip:port/r/cloud-core.git)
- idea需安装lombok插件
- 修改数据库配置文件，需连线上开发数据库，开发时修改dev目录下db.properties
- 修改redis配置文件，可本地搭建redis服务
- 本地下载安装tomcat，idea修改配置，项目部署到tomcat下
- 运行tomcat，则可启动项目
- Swagger文档路径：http://localhost:8080/swagger-ui.html

<br> 

 **前端部署**
 - 本项目是前后端分离的，还需要部署前端，才能运行起来
 - 前端通过git下载源码
   (http://{username}@ip:port/r/cloud-website.git)
 - 前端使用vscode进行开发
 - 开启终端，下载相关依赖，并运行。相关命令:
 ```
npm install
npm run serve
```
 - 前端部署完毕，就可以访问项目了
 
 <br>


**接口文档效果图：**
![输入图片说明](http://139.9.154.156/zentao/file-read-3.png?onlybody=yes "在这里输入图片标题")

<br> <br>


**网站效果图：**
![门户网站](http://139.9.154.156/zentao/file-read-4.png?onlybody=yes "门户网站")
<br><br>
<!--![后台管理系统](http://139.9.154.156/zentao/file-read-5.png?onlybody=yes "后台管理系统")-->

<br>

