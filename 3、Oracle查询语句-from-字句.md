---
title: 3、Oracle查询语句 from 字句
date: 2017-11-01 20:19:58
tags: Oracle
---

1、如何从表中查询一个字段对应的内容
	
``` bash

select 字段名 from 表名;

```

查询 s_emp 表中 每个员工的月薪

``` bash

select salary from s_emp;

```

2、如何查询多个字段对应的内容

``` bash

select 字段名1,字段名2,字段名3 from 表名;

```

查询 s_emp 表中 每个员工的id,first_name,salary

``` bash

select id,first_name,salary from s_emp;

```

	限制某个字段名的这一列 一行最多显示21个字符
	
	col first_name for a21;

3、查询 s_emp 表中所有的字段对应的内容

``` bash

select id,first_name,last_name,hire_date,title,comments,salary,commission_pct from s_emp;
SQL>edit
SQL>/

```

	* 号可以代表所有的字段名 但是比较耗性能
	
``` bash

select * from s_emp;

```

4、sql 中的数学运算

``` bash

+  -  *  /
select  salary,salary+1000 from  s_emp; // 加薪1000
select  salary,salary*6  from  s_emp;   //计算半年薪
这里没有取整特性 
select  salary,salary/22 from s_emp;	//计算每个工作日收入

```
	
	练习：
	显示s_emp表中每个人的 月薪 和 对应的年薪
	select salary,salary*13 from s_emp;

---

	字段 和 表达式 别名
	只要在 字段 或 表达式的后面 加 空格 别名 即可
	
	注意： 每个字段或表达式的别名只能有一个,如
	
	select salary sal,salary*13  yearsal from s_emp;
	
	如何解决带有空格的别名,加双引号

	select salary "sAl",salary*13 "year   sal" from s_emp;

	单引号在sql中的意思是字符串

	'' ' ' 'a' 'hello world'

5、	字符串的连接

	Oracle 中字符串的拼接 使用 || ,如
	字符串 || 字符串 || 字符串

显示 s_emp 表中的 姓名

``` bash

select  first_name,last_name  from s_emp;

select  first_name  || last_name  from s_emp;

```

在名 和 姓之间 拼接一个下划线字符串

``` bash

select first_name ||'_'||last_name from s_emp;

```

在名 和 姓之间拼接一个单引号字符串
这里用到了转义思想 需要使用两个单引号代表单引号

``` bash

select  first_name  ||''''||last_name  from s_emp;

```

在名 和 姓直接 拼接两个单引号字符串

``` bash

select  first_name  ||''''''||last_name  from s_emp;

select  first_name  ||''''||''''||last_name  from s_emp;

```

6、NULL 值的处理 
	
    为什么要进行NULL值处理?
    因为 NULL 值和任何值做运算结果都是NULL
	
	比如有的人有commission_pct（提成）,有的人没有，现在我们要给所有人的提成增加20,那么没有提成的人就会很受伤了
	select  commission_pct,commission_pct+20 from  s_emp;

	在比如我们要计算 月薪乘以 12  再加上 月薪乘以 12乘以提成/100，那么不做NULL处理的话，有人可是一年就白忙活了
	select salary*12,salary*12+salary*12*commission_pct/100  from s_emp

    那么怎么样处理NULL 值呢?
	
	nvl(par1, par2)   当par1 为NULL 时会返回par2的值，否则返回 par1 本身
	null 值要尽早处理 否则运算完成之后 结果就不正确了

	select salary*12,salary*12+salary*nvl(commision_pct,0)/100 from s_emp;

---
	
	练习：
	显示每个员工的 id  first_name manager_id  如果manager_id为null 则显示成 -1
	
	select  id,first_name,nvl(manager_id,-1) from  s_emp;

7、数据的排重 （重复的数据值留一份）
	
	使用排重关键字 distinct 就可以完成
	如：查询所有的月薪，数目相同只留一份
	select distinct salary from s_emp;

	多重字段排重 当多个字段的值都相同时,才进行去重
	select distinct id,title,salary from s_emp;
	
	注意：排重关键字 distinct 必须出现在最前面 不能出现多对一 现象
	select id,title, distinct salary from s_emp; （X）
	












