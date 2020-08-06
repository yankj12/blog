# postgres SQL性能优化

项目在使用postgres过程中，对性能有要求，其他中间件优化之外，也需要考虑SQL方面的优化。

## postgresql中的各种scan的比较

参考[postgresql中的各种scan的比较](https://www.cnblogs.com/flying-tiger/p/6702796.html)

## PostgreSQL中的位图索引扫描

PostgreSQL中的位图索引扫描（bitmap index scan）
参考[PostgreSQL中的位图索引扫描](https://www.cnblogs.com/wy123/p/13376991.html)

## 时间字段是否适合建立索引

[时间字段是否适合建立索引](https://blog.csdn.net/hello_sgw/article/details/78601762)

时间字段是否适合建索引？

可以建立索引的；至于建立聚集索引或者是非聚集索引，那要看你这个时间字段的具体情况以及使用或变更频繁程度。

一般来说，适合建立聚集索引的要求：“既不能绝大多数都相同，又不能只有极少数相同”的规则。

先说说一个误区：有人认为：只要建立索引就能显著提高查询速度。这个想法是很错误的。建立非聚集索引，确实，一般情况下可以提高速度，但是一般并不会达到你想要的速度。只有在适当的列建立适当的（聚集）索引，才能达到满意的效果。

下面的表总结了何时使用聚集索引或非聚集索引（很重要！） 

```text
动作描述             使用聚集索引   使用非聚集索引 
列经常被分组排序     应             应 
返回某范围内的数据   应             不应 
一个或极少不同值     不应           不应 
小数目的不同值       应             不应 
大数目的不同值       不应           应 
频繁更新的列         不应           应 
外键列               应             应 
主键列               应             应 
频繁修改索引列       不应           应 
```

## PostgreSQL 语句调优

参考[PostgreSQL 语句调优](https://blog.csdn.net/Jamel_LiToo/article/details/85332327)

### PostgreSQL 语句调优-Sql语句优化

```
作为一名优秀的码农，对于了解Sql如何调优是很有必要的。。简单总结一下，
	
	1.对查询进行优化，应尽量避免全表扫描，首先应考虑在 where 及 order by 涉及的列上建立索引(单表索引不能超过六个)
	2.使用更多的条件，缩小查找范围
	3.使用关联时，用小结果集驱动大结果集

(ps:下面的语句推荐给司机们，赶快上车)
--EXPLAIN：表示打出某sql的执行计划，看看是否走了全表，ANALYZE： 表示需要消耗的时间耗时，
EXPLAIN ANALYZE SELECT id,a FROM A; 
```

下面是我日常用到的一些Sql优化点，大家可以借鉴一下

#### 1. 查询字段优化 千万不要使用 SELECT * 用具体的字段列表替换 * ，不要返回用不到的字段

>返回了不必有的数据，就会浪费内存，加重网络的负担降低性能 。如果表大，在表扫描的期间将表锁住，禁止其他的链接访问表，后果严重！！

```SQL
--correct SQL
SELECT * FROM A; 
--error SQL
SELECT id FROM A WHERE create_time >'2019-1-1';
```

#### 2. where子句 like调优

>若在关键词abc前面用了“%”，会导致该Sql走全表查询，除非必要，否则不要在关键词前加%
ps: 查询耗时和字段值总长度成正比

```
--error SQL 
SELECT id FROM A WHERE name LIKE '%abc%'；
--correct SQL
SELECT id FROM A WHERE name LIKE 'abc%'；
```

#### 3. where子句 避免对null做判断

>该判断将导致引擎放弃使用索引而进行全表扫描，建议针对null字段设置默认值0

```
--error SQL 
SELECT id FROM A WHERE a ISNULL；
SELECT id FROM A WHERE a NOTNULL；
--correct SQL 可以在a上设置默认值0，确保表中a列没有null值
SELECT id FROM A WHERE a =0；
SELECT id FROM A WHERE a >0；
```

#### 4. where子句 避免使用 != 或者 <>

>该判断将导致引擎放弃使用索引而进行全表扫描，建议将不等于 拆成 大于或者小于

```SQL
--error SQL 
SELECT id FROM A WHERE a !=2017；
--correct SQL 
SELECT id FROM A WHERE a >2017 OR a <2017；
```

#### 5. where子句 避免使用 or

>使用or的子句可以分解成多个查询，并且通过union链接多个查询。它们的速度只同是否使用索引有关，如果查询使用到联合索引，用unionAll执行的效率更高，多个or字段的字句没有用到索引，改写成union的形式，再视图与索引匹配

```SQL
--error SQL 
SELECT id FROM A WHERE a >2017 OR a <2017；
--correct SQL 
SELECT id FROM A WHERE a >2017
UNION ALL
SELECT id FROM A WHERE a <2017；
```

#### 6. where子句 避免使用 NOT IN 或者 IN

>NOT IN sql执行时，会转成 <> 将导致引擎放弃使用索引而进行全表扫描，不推荐使用NOT IN
>IN 也会使系统无法使用索引，而只能直接搜索表中的数据（ps：如果一定要使用in 注意在in后面值的列表中，将出现最频繁的值放在最前面，出现的最少的放在最后面，减少判断的次数）

```SQL
--error SQL 
SELECT id FROM A WHERE a IN (2017,2018,2019)；
SELECT id FROM A WHERE a IN (SELECT id FROM B)；
--correct SQL  如果查询的是连续的值，可以使用BETWEEN AND 函数
SELECT id FROM A WHERE a BETWEEN 2017 AND 2019
--correct SQL  如果只是IN中的子表结果集比较大，建议使用 EXISTS
SELECT id FROM A WHERE EXISTS (SELECT 1 FROM B WHERE B.id=a)
```

#### 7. where子句 EXISTS 和 IN 的使用方式

>IN 是在内存中比较的，只执行一次，把B表中的所有id字段缓存起来，之后检查A表的id是否与B表中的id相等，如果id相等则将A表的记录加入到结果集中，直到遍历完A表的所有记录
>EXISTS 需要查询数据库，所以当B的数据量比较大时，EXISTS效率优于IN

```SQL
--error SQL  
SELECT id FROM A WHERE a IN (SELECT id FROM B)；
--correct SQL  
SELECT id FROM A WHERE EXISTS (SELECT 1 FROM B WHERE B.id=a)
--correct SQL  如果只是IN子表查询结果，建议使用 EXISTS
SELECT id FROM A WHERE EXISTS (SELECT 1 FROM B WHERE B.id=a)
```

#### 8. where子句 避免在条件左侧使用算法

>在where子句中的“=”左边进行函数、算数运算或其他表达式运算，系统可能无法正确的使用索引

```SQL
--error SQL 
SELECT * FROM A WHERE a/2=100;
SELECT * FROM A WHERE SUBSTRING(a,1,4)=’6666’;
--correct SQL  
SELECT * FROM A WHERE a=100*2;
SELECT * FROM A WHERE a LIKE ’6666%’;
```

#### 9. 避免使用 DISTINCT 和 ORDER BY

>它会使查询变慢，这些动作可以改在客户端执行也可以

#### 10. GROUP BY 和 HAVING 的优化

>如果能在group by的having字句之前就能剔除多余的行，所以尽量不要用他们来做剔除行的动作。最优执行顺序：select 的where字句选择所有合适的行，group用来分组统计，having用于剔除多余的分组。这样group by和having的开销小，查询快。对于大的数据进行分组和having十分消耗资源。如果group by的目的不包括计算，只是分组。Distinct更快

#### 11. INNER JOIN 比 LEFT JOIN和RIGHT JOIN快

>因为inner join是等值连接，或许返回的行数比较少.提倡使用内联INNER JOIN

#### 12. UNION ALL 比 UNION 快

>UNION在进行表链接后会筛选掉重复的记录，UNION ALL不会去除重复记录
>UNION将会按照字段的顺序进行排序，UNION ALL只是简单的将两个结果合并后就返回

#### 13. 一次更新多条记录比分多次更新每次一条快

>意思就是使用批处理更有效率

```SQL
--error SQL 
INSERT INTO A(id,a) VALUES (1,10);
INSERT INTO A(id,a) VALUES (2,16);
--correct SQL  
INSERT INTO A(id,a) VALUES (1,10),(2,16);
```

## postgres中管理命令

### postgresql 查看数据库连接数

查看所有连接的用户：

```SQL
select * from pg_stat_activity;
```

查看连接总数：

```SQL
select count(*) from pg_stat_activity;
```

