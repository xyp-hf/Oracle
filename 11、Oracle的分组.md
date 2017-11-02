---
title: 11、Oracle的分组
date: 2017-11-02 07:24:12
tags: Oracle
---

### 1、分组的概念
	
	分组 就按照一定的标准 把数据分成若干部分

### 2、语法格式
	
	  select  字段       
           from  表名 
                   where  条件  
                           group  by  分组标准    having   组函数的过滤条件
                                   order  by  排序标准 排序方式;

### 3、
	举例
	按照部门编号分组  统计部门的人数 
	select dept_id,count(id)     
           from  s_emp   
              	group by dept_id;
	------------------------------------------------------------------------------------------
		显示人数大于2 的部门
		select dept_id,count(id)     
	           from  s_emp   
	              	group by dept_id   having  count(id) > 2; 
	------------------------------------------------------------------------------------------
	
### 4、
	按照部门编号分组 统计每个部门的平均工资  显示平均工资大于1400 的部门
		select dept_id,avg(salary)     
	           from  s_emp   
	              	group by dept_id   having  avg(salary) > 1400;   

### 5、sql语句的执行顺序
	from     where     group  by   having   select   order by 

### 6、按照部门编号分组 统计每个部门的工资和   显示工资和大于3000的部门
	      select dept_id,sum(salary) 
             from s_emp   
                     group by dept_id having sum(salary) > 3000;

### 7、在 6 的基础上 显示每个部门的名字 
	
	     select dept_id,sum(salary),max(name)
             	from s_emp,s_dept where dept_id = s_dept.id  
                     	group by  dept_id   having  sum(salary) > 3000;
	------------------------------------------------------------------------------------------
	结论:在分组语句中  select 后的字段要么是分组标准  要么是经过合适的组函数处理过的 

### 8、
	按照地区编号分组  统计每个地区中部门个数  并显示地区名
	      select  region_id,count(d.id),max(r.name)  name
            	from s_dept  d, s_region r
                    	where d.region_id = r.id 
                            	group by  region_id; 