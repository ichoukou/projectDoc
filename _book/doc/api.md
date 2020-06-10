# 前后端接口文档

本项目前后端接口规范和接口文档。

注意：
> 1. 接口文档处于开发中，如果发现接口描述和接口实际不对应，欢迎提建议和报告。

## 1 前后端接口规范

### 1.1 请求格式

项目内采用RESTful风格的接口，但没有定义具体语义。
目前只使用`GET`和`POST`来表示请求内容和更新内容两种语义。

#### 1.1.1 GET请求

    GET API_URL?params

例如
    
    GET /api/ncovDataArea/homePage

或者
    
    GET /api/webManage/news?pageSize=10&pageNum=2

#### 1.1.2 POST更新
    
    POST API_URL
    {
        body
    }

例如
    
    POST /apply/delete/{id}

或者    
    
    POST /workflow/add
    {
      "id":"",
      "workflowname":"wxx",
      "defaultProcess":"IAAS",
      "routername":"wxxdesc",
      "area":"省厅",
      "policeCategory":"",
      "version":null,
      "recheck":
      {
        "id":"","defaulthandleperson":"410184198209096919","adviserperson":"511321198706011277","noticeperon":"410184198209096919,511321198706011277"
      },
      "approve":
      {
        "id":"","defaulthandleperson":"410184198209096919","adviserperson":"","noticeperon":""
      },
      "carry":
      {
        "id":"","defaulthandleperson":"410184198209096919","adviserperson":"","noticeperon":""
      }
    }

#### 1.1.3 分页请求参数

当GET请求后端获取数组数据时，需要传递分页参数。

例如

    GET /api/webManage/news?pageSize=10&pageNum=2
    
本项目的通用分页请求参数统一传递2个：

    pageNum: 请求页码
    pageSize: 每一页数据量


此外，这里两个个参数是可选的，后端应该设置默认参数，因此即使前端不设置，
后端也会自动返回合适的对象数组响应数据。

讨论：
> 有些请求后端是所有数据，这里pageNum和pageSize可能设置是无意义的。但是
> 仍然建议加上两个参数，例如pageNum=1, pageSize=1000。


### 1.2 响应格式

    Content-Type: application/json;charset=UTF-8
    
    {
        body
    }
    

而body是存在一定格式的json内容：
    
    {
        code: xxx,
        msg: xxx,
        data: {}
    }

#### 1.2.1 失败异常

    {
        code: 500,
        msg: "未知异常，请联系管理员"
    }

* 请求正常和错误时需根据code判断。
    
#### 1.2.2 操作成功

    {
        code: 200,
        errmsg: "操作成功",
    }

#### 1.2.3 数组对象

    {
        code: 200,
        errmsg: "操作成功",
        data: {
            list: [],
            current: 2，
            pages: 5,
            size: 10,
            total: 45
        }
    }
    

list是对象数组，current时当前页码，pages是总页数，size是每页的数据量，total是总的数量。

### 1.3 错误码

#### 1.3.1 系统通用错误码

系统通用错误码包括4XX和5XX，500是业务错误，其余请查看项目httpstatus工具类的状态码定义。

<!--#### 1.3.2 前台业务错误码

* AUTH_INVALID_ACCOUNT = 500
* AUTH_CAPTCHA_UNSUPPORT = 501

    
#### 1.3.3 管理后台业务错误码

* ADMIN_INVALID_NAME = 601
* ADMIN_INVALID_PASSWORD = 602
-->
### 1.4 Token

前后端采用session来验证访问权限。

#### 1.4.1 Header&Token

前后端Token交换流程待完善文档

<!--前后端Token交换流程如下：

1. 前端访问商场登录API或者管理后台登录API;

2. 成功以后，前端会接收后端响应的一个token，保存在本地；
    
3. 请求受保护API则，则采用自定义头部携带此token

4. 后端检验Token，成功则返回受保护的数据。

#### 1.4.2 商场自定义Header

访问受保护商场API采用自定义`X-Litemall-Token`头部

1. 小商城（或轻商场）前端访问小商城后端登录API`/wx/auth/login`

        POST /wx/auth/login
    
        {
            "username": "user123",
            "password": "user123"
        }

2. 成功以后，前端会接收后端响应的一个token，

        {
          "errno": 0,
          "data": {
            "userInfo": {
              "nickName": "user123",
              "avatarUrl": "https://wpimg.wallstcn.com/f778738c-e4f8-4870-b634-56703b4acafe.gif"
            },
            "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ0aGlzIGlzIGxpdGVtYWxsIHRva2VuIiwiYXVkIjoiTUlOSUFQUCIsImlzcyI6IkxJVEVNQUxMIiwiZXhwIjoxNTU3MzI2ODUwLCJ1c2VySWQiOjEsImlhdCI6MTU1NzMxOTY1MH0.XP0TuhupV_ttQsCr1KTaPZVlTbVzVOcnq_K0kXdbri0"
          },
          "errmsg": "成功"
        }    
    
3. 请求受保护API则，则采用自定义头部携带此token

        GET http://localhost:8080/wx/address/list
        X-Litemall-Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ0aGlzIGlzIGxpdGVtYWxsIHRva2VuIiwiYXVkIjoiTUlOSUFQUCIsImlzcyI6IkxJVEVNQUxMIiwiZXhwIjoxNTU3MzM2ODU0LCJ1c2VySWQiOjIsImlhdCI6MTU1NzMyOTY1NH0.JY1-cqOnmi-CVjFohZMqK2iAdAH4O6CKj0Cqd5tMF3M

#### 1.4.3 管理后台自定义Header

访问受保护管理后台API则是自定义`X-Litemall-Admin-Token`头部。

1. 管理后台前端访问管理后台后端登录API`/admin/auth/login`

        POST /admin/auth/login
    
        {
            "username": "admin123",
            "password": "admin123"
        }

2. 成功以后，管理后台前端会接收后端响应的一个token，

        {
            "errno": 0,
            "data": {
                "adminInfo": {
                    "nickName": "admin123",
                    "avatar": "https://wpimg.wallstcn.com/f778738c-e4f8-4870-b634-56703b4acafe.gif"
                },
                "token": "f2dbcae8-6e25-4f8e-bc58-aa81d512c952"
            },
            "errmsg": "成功"
        }
    
3. 请求受保护API时，则采用自定义头部携带此token

        GET http://localhost:8080/wx/address/list
        X-Litemall-Admin-Token: f2dbcae8-6e25-4f8e-bc58-aa81d512c952
-->
### 1.5 API自动生成文档

项目内使用swagger-ui生成接口文档，可查看每个接口API的说明。

GET /swagger-ui.html

由于仍处于迭代中，因此目前未有更详细的API说明，前端可能不理解接口的输入输出。

![](./pics/readme/swagger-ui.png)

<!-- ### 1.6 API格式

这里定义一个API的格式：

应用场景

    xxx
    
接口链接

    xxx
    
请求参数

    xxx
    
响应内容

    xxx
    
错误码

    xxx
-->
### 1.6 API预览

接下来会分别从用户层面和管理员层面构建门户网站API服务和管理系统后台API服务。

门户网站API服务涉及

* 首页服务
* 搜索服务
* 文档服务
* 资源申请服务
* 购物车服务
* 新闻服务
* 应用市场
* 软件服务（SAAS）
* 数据服务（DAAS）
* 平台服务（PAAS）
* 基础设施服务（IAAS）
* 反馈服务
* 足迹服务


管理系统后台API涉及
* 工作台
* 申请审批
* 租户上报管理
* 租户资源管理
* 台账管理
* 资源回收管理
* 服务管理
* 平台管理
* 文档管理
* 系统管理



<!--### 1.7 API保护

暂未知本项目API保护机制，是否能防御一般的XSS、CSRF、JS脚本注入等攻击。-->

### 1.7 API使用

当前API使用还存在一些问题，后面需要继续优化和完善。

* 前端页面有通用模块，大量引用API，使无关页面也引用过多无用API，造成资源浪费，需完善。

* API模块未整理，需完善。


<!--## 2 商城API服务

### 2.1 安全服务

#### 2.1.1 小程序微信登录

应用场景

    小程序环境下微信登录。
      
接口链接

    xxx
    
请求参数

    xxx
    
响应内容

    xxx
    
错误码

    xxx
    
#### 2.1.2 账号登录

应用场景

    基于用户名和密码的账号登录
    
接口链接

    POST /wx/auth/login
    
请求参数

    {
        "username": "user123",
        "password": "user123"
    }    
    
响应内容

    {
      "errno": 0,
      "data": {
        "userInfo": {
          "nickName": "user123",
          "avatarUrl": "https://wpimg.wallstcn.com/f778738c-e4f8-4870-b634-56703b4acafe.gif"
        },
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ0aGlzIGlzIGxpdGVtYWxsIHRva2VuIiwiYXVkIjoiTUlOSUFQUCIsImlzcyI6IkxJVEVNQUxMIiwiZXhwIjoxNTU3MzI2ODUwLCJ1c2VySWQiOjEsImlhdCI6MTU1NzMxOTY1MH0.XP0TuhupV_ttQsCr1KTaPZVlTbVzVOcnq_K0kXdbri0"
      },
      "errmsg": "成功"
    }   
    
错误码

    略
    
#### 2.1.3 注册

应用场景

    xxx
    
接口链接

    xxx
    
请求参数

    xxx
    
响应内容

    xxx
    
错误码

    xxx
    
#### 2.1.4 退出

应用场景

    账号退出
    
接口链接

    POST /wx/auth/logout
    
请求参数

    {
        "username": "user123",
        "password": "user123"
    }    
    
响应内容

    {
        "errno": 0,
        "errmsg": "成功"
    }
    
错误码

    略
    -->

