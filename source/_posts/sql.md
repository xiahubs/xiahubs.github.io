---
title: SQL 语法汇总
tags: 
  - SQL
date: 2023-02-07
---

## 数据查询

* *SELECT* ：用于从数据库中选择数据

```sql
select * from table_name;
select top m * from tablename where id not in (select top n id from tablename);
```

* *DISTINCT*：过滤掉重复的值并返回指定列的行

```sql
select distinct  column_name;
```

* *WHERE* ：用于过滤记录/行

```sql
select column1,column2 from table_name where conditions;
select * from table_name where condition1  and condition2;
select * from table_name where condition1 or condition2;
select * from table_name where not condition;
select * from table_name where condition1 and (condition2 or condition3);
select * from table_name where exists(select column_name from table_name where condition);
```

* *ORDER BY* ：用于按升序或降序对结果集进行排序

```sql
select * from table_name order by column;
select * from table_name order by column desc;
select * from table_name order by column1 asc,column2 desc;
```

* *SELECT TOP* ：用于指定要从表顶部返回的记录数

```sql
select top count column_names from table_name where conditions;
select top column_names from table_name where conditions;
-- 并非所有的数据库系统都支持SELECT TOP. MySQL的等价物是LIMIT从句
select column_names from table_name LIMIT offset,entry;
```

* *LIKE* ：在 *WHERE* 子句中使用的运算符，用于在列中搜索特定的模式

```sql
-- %(百分号)是通配符，表示零个，一个或多个字符
-- _(下划线)是表示单个字符的通配符
select column_names from table_name where column_name like 模式;
like 'a%'-- (查找所有以“ a”开头的值)
like '%a'-- (查找所有以“ a”结尾的值)
like '%or%'-- (查找在任何位置带有“or”的任何值)
like '_r%'-- (找到第二个位置带有“ r”的值)
like 'a_%_%'-- (查找所有以“ a”开头且长度至少为3个字符的值)
like '[abc]%'-- (查找以“a”，“b”或“c”开头的任何值
```

* *IN* ：用于 *WHERE* 子句中指定多个值的运算符

```sql
-- 本质上，IN运算符是多个 OR 条件的简写
select column_names FROM table_name where column_name in(value1,value2,…);
select column_names FROM table_name where column_name in(select statement);
```

* *BETWEEN* ：运算符选择给定范围内(包括端值)的值

```sql
select column_names from table_name WHERE column_name betweed left_value and right_value;
select * from table_name where(column_name between value1 and value2) and not column_name2 in(value3,value4);
select * from table_name where column_name between #1/07/1999# and #03/12/1999#;
```

* *NULL* ：没有值的字段中的值

```sql
select * from table_name where column_names is null;
select * from table_name where column_names is not null;
```

* *AS* ：别名用于为表或列分配临时名称

```sql
select column_name as alias_name from table_name;
select column_name from table_name AS alias_table_name;
select column_name as alias_name1,column_name2 as alias_name2 from table_name;
select column_name1,column_name2 +','+ column_name3 as alias_name from table_name;
```

* *UNIONK* ：用于组合两个或多个 *SELECT*  语句的结果集（并集）

```sql
-- UNION中的每个SELECT语句必须具有相同的列数
-- 这些列必须具有相似的数据类型
-- 每个SELECT语句中的列也必须具有相同的顺序
-- UNION运算符仅选择不同的值，UNION ALL将允许重复项
select columns_name from left_table union select columns_name from right_table;
```

* *INTERSECT* ：用于返回两个 *SELECT* 语句共有的记录（交集）

```sql
-- 通常使用与上面的UNION相同的方式
select columns_names from left_table intersect select columns_names from right_table;
```
* *EXCEPT* ：用于返回后一条 *SELECT*    语句中未找到的前一 *SELECT* 语句中的所有记录（差集）

```sql
-- 通常使用与上面的UNION相同的方式
select columns_names from left_table except select columns_names from right_table;
```

* *ANY | ALL* ：用于检查 *WHERE* 或 *HAVING* 子句中使用的子查询条件的运算符

```sql
-- ANY：如有子查询值满足条件运算符返回true
-- ALL：如果所有子查询值满足条件运算符返回true
select columns_names from table where table.column <>≠ (any|all)(select columns from table_name where conditions);
```

* *GROUP BY* ：语句通常与聚合函数( *COUNT，MAX，MIN，SUM，AVG* )一起使用，以将结果集按一列或多列分组

```sql
select column_name1,count(column_name2) from table_name having conditions group by column_name1 order by count(column_name2) desc;
```

* *HAVING* ：此子句已添加到 *SQL*，因为***WHERE* 关键字不能与聚合函数一起使用**

```sql
select count(column_name1),column_name2 from table_name2 group by column_name2 having count(column_name1 ) > 5;
```

* *WITH* ：通常用于检索层次结构数据或在查询中多次重用临时结果集。也称为“公用表达式”

```sql
with RECURSIVECTE as(
  select * from as C0 where conditions
  union all
  select * from as C1 join CTE on c1.parent_category_id = cte.id
)
select * from CTE
```

## 函数查询

```sql
select count(distinct column_name);-- COUNT(): 返回出现的次数
select min(column_names) from table_name [having conditions];-- MIN(): 返回所选列的最小值
select max(column_names) from table_name [having conditions];-- MAX(): 返回所选列的最大值
select avg(column_name) from table_name [having conditions];-- AVG(): 返回数字列的平均值
select sum(column_name) from table_name [having conditions];-- SUM(): 返回数字列的总和
select first(column_name) from table_name [having conditions];-- FIRST(): 返回第一个记录的值
select last(column_name) from table_name [having conditions];-- LAST(): 返回最后一个记录的值
select ucase(column_name) from table_name [having conditions];-- UCASE(): 转换该列为大写
select lcase(column_name) from table_name [having conditions];-- LCASE(): 转换该列为小写
```

## 时间运算

* *NOW()*、*CURDATE()*、*CURTIME()*

```sql
select NOW(),CURDATE(),CURTIME()-- 返回当前的日期和时间、返回当前的日期、返回当前的时间
```

* *DATE(date)* ：返回日期或日期/时间表达式的日期部分。

* *EXTRACT()* ： 函数用于返回日期/时间的单独部分

```sql
select extract(unit from date);
-- unit可取值：
-- MICROSECOND、SECOND、MINUTE、HOUR、DAY、WEEK、MONTH、QUARTER、YEAR、SECOND_MICROSECOND、MINUTE_MICROSECOND、MINUTE_SECOND、HOUR_MICROSECOND、HOUR_SECOND、HOUR_MINUTE、DAY_MICROSECOND、DAY_SECOND、DAY_MINUTE、DAY_HOUR、YEAR_MONTH
```

* *DATE_ADD()* ：向日期添加指定的时间间隔

```sql
select columns,DATE_ADD(data_column_name,INTERVAL 2 DAY) from table_name;
```

* *DATE_SUB()* ：向日期减去指定的时间间隔

```sql
select columns,DATE_SUB(data_column_name,INTERVAL 2 DAY) from table_name;
```

* *DATEDIFF()* ：计算时间差

```sql
select DATEDIFF('2008-12-30','2008-12-29') as DiffDate
```

## 连接查询

* *INNER JOIN*：返回两个表中具有匹配值的记录

```sql
select column_names from left_table inner join right_table on left_table.column_name = right_table.column_name;
select column_names from ((table1 inner join table2 on relationship) inner join table3 on relationship);
```

* *LEFT(OUTER) JOIN*：返回左侧表(表1)中的所有记录，以及右侧表(表2)中的匹配记录

```sql 
select column_names from left_table left join right_table on left_table.column_name = right_table.column_name;
```

* *RIGHT(OUTER) JOIN*：从右表(表2)返回所有记录，并从左表(表1)返回匹配的记录

```sql
select column_names from left_table right join right_table on left_table.column_name = right_table.column_name;
```

* *FULL(OUTER) JOIN*：当左表或右表中有匹配项时，返回所有记录

```sql
select column_names from left_table full outer join right_table on left_table.column_name = right_table.column_name;
```

* *Self JOIN*：常规联接，但是表与其自身的联接

```sql
select column_names from table1 T1, table1 t2 where conditions;
```

## 视图查询
> 视图是一张由查询内容定义的虚拟表，同真实表一样，视图中包含系列带有名称的列和行数据.但是它并不在数据库中以存储的数据值集形式存在.行和列数据来自由定义视图的查询所引用的表,并且在引用视图时动态生成. 视图在数据库中不存储数据值,即视图所查数据集不占空间.只在系统表中存储对关于视图的定义。

* *CREATE* ：创建视图

```sql
create view view_name as select columns_names from table_name where conditions;
```

* *SELECT* ：检索视图

```sql
select * from view_name
```

* *DROP* ：删除视图

```sql
drop view view_name;
```

## 数据修改

* *INSERT INTO* ：用于在表中插入新记录/行

```sql
insert into table_name(columns_name1,columns_name2,...,columns_namen) values(value1,value2,...valuen);
insert into table_name values(value1,value2,…);
```

* *UPDATE* ：用于修改表中的现有记录

```sql
update table_name set column_name = value;
update table_name set column1 = value1, column2 = value2 where conditions;
```

* *DELETE* ：用于删除表中的现有记录/行

```sql
delete * from table_name;
delete from table_name where conditions;
```

## 创建查询

* *CREATE DATABASE* ：创建数据库

```sql
create database database_name
-- 数据文件
on primary
(
    name = logic_name,-- 逻辑文件名
    filename = 'filepath\{.mdf}',-- 物理地址
    size = 5mb,-- 初始文件大小
    maxsize = 15mb,-- 最大容量
    filegrowth = 20%-- 递增长量
)
-- 日志文件
log on
(
    name = logic_name,-- 逻辑文件名
    filename = 'filepath\{.ldf}',-- 物理地址
    size = 5mb,-- 初始文件大小
    maxsize = 10mb,-- 最大容量
    filegrowth = 1mb-- 递增长量
);
```

* *DROP* ：删除数据库

```sql
drop database database_name;
```

* *RENAME* ：数据库重命名

> 在 *SQL Server* 中只有 *sysadmin* 和 *dbcreator* 固定服务器角色的成员才能执行 *sp_renamedb*


```sql
exec sp_renamedb 'source name', 'target';
```

* 向数据库中添加文件组和文件

```sql
-- 添加文件组
alter database source_database
add filegroup target_database_config_file
-- 向文件组中添加数据文件
alter database source_database
add file
(
	name = 'logic_name',
	filename='filepath\{.ndf}',
	size = 1mb,
	maxsize = 10mb,
	filegrowth = 1mb
),
(
	name = 'logic_name',
	filename='filepath\{.ndf}',
	size = 1mb,
	maxsize = 10mb,
	filegrowth = 1mb
)
to filegroup target_database_config_file
```

* 修改数据库文件

```sql
-- 修改文件初始大小
alter database students
modify file
(
	name = modify_file_name,
	size = 15mb
);
-- 删除一个数据文件
alter database database_name
remove file remove_file;
```

* 查询数据库信息

```sql
sp_helpdb;-- 查询全部数据库信息
sp_helpdb 'database_name';-- 查询某一具体数据库信息
```

* 删除数据库

```sql
drop database database_name;
```

* 创建表结构

```sql
create table table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
    column4 datatype,
    ...
);
```

* 删除表结构 ：整表删除

```sql
drop table table_name;
```

* 添加表结构 ：添加一列

```sql
alter table table_name add column_name column_definition;
```

* 修改表结构：更改列的数据类型

```sql
alter table table_name modify column_name column_type;
```

* 删除表结构 ：删除其中某一列

```sql
alter table table_name drop column column_name;
```

## [索引](https://blog.csdn.net/happyheng/article/details/53143345)

> 索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，于此可以大大加快查询速度，但如果创建了不合适的索引，可能还会降低查询效率。

* 创建索引：依据标识的某一指定列创建索引

```sql
create index index_name on table_name(column_name);
create unique index index_name on table_name(columns_names);
alter table table_name add index table_name(column_names);
```

* 删除索引

```sql
drop index index_name on table_name;
alter table table_name drop index index_name;
```

* 索引查询

```sql
show index from table_name;
show keys from table_name;
```

* 索引类型

在创建索引时，可以规定索引能否包含重复值。如果不包含，则索引应该创建为 *PRIMARY KEY* 或 *UNIQUE* 索引。对于单列惟一性索引，这保证单列不包含重复的值。对于多列惟一性索引，保证多个值的组合不重复。
*PRIMARY KEY* 索引和 *UNIQUE* 索引非常类似。事实上，*PRIMARY KEY* 索引仅是一个具有名称 *PRIMARY* 的 *UNIQUE* 索引。这表示一个表只能包含一个 *PRIMARY KEY*，因为一个表中不可能具有两个同名的索引。

## [存储过程](https://blog.csdn.net/tojohnonly/article/details/70738629)

* 存储过程的优点：可以类比程序语言中的函数

1. 存储过程只在创造时进行编译，以后每次执行存储过程都不需再重新编译，而一般 *SQL* 语句每执行一次就编译一次,所以使用存储过程可提高数据库执行速度，效率要比 *T-SQL* 语句高。
2. 当对数据库进行复杂操作时，可将此复杂操作用存储过程封装起来与数据库提供的事务处理结合一起使用。
3. 一个存储过程在程序在网络中交互时可以替代大堆的 *T-SQL* 语句，所以也能降低网络的通信量，提高通信速率。
4. 存储过程可以重复使用，可减少数据库开发人员的工作量。
5. 安全性高，可设定针对特定某些用户才具有对指定存储过程的使用权限


* 创建存储过程

```sql
create proc[EDURE] procedure_name
	#begin global
	@sname varchar(100),
	@IsRight int  output-- 传出参数
	[...]
	#end global
as
	#begin local
	declare @sname varchar(100)
	set @sname='tempsname'
	#end local
	
	#begin sql_statement
	if exists (select S#,Sname,Sage,Ssex from student where sname=@sname)
		set @IsRight =1
	else
		set @IsRight=0
	#end sql_statement
go
```

* 修改删除

```sql
alter proc[EDURE] [dbo].[PROC_SEINFO]
as
begin
	-- sql_statement
	select * from tb_Employee where empAge > 35  
end
```

* 删除存储过程

```sql
drop procedure procedure_name if exists procedure_name;-- 在存储过程中可以调用另外一个存储过程，而不能删除另外一个存储过程
```

* 调用存储过程

```sql
exec procedure_name ''-- 存储过程如果有参数，后面加参数格式为：@参数名=value，也可直接为参数值value

declare @IsRight int 
exec StuProc 'tempsname' , @IsRight output
select @IsRight
-- 调用方式
-- 其中参数列表中的参数有 input 和 output 两种基本类型，input表示传入，output表示返回，存储过程中允许返回多个值。
```

## [触发器](https://www.cnblogs.com/wangprince2017/p/7827091.html)
> 触发器是指在没有外键索引的情况下实现业务上的逻辑闭环

* 创建触发器

```sql
create trigger TriggerName
on table_name –-触发对象（在修改某张表、视图等时的操作）
for [update|delete|insert] -–触发事件（修改、删除、新增）
as –-触发后的执行行为
if update(StudentID)
begin
	update BorrowRecord
	set StudentID = i.StudentID
	from BorrowRecord br , Deleted d ,Inserted i-- Deleted和Inserted临时表
	where br.StudentID = d.StudentID
end
```

可以触发触发器的事件有:
**新增** 存放新增的记录 不存储记录
**修改** 存放用来更新的新记录 存放更新前的记录
**删除** 不存储记录 存放被删除的记录

触发器执行过程中会有两张系统生成的临时表：*[INSERTED]*、*[DELETED]* 为系统表，不可创建、修改、删除，但可以调用。

* 修改触发器

```sql
alter trigger TriggerName
on table_name –-触发对象（在修改某张表、视图等时的操作）
for [UPDAET|DELETE|INSERT] -–触发事件（修改、删除、新增）
as –-触发后的执行行为
if Update(StudentID)
begin
	update BorrowRecord
	set StudentID = i.StudentID
	from BorrowRecord br , Deleted d ,Inserted i-- Deleted和Inserted临时表
	where br.StudentID = d.StudentID
end
```

* 删除触发器

```sql
drop trigger TriggerName;
```

## [事务](https://blog.csdn.net/laizhixue/article/details/100729016)
> 如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。以此保证数据的前后一致性.当然事务还有原子性、隔离性、持久性的特性。

* 创建事务

```sql
begin tran-- 开启事务，transcation 的简写
declare @errorNo int-- 定义变量，用于记录事务执行过程中的错误次数
set @errorNo=0
begin try
	#begin sql_statement
    update Student set C_S_Id='2' where S_StuNo='003'
    set @errorNo = @errorNo + @@ERROR-- @@ERROR 系统全局变量，记录错误次数，出现一次错误 @@ERROR 值  +1
    select S_StuNo=003
    update Student set C_S_Id='3' where S_StuNo='002' 
    set @errorNo = @errorNo + @@ERROR-- @@ERROR 系统全局变量，记录错误次数，出现一次错误 @@ERROR 值  +1
    select 'S_StuNo=002 已经修改啦'
    #end sql_statement
    if(@errorNo>0)
    begin
        -- 抛出自定义的异常,在最后的catch块中统一处理异常
        raiserror(233333,15,3)
    end
end try

begin catch
    select ERROR_NUMBER() errorNumber,-- 错误代码
    ERROR_SEVERITY() errorSeverity,-- 错误严重级别，级别小于10 try catch 捕获不到
    ERROR_STATE() errorState,-- 错误状态码
    ERROR_PROCEDURE() errorProcedure,-- 出现错误的存储过程或触发器的名称
    ERROR_LINE() errorLine,-- 发生错误的行号
    ERROR_MESSAGE() errorMessage-- 错误的具体信息
    if(@@trancount>0)-- @@trancount 系统全局变量，事务开启 @@trancount 值+1，判断事务是否开启
    begin
    	rollback tran;-- 回滚撤销事务
    enD
end catch

if(@@trancount>0)
begin
    commit tran;-- 提交执行事务
end
```

## [游标](http://c.biancheng.net/view/7823.html)
> 要用于T_SQL脚本，存储过程和触发器中使用，并按行逻辑或向下、或循环取出查询结果集中的每一行数据并处理。有一定数据量的限制，大数据量处理会占用大量系统级资源产生消耗。

* 游标的执行逻辑

> 1、定义游标
> 2、打开游标
> 3、使用游标
> 4、关闭游标
> 5、释放游标

```sql
create procedure procedure_name ([参数列表…])
begin
	declare cursor_name cursor [ local | global]
						-- *cursor_name*：是所定义的 *t_sql* 服务中游标的名称;
						-- *local*：对于在其中创建批处理、存储过程或触发器来说，该游标的作用域是局部的;
						-- *global*：指定该游标的作用域是全局的;
	[ forward_only | scroll ]
	-- *forward_only*：指定游标只能从第一行滚动到最后一行。*fetch next* 是唯一支持的提取选项，如果在指定 *forward_only* 时不指定 *static*、*keyset* 和 *dynamic* 关键字，则游标作为 *dynamic* 游标进行操作，如果 *forward_only* 和 *scroll* 均为指定，则除非指定 *static*，*keyset* 为 *dynamic* 关键字，否则默认为 *forward_only*。*static*，*keyset* 和 *dynamic* 游标默认为 *scroll*。与 *odbc* 和 *ado* 这类数据库 *api* 不同，*static*，*keyset* 和 *dynamic* *t_sql* 游标支持 *forward_only*;
	[ static | keyset | dynamic | fast_forward ]
	-- *static* ：定义一个游标，以创建一个游标使用的数据临时复本，对游标的所有请求都从 *tempdb* 中的这一临时表中得不到应答；因此，在对该游标进行提取操作时返回的数据中不反映对基表所做的修改，并且该游标不允许修改;
	-- *keyset* :指定当游标打开时，游标重的行的成员身份和顺序已经固定。对行进行唯一标识的键值内置在tempdb内一个称为 *keyset* 的表中;
	-- *dynamic* ：定义一个游标，以反映在滚动游标时对结果集内的各行所做的所有数据更改。行的数据值、顺序和成员身份在每次提取时都会更改，动态游标不支持*absolute*提取选项;
	-- *fast_forward*：指定启动了性能优化的*forward_only*、*read_only*游标。如果指定了*scroll*或*for_update*,则不能指定*fast_forward*;
	[ read_only | scroll_locks | optimistic ]
	-- *scroll_locks*：指定通过游标进行的定位更新或删除一定会成功。将行读入游标时*sql server*将锁定这些行，以确保随后可对它们进行修改，如果还指定了*fast_forward*或*static*，则不能指定*scroll_locks*;
	-- *optimistic*：指定如果行自读入游标以来已得到更新，则通过游标进行的定位更新或定位删除不成功。当将行读入游标时，*sql server*不锁定行，它改用*timestamp*列值比较结果来确定行读入游标后是否发生了修改，如果表不包含*timestamp*列，它改用校验和值进行确定，如果以修改该行，则尝试进行的定位更新或删除将失败，如果还指定了*fast_forward*，则不能指定*optimistic*;
	[ type_warning ]
	-- *type_warning*:指定游标从所请求的类型隐式转换为另一种类型时，向客户端发送警告消息;
	for select_statement -- *select_statement*：是定义游标结果集中的标准*select*语句;
		[ for update [ of column_name [,...n] ] ]
		open cursor_name
		exex update /delete
		close cursor_name
end;
```

## [数据库的备份与还原](https://blog.csdn.net/mqmmx/article/details/65659003)

> 数据库的备份与还原操作均可以在SSMS上操作实现。
* 备份

```sql
backup database [database_name]
to disk = n'filenamewithpath.bak'  with noformat, noinit,
name = n'filename', skip, norewind, nounload,  stats = 10 go
```

* 还原

```sql
use [master]
restore database [database_name] 
from disk = n'storefilepath.bak' with  file = 1,  nounload,  stats = 5
go
```

## 数据库灵活查询

* 查询非正常排序中，当前数据的前一条和后一条

```sql
select * from table_name where id IN( (select id from table_name where id < id order by id desc limit 1),id,(select id from table_name where id > id order by id limit 1) ) order by id;
```

## 数据库的优化

1. 建立索引，优先考虑 *where* 、*group by* 使用到的字段
2. 尽量避免使用整表全查（全字段查询），返回无用的字段会降低（*CPU*、*IO* 、内存、网络带宽）效率。因此，使用具体的字段，只返回需要的字段
3. 尽量避免使用 *in* 、*or* 和 *not in* ，会导致数据库引擎**放弃索引**进行全表扫描
4. 如果是连续数值区间，可以用 *between* 代替，如果是子查询，可以用 *exists* 代替
5. *like* 子句尽可能避免在字段开头模糊查询，会导致数据库引擎放弃索引进行全表扫描。尽量在字段后面使用模糊查询
6. 尽量避免进行 *null* 值的判断，会导致数据库引擎放弃索引进行全表扫描。可以给字段添加默认值，对默认值进行判断比较
7. 尽量避免在 *where* 条件中的左侧进行表达式、函数操作，会导致数据库引擎放弃索引进行全表扫描
8. 数据量大时，避免使用 *where 1=1* 的条件。通常为了方便拼装查询条件，我们会默认使用该条件，数据库引擎会放弃索引进行全表扫描。用代码拼装 *sql* 时进行判断，没 *where* 加 *where*，有 *where* 加 *and*
9. *count()* 与 *count(column)* 的区别是 *count(columns)* 统计字段非 *null* 的行数。 *count(1)* 与 *count()* 都是统计 *Primary Key*，效率一样，如果没有 *Primary Key*，全表扫描.如果 *count(column)* ，其中 *column* 为索引，*count(column)* 和 *count(1)* 一样快，否则 *count(column)* 走全表扫描
10. *order by* 有序索引 *extra* 显示 *using Index* 的话，*MySQL* 有序返回，效率非常高。返回数据排序，*Using file sort*，使用排序算法在 *sort_buffer_size* 系统变量设置的内存排序区中进行排序，内存磁盘并用，然后合并结果。*file sort* 优化，减少额外排序，创建合适索引，全字段查询改成指定的字段，减少排序区的使用
11. *group by* 默认加了 *order by* ,不用额外的 *order by* 。*group by* 如果不关系排序，可以 *order by null* 禁止排序降低排序消耗
12. *limit* 语句指定的范围小可以先取出 *key* 值再去查全部字段, 避免查太多数据
13. 不要使用 *order by rand()*。这个不是分组，只是排序，*rand()* 生成一个随机数。*order by rand()*  ,每次检索的结果排序会不同
14. 使用合理的分页和分段的方式以提高查询的效率
15. 连表查询时用小表驱动大表查询以提高效率
