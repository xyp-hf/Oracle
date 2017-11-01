---
title: 4、Oracle查询语句 where字句
date: 2017-11-01 21:18:39
tags: Oracle
---

1、where字句的作用
	
	限制表中数据的返回,符合where条件的数据被选中,不符合where条件的数据被过滤掉。

2、where的语法格式

	select 字段名 from 表名 where 条件;

3、两个极限条件

1 = 1     恒等  永真

1 ！= 1   恒假 永假

	select  id,first_name from  s_emp where  1=1;
	select  id,first_name from  s_emp where  1!=1;

4、数字类型的表单
	
	查询月薪是1400的员工,显示 id first_name salary

	select id,first_name,salary from s_emp where salary=1400;
	注意：在sql语句中,与java不同,等于是单等号,也就是 = 
		  sql不区分字母大小写

5、字符类型的表达
	
	查询s_emp表中 first_name 叫 Carmen的员工  显示 id  first_name  salary

	select id,first_name,salary from s_emp where first_name='Carmen';
	注意：字符串必须使用单引号
		  sql不区分大小写，但是字符串的值严格区分大小写

6、常见的条件运算符
	等于    =
	不等于  !=  ^=  <> 
	
	如：查询出所有月薪不是1400的员工
	select   id,first_name,salary from  s_emp where salary!=1400;
	select   id,first_name,salary from  s_emp where salary^=1400;
	select   id,first_name,salary from  s_emp where salary<>1400;


6.1、sql提供的条件运算符

6.1.1 表达闭区间的运算符 [a,b]
	
	where 字段名 between a and b;

	------------------------------------------------------------------------------------------
	如：显示 s_emp 表中 每个员工的 id  first_name  salary  要求salary 在[1400,2500] 中
	select  id,first_name,salary  from  s_emp where salary  between 1400 and 2500;

6.1.2 表达一个字段的值 出现在某个列表内

	where 字段名 in 列表;
	列表的表达是    (值1,值2,值3)
	------------------------------------------------------------------------------------------
	如：查询s_emp 表的员工 在 31部门 或者在 41部门   或者在 50  最终显示 id  first_name dept_id
	select id,first_name,dept_id from s_emp where dept_id in (31,41,50);

6.1.3 空值判断运算符
	
	原因： 除了 is null 运算符 其他运算符 对 NULL值判断无效
	如：查询 提成是 10 的员工  显示  id   commission_pct  first_name
	select id,commission_oct,first_name from s_emp where commission_pct = 10;

	查询s_emp 表中 manager_id为null的人 显示 first_name  manager_id salary
	select first_name,manager_id,salary from s_emp where manager_id is null;

6.1.4 模糊查询运算符
	
	举例:
	找出所有姓李的人       李密   李世民   李靖  李超 李达康
	找出所有带龙的昵称    史泰龙   李小龙   成龙   小龙女   龙骑    龙珠

---	

	语法：
	where 字段 like '统配符'
	统配串 中最关键 是 两个 统配符:
	- 代表任意一个字符
	% 代表 0-n 个字符串
	------------------------------------------------------------------------------------------
	找出所有姓李的人       李密   李世民   李靖  李超 李达康
	where name like '李%';

	找出所有带龙的昵称    史泰龙   李小龙   成龙   小龙女   龙骑    龙珠
	where nickname like '%龙%';
	------------------------------------------------------------------------------------------
	找出 s_emp 表中 first_name 中带 a 字符的 显示 first_name
	select first_name from s_emp where first_name like '%a%';

	找出 s_emp 表中 first_name 中 第二个字符是a字符的 显示 first_nmae
	select first_name from s_emp where first_name like '_a%';
	
	------------------------------------------------------------------------------------------
	特例：
	user_table 中有一个字段 table_name 代表表名
	select table_name from user_tables;
	找出所有的以 S_ 开头的表名
	select table_name from user_tables where table_name like 'S_%'; (X)

	要把 _ 处理成普通字符 不让他代表任意一个字符才行
	所以应该这样写
	select table_name from user_tables where table_name like 'S\_%' escape '\';

6.2 逻辑运算符 （多条件连接）
	
	与    where a and b;      其实就是把java中的 && 换成了 and
	------------------------------------------------------------------------------------------
	如：显示 s_emp 表中 每个员工的 id first_name salary 要求salary在[1400,2500]中
	select id,first_name,salary from  s_emp where salary>=1400 and salary<=2500;

	或 的逻辑运算符就是 把java中的 || 换成 or
	------------------------------------------------------------------------------------------
	如：查询s_emp 表的员工 在 31部门 或者在 41部门   或者在 50  最终显示 id  first_name dept_id
	select id,first_name,dept_id from s_emp where dept_id=31 or dept_id=41 or dept_id=50;

	 非             !        not 
	------------------------------------------------------------------------------------------
    对立面 
     =                       !=   <>  ^=     
     >                        <=
     <                       >= 
     between a and b         not  between  a  and  b 
     in                      not  in     ( where  id  not in (1,3,5)   id!=1  and id!=3 and id!=5)
     like                    not  like    
     is null                 is  not null 

	除了is null 之外，其他运算符 都要主要null值的问题
	
1.3 多条件的优先级
	
	select  id,dept_id,salary from s_emp  where salary>1000 and dept_id=31 or dept_id=41;

	select  id,dept_id,salary from s_emp  where (salary>1000 and dept_id=31) or dept_id=41;

	select  id,dept_id,salary from s_emp  where salary>1000 and (dept_id=31 or dept_id=41);
	
	

	
	
	
	