## HBase
#### 1、HBase简介

高可靠、高性能、面向列、可伸缩的分布式存储系统，分布式数据库。可以用来存储非结构化和半结构化的松散数据

###### 为什么要去设计HBase数据库？

内部切分、水平扩展都是后台全自动实现，具有非常好的水平可扩展性

数据结构多变，HBase能满足不断增长的数据存储需求

#### 2、HBase数据模型

HBase是一个稀疏的多维度的排序映射表

![](/Users/fang/Documents/markdown-笔记/HBase/HBase数据模型.png)

##### 列族

支持动态扩展

保留旧的版本（由于HDFS只支持写入操作，不允许修改）

###### 列限定符
![](/Users/fang/Documents/markdown-笔记/HBase/列限定符.png)

###### 时间戳：

在一个单元格里面，旧的版本会被保存，新的版本会通过时间戳来进行区分

###### 数据坐标概念

![](/Users/fang/Documents/markdown-笔记/HBase/数据坐标概念.png)

###### 面向列的存储

便于数据分析

#### 3、HBase实现原理

##### HBase的功能组件

![](/Users/fang/Documents/markdown-笔记/HBase/HBase功能组件.png)

###### Master服务器

![](/Users/fang/Documents/markdown-笔记/HBase/Master服务器.png)

###### Region服务器

具体负责数据的存储

![](/Users/fang/Documents/markdown-笔记/HBase/Region服务器.png)

![](/Users/fang/Documents/markdown-笔记/HBase/Regin介绍.png)

###### HBase三层结构

![](/Users/fang/Documents/markdown-笔记/HBase/HBase三层结构.png)

-ROOT-表：存取的是元数据表的元数据信息被存放到哪里

元数据的.META.表：负责存储具体数据存取到哪些服务器上

.META.表的全部Region都会被保存在内存中

![](/Users/fang/Documents/markdown-笔记/HBase/HBase三层结构中各层次的名称和作用.png)

#### 4、HBase运行机制

##### HBase系统架构

![](/Users/fang/Documents/markdown-笔记/HBase/HBase系统架构.png)

###### 客户端和Zookeeper

![](/Users/fang/Documents/markdown-笔记/HBase/客户端和Zookeeper.png)

###### Master（主服务器）

![](/Users/fang/Documents/markdown-笔记/HBase/Master服务器作用.png)

###### Region服务器

Region服务器向HDFS文件系统中读写数据

- 用户读写数据过程

![](/Users/fang/Documents/markdown-笔记/HBase/用户读写数据过程-写.png)

![](/Users/fang/Documents/markdown-笔记/HBase/用户读写数据过程-读.png)

- 缓存的刷新

![](/Users/fang/Documents/markdown-笔记/HBase/缓存的刷新.png)

- StoreFile的合并和分裂==>即HBase的分裂操作

![](/Users/fang/Documents/markdown-笔记/HBase/StoreFIle的合并.png)

![](/Users/fang/Documents/markdown-笔记/HBase/StoreFIle的分裂.png)

- HLog的工作原理

通过日志的方法来恢复数据

![](/Users/fang/Documents/markdown-笔记/HBase/日志恢复.png)

Zookeeper负责监视整个集群，告诉哪个服务器出现问题，并通知给相关的Master。则Master就会迁移故障服务器上的数据

#### 5、HBASE应用方案

##### 性能优化方法

- 时间戳

![](/Users/fang/Documents/markdown-笔记/HBase/性能优化方法-时间戳.png)

- 提升读写性能，将必要表方案Region服务器的缓存中

![](/Users/fang/Documents/markdown-笔记/HBase/HBase性能优化-提升读写性能.png)

- 自动清除过期数据

![](/Users/fang/Documents/markdown-笔记/HBase/HBase性能优化-自动清除.png)

##### HBase怎么检测性能？

性能检测相关工具

![](/Users/fang/Documents/markdown-笔记/HBase/HBase性能检测相关工具.png)

##### 在HBase进行SQL查询工具

![](/Users/fang/Documents/markdown-笔记/HBase/HBase应用-SQL.png)

##### 构建HBase二级索引

对不同的列进行索引

![](/Users/fang/Documents/markdown-笔记/HBase/HBase应用-二级索引.png)

![](/Users/fang/Documents/markdown-笔记/HBase/HBase应用-二级索引-补.png)

![](/Users/fang/Documents/markdown-笔记/HBase/HBase应用-二级索引-补1.png)

#### 6、HBase安装与配置

![](/Users/fang/Documents/markdown-笔记/HBase/HBase安装注意.png)

#### 7、Shell命令

![](/Users/fang/Documents/markdown-笔记/HBase/Shell命令.png)

###### 创建表

create '表名' ,‘列名’,‘列名’,‘列名’

###### 如何添加数据--put

列名称可以添加时自定义

![](/Users/fang/Documents/markdown-笔记/HBase/Shell命令-添加数据.png)

###### 如何查看数据--get

![](/Users/fang/Documents/markdown-笔记/HBase/Shell命令-查看数据.png)

###### 如何删除一个表--disable   drop

![](/Users/fang/Documents/markdown-笔记/HBase/Shell命令-删除.png)

#### 8、HBase常用Java API及应用实例

##### HBase使用Java API准备过程

![](/Users/fang/Documents/markdown-笔记/HBase/HBase使用Java语言.png)

##### 实例

###### 整体过程

![](/Users/fang/Documents/markdown-笔记/HBase/HBase-Java API-实例.png)

###### 建立连接、关闭连接、创建表

- 建立连接

![](/Users/fang/Documents/markdown-笔记/HBase/HBase-Java API-建立连接.png)

- 关闭连接

![](/Users/fang/Documents/markdown-笔记/HBase/HBase-Java API-关闭连接.png)

- 创建表

![](/Users/fang/Documents/markdown-笔记/HBase/HBase-Java API-创建表.png)

- 添加数据

![](/Users/fang/Documents/markdown-笔记/HBase/HBase-Java API-添加数据.png)

指定参数

![](/Users/fang/Documents/markdown-笔记/HBase/HBase-Java API-添加数据-指定参数.png)

- 浏览数据

![](/Users/fang/Documents/markdown-笔记/HBase/HBase-Java API-浏览数据.png)

指定参数

![](/Users/fang/Documents/markdown-笔记/HBase/HBase-Java API-浏览数据-指定参数.png)











