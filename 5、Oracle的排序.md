---
title: 5、Oracle的排序
date: 2017-11-01 23:19:36
tags: Oracle
---

1、排序的概念
	
	即按照一定的标准 和 方式，把数列进行组织排序
	标准：排序标准

	排序方法：默认 升序 关键字 asc 指自然排序 字典顺序
			降序 关键字 desc 反自然顺序 反字典顺序

-

2、Oracle排序的语法规则
	
	select 字段
	       from  表名
                 where 条件
                       order by 排序标准 排序方式;
	------------------------------------------------------------------------------------------
	order by 一定出现在语句的最后

-

3、按照salary 进行排序 显示 s_emp 表中的 id first_name salary

	正序
	select id,first_name,salary from s_emp order by salary;

	逆序
	select id,first_name,salary from s_emp order by salary desc;

4、NULL 值在排序中的处理
	
	如：按照 manager_id 进行排序,显示 id first_name manager_id
	select id,first_name,manager_id from s_emp order by manager_id;

5、多字段排序

	当第一排序字段的值 相同时 可以启用的第二排序进行排序,如
	
	select id,first_name,manager_id from s_emp order by manager_id,first_name desc;