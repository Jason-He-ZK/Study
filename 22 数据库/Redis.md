数据类型
常用操作
Java 操作 Redis
主从模型搭建
哨兵集群搭建
日志持久化
应用场景


数据类型
String
key - value
二进制安全的。意思是 redis 的 string 可以包含任何数据。比如jpg图片或者序列化的对象。

Hash
key - value 的集合

List
String 集合（有序，添加到头或尾）

Set
String 集合（无序）

zset
String 集合（有序，不重复）
每个元素都会关联一个double类型的分数。通过分数来从小到大排序。

事务
MULTI 开启事务 -> 将多个命令入队到事务中 -> EXEC 触发事务

事务中任意命令执行失败，其余的命令依然被执行。
事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中。

DISCARD 取消事务

WATCH key [key ...] 监视一个(或多个) key 
如果在事务执行之前 key 被其他命令改动，事务将被打断

UNWATCH 取消 WATCH 命令对所有 key 的监视。
