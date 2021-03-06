
# 商品模块

## 简介
商品模块是电商系统核心模块，本文主要讲解商品模块功能需求、术语概念、功能设计、数据结构、相关功能设计思路。



## 需求分析

1. 电商系统后台可以管理商品（功能需求）；
2. 管理商品包括添加、修改、上下架、删除、审核等动作（操作行为）；
3. 添加修改商品时，每种商品有不同的字段、参数、图片、规格、价格（字段属性）；



## 术语概念

1. SPU：Standard Product Unit（标准化产品单元），一种商品，各种规格集合，如：iPhone 12；

2. SKU：Stock Keeping Unit（库存量单位），也称单品，一种商品的具体规格，如：一部 黑色 128G 的iPhone 12 手机，黑色 + 128G 就是商品的具体规格。

   

   可以看到一种商品SPU包含多种SKU，SPU（SKU1、SKU2……SKU n），且SKU唯一。



## 功能设计

商品模块一般包括商品、商品分类、商品规格管理，可能还有商品审核。商品模块比较复杂的地方在于商品参数、规格管理。



#### 关于商品参数

* 商品参数仅用于商品信息展示，一般不参与实际业务逻辑；

* 手机参数关注点是内存、存储、摄像头、CPU等；

* 手表参数关注点是表盘材质、表镜材质、适用人群、是否防水；

* 化妆品参数关注点是功能、肤质、产地；

  

如果是全品类平台，不同行业商品存在不同参数，我们可以按行业构建参数模板。如果是垂直行业，如农产品、五金等，可以将商品参数直接存储商品表，存储商品表的问题在于如果参数比较多，那么商品表参数字段会很多，如果跨行业，冗余字段会比较多，但单表维护比较方便。前期建议不过渡设计，直接单表维护，后期需要可以再拆表，并将原商品表参数字段迁移到子表。



#### 关于商品规格
商品规格管理是商品模块比较重要的部分，一般有单规格、多规格。如果将系统商品规格设计成单规格，后期如果有多规格需求，系统扩展会比较麻烦，所以建议将系统设计成多规格方式或支持单规格、多规格两种方式。如果为了快速验证业务，项目很大程度上需要重写，行业类目需要，可以考虑用单规格方式以降低开发成本。



**什么是单规格**
以农产品大米为例，如果将大米按重量分成 2.5kg / 5kg，规格相对简单，只有一个层级；

**什么是多规格**
以服装为例，如T恤，有颜色（黑、白、黄、绿……），有尺寸（S、M、L……）、材质（棉、涤纶），规格有多个层级，比较复杂；



## 数据结构
多规格字段数量具有不确定性，可采用JSON实现，如服装类商品；
* 规格-颜色（黑、白、红、黄、蓝）；
* 规格-大小（S、M、L）；
* 规格-材质（纯棉、涤纶）；

通过规格配置表、规格值表，两表可以完成规格数据存储，我们以最简单的基础字段作分析。



### proudcts 商品表

| 字段名      | 字段类型 | 字段说明                 | 默认值 |
| ----------- | -------- | ------------------------ | ------ |
| id          | int      | 商品ID主键               |        |
| name        | varchar  | 商品名称                 |        |
| sale_amount | int      | 零售价                   | 0      |
| use_attrs   | tinyint  | 是否使用规格 0：否 1：是 | 1      |
|             |          |                          |        |

### product_attrs 商品规格配置表

| 字段名     | 字段类型 | 字段说明                       | 默认值 |
| ---------- | -------- | ------------------------------ | ------ |
| name       | varchar  | 规格名，如：颜色、尺寸         |        |
| values     | json     | 规格配置项，["黑", "白", "橙"] |        |
| product_id | int      | 商品ID外键                     | 0      |
|            |          |                                |        |

### product_attr_vals 商品规格值表

| 字段名      | 字段类型 | 字段说明                      | 默认值 |
| ----------- | -------- | ----------------------------- | ------ |
| product_id  | int      | 商品ID外键                    | 0      |
| attr_vals   | json     | 规格配置项组合值，["黑", "S"] |        |
| sale_amount | int      | 零售价                        | 0      |
| stock       | int      | 库存                          | 0      |
| sku         | varchar  | SKU编码                       |        |
|             |          |                               |        |

我们以商品iPhone 12为例，分析具体数据存储

* products 商品表

  | id   | name      | sale_amount |
  | ---- | --------- | ----------- |
  | 1    | iPhone 12 | 5499        |

* product_attrs 商品规格配置表

  | id   | name | values         | product_id |
  | ---- | ---- | -------------- | ---------- |
  | 1    | 颜色 | ["黑","白"]    | 1          |
  | 2    | 存储 | ["64G","128G"] | 1          |
  |      |      |                |            |

* product_attr_vals 商品规格值表

  | id   | product_id | attr_vals     | sale_amount | stock | sku  |
  | ---- | ---------- | ------------- | ----------- | ----- | ---- |
  | 1    | 1          | ["黑","64G"]  | 5499        | 999   | 001  |
  | 2    | 1          | ["黑","128G"] | 5999        | 999   | 002  |
  | 3    | 1          | ["白","64G"]  | 5499        | 999   | 003  |
  | 4    | 1          | ["白","128G"] | 5999        | 999   | 004  |
  |      |            |               |             |       |      |

1. 如果觉得 2 表维护繁琐，可以将 product_attrs 表冗余至 products 表字段，但products表存储会增大；

2. 对于属性值存储类型，有些喜欢用text / varchar存储，Mysql 5.7及以上支持JSON类型，可以考虑直接采用JSON存储；

3. 价格字段采用了int类型，也可以采用decimal类型，int类型可以降低精度计算问题；（微信支付采用整型，支付宝采用浮点型）

4. 如果不想用JSON存储，可以将规格属性映射成数字，如给具体的颜色、尺寸、款式属性标记成1、2、3（唯一）……通过数字组合产生对应SKU，那么上面 product_attr_vals 商品规格值表变成如下，但这种方式在DB层的数据可读性比较差。

   | id   | product_id | attr_vals | sale_amount | stock | sku  |
   | ---- | ---------- | --------- | ----------- | ----- | ---- |
   | 1    | 1          | 1,3       | 5499        | 999   | 001  |
   | 2    | 1          | 1,4       | 5999        | 999   | 002  |
   | 3    | 1          | 2,3       | 5499        | 999   | 003  |
   | 4    | 1          | 2,4       | 5999        | 999   | 004  |
   |      |            |           |             |       |      |

### 其它商品字段参考

* merchant_id 供应商ID

* shop_id 店铺ID

* share_title 分享标题，用于微信分享

* share_descrip 分享描述

* category_id 商品分类ID

* unit 单位

* vip_amount 会员价

* market_amount 市场价

* max_amount / min_amount 最大/最小价格

* content 图文详情

* logo 商品图

* orderby 排序

* status 状态（上架、下架）

* stock  总库存（展示用）

* sale 销售量（还可以细分真实、虚拟）

* is_delete 是否删除（逻辑删除）

* check_stats 审核状态

* admin_id 操作员

  



## 相关功能

### 商品费用

商品除了售价，还包括一部分费用，是集成在商城系统还是交给ERP系统处理，看具体系统规模和使用场景，商品的价格在零售价背后其实还包括采购价、包装费、运费、仓管费、订单处理费、税费、推广费，这些费用都是商品的成本，如果忽略这些成本，那么商品售卖的时候可能会产生负利润，也就是亏本。所以考虑商品的隐性费用成本很重要。



### 组合商品

* TODO...

### 建表SQL

```sql
CREATE TABLE products(
    id INT NOT NULL AUTO_INCREMENT  COMMENT '主键' ,
    name VARCHAR(30)    COMMENT '商品名称' ,
    title VARCHAR(50)    COMMENT '商品标题' ,
    share_title VARCHAR(50)    COMMENT '分享标题' ,
    share_descip VARCHAR(50)    COMMENT '分享描述' ,
    shop_id INT NOT NULL  DEFAULT 0 COMMENT '店铺ID' ,
    mch_id INT NOT NULL  DEFAULT 0 COMMENT '供应商ID' ,
    category_id INT    COMMENT '商品分类' ,
    unit VARCHAR(15)    COMMENT '单位' ,
    sale_amount INT NOT NULL  DEFAULT 0 COMMENT '零售价' ,
    vip_amount INT NOT NULL  DEFAULT 0 COMMENT '会员价' ,
    marker_amount INT NOT NULL  DEFAULT 0 COMMENT '市场价' ,
    max_amount INT NOT NULL  DEFAULT 0 COMMENT '最高价' ,
    min_amount INT NOT NULL  DEFAULT 0 COMMENT '最低价' ,
    content TEXT    COMMENT '商品详情' ,
    logo VARCHAR(150)    COMMENT '商品LOGO' ,
    orderby MEDIUMINT(5) NOT NULL  DEFAULT 99 COMMENT '排序' ,
    status TINYINT(2) NOT NULL  DEFAULT 0 COMMENT '状态' ,
    stock INT NOT NULL  DEFAULT 0 COMMENT '库存' ,
    sale INT NOT NULL  DEFAULT 0 COMMENT '销量' ,
    is_delete TINYINT(2) NOT NULL  DEFAULT 0 COMMENT '是否删除' ,
    check_status TINYINT(2) NOT NULL  DEFAULT 0 COMMENT '审核状态' ,
    shipping_template_id INT NOT NULL  DEFAULT 0 COMMENT '运费模板ID' ,
    admin_id INT NOT NULL  DEFAULT 0 COMMENT '操作员' ,
    created_at DATETIME    COMMENT '创建时间' ,
    updated_at DATETIME    COMMENT '更新时间' ,
    PRIMARY KEY (id)
) COMMENT = '商品表';

CREATE TABLE product_attrs(
    id INT NOT NULL AUTO_INCREMENT  COMMENT '主键' ,
    name VARCHAR(30)    COMMENT '规格名' ,
    product_id INT NOT NULL  DEFAULT 0 COMMENT '商品ID' ,
    values JSON    COMMENT '规格配置项' ,
    created_at VARCHAR(30)    COMMENT '创建时间' ,
    updated_at VARCHAR(30)    COMMENT '更新时间' ,
    PRIMARY KEY (id)
) COMMENT = '商品规格配置表';

CREATE TABLE product_attr_vals(
    id INT NOT NULL AUTO_INCREMENT  COMMENT '主键' ,
    product_id INT NOT NULL  DEFAULT 0 COMMENT '商品ID' ,
    attr_vals JSON    COMMENT '规格配置项组合值' ,
    sale_amount INT NOT NULL  DEFAULT 0 COMMENT '零售价' ,
    vip_amount INT NOT NULL  DEFAULT 0 COMMENT '会员价' ,
    market_amount INT NOT NULL  DEFAULT 0 COMMENT '市场价' ,
    stock INT NOT NULL  DEFAULT 0 COMMENT '库存' ,
    sale INT NOT NULL  DEFAULT 0 COMMENT '销量' ,
    thumb VARCHAR(150)    COMMENT '预览图' ,
    orderby MEDIUMINT(5) NOT NULL  DEFAULT 99 COMMENT '排序' ,
    created_at DATETIME    COMMENT '创建时间' ,
    updated_at DATETIME    COMMENT '更新时间' ,
    PRIMARY KEY (id)
) COMMENT = '商品规格值表';
```



