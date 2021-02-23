# 用户模块

## 简介

用户模块是大部分软件系统的基础模块，良好的用户模块设计便于功能扩展、系统交互、数据分析。

## 需求分析

* 用户可以通过手机号、微信等方式注册并登录系统进行相关功能操作；

* 用户查看 / 操作各类功能模块，且需要权限验证；

* 用户属性包括用户标识、昵称、手机号、姓名等；

* 与第三方系统数据交互；

  

## 功能设计

一般我们理解的用户模块主要包括登录、注册、修改 / 查看个人信息、密码校验，还有操作各个模块的权限校验。但随着业务发展可能需要对接第三方系统、扩展用户个性化能力。我们可以分析在这些过程中哪些是不变的，哪些是不断变化的。通过将不变的功能抽象，可以适用于大部分场景。

* 变：与用户相关的个性化需求；比如用户组、等级、积分、上下级推荐关系；
* 不变：注册/登录/鉴权等逻辑；通过用户名/密码、手机号/验证码进行登录，修改信息，退出；

通过分析，我们可以将原有用户模块抽象成基础用户和个人信息两部分。

* 基础表：只关注注册/登录等通用逻辑，和具体哪类项目业务无关，不同类型的项目可复用；
* 信息表：不同模块/不同产品间的个性化需求；



**关于用户标识**

PC 互联网时代很多基于 email 作用户名，移动互联网时代基本都以手机号为标识，如果非业务必须，可以只考虑手机号。用户密码需求也可以直接使用短信验证码。



**关于第三方登录 ** 

国内互联网应用使用比较多的是微信登录，使用第三方登录时会产生账户绑定问题，账户绑定操作时间节点建议在第三方登录成功后立即完成账户绑定。账户绑定形式分为一对一、一对多，如果非业务必须，建议采用一对一。建议用户主标识为手机号。

## 数据结构

### users 用户基础表

| 字段名   | 字段类型 | 字段说明 | 默认值 |
| -------- | -------- | -------- | ------ |
| id       | int      | ID主键   |        |
| phone    | varchar  | 手机号   |        |
| password | varchar  | 密码     |        |
| salt     | varchar  | 密码盐   |        |
| username | varchar  | 用户名   |        |
|          |          |          |        |

### profiles 用户信息表

| 字段名     | 字段类型 | 字段说明       | 默认值 |
| ---------- | -------- | -------------- | ------ |
| id         | int      | ID主键         |        |
| user_id    | varchar  | 用户ID         |        |
| nickname   | varchar  | 昵称           |        |
| avatar     | varchar  | 头像           |        |
| wx_openid  | varchar  | 公众号openid   |        |
| xcx_openid | varchar  | 小程序openid   |        |
| unionid    | varchar  | 微信开放平台id |        |
| parent_id  | int      | 上级用户ID     |        |
|            |          |                |        |

如果对第三方账号绑定没有特殊要求，1对1能满足大部分需求，可以考虑将 profiles 表合并至 users 表。



## 相关功能

### 用户关系链

* 如何存储用户上下级关系；
* 如何快速查询用户上下级关系；
* 如何查询用户任意层级关系；
* TODO ...

### 用户分组

* TODO ...

### 建表SQL

```sql
CREATE TABLE users(
    id INT NOT NULL AUTO_INCREMENT  COMMENT '主键' ,
    phone VARCHAR(30) NOT NULL   COMMENT '手机号' ,
    password VARCHAR(30)    COMMENT '密码' ,
    salt VARCHAR(30)    COMMENT '密码盐' ,
    username VARCHAR(30)    COMMENT '用户名' ,
    created_at DATETIME    COMMENT '创建时间' ,
    updated_at DATETIME    COMMENT '更新时间' ,
    status TINYINT(2)    COMMENT '状态' ,
    lasted_ip VARCHAR(30)    COMMENT '最后登录IP' ,
    PRIMARY KEY (id)
) COMMENT = '用户基础表';

CREATE TABLE profiles(
    id INT NOT NULL AUTO_INCREMENT  COMMENT '主键' ,
    user_id INT    COMMENT '用户ID' ,
    nickname VARCHAR(30)    COMMENT '昵称' ,
    avatar VARCHAR(150)    COMMENT '头像' ,
    wx_openid VARCHAR(30)    COMMENT '公众号openid' ,
    xcx_openid VARCHAR(30)    COMMENT '小程序openid' ,
    unionid VARCHAR(30)    COMMENT '微信开放平台id' ,
    parent_id INT    COMMENT '上级用户ID' ,
    created_at DATETIME    COMMENT '创建时间' ,
    updated_at DATETIME    COMMENT '更新时间' ,
    PRIMARY KEY (id)
) COMMENT = '用户信息表';
```



