---
title: 7、Oracle多表查询
date: 2017-11-02 00:41:52
tags: Oracle
---

### 1、表连接

	查询s_emp 表中的first_name 和 对应的部门编号
	select first_name,dept_id from s_emp;
	会发现 需要的部门名 在s_dept 表中 表结构 如下 
	会发现 需要的部门名  在 s_dept 表中

### 2、如何取得需要的数据 如果出现同名字段 则使用 表名区分 
	select first_name,dept_id,name
	         from s_emp,s_dept
                     where dept_id = s_dept.id;

### 3、表连接的语法
	
	select 想要的字段数据
           from 表1,表2
                where 条件;
	
	如果碰到同名字段,则使用表名区分

### 4、
	
	s_dept表                            s_region表
	id          部门编号                 id          地区编号
	name        部门名                   name    	地区名
	region_id   地区编号
	------------------------------------------------------------------------------------------
	
	查询每个 部门的名字 和 对应的地区的名字
	    select s_dept.name,s_region.name
               from s_dept,s_region
                        where region_id = s_region.id;

### 5、可以通过给表起别名的方法 来简化表连接的sql
		select d.name,r.name
		          from s_dept d,s_region r
	                      where region_id=r.id;

### 6、【三表查询】显示每个员工的first_name 和 对应的部门的名字 已经 部门对应的地区的名字
	
     s_emp表                        s_dept表                         s_region表
     id          员工编号            id         部门编号               id    地区编号
     first_name  员工名              name       部门名                name   地区名
     dept_id     部门编号            region_id  地区编号
	------------------------------------------------------------------------------------------
		select first_name, d.name dname,r.name rname
		     from s_emp e,s_dept d,s_region r
		             where  e.dept_id=d.id and d.region_id=r.id;

### 7、非等值连接
	
	4.7.1  等值连接 和 非等值连接
	等值连接
	       使用等号作为表的连接条件 叫等值连接
	非等值连接:
	       不使用等号作为表的连接条件  叫非等值连接

	------------------------------------------------------------------------------------------

	4.7.2 非等值连接举例
	salgrade
	grade    工资级别
	losal    级别对应的低工资
	hisal    级别对应的高工资
	显示每个员工的 id   salary  和 对应的工资级别

	select id,salary,grade
           from s_emp,salgrede
                 where salary between losal and hisal;

### 8、自连接
	通常在一张物理表中 存在两层业务含义的数据 查询出其中的一层数据，就会使用到自连接。

	如：找出所有的领导
	如果有人的manager_id是你的id，则你就是领导

		select distinct m.id,m.first_name
		       from s_emp e,s_emp m
	                  where e.manager_id=m.id;