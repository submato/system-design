作者创建了高质量的系统设计仓库，欢迎star：https://github.com/submato/system-design-pro

# system-design
后端面试 场景题、程序设计题


列举了5个经典的场景题/程序设计题，学会这几个场景的系统设计题，大部分场景题一定可以轻松pass，即使不在这五个场景里，知识也是通用的。

支付宝扫码 获取完整pdf只需9.9元，请作者喝杯咖啡吧

(请在支付订单备注你的邮箱，pdf将发送到备注的邮箱中； 工作日将在2个小时内发送到你的邮箱，发送时间10:00 - 22:00，节假日在22点统一发送)

> 关于退款

如果您对PDF不满意，请按照以下格式回复邮件，作者将为您全额退款

使用备注的邮箱回复邮件，付款订单截图(必需) + 不满意原因(非必需)

二维码：<img width="200" alt="image" src="https://github.com/submato/system-design/assets/55040284/52539886-2bee-4f47-aa5a-27903a64ac0c">


## 1. 商城秒杀：

a. 依赖的中间件：网关、分布式缓存、消息队列、限流、数据库、TCC等

b. 需要考虑的点：缓存预热、缓存与数据库的一致性方案、降级、熔断、削峰

c. 加分点：蓄洪与事后泄洪

> 设计方案

设计难点主要在QPS及数据最终一致上

1. 过滤无***************、风控等等。
2. 用分布式缓存，在缓存中预扣库存：
   1. 缓存预热：通过定***************、击穿。
   2. incrby -1(原子的) ***************、下过单也可以存一下
3. 可能库存有很多，下单的QPS也很高。创建***************、消费消息创建订单。
4. 前端轮训订单***************、支付。12306就是这样做的。



如何加速消费(泄洪)和减慢消费(蓄洪)



1. 重复消费问题：
   1. 也可以简***************、本号
   2. 消费者***************、一把cid)。
   3. 但是有时候我们没有***************、个事物中，会因为写消息表失败而影响主流程。
2. 消息丢失问题：
   1. 消息队***************、接口通知
   2. 内存存一个失败队列，或者***************、败的消息。（如果有顺序则不行）



## 2. 排行榜（微信步数等）

a. 依赖的中间件：网关、redis sorted set、数据库等

b. 需要考虑的点：并发、数据库排序

c. 加分点：有些排行榜可以考虑在前端/客户端做，比如：排序数据量不大/排序场景很固定，面试时提到这点很加分。

难点是：高QPS、高实时性的、数据量大的排行榜。

存储：

1. 如果数据***************、序需求的也可以走es。
2. 如果QPS比较大的话，es***************、、改、查都能达到logN。

过程：



## 3. 红包系统

a. 依赖的中间件：网关、分布式缓存、消息队列、数据库、TCC等

b. 需要考虑的点：并发、红包如何拆分、读写分离、异步化

c. 加分点：红包核对等

比较系统的红包方案：https://zh***************、17340

存储：

1. red***************、余数
2. 红包表***************、剩余金额)
3. 红***************、取用户id) 

场景及过程

1. 普通场景：二倍***************、但是还是有缓存穿透的风险
2. 大型活动：红包总量很大时，瞬***************、edis中红包id，并从redis中获取计***************、回给用户。并通过消息***************、对mysql进行保护。



## 4. 类微博的feed流系统

a. 依赖的中间件：网关、数据库、缓存、消息队列等

b. 需要考虑的点：并发、实时推送、消息推拉模式、数据库设计

c. 加分点：根据用户活跃场景采用推+拉模式

这个讲的挺好的：https://blog.csdn***************、/131288054

存储：

1. 每一条feed的内***************、edis缓存。

2. 用户的timeli***************、d，score是ctime，自动就排好序了，在更具feedId获***************、是活跃用户。

采用推+拉模式

1. 对活跃用户，将他***************、ine页中（推模式，有写扩散）
2. 对于非活跃用户，从m***************、标记为活跃用户(拉模式，有读扩散)



## 5. 消息系统

a. 依赖的中间件：网关、数据库、缓存、消息队列、冷热库存储

b. 需要考虑的点：如何收发消息（推/拉），消息如何聚合（多条消息聚合成一个通知提醒）

c. 加分点：按照场景存储消息（点赞/私信/广告），冷热库


