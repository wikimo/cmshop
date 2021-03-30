适合二次开发/学习/商用的开源电商系统V1.1

这几年电商行业飞速发展，越来越多的人参与到带货队伍中来。随着业务增长，如何科学高效卖货是大家需要解决的问题。在订单量规模较小的情况下，通过微信个人号/微信群就可以实现卖货，但在订单量起来后，微信个人号卖货会变得非常低效，需要通过技术手段来提高生产力。目前市面上有很多成熟的SASS系统，可通过免费/收费的方式开箱即用，但有一些个性化需求第三方SaaS系统无法满足，这时会考虑自建系统或使用开源系统。

自建系统可以按实际业务需求定制，但花费时间、费用成本较高。我们可以考虑基于开源电商系统二次开发来满足需求，达到快速上线，节约成本的目的。

首先看下一般电商系统基础功能模块有哪些：

1. 用户（基础信息、地址管理、授权登录/退出……）
2. 商品（基础信息、SKU、SPU、商品图、详情图文……）
3. 订单（详情、物流……）
4. 支付（微信、支付宝、余额……）
5. 售后（退款、退换货……）
6. 管理系统后台、用户客户端（微信小程序、H5、APP）
7. 各种营销类功能（可选，优惠券、团购、砍价、秒杀……）
8. 与电商系统相关的进销存、ERP等

有了上面这些，一个商品可以通过商家上架、用户下单支付、商家发货流转到用户手上。市面上有很多开源电商系统，我们实际用的时候发现可能是半开源的，比如服务端核心模块/授权模块加密，不提供客户端代码等。那么市面上比较好用的开源电商系统有哪些，这里整理了一部分供大家参考学习。

### FaShop
* 官网：[https://www.fashop.cn/]
* 源码：[https://github.com/mojisrc]
1. 后端基于PHP、Swoole；
2. 客户端包括微信小程序、APP；
3. 提供了Docker部署；
4. 有一部分技术细节文档；
5. 代码挺干净，已经停止维护了，但可以拿来学习或做基础构建；
6. 有GO、Objective-C、Swift、Flutter意思，但没有填坑；

### CRMEB
* 官网：[https://www.crmeb.com/]
* 源码：[https://gitee.com/ZhongBangKeJi/CRMEB]

1. 采用PHP，基于ThinkPHP框架，安装配置比较方便；

2. 客户端微信小程序；

3. 已产品化，可以购买商业版部署上线；

4. 营销插件功能比较全（拼团、砍价、秒杀、优惠券……）；

5. 有Java版本、多商户版、知识付费系统，从产品到销售、售后相对比较成熟；

6. 我们实践过该项目，可以考虑商用；

   

###  果酱小店
* 官网：[https://guojiang.club/]
* 源码：[https://github.com/guojiangclub]

1. 采用PHP，基于Laravel框架，代码结构设计良好；
2. 有部分技术细节说明，如API（[https://guojiang.club/docs/api/v1/]），和FaShop都属于偏技术流；
3. 可以考虑用来做项目基础框架及二次开发；

### Fcmall
* 官网： [http://www.fecmall.com/]
* 源码： [https://github.com/fecshop/yii2\_fecshop ]
1. 基于PHP，Yii2框架，代码结构设计良好；
2. 有相关文档资料，插件市场，适合二次开发；
3. 源码长期维护，比较靠谱；



### NiuShop

* 官网：[https://www.niushop.com/]
* 源码：[https://gitee.com/niushop_team]
* 解决方案完整，还有 SaaS 开源版本，值得研究学习；
* 和 CRMEB 旗鼓相当，可以考虑商用；


## 其它开源电商项目
### 萤火
* 官网：[https://www.yiovo.com/] 
* 源码：[https://gitee.com/xany]
*  后端基于ThinkPHP，代码长期维护；

###  来客推
* 官网：[http://www.laiketui.com/]
* 源码：[https://gitee.com/laiketui/open]
* 有APP版本，需另外购买；
* 用一个叫Mojavi的开发框架（感觉比较冷门），但怎么把框架的版权改成自己公司了；
* 代码长期维护；

### ShopXO
* 官网：[https://shopxo.net/]
* 源码：[https://gitee.com/gongfuxiang/shopxo]
* 个人开源项目，代码长期维护；

------



##  Java 系开源项目

### Mall

* 官网：[http://www.macrozheng.com/]
* 源码：[https://github.com/macrozheng/mall]
* Java系各方面比较全面的电商开源项目，项目文档比较全面，包括系统技术原型、业务架构、各模块介绍、项目搭建等一系列内容；
* 客户端基于H5，没有小程序端；
* 管理端基于Vue，前后分离；



### litemall

* 官网：[https://linlinjava.gitbook.io/litemall/]
* 源码：[https://github.com/linlinjava/litemall]
* Java系电商项目，相对mall文档资料没有那么齐全，但也值得学习；
* 客户端有小程序端；



### newbee-mall

* 源码：[https://github.com/newbee-ltd/newbee-mall]
* Java系



### mall4j

* 官网：[https://www.mall4j.com/]
* 源码：[https://github.com/gz-yami/mall4j]
* Java系，偏商业项目，解决方案，营销类插件比较丰富，支持多商户模式；

### onemall

* 源码：[https://github.com/YunaiV/onemall]
* Java系

###  mallcloud-platform

* 源码：[https://gitee.com/catshen/zscat_sw]
* Java系

------



## 其它PHP系电商系统

### ShopWind
* 官网：[http://www.shopwind.net/]

### ECTouch
* 官网：[https://www.ectouch.cn/]

### TPShop
* 官网：[http://www.tp-shop.cn/]

### ShopEx / ECShop
* 官网：[https://www.shopex.cn/]

### 禾匠商城
* 官网：[https://www.zjhejiang.com/]
* 可能是目前市面上做的算比较好的电商系统，基础功能比较完整、营销插件丰富；
* 有微擎版、独立版，从产品、技术、售后已经很完整，团队很靠谱，持续更新中；
* 适合于技术团队比较弱，但又想做独立电商的创业者；


