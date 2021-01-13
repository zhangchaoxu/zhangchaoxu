# 数据库设计规范和实现

程序开发基于MySQL5.7实现,理论上支持Oracle、SQLServer、DB2等其它数据库  
_注意_ 修改表中各个数据库中有区别的地方 _注意_ 修改Mapper文件中是否有MySQL特有的查询语句

## 数据持久化实现

数据库的持久化在比较JPA、Hibernate、MyBatis之后选择由[MyBatis-Plus](https://mybatis.plus/)实现,兼具易用性和灵活性。

## 表命名与字段设计规范

1. 数据库名称、表名称、字段名称一律小写,用\_分割
2. 遵守模块名称\_业务名称的形式,比如uc\_user
3. 关联表使用两个表名连接方式，比如uc\_user\_role
4. 建议所有表都带上id、deleted、create\_time、update\_time、create\_id、update\_id等标准字段
5. 多租户表都带上tenant\_id和tenant\_name,其中tenant\_name为冗余租户名
6. 表示时间的字段用xx\_time\(datetime\),表示日期的字段用xx\_date\(date\)
7. 索引命名:
   * pk_表名缩写_字段名: 为兼容多数据库,索引必须全库唯一
   * 唯一索引命名: uk_表名缩写_字段名
   * 普通索引命令: idx_表名缩写_字段名

## 主键

数据主键id使用MyBatisPlus的ID_Worker，一律使用20位的unsigned tinyint，注意在pojo中使用Long定义  
优点是兼具自增主键的连续性\(优化查询效率\)和uuid的不可推测性_  
注意\_ JavaScript无法处理Java的Long,会导致精度丢失,具体表现为主键最后两位永远为0,解决思路:Long 转为 String 返回 [更多文档](https://mybatis.plus/guide/logic-delete.html)

## 逻辑删除

逻辑删除deleted使用1位的unsigned tinyint,1表示删除,0表示未删除  
在Entity中为deleted加上@TableLogic注解,对于使用Wrapper查询的sql会自动加上deleted = 1  
但是如果在mapper中做了自定义级联查询,需要手动加上deleted = 0的条件  
[更多文档](https://mybatis.plus/guide/logic-delete.html)

## 自动填充

自动填充通过AutoFillMetaObjectHandler实现,实现方式是填充到入参的entity内  
_注意_ 对于update方法只有update\(entity, updateWrapper\)才会自动填充，直接调用update\(updateWrapper\)是不会自动填充的 [更多文档](https://mybatis.plus/guide/auto-fill-metainfo.html)

## 分页

使用MybatisPlus的数据分页实现 [更多文档](https://mybatis.plus/guide/page.html) 更多问题见[MyBatisPlus文档](https://mybatis.plus/)

## 字段业务冗余

对于关联表的字段,适当考虑冗余到表中,比如文章表中的文章分类名称,可以考虑冗余到文章表中,在文章分类修改的时候,统一修改一下文章中的冗余字段即可。

## 大小写敏感

数据库设计中一律采用小写表名和字段名,其中定时任务默认创建表为大写,建议将mysql设置为大小写不敏感  
修改/etc/my.cnf文件,在\[mysqld\]节点加入配置`lower_case_table_names = 1`,然后重启mysql\(`service mysqld restart`\)即可

## ref

* [并发扣款，如何保证数据的一致性？](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651962738&idx=1&sn=d2d91a380bad06af9f7b9f7a80db26b3)
* [并发扣款一致性，幂等性问题，这个话题还没聊完！！！](https://mp.weixin.qq.com/s/xXju0y64KKUiD06QE0LoeA)

