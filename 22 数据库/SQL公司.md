INSERT INTO 表名 [(字段1,字段2,字段3,...)] VALUES('值1','值2','值3'),('值1','值2','值3');
UPDATE 表名 SET column_name=value,column_name2=value2 [WHERE condition];
DELETE FROM 表名 [WHERE condition];
TRUNCATE [TABLE] table_name;
相同 : 都能删除数据 , 不删除表结构 , 但TRUNCATE速度更快
不同 :TRUNCATE会重新设置AUTO_INCREMENT计数器；不会对事务有影响

PRIMARY KEY `PK_ID` _(_`ID`_)_,
UNIQUE KEY `UK_CODE` _(_`CODE`_) _USING BTREE,
KEY `IDX_SCENE_CATEGORY_SCENE_TYPE` _(_`SCENE_CATEGORY`,`SCENE_TYPE`_)_

-- 创建后添加
ALTER TABLE `result` ADD KEY`idx_studentNo`(`studentNo`);

-- 我们可以在创建上述索引的时候，为其指定索引类型，分两类
hash类型的索引：查询单条快，范围查询慢
btree类型的索引：b+树，层数越多，数据量指数级增长（我们就用它，因为innodb默认支持它）



create index 创建索引 
drop index 删除索引

主键索引
唯一索引
组合索引（考虑最左匹配）
单列索引 / 普通索引

聚簇索引：同一个B-Tree中保存了索引列和具体的数据（Innodb），适合范围查询
非聚簇索引 / 二级索引：叶子节点中保存的是行的主键值

覆盖索引 回表查询

缓存
DDL DML？会使缓存失效

SELECT SQL_CACUE id,name FROM customer;
SELECT SQL_NO_CACHE id,name FROM customer;
故在进行SQL优化时，则可以使用SQL_NO_CACHE不走缓存进行查询。

Explain todo
id
id值越大，优先级越高，越先执行
id相同时，执行顺序由上至下,可以认为是一组，从上往下顺序执行


字符型
	VARCHAR(M)：可变字符串，M实测最大21841
日期型
	DATETIME会丢失时区信息 TIMESTAMP不会丢失时间
整型
	所有整形后可以指定数字，如TINYINT(N)，N表示zerofill的位数，当只该字段指定了zerofill才有用，当位数不满足N时，左侧补零，`STAT` tinyint(2) unsigned ZEROFILL **DEFAULT** '0' 
TINYINT通常可用于小范围的枚举值/字典，是否判断等
SMALLINT可用于更大范围的字典、端口等
BIGINT通常用于自增加主键
浮点型
DOUBLE (5,2) 总共五位，小数占两位

