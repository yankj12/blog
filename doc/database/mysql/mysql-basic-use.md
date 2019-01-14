# MySQL基础使用

## 导入导出

参考[mysql数据的导入导出](https://www.cnblogs.com/regit/p/8041762.html)

>mysqldump工具基本用法，不适用于大数据备份

### 使用 SELECT ...INTO OUTFILE ...命令来导出数据

具体语法如下。

```SQL
mysql> SELECT * FROM tablename INTO OUTFILE 'target_file' [option];
```

例子1，将 emp 表中数据导出为数据文本，其中，字段分隔符为“,”，每个字段用双引号引用起来，记录结束符为回车符(默认如此，可以不写)

```SQL
mysql> select * from emp into outfile '/tmp/emp.txt' fields terminated by "," enclosed by '"';
```

输出结果如下

```SQL
mysql> system more /tmp/emp.txt
"1","z1","aa"
"2","z1","aa"
"3","z1","aa"
```

例子2，发现例1中第一列是数值型，如果不希望字段两边用引号，则语句改为如下

```SQL
mysql> select * from emp into outfile '/tmp/emp.txt' fields terminated by ","  optionally enclosed by '"' ;
```

结果输出如下

```SQL
mysql> system more /tmp/emp.txt
1,"z1","aa"
2,"z1","aa"
3,"z1","aa"
```

### 使用“LOAD DATA INFILE…”命令导入数据

具体语法如下。

例子1：将文件“/tmp/emp.txt”中的数据加载到表 emp 中

```SQL
mysql> load data infile '/tmp/emp.txt' into table emp fields terminated by ',' enclosed by'"' ;
```

例子2：如果不希望加载文件中的前2行，加上ignore字段

```SQL
mysql> load data infile '/tmp/emp.txt' into table emp fields terminated by ','  enclosed by '"'  ignore 2 lines;
```

例子3：如果发现文件中的列顺序和表中的列顺序不符，或者只想加载部分列，可以在命令行中加上列的顺序

```SQL
mysql> load data infile '/tmp/emp.txt' into table emp fields terminated by ',' enclosed by '"' ignore 2 lines (id,content,name);
```

如果只想加载第一列，字段的列表里面可以只加第一列的名称：

```SQL
mysql> load data infile '/tmp/emp.txt' into table emp fields terminated by ',' enclosed by '"'  ignore 2 lines (id);
```
