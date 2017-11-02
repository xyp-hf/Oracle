---
title: 10、Oracle的组函数
date: 2017-11-02 07:16:51
tags: Oracle
---

### 1、组函数的特点

	组函数 是 对一组数据处理之后 只返回一个结果

### 2、
	常见的组函数
	  count(par1)  统计数据的个数
      max(par1)    统计最大值
      min(par1)    统计最小值 
      avg(par1)    统计平均值
      sum(par1)    统计和

### 3、
	举例
	统计s_emp 表中的员工人数  最高工资  和 最低工资
	
		select count(id),max(salary),min(salary) from  s_emp;

### 4、组函数处理NULL值的策略   ------ 忽略
		select  count(commission_pct),avg(commission_pct),sum(commission_pct) from s_emp;

### 5、组函数的特殊用户
	
		select  count(*)  from  s_emp;
		select  count( distinct commission_pct),avg(distinct commission_pct),sum(commission_pct) from s_emp;
	