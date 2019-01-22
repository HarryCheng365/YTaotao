# YTaotao




## 一个完整的APP架构设计

需要经过

> 需求分析 -> 功能分析 -> 用户流程分析 -> 非功能需求分析 -> 架构层次分析 
>
> -> UI设计 -> 数据库设计 -> 模块设计



## 用户需求

租赁服装考虑到的因素：

1.租赁价格

2.服装款式

3.卫生状况

4.面料 5.店铺评价

租赁服装时候遇到的困难：

1.款式少，找不到合适的衣服

2.店面太远，不易搬运服装

3.租价太高

4.服装太多，难以挑选；客流量大，环境杂乱无序；

5.尺码不合

6.店主不耐心，服务态度差

关键词：

| 关键词 | 具体困难                                                     | 指导意义                                                     | 技术需求                                                     |
| :----- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 找不到 | 据走访调查，一家20平的店铺，库存都有几千件衣服，所以款式并不是真的少，而是摆放乱，空间小，在限定时间内难以找到 | 让用户在有限时间内尽快的找到心仪的商品                       | 1.将租赁商品带必要参数摆上线    2.最基本的关键词法搜索，再利用机器学习相关算法辅助搜索，在一开始摆放商品的时候就想好怎么存放信息可以服务于搜索3.智能推送和产生消息，促进消费 |
| 店面远 | 首先看衣服，因为时间问题，可能就要到场好几次，根据流程，看好衣服还要预定，需要第二天来取，这又要跑一次；租衣服由于时效一般是一天，所以立马又要跑腿去还，把押金要回来 | 店面远这个问题不在APP解决范围内，所以减少或避免或让其他人代劳这个跑腿的过程，是需要注意的 | 1.让顾客在线上选好心仪商品，发出预定看衣服需求，或者直接不试发出订单，只需要跑取和拿两步  2.或者和骑手签订配送协议，但是这样就涉及我们需要引入高德等的地图接口 |
| 租价高 | 垄断，信息不透明，不知道租价为什么这么高                     |                                                              |                                                              |
| 服务差 | 衣服干不干净，穿着体验如何，某些店铺老板服务态度差，店铺环境脏乱 | 需要让用户看到用户反馈，从而对这些老顽固们来一次自底向上的革命，让商家看到用户反馈 | 1.对商家引入用户评价系统，对商品引入用户评价系统，行成类似小社区 |

找不到（问题1和问题4）->一家20平的店铺，库存都有几千件衣服，所以款式并不是真的少

店面远（问题2）

租价高，老板服务态度差（问题3和问题6）

尺码找不到（问题5）->一是借助用户信息反向指导采购，二是采用可视化的预定方式

当然 我们都知道，当体量上升到一定层次后，就会产生新的问题，

所以我们目前假定1000人同时在线浏览产生数据交互的规模来做。

其他APP 是怎么做到 尤其是在服装方面，让用户找到自己最心仪的商品的



## 核心思想

> 1.15

- 小开发团队不重复造轮子，只做轮子的整合工
- MVP程序设计模式 参考MVC思想 模型层 结构层 核心层 界面层 
- 能画图就少用话BB processOn协作分工
- WEB端和Android端应该仅仅是展示界面的问题，应该有统一的后台http请求接口，接同一个数据库，保证实时同步，同时一些功能比如商家的 店铺管理和商品管理 可以只做WEB端



## 业务功能需求分析

### 核心功能

- **用户管理**

  实现用户的注册、用户登录退出、忘记密码、找回密码等功能

  **Tips：**这里要涉及到用短信给手机发验证码，第三方账户关联先不引入，再说；同时数据库要有一个专门的表来存储放行；同时从安全性角度出发，密码至少6位，数字和字母混合，字母区分大小写，并且对密码采用MD5加密，并且登录时效性采用 JWT 令牌机制。

- **商品管理**

  商品的列表展示，点击后商品详情的图文多元化展示，商品多功能排序以及商品收藏，用户可以选择商品的规格参数

  **Tips：**商品展示要分页，避免一次取过多导致前端缓存过大；商品的列表展示最好也可以像淘宝一样切换风格；商品的规格参数库存要实时更新；单个商品由哪几项构成自己要清楚，然后必要的可以留给商家自主添加

- **搜索功能**

  暂时可以先做成基于关键词的 模糊搜索

  **Tips：**这个关键词应不仅仅在商品名称内 搜索完毕应该刷新商品界面并显示

- **店铺管理**

  店铺的列表展示，点击后进入店铺主页面，店铺主页面应留有可供商家编辑的广告，商家经过认证后登录还可以进行货物的增删改查，库存修改等（这点应该做成WEB后台），因此登录部分要把普通用户和商家分开，并且要显示不同的UI和功能

  **Tips：**主页面可以尽量简单优化为类似美团，商品+侧边栏导航+必要的图片文字展示即可

- **购物车管理**

  购物车商品展示，商品编辑包含修改数量、删除商品，显示购物车商品总价值，资金结算

  **Tips：**购物车的信息应该保留在前端，只存上一次的信息，但是App退出后，进程退出后，就需要依赖持久化存储，因此单独拿一个表出来做上一次购物车的记录存储

- **订单管理**

  包括提交订单，最近订单，订单跟踪（应与美团类似，可以调用地图查看货物当前位置），订单评价，历史订单（或说全部订单） 同时留好 待付款，待退款/售后的接口

  **Tips：**订单跟踪应与美团类似，可以调用地图查看货物当前位置，这个应该是可以外包给第三方配送平台的，这个不需要我们自己做，我们调用接口 只做展示，甚至展示也不用做，把第三方平台给的网址以短信形式发给用户 如 瑞幸咖啡就是这么和顺丰合作的

- **收货地址管理**

  新增和删除收货地址，应该和美团类似，可以由地图定位，并根据输入关键字进行匹配

- **支付管理**

  暂时搁置

- **内容管理**

  专题 在附近 暂时搁置

- **广告管理**

  主页面UI留出Banner的位置即可，同时也要学习引导页如何动态加载不同的图片 其他暂时搁置

- **适配**

  包含设备本身适配，不同安卓系统的适配（深度定制化安卓系统太多了），以及手机端和WEB端的适配



## 非功能性需求分析

#### 可用性

充分测试，出错处理并充分考虑程序崩溃无响应的出口，界面逻辑清晰，操作方便友好等等

#### 兼容与性能

上一节适配中也提到了，兼容就是适配，然后在性能要肯定要运行流畅，不会出现闪退、ANR（？？）、内存泄漏（buffer overflow）等问题，主要体现在数据流、图片缓存、列表优化、数据库操作等等方面。

一定把大部分的计算放在前端完成，前后交互只收发包最好



#### 暂定开发配置

预设应用场景：5000用户，可承受100-300并发访问不卡顿

- 服务端：Android和WEB端，其中商家管理和运行后台管理都放在WEB端

- 服务器：由于是电商服务，比较吃图片，100-300并发访问量下，预估考虑高带宽占用，

  4核8G 5M带宽最理想了，当然后面还有更多技术，如果真做大的话，什么集群，负载均衡，异地缓存什么的

- 数据库：暂定Mysql Community Server 充分学习优化技术 必要时可以考虑升级 Mysql Enterprise Server

  连接池技术，最大连接数

- 短信服务：阿里云短信服务SMS，用以发送验证码，短信收费，一定是必要验证时发送

- 身份认证：需要一定的计算性能，但是4核应该能Cover这个认证又不是并发的




## 程序总体设计

> 这玩意需要我们分期来做  MVP 减少代码耦合性，但是做出来的东西要能够复用  
>
> https://www.jianshu.com/p/1f91cfd68d48

### 架构层次分析

#### 界面层：

1.首页，链接（button 或者组件) 要能跳转进去（信息应该是WEB形式呈现的 这也是为什么做Hybrid应用的原因）

2.搜索界面 和淘宝一致就行

3.备选商品界面

4.商品介绍界面

5.确认购买/预订界面

6.购物车界面

7.商家店铺界面（商家店铺界面，按美团风格来走就行，不必像淘宝那么复杂，每个商家还都有自己的主页什么的）

8.用户个人中心界面，涉及各色管理

9.商家管理界面

10.管理员管理界面

这一个工程，也是让我们探索怎么去设计实现一个应用最好的途径

 

#### 接口层

1.接口层封装了服务器提供的API，定义一个请求引擎类，对请求的发送响应结果进行处理，方便给核心层调用。

2.搜索 用关键词找到商家  

3.首页信息 要么抓取，要么自己自动生成新闻（这就涉及到机器学习的技术了，但是很明显我们的机器是跑不成机器学习任务的，算力不够，做实验可以

#### 

#### 数据模型/处理层 

1.根据不同板块的需求，存取数据



#### Reference 可能会用到的

>  三层架构
>
> https://baike.baidu.com/item/%E4%B8%89%E5%B1%82%E6%9E%B6%E6%9E%84/11031448?fr=aladdin



>  UI层框架：  
>
> https://github.com/Tim9Liu9/TimLiu-Android#%E5%87%BA%E5%90%8D%E6%A1%86%E6%9E%B6
>
> 图片加载框架 Glide
>
> 上面的link 小到button或者menu都有框架 已经很全面了



>  接口层框架:
>
> https://www.cnblogs.com/angrycode/p/5956704.html





## UI设计



## 数据库设计

### 命名规范

- **表名**

  表名统一小写，采用字母和下划线组成，不同含义单词采用下划线分割。

  例：

  账户表account；

  用户清单表users_list；

  衣物评论clothes_comment等

- **属性名**

  属性名采用java变量命名规范，首词的首字母小写，从第二个单词含义开始首字母大写。

  例：

  用户表的userID；

  用户注册时间registerDate；

  用户最后登录时间

  lastLoginDate等

- **触发器和约束**

  触发器命名规范类似表名，采用字母和下划线组成，统一小写。格式为表名+下划线+触发器标志trg+触发器类型和操作类型（由于存储过程太费时，因此废弃）。

  例：

  ​       用户表主键自增触发器：user_trg_aftinsert;

  ​       用户表修改信息触发器：user_trg_aftupdate;

  ​       插入检测触发器：<tablename>_trg_befinsert；等

  ​       约束命名与触发器类似，采用字母和下划线组成，统一小写。格式为表名+属性名+类型。

  例：

  ​       用户昵称唯一性约束：user_Name_unique；

  ​       用户性别检查约束：user_sex_check；等

- **视图**

  视图命名规范类似表名，采用字母和下划线组成，统一小写。格式为逻辑表名+下划线+view。

  例：

  ​       用户租赁记录视图：user_order_view;

  ​       商家商品库存视图：business_store_view;等

- **索引**

  索引能够提高查询效率，因此比较重要。索引命名由表名，和属性名组成，中间采用下划线分开（MySql会自动为主键建立索引）。

  例：

  商品价格索引：

  clothes_set_price;

### 数据库优化

- **建立索引**

  为了避免全表扫描，最好为可能大量用到where以及order by的列建立索引，从而提高查找效率。例如用户需要经常搜索商品租赁价格，因此商品的price属性会经常被访问，所以最好为price添加索引。语句如下：

  ```
  CREATE INDEX clothes_set_price ON clothes_set(price ASC/DES);
  ```

  但是索引并不是越多越好，应当考虑最小化索引字段，因为每次UPDATE或者INSERT都会重建索引，引起UPDATE和INSERT语句低效率。一般情况下，一个表的索引最多不能超过5-6个。

- **避免NULL值**

  避免对表中的NULL值进行WHERE语句判断，否则MySql搜索引擎会对全表进行扫描。例如:

  ```
  SELECT clothesID WHERE price IS NULL;
  ```

  这样即使我们创建了index，搜索引起也会放弃，从而进行全表查询。当数据量达到百万、千万级别的时候将会非常耗时。而避免NULL值的方法有多种：

  ①尽量对可能多的属性设置”NOT NULL”约束避免NULL值。（描述，评论之类的除外）；

  ②能够使用INT类型数据的尽量使用INT，因为INT类型可以用0代替VARCHAR的NULL。

- **避免!=和 <>**

  与2.2相同，如果对WHERE语句使用!=或者<>的话，搜索引擎会自动放弃已经建立好的（如果有）的索引，进行全表查询。 

- **采用UNION代替OR**

  我们WHERE语句中使用了OR，当OR两段的属性一个有索引而另一个无索引的情况下，搜索引擎会自动放弃已经建立好的字段的索引进行全表查询，例：

  ```
  SELECT clothesID FROM clothes_set WHERE price = 100 OR address = WHU；
  ```

  上面的查询语句中，显然price应该建立索引而address是没必要建立索引的，但是MySql会对全表进行扫面，采用UNION替换成如下:

  ```
  SELECT clothesID FROM clothes_set WHERE price = 100
  
  UNION ALL
  
  SELECT clothesID FROM clothes_set WHERE address = WHU;
  
  ```

- **采用BETWEEN替换IN和NOT IN**

  WHERE语句中的IN和NOT IN同样会覆盖索引引起全表扫描。比如：

  ```
  SELECT lastLoginDate FROM users WHERE userID IN(1,2,3)
  ```

  类似上面这种连续数值的语句，尽量使用BETWEEN来查找，BETWEEN语句首先会去查找是否有索引，如果没有则全表扫描:

  ```
  SELECT lastLoginDate FROM users WHERE userID BETWEEN 1 AND 3;
  ```

- **强制索引** 

  动态查询的时候，如果WHERE语句中后面出现参数，MySql只有在运行的时候才会解析局部变量， 但是默认搜索引擎是需要在编译阶段就已经确定，不能推迟。例如PreparedStatement中，我们定义如下查询语句：

  ```
  SELECT password FROM accout WHERE username = ?；
  ```

  后面的？只有在运行的时候通过setString方法才能获取参数，即使我们建立了索引，还是没用。但是我们可以强制其使用索引index，如下:

  ```
  SELECT password FROM accout WITH(INDEX(name)) WHERE username = ?；
  ```

  其中name是username字段的索引名称，详见命名规范1.5。此处直接体现了2.1优化的重要性。

- **避免表达式**

  WHERE语句中尽量避免表达式，因为出现表达式搜索引擎在编译的时候无法去查找索引从而引起全表查询，通过事先计算表达式来消除。

- **避免SELECT… INTO语句**

  SELECT…INTO语句不会产生任何结果集，反而会频繁消耗时间，特别是创建临时表的时候，比如：

  ```
  SELECT username, clothesID INTO tem_shopping_cart WHERE…；
  ```

  应该为CREATE TABLE tem_shopping_car(…)；

- **UPDATE局部**

  需要更新信息的时候，UPDATE语句更新的字段越少越好。否则会带来极大内存消耗，产生大量数据库日志。

- **跨表多时采用分页连接**

  当需要跨多个表，并且表的数据量超过100条时，尽量避免直接进行外连接。采用分页方法替代，分页方法即SELECT嵌套。即：

  ```
  SELECT userID FROM(SELECT(…))…;
  ```

-  **数据类型**

  MySql4.1以后支持变长类型VARCHAR，能使用VARCHAR就使用VARCHAR替代CHAR，因为CHAR固定长度，不管该字段的值达没达到最大长度，都会占用指定的空间，但是变长VARCHAR就是根据具体的内容分配，极大节省了空间。

  能用INT、FLOAT或者DOUBLE数值类型的，就用数值类型，因为数值类型存储方式简单。而字符型对判等敏感，从而直接降低WHERE，JOIN等语句性能，另一方面同样内容，字符型的存储空间比数值型大很多。

- **避免游标**

  游标会带来一定安全隐患，并且很容易出错，游标的效率也很低。如果游标指向的数值超过了一万行，就应该改写。

- **事务处理**

  当完成一个事务需要多个步骤的时候，不一定每个步骤都会成功执行，特别是当用户众多，并发量大的时候，如果处理不当轻则产生死锁，重则损坏数据。因此要么所有的步骤全部成功，但如果一个失败，则全部放弃。采用BEGIN…COMMIT语句进行事务处理：

  ```
  BEGIN
  
        transaction start
  
  COMMIT;
  ```

- **锁定表维护数据**

  事务是维护数据库完整性的一个非常好的方法，但却因为它的独占性，有时会影响数据库的性能，尤其是在很大的应用系统中。由于在事务执行的过程中，数据库将会被锁定，因此其它的用户请求只能暂时等待直到该事务结束。如果一个数据库系统只有少数几个用户来使用，事务造成的影响不会成为一个太大的问题；但假设有成千上万的用户同时访问一个数据库系统，就会带来比较严重的延迟。有些情况下我们可以通过锁定表的方法来获得更好的性能。例如:

  ```
  LOCK TABLE clothes_set WRITE INSERT < clothes >  INTO  clothes_set;
  UPDATE clothes_set SET stroe=11…UNLOCKTABLES;
  ```

  在UNLOCKTABLES执行前，其他用户无法访问该表。







主要仿照美团，需要几个表，每个表有哪几个属性

- 关于会员模块

  都是userID没问题，username用户名限制一下字母数字不可有空格乱七八糟字符之类的

  但是不能重复，然后注册只能普通用户注册，商家入驻需要联系我们

  一张表 userID（auto Increment），username（普通用户注册只能用手机号，并且发短信验证码）type password，一张表 管登录 管跳转 登录查这一张表就可以了，店铺名我们自分配的（根据这个type，去不同的表取信息）

  - userID,username（fk）,password,type

  - username（pk） ，NickName，Sex，Age ，Email Phone（默认和UserName一致）， registerDate ，LastLoginDate LastLoginDevice or Location，UserType（指VIP之类的）， VIPID，Identity（身份信息）
  - username（pk），店铺名称，店铺地址，店铺营业执照信息（社会统一信用代码），
  - 管理员不需要表，根据type直接登录就好，因此 type很关键 不能留能修改type的接口 
  - VIPID...other必要信息 可以补充一下 比如积分
  - addressID userID（fk） name phone Address （一对多的表）userID userName为索引都可以

- 关于商品模块

  商品目前只有一种 

  - 商品ID（pk），username（店铺名 fk），商品name，sex，品牌brand，尺码，颜色，货号，风格，厚薄，适用场景，版型，衣长,resource_URL
  - clothesID（pk），商品ID（fk），color，size, stock，price，resource_URL
  - commentID(pk),商品ID（fk），订单ID，date，
  - 订单ID，clothesID（fk），商品名（fk）用户名，店铺名，数量，单价，价格

- 用户列表

```
users_list
(
	userID(PK),
	username(FK) REFERENCES users(username)
)

```

- 用户信息表

```
users
(
	username(PK),
	vipID(FK) REFERENCES vip(vipID)
	sex,
	age,
	nickName,
	phone,
	email,
	realName,//真实姓名
	identity,//身份证号
	registerDate,
	lastLoginDate,
	lastLoginDevice,
	lastLoginLocation,//安全保护
	attentionBusiness,？？
	latelyTrack
)

```

- 登录验证表

```
account
(
	username(PK),
	type,       //common users or DBM or Admin
	password   //extremely important 传输加密
)
```

- 会员表

```
vip
(
	vipID(PK),//暂定，其实这个版本用不到
	vipStartTime,
	vipEndTime,
	vipRestTime,
	vipScore,
	vipDiscount,
	vipLevel,
	autoRenew,
)
```

- 商家列表

```
商家需要手动联系我们注册。
business_list
(
	businessID(PK),
	username(FK) REFERENCES business(username)
)

```

- 商家信息表

```
business
(
	username(PK),//店铺id 当然也可以用店铺主手机号注册     
	identity,//社会统一信用代码，需认证
	email,
	name,//店铺名称
	phone
	registerDate,
	lastLoginDate,
	lastLoginLocation,
	address//店铺地址
)

```

- 商品信息

```
goods
(
	goodsID(PK),
	businessID (FK), REFERENCES business_list(businessID),
	name,//商品名称
	sex,//男 女 通用
	brand,
	size,//尺码
	code,//货号
	color,//颜色
	style,//风格
	thick,//厚薄
	scenarios,//适用场景 方便分类
	model,//版型
	clothes_length//衣长
)

```

- 服装

```
clothes_set
(
	clothesID(PK),
	goodsID(FK) REFERENCES goods(goodsID),
	color,
	size,
	stock,
	price,
	deposit
)

```

- 订单

```
order
(
	orderID(PK),
	clothesID(FK),REFERENCES clothes(clothesID),
	userID(FK), REFERENCES users_list(userID),
	goodsID(FK),REFERENCES goods(goodsID),//对，应该是产生不同的订单号，但是是同一个用户购买的
	businessID (FK), REFERENCES business_list(businessID),//店铺名
	number,//数量 一件clothes批处理 不同clothes就分开 在订单显示这栏
	primaryPrice,//初始价格 其实就是求和 每件clothes的价格*每天租金
	totalPrice,//总价，可能会有折扣
	rentalDate,
	returnDate,//可以选择归还日期
	state
)
```

- 订单评论

```
order_comment	
(
	commentID(PK),
	orderID(FK), REFERENCES order(orderID),
	commentDate,
	commentContent
)
```

- 购物车（临时）

```
shopping_cart
(
	cartID(PK),//临时，每次插入新的就会把上一次记录删除，cartID应该是由时间生成的guid
	userID(FK), REFERENCES users_list(userID),//以用户名为primary key只记录last
	goodsID(FK), REFERENCES goods(goodsID),
	businessID(FK),
	clothesID(FK), REFERENCES clothes(clothesID),//具体 size color 可以查clothes表获得
	number,
	totalPrice,
)
```



## 模块划分

> 模块设计

- 会员模块

  实现普通用户注册与登录，实现普通用户个人信息管理，实现普通用户注册短信验证，并留有身份验证接口（如接下来拍摄身份证识别返回信息等等）

  实现店铺用户注册与登录，实现普通用户和店铺用户，留有两者不同界面的跳转的接口，写好店铺用户修改店铺信息的接口，实现店铺用户注册短信验证，并留有店铺用户身份验证的接口

  实现系统管理员的登录，实现不同的跳转

  增强安全保护，对http请求，前端密码和后台数据库传输交互这个过程进行MD5加密传输

  采用Token令牌机制，为用户的每次登录发放时效令牌，降耦合不将用户的登录状态存在后端

- 商品模块

  实现商品管理的所有功能

  实现商品搜索界面，预期效果是点击搜索，展示搜索历史，并且覆盖掉多余UI元素

  点击搜索后将搜索结果展示到商品列表里

  仿照美团实现商品的展示列表，类似于tableview

- 购物车管理

  购物车管理

