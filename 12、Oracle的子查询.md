---
title: 12、Oracle的子查询
date: 2017-11-02 07:39:39
tags: Oracle
---

### 1、子查询的概念
		把一个查询的结果 作为另一个查询的基础
	
### 2、子查询可以出现的位置 
	   where 之后
       having 之后 
       from  之后 

### 3、
	举例
	选出了所有的领导编号
	select distinct manager_id from s_emp;
	
	只要让员工的id  出现在 领导编号这个列表中 则员工就是领导
	select id,first_name from s_emp where id in (select distinct manager_id from s_emp);

###	4、where 之后使用子查询
	
	 查询 工资 大于 10号员工的工资的员工 显示  id  first_name  salary

	 先求出 10号员工的工资
	 select salary from  s_emp where  id = 10;
	 在查询
	 select  id,first_name,salary from s_emp where salary > (select salary from  s_emp where  id = 10); 

### 5、having 之后使用子查询
	
	  查询  平均工资 大于  42部门平均工资的部门  显示 部门编号 和  平均工资 
	  先求出42部门平均工资
	  select  avg(salary)  from  s_emp where dept_id=42;
	  查询
	  select dept_id,avg(salary) from s_emp  
           	group by dept_id having avg(salary) >
              		(select  avg(salary)  from  s_emp where dept_id=42); 

### 6、from 之后使用子查询    数据分页

	  任何合法的select语句 都可以看成一张内存表 
	  select  id, first_name name,salary  from s_emp; 

	  select id,name from ( select  id, first_name name,salary  from s_emp) where salary > 1000;
	  