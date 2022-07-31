

brew services start mysql

mysql -u root -p



## 概述

### SQL、SQL脚本
SQL：结构化查询语言
以 ; 结尾
不区分大小写
字符串用‘ ’括起来
SQL 语句在执行的时候，实际上内部也会先进行编译，然后再执行 sql。（sql 语句的编译由 DBMS 完成。）

- 文件的扩展名是.sql，并且该文件中编写了大量的 sql 语句
- source 命令可以直接执行 sql 脚本（脚本数据量太大无法打开时，可以用 source 完成初始化）

### SQL 语句分类

- DQL（数据查询语言）：查询语句，凡是 select 语句都是 DQL
- DML（数据操作语言）：对表中的数据的增删改，insert delete update
- DDL（数据定义语言）：对表结构的增删改，create drop alter
- DCL（数据控制语言）：grant 授权、revoke 撤销权限等
- TCL（事务控制语言）：commit 提交事务，rollback 回滚事务

### 常用命令

```latex
// 登录MySQL
mysql -u用户名 -p密码
mysql -uroot -p333

// 退出mysql
exit
```

```latex
plaintext// 不是SQL语句，属于MySQL的命令

// 查看有哪些数据库
show databases;

// 创建数据库
create database 数据库名;
// 删除数据库
drop database 数据库名;

// 使用某个数据库
use 数据库名;

// 查询当前使用的数据库
select database();

// 查询数据库版本
select version();

// 查看当前使用的数据库中有哪些表
show tables;

// 查看某个数据库中有哪些表
show tables from 数据库名 ;

// 初始化数据（给数据库导入数据）
source sql脚本路径
source D:\course\05-MySQL\resources\bjpowernode.sql

// 查看创建表的语句
show create table 表名; 

// 查看表结构
desc 表名

// 结束一条语句
\c
```

## 数据查询语言（DQL）

### 简单查询，条件，排序

-  语法格式    
```sql
select           3
  字段,字段...
from             1
  表名
where            2
  条件
order by         4
  字段,字段...
```
```sql
// 给查询结果的列重命名（as关键字可以省略）
select ename,sal * 12 as yearsal from emp; // 字段可以进行运算

// 别名中有中文，用单引号括起来
select ename,sal * 12 as 年薪 from emp; // 错误
select ename,sal * 12 as '年薪' from emp;

// 查询所有字段
select * from emp; // 实际开发中不建议使用*，效率较低
```
```sql
查询结果集的去重：distinct（所有字段一起联合去重）

select distinct job from emp;

// 错误，distinct只能出现在所有字段的最前面(后面的去重后不好排序)
select ename,distinct job from emp;

// 案例：统计岗位的数量
select count(distinct job) from emp;
```

-  条件 where  
```sql
>   >=   <   <=   !=(<>)

between 值1 and 值2（ 闭区间，值1>值2 ）

字段名 is (not) null

and，or（and优先于or，可用()调整优先级）

in，not in（等同于or）
  字段名 in（值1，值2，值3...）
  字段名 = 值1 or 字段名 = 值2 or 字段名 = 值3

模糊查询like，\转义
  字段名 like '%aso_'
  %代表任意多个字符
  _代表任意1个字符（\_）
```

-  排序 order by  
```sql
asc表示升序（默认），desc表示降序

字段1 asc，字段2 desc（先字段1升序，如相同，再字段2降序）
```

### 多行、单行处理函数

-  多行处理函数（分组函数）
输入多行，最终输出的结果是 1 行（多行处理函数）
自动忽略 NULL
一般都会和 group by 联合使用，这也是为什么它被称为分组函数
一共 5 个 
不能直接使用在 where 子句当中，因为 group by 是在 where 执行之后才会执行的  
```sql
count() 计数
  count(*)    统计总记录条数（不是统计某字段中数据的个数）
  count(字段)  表示统计字段中不为NULL的数据总数量
sum(字段名) 求和
avg(字段名) 平均值
max(字段名) 最大值
min(字段名) 最小值
```
```sql
// 错误
select ename,sal from emp where sal > avg(sal);

// 可以分成两句写，用括号
select ename,sal from emp where sal > (select avg(sal) from emp);
```

-  单行处理函数
输入一行，输出一行。
空行处理函数：ifnull()
所有数据库都规定，只要有 NULL 参与的运算结果一定是 NULL。  
```sql
// 语法：ifnull(可能为NULL的数据,被当做什么处理)
select ename,ifnull(comm,0) as comm from emp;
```

### group by ， having

```sql
select      5
  字段,字段...
from        1
  ..
where       2
  ..
group by    3
  字段,字段...
having      4
  ..
order by    6
  ..
```

- group by：按照某个字段或者某些字段进行分组
所有分组函数都是在 group by 语句执行结束之后才会执行
当一条 sql 语句没有 group by 的话，整张表的数据会自成一组
有 group by 时，select 只能出现分组函数及参与分组的字段
多个字段联合分组 
```sql
// 找出每个部门不同工作岗位的最高薪资。
select
  deptno,job,max(sal)
from
  emp
group by
  deptno,job;
```

- having：对分组之后的数据进行再次过滤
一定与 group by 一起用
能够使用 where 过滤的尽量使用 where，效率更高
只能使用 having（涉及分组函数时） 
```sql
// 错误
select deptno,avg(sal) from emp where avg(sal) > 2000 group by deptno;

// 使用having的正确方法
select deptno,avg(sal) from emp group by deptno having avg(sal) > 2000;
```
group by 取最新的数据
```sql
select b.* from
(select max(`id`) as id from `target` group by `name`) as a
join `target`as b on
a.id=b.id
```

### 连接查询

#### 概述

- 什么是连接查询
在实际开发中，大部分的情况下都不是从单表中查询数据，一般都是多张表联合查询取出最终的结果
- 分类 
   - 根据语法出现的年代划分
SQL92（一些老的 DBA 可能还在使用这种语法。DBA：DataBase Administrator，数据库管理员）
SQL99（比较新的语法）
   - 根据连接方式划分 
      - 内连接：
等值连接
非等值连接
自连接
      - 外连接：
左外连接（左连接）：表示左边的表是主表
右外连接（右连接）：表示右边的表是主表
左连接有右连接的写法，右连接也会有对应的左连接的写法。
      - 全连接：（这个不讲，很少用）
   - 内连接与外连接
内连接
AB 两张表没有主副之分，两张表是平等的。
凡是 A 表和 B 表能够匹配上的记录查询出来，这就是内连接。
外连接
AB 两张表中有一张表是主表，一张表是副表。
主要查询主表中的数据，捎带着查询副表，当副表中的数据没有和主表中的数据匹配上，副表自动模拟出 NULL 与之匹配。

- 笛卡尔积现象（笛卡尔乘积现象）
当两张表进行连接查询时，如没有任何条件进行限制，最终的查询结果条数是两张表记录条数的乘积
避免方法：加条件进行过滤（不会减少记录的匹配次数，只不过显示的是有效记录） 
```sql
// SQL92:（太老，不用了）
select 
  e.ename,d.dname
from
  emp e, dept d
where
  e.deptno = d.deptno;
```

- 关于表的别名
select e.ename,d.dname from emp e,dept d;
表的别名好处
第一：执行效率高（会去特定的表找）
第二：可读性好

#### 内连接

- 等值连接
最大特点：条件是等量关系。 
```sql
// SQL99：常用的，语法结构更清晰，表的连接条件和后来的where条件分离了
// 案例：查询每个员工的部门名称，要求显示员工名和部门名。
select 
  e.ename,d.dname
from
  emp e
inner join // inner可以省略的，带着inner目的是可读性好一些。
  dept d
on
  e.deptno = d.deptno;

// 语法
...
  A
join
  B
on
  连接条件
where
  ...
```

- 非等值连接
最大的特点是：连接条件中的关系是非等量关系 
```sql
// 案例：找出每个员工的工资等级，要求显示员工名、工资、工资等级。
select 
  e.ename,e.sal,s.grade
from
  emp e
inner join // inner可以省略
  salgrade s
on
  e.sal between s.losal and s.hisal;
```

- 自连接
最大的特点：一张表看做两张表。自己连接自己 
```sql
// 案例：找出每个员工的上级领导，要求显示员工名和对应的领导名
// （员工的领导编号 = 领导的员工编号）
select 
  a.ename as '员工名',b.ename as '领导名'
from
  emp a
inner join
  emp b
on
  a.mgr = b.empno;
```

#### 外连接（使用比内连接多）

最重要的特点：主表的数据无条件的全部查询出来

```sql
// 案例：找出每个员工的上级领导？（所有员工必须全部查询出来。包括最大的boss）

外连接：（左外连接/左连接）
select 
  a.ename '员工', b.ename '领导'
from
  emp a
left outer join // outer是可以省略的。
  emp b
on
  a.mgr = b.empno;

外连接：（右外连接/右连接）
select 
  a.ename '员工', b.ename '领导'
from
  emp b
right outer join // outer可以省略。
  emp a
on
  a.mgr = b.empno;
```

```sql
案例：找出哪个部门没有员工？
EMP表
+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
DEPT
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+

select
  d.*
from
  emp e
right join
  dept d
on
  e.deptno = d.deptno
where
  e.empno is null;

+--------+------------+--------+
| DEPTNO | DNAME      | LOC    |
+--------+------------+--------+
|     40 | OPERATIONS | BOSTON |
+--------+------------+--------+
```

#### 多表连接查询

```sql
案例：找出每一个员工的部门名称以及工资等级。
EMP e
+-------+--------+---------+--------+
| empno | ename  | sal     | deptno |
+-------+--------+---------+--------+
|  7369 | SMITH  |  800.00 |     20 |
|  7499 | ALLEN  | 1600.00 |     30 |
|  7521 | WARD   | 1250.00 |     30 |
|  7566 | JONES  | 2975.00 |     20 |
|  7654 | MARTIN | 1250.00 |     30 |
|  7698 | BLAKE  | 2850.00 |     30 |
|  7782 | CLARK  | 2450.00 |     10 |
+-------+--------+---------+--------+
DEPT d
+--------+------------+----------+
| DEPTNO | DNAME      | LOC      |
+--------+------------+----------+
|     10 | ACCOUNTING | NEW YORK |
|     20 | RESEARCH   | DALLAS   |
|     30 | SALES      | CHICAGO  |
|     40 | OPERATIONS | BOSTON   |
+--------+------------+----------+
SALGRADE s
+-------+-------+-------+
| GRADE | LOSAL | HISAL |
+-------+-------+-------+
|     1 |   700 |  1200 |
|     2 |  1201 |  1400 |
|     3 |  1401 |  2000 |
|     4 |  2001 |  3000 |
|     5 |  3001 |  9999 |
+-------+-------+-------+


select 
  e.ename,d.dname,s.grade
from
  emp e
join
  dept d
on
  e.deptno = d.deptno
join // 表示：e表和d表先进行表连接，连接之后的表继续和s表进行连接。
  salgrade s
on
  e.sal between s.losal and s.hisal;

+--------+------------+-------+
| ename  | dname      | grade |
+--------+------------+-------+
| SMITH  | RESEARCH   |     1 |
| ALLEN  | SALES      |     3 |
| WARD   | SALES      |     2 |
| JONES  | RESEARCH   |     4 |
| MILLER | ACCOUNTING |     2 |
+--------+------------+-------+
```

```sql
案例：找出每一个员工的部门名称、工资等级、以及上级领导。

select 
  e.ename '员工',d.dname,s.grade,e1.ename '领导'
from
  emp e
join
  dept d
on
  e.deptno = d.deptno
join
  salgrade s
on
  e.sal between s.losal and s.hisal
left join
  emp e1
on
  e.mgr = e1.empno;

+--------+------------+-------+-------+
| 员工      | dname      | grade | 领导    |
+--------+------------+-------+-------+
| SMITH  | RESEARCH   |     1 | FORD  |
| ALLEN  | SALES      |     3 | BLAKE |
| SCOTT  | RESEARCH   |     4 | JONES |
| KING   | ACCOUNTING |     5 | NULL  |
| TURNER | SALES      |     3 | BLAKE |
+--------+------------+-------+-------+
```

### 子查询

子查询：select 语句当中嵌套 select 语句，被嵌套的 select 语句就是子查询。

使用位置：where，from，select

where 子句中使用子查询

---

```sql
案例：找出高于平均薪资的员工信息。
select * from emp where sal > avg(sal); //错误的写法，where后面不能直接使用分组函数。

第一步：找出平均薪资
  select avg(sal) from emp;
第二步：where过滤
  select * from emp where sal > 2073.214286;

第一步和第二步合并：
  select * from emp where sal > (select avg(sal) from emp);
```

from 后面嵌套子查询

```sql
案例：找出每个部门平均薪水的等级

第一步：找出每个部门平均薪水（按照部门编号分组，求sal的平均值）
select deptno,avg(sal) as avgsal from emp group by deptno;
+--------+-------------+
| deptno | avgsal      |
+--------+-------------+
|     10 | 2916.666667 |
|     20 | 2175.000000 |
|     30 | 1566.666667 |
+--------+-------------+
第二步：将以上的查询结果当做临时表t，让t表和salgrade s表连接，条件是：t.avgsal between s.losal and s.hisal
select 
  t.*,s.grade
from
  (select deptno,avg(sal) as avgsal from emp group by deptno) t
join
  salgrade s
on
  t.avgsal between s.losal and s.hisal;
+--------+-------------+-------+
| deptno | avgsal      | grade |
+--------+-------------+-------+
|     30 | 1566.666667 |     3 |
|     10 | 2916.666667 |     4 |
|     20 | 2175.000000 |     4 |
+--------+-------------+-------+
```

```sql
案例：找出每个部门平均的薪水等级。
第一步：找出每个员工的薪水等级。
select e.ename,e.sal,e.deptno,s.grade from emp e join salgrade s on e.sal between s.losal and s.hisal;
+--------+---------+--------+-------+
| ename  | sal     | deptno | grade |
+--------+---------+--------+-------+
| SMITH  |  800.00 |     20 |     1 |
| ALLEN  | 1600.00 |     30 |     3 |
| FORD   | 3000.00 |     20 |     4 |
| MILLER | 1300.00 |     10 |     2 |
+--------+---------+--------+-------+
第二步：基于以上结果，继续按照deptno分组，求grade平均值。
select 
  e.deptno,avg(s.grade)
from 
  emp e 
join 
  salgrade s 
on 
  e.sal between s.losal and s.hisal
group by
  e.deptno;
+--------+--------------+
| deptno | avg(s.grade) |
+--------+--------------+
|     10 |       3.6667 |
|     20 |       2.8000 |
|     30 |       2.5000 |
+--------+--------------+
```

select 后面嵌套子查询

```sql
案例：找出每个员工所在的部门名称，要求显示员工名和部门名。

select 
  e.ename,d.dname
from
  emp e
join
  dept d
on
  e.deptno = d.deptno;


select 
  e.ename,(select d.dname from dept d where e.deptno = d.deptno) as dname 
from 
  emp e;

+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ALLEN  | SALES      |
| ADAMS  | RESEARCH   |
| MILLER | ACCOUNTING |
+--------+------------+
```

### union

可以将查询结果集相加

```sql
案例：找出工作岗位是SALESMAN和MANAGER的员工

第一种：select ename,job from emp where job = 'MANAGER' or job = 'SALESMAN';
第二种：select ename,job from emp where job in('MANAGER','SALESMAN');
第三种：(union)
select ename,job from emp where job = 'MANAGER'
union
select ename,job from emp where job = 'SALESMAN';

union的独特之处：可以将两张不相干的表中的数据拼接在一起显示
select ename from emp
union
select dname from dept;

两个相加的查询的列数要一致
mysql> select ename,sal from emp
    -> union
    -> select dname from dept;
ERROR 1222 (21000): The used SELECT statements have a different number of columns
```

### limit (重点，分页查询基础)

limit 是 mysql 特有的，其他数据库中没有，不通用
（Oracle 中有一个相同的机制，叫做 rownum）

作用：取结果集中的部分数据

语法

```sql
limit startIndex, length
  startIndex表示起始位置，从0开始，0表示第一条数据。
  length表示取几个

案例：取出工资前5名的员工（思路：降序取前5个）
  select ename,sal from emp order by sal desc;
  取前5个：
    select ename,sal from emp order by sal desc limit 0, 5;
    select ename,sal from emp order by sal desc limit 5;
  
案例：找出工资排名在第4到第9名的员工？
  select ename,sal from emp order by sal desc limit 3,6;
```

limit 是 sql 语句最后执行的一个环节

```sql
select      5
  ...
from        1
  ...  
where       2
  ...  
group by    3
  ...
having      4
  ...
order by    6
  ...
limit       7
  ...;
```

通用的标准分页 sql

```sql
每页显示3条记录：
第1页：0, 3
第2页：3, 3
第3页：6, 3

每页显示pageSize条记录：
第pageNo页：(pageNo - 1) * pageSize, pageSize

pageSize（每页显示多少条记录）
pageNo（显示第几页）

java代码{
  int pageNo = 2; // 页码是2
  int pageSize = 10; // 每页显示10条
  limit (pageNo - 1) * pageSize, pageSize
}
```

## 数据操作语言（DML）

### 插入数据（insert）

```sql
要求
  字段的数量和值的数量相同，并且数据类型要对应相同
  表名后的括号可省略，此时后面的values顺序和数量要和表中相同
  字段名的顺序随意，与数据一一对应即可
  字段名可不写全，剩下的所有字段自动插入NULL
  可一次插入多行数据

insert into t_student(no,name,sex,classno,birth) values(1,'zhangsan','1','gaosan1ban','1950-10-12');
insert into t_student values(1,'zhangsan','1','gaosan1ban','1950-10-12');
insert into t_student(name,sex,classno,birth,no) values('lisi','1','gaosan1ban', '1950-10-12',2);
insert into t_student(name) values('wangwu');
insert into t_student
  (no,name,sex,classno,birth) 
values
  (3,'rose','1','gaosi2ban','1952-12-14'),(4,'laotie','1','gaosi2ban','1955-12-14');
```

```sql
// 将查询结果插入到一张表中（表结构要相同）
insert into 表名 select语句;
```

### 修改数据（update）

```sql
注意
  如果没有条件整张表数据全部更新
update 表名 set 字段名1=值1,字段名2=值2... where 条件;
```

### 删除数据（delete ）

```sql
注意
  没有条件全部删除
delete from 表名 where 条件;
```

```sql
// 删除大表中的数据，表被截断，不可回滚。永久丢失。
truncate table 表名;
```

## 数据定义语言（DDL）

实际开发中表一旦设计好之后，对表结构的修改是很少的，修改表结构就是对之前的设计进行了否定，即使需要修改表结构，我们也可以直接使用工具操作。
修改表结构的语句不会出现在 Java 代码当中。出现在 java 代码当中的 sql 包括：insert delete update select（这些都是表中的数据操作。）
增删改查有一个术语：CRUD 操作，Create（增） Retrieve（检索） Update（修改） Delete（删除）

### 创建表（create）

语法

```sql
create table 表名(
  字段名1 数据类型 default 值, // default指定默认值
  字段名2 数据类型,
  ....
);

表名在数据库当中一般建议以：t_或者tbl_开始。

创建学生表：
学生信息包括：
  学号、姓名、性别、班级编号、生日
  学号：bigint
  姓名：varchar
  性别：char
  班级编号：int
  生日：char

create table t_student(
  no bigint,
  name varchar(255),
  sex char(1) default 1, // 默认为1
  classno varchar(255),
  birth char(10)
);
```

```sql
// 将查询结果当做表创建出来
create table 表名 as select语句;
```

MySQL 中字段的常见数据类型

```sql
  int      整数型(java中的int)
  bigint   长整型(java中的long)
  float    浮点型(java中的float)
  double   浮点型(java中的double)
  char     定长字符串(java中的String)
  varchar  可变长字符串(java中的StringBuffer/StringBuilder)
  date     日期类型 （对应Java中的java.sql.Date类型）
  BLOB     二进制大对象（存储图片、视频等流媒体信息） Binary Large OBject （对应java中的Object）
  CLOB     字符大对象（存储较大文本，比如，可以存储4G的字符串） Character Large OBject（对应java中的Object）
  ......

char和varchar怎么选择
  在实际的开发中，当某个字段中的数据长度不发生改变的时候，是定长的，例如：性别、生日等都是采用char。
  当一个字段的数据长度不确定，例如：简介、姓名等都是采用varchar。
```

### 删除表（drop）

```sql
drop table 表名; // 这个通用。
drop table if exists 表名; // 当这个表存在的话删除，oracle不支持这种写法。
```

### 约束（Constraint）

约束：在创建表的时候，可以给表的字段添加相应的约束

目的：为了保证表中数据的合法性、有效性、完整性

常见约束

- 非空约束(not null)：约束的字段不能为 NULL
- 唯一约束(unique)：约束的字段具有唯一性，不能重复。但可以为 NULL。
- 主键约束(primary key)：约束的字段既不能为 NULL，也不能重复（简称 PK）
- 外键约束(foreign key)：...（简称 FK）
- 检查约束(check)：（Oracle 数据库有 check 约束，但是 mysql 没有，目前 mysql 不支持该约束）

#### 非空约束（not null）

```sql
create table t_user(
  id int,
  username varchar(255) not null,
  password varchar(255)
);
```

#### 唯一性约束（unique）

约束的字段具有唯一性，不能重复。但可以为 NULL。

```sql
create table t_user(
  id int,
  username varchar(255) unique  // 列级约束
);

create table t_user(
  id int, 
  usercode varchar(255),
  username varchar(255),
  unique(usercode,username) // 多个字段联合起来添加1个约束unique 【表级约束】
);

注意：not null约束只有列级约束。没有表级约束。
```

#### 主键约束（primary key）

约束的字段既不能为 NULL，也不能重复

一张表的主键约束只能有 1 个。（必须记住）

```sql
create table t_user(
  id int primary key,  // 列级约束
  username varchar(255),
  email varchar(255)
);

create table t_user(
  id int,
  username varchar(255),
  primary key(id) // 表级约束
);
 

create table t_user(
  id int,
  username varchar(255),
  password varchar(255),
  primary key(id,username) // 复合主键，不需要掌握
);
```

自增序列：auto_increment

删除所有数据不会改变其值，删除表再重建会重置

插入语句中手动定义后，下一个会按手动定义的值后增加

```sql
CREATE TABLE Users(
  userId int primary key auto_increment, #用户编号
  userName varchar(50),    #用户名称
  password varchar(50),    #用户密码
  sex      char(1),        #用户性别 '男' 或则 '女'
  email    varchar(50)     #用户邮箱
)
```

- 主键作用
表的设计三范式中有要求，第一范式就要求任何一张表都应该有主键。
主键的作用：主键值是这行记录在这张表当中的唯一标识。（就像一个人的身份证号码一样。）
- 主键的分类
根据主键字段的字段数量来划分
单一主键（推荐且常用）
复合主键（多个字段联合起来添加一个主键约束）（不建议使用，违背三范式）
根据主键性质来划分
自然主键：主键值最好就是一个和业务没有任何关系的自然数（推荐）
业务主键：主键值和系统的业务挂钩，例如：拿着银行卡的卡号做主键，拿着身份证号码作为主键（不推荐，因为业务发生改变，主键值可能也会随着发生变化，但有的时候没有办法变化，因为变化可能会导致主键值重复）

#### 外键约束（foreign key）

外键可以为 NULL

外键引用的字段不一定是主键，但至少具有 unique 约束。

删除数据、删除表的时候，先子后父
添加数据、创建表的时候，先父后子

```sql
t_student中的classno字段引用t_class表中的cno字段，此时t_student表叫做子表。t_class表叫做父表。

create table t_class(
  cno int,
  cname varchar(255),
  primary key(cno)
);

create table t_student(
  sno int,
  sname varchar(255),
  classno int,
  primary key(sno),
  foreign key(classno) references t_class(cno) // references引用
);
```

## 存储引擎（了解）

### 完整的建表语句（show create table 表名;）

```sql
CREATE TABLE `t_x` (
  `id` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

MySQL 中，凡是标识符是可以使用飘号括起来的（最好别用，不通用）
建表的时候可以指定存储引擎（默认 InnoDB），也可以指定字符集。（默认 UTF8）

### 存储引擎

存储引擎这个名字只有在 mysql 中存在

（Oracle 中有对应的机制，但是不叫做存储引擎。Oracle 中没有特殊的名字，就是“表的存储方式”）

mysql 支持很多存储引擎，5.5.36 有 9 种，每一个存储引擎都对应了一种不同的存储方式。
查看当前 mysql 支持的存储引擎：show engines \G

### 常见的存储引擎（使用默认就足够了）

```latex
Engine: MyISAM（最常用，但不是默认的）
  Support: YES
  Comment: MyISAM storage engine
  Transactions: NO
  XA: NO
  Savepoints: NO

MyISAM这种存储引擎不支持事务。
MyISAM采用三个文件组织一张表：
  xxx.frm（存储表的结构的文件）
  xxx.MYD（存储表中数据的文件）
  xxx.MYI（存储表中索引的文件）

优点：可被压缩，节省存储空间。并且可以转换为只读表，提高检索效率。
缺点：不支持事务。
```

```latex

Engine: InnoDB（默认）
  Support: DEFAULT
  Comment: Supports transactions, row-level locking, and foreign keys
  Transactions: YES
  XA: YES
  Savepoints: YES

表的结构存储在xxx.frm文件中
数据存储在tablespace这样的表空间中（逻辑概念），无法被压缩，无法转换成只读。
这种InnoDB存储引擎在MySQL数据库崩溃之后提供自动恢复机制。
InnoDB支持级联删除和级联更新。

优点：支持事务、行级锁、外键等。这种存储引擎数据的安全得到保障。
```

```latex
Engine: MEMORY（以前叫做HEPA引擎）（数据存储在内存中）
  Support: YES
  Comment: Hash based, stored in memory, useful for temporary tables
  Transactions: NO
  XA: NO
  Savepoints: NO

优点：查询速度最快
缺点：不支持事务。数据容易丢失。因为所有数据和索引都是存储在内存当中的。
```

## 事务（Transaction）

事务：一个完整的业务逻辑单元，不可再分（几个操作要么同时成功或同时失败）

事务的存在是为了保证数据的完整性，安全性。

只有 DML 语句（insert delete update）和事务有关，因为这三个语句都是和数据库表中的“数据”相关的。

事务的四大特性：ACID

- A：原子性：事务是最小的工作单元，不可再分。
- C：一致性：事务必须保证多条 DML 语句同时成功或者同时失败。
- I：隔离性：事务 A 与事务 B 之间具有隔离。
- D：持久性：持久性说的是最终数据必须持久化到硬盘文件中，事务才算成功的结束。

事务之间的隔离性，理论上隔离级别包括 4 个隔离级别

```latex
第一级别：读未提交（read uncommitted）
  对方事务还没有提交，我们当前事务可以读取到对方未提交的数据。
  问题：脏读（Dirty Read）现象：表示读到了脏的数据。
第二级别：读已提交（read committed）
  对方事务提交之后的数据我方可以读取到。（造成了我方一次事务中两次读取的数据不同）
  这种隔离级别解决了: 脏读现象没有了。
  问题：不可重复读。
第三级别：可重复读（repeatable read）
  这种隔离级别解决了：不可重复读问题。
  问题：读取到的数据是幻象。
第四级别：序列化读/串行化读（serializable） 
  解决了所有问题。
  问题：效率低。需要事务排队。

oracle默认的隔离级别是：读已提交（第二级别）
mysql默认的隔离级别是：可重复读（第三级别）
```

两个主要操作：commit，rollback（savepoint）

mysql 事务默认情况下是自动提交的（只要执行任意一条 DML 语句则提交一次）
提交之后不能回滚了
怎么关闭自动提交：start transaction;

悲观锁（行级锁）：select 语句最后加“for update”，事务期间锁定数据

乐观锁：给数据加版本号，视情况回滚

## 索引

在数据库方面，查询一张表的时候有两种检索方式：

- 第一种方式：全表扫描
- 第二种方式：根据索引检索（效率很高）

什么是索引？有什么用？

- 索引就相当于一本书的目录，通过目录可以快速的找到对应的资源。
- 索引提高检索效率的根本原因：缩小了扫描的范围
- 添加索引是给某一个字段（某些字段）添加索引。字段上没有索引时，sql 语句会进行全表扫描
- 索引虽然可以提高检索效率，但是不能随意的添加索引，因为索引也是数据库当中的对象，也需要数据库不断的维护，表中的数据经常被修改这样就不适合添加索引，因为数据一旦修改，索引需要重新排序
- 索引底层采用的数据结构是：B + Tree

创建，删除索引对象

```sql
# 创建
create index 索引名称 on 表名(字段名);
# 删除
drop index 索引名称 on 表名;
```

什么时候考虑给字段添加索引

- 数据量庞大。（根据客户的需求，根据线上的环境）
- 该字段很少的 DML 操作。（因为字段进行修改操作，索引也需要维护）
- 该字段经常出现在 where 子句中。（经常根据哪个字段查询）

注意

- 主键和具有唯一性约束（unique）的字段自动会添加索引。
- 根据主键查询效率较高。尽量根据主键检索。

查看 sql 语句的执行计划：explain  SQL 语句

```sql
mysql> explain select ename,sal from emp where sal = 5000;
+----+-------------+-------+------+---------------+------+---------+------+------+-------------+
| id | select_type | table | type | possible_keys | key  | key_len | ref  | rows | Extra       |
+----+-------------+-------+------+---------------+------+---------+------+------+-------------+
|  1 | SIMPLE      | emp   | ALL  | NULL          | NULL | NULL    | NULL |   14 | Using where |
+----+-------------+-------+------+---------------+------+---------+------+------+-------------+

给薪资sal字段添加索引：create index emp_sal_index on emp(sal);
（type项不同了）

mysql> explain select ename,sal from emp where sal = 5000;
+----+-------------+-------+------+---------------+---------------+---------+-------+------+-------------+
| id | select_type | table | type | possible_keys | key           | key_len | ref   | rows | Extra       |
+----+-------------+-------+------+---------------+---------------+---------+-------+------+-------------+
|  1 | SIMPLE      | emp   | ref  | emp_sal_index | emp_sal_index | 9       | const |    1 | Using where |
+----+-------------+-------+------+---------------+---------------+---------+-------+------+-------------+
```

索引的实现原理

- 通过 B Tree 缩小扫描范围，底层索引进行了排序，分区，索引会携带数据在表中的“物理地址”，最终通过索引检索到数据之后，获取到关联的物理地址，通过物理地址定位表中的数据，效率是最高的。
- select ename from emp where ename = 'SMITH';
通过索引转换为：select ename from emp where 物理地址 = 0x3;

索引的分类

- 单一索引：给单个字段添加索引
- 复合索引：给多个字段联合起来添加 1 个索引
- 主键索引：主键上会自动添加索引
- 唯一索引：有 unique 约束的字段上会自动添加索引
- ....

索引什么时候失效

- 模糊查询的时候，第一个通配符使用的是 %，这个时候索引是失效的。
select ename from emp where ename like '%A%';

## 视图(view)

是什么

- 站在不同的角度去看到数据。（同一张表的数据，通过不同的角度去看待）。

作用

- 视图可以隐藏表的实现细节
例如：保密级别较高的系统，数据库只对外提供相关的视图，程序员只对视图对象进行 CRUD。

创建，删除视图（只有 DQL 语句才能以视图对象的方式创建出来）

```sql
create view myview as select empno,ename from emp;
drop view myview;
```

对视图进行增删改查，会影响到原表数据，所以可以对视图进行 CRUD 操作
（通过视图影响原表数据的，不是直接操作的原表）

```sql
# CRUD操作与表相同
create view myview1 as select empno,ename,sal from emp_bak;
update myview1 set ename='hehe',sal=1 where empno = 7369; // 通过视图修改原表数据。
delete from myview1 where empno = 7369; // 通过视图删除原表数据。
```

面向视图操作

## DBA 命令

导出数据

```
在windows的dos命令窗口中执行：（导出整个库）
  mysqldump bjpowernode>D:\bjpowernode.sql -uroot -p333

在windows的dos命令窗口中执行：（导出指定数据库当中的指定表）
  mysqldump bjpowernode emp>D:\bjpowernode.sql -uroot –p123
```

导入数据

```
create database bjpowernode;
use bjpowernode;
source D:\bjpowernode.sql
```

## 数据库设计三范式（重点，面试经常问）

是什么

- 设计表的依据（按照这个三范式设计的表不会出现数据冗余）

三范式

- 第一范式：任何一张表都应该有主键，并且每一个字段原子性不可再分。
- 第二范式：建立在第一范式的基础之上，所有非主键字段完全依赖主键，不能产生部分依赖。
多对多（老师与学生）：三张表，“关系”表加两个外键
- 第三范式：建立在第二范式的基础之上，所有非主键字段直接依赖主键，不能产生传递依赖。
一对多（班级与学生）：两张表，“多”表加外键
（在实际的开发中，以满足客户的需求为主，有的时候会拿冗余换执行速度）

一对一怎么设计

```latex
一对一设计有两种方案：主键共享
  t_user_login  用户登录表
  id(pk)    username      password
  --------------------------------------
  1        zs          123
  2        ls          456

  t_user_detail 用户详细信息表
  id(pk+fk)  realname      tel      ....
  ------------------------------------------------
  1        张三        1111111111
  2        李四        1111415621

一对一设计有两种方案：外键唯一。
  t_user_login  用户登录表
  id(pk)    username      password
  --------------------------------------
  1        zs          123
  2        ls          456

  t_user_detail 用户详细信息表
  id(pk)     realname      tel        userid(fk+unique)....
  -----------------------------------------------------------
  1        张三        1111111111    2
  2        李四        1111415621    1
```
