https://blog.csdn.net/daemon329/article/details/106170743/

表
create table [if not exists] `表名`(
'字段名1' 列类型 [属性][索引][注释],
'字段名2' 列类型 [属性][索引][注释],
#...
'字段名n' 列类型 [属性][索引][注释]
)[表类型][表字符集][注释];

修改表名 :ALTER TABLE 旧表名 RENAME AS 新表名
DROP TABLE [IF EXISTS] 表名

字段
添加字段 : ALTER TABLE 表名 ADD字段名 列属性[属性]
修改字段 :

- ALTER TABLE 表名 MODIFY 字段名 列类型[属性]
- ALTER TABLE 表名 CHANGE 旧字段名 新字段名 列属性[属性]

删除字段 : ALTER TABLE 表名 DROP 字段名

可用反引号（`）为标识符（库名、表名、字段名、索引、别名）包裹，以避免与关键字重名！中文也可以作为标识符！

模式通配符：
_ 任意单个字符
% 任意多个字符，甚至包括零字符
单引号需要进行转义 \'
