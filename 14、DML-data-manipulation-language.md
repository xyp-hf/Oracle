---
title: 14、DML(data manipulation language)
date: 2017-11-03 00:54:18
tags: Oracle
---

### 1.1 插入语句 insert into
		1.1.1 全字段插入 插入数据的顺序和 desc 表名之后的顺序相同
		insert into 表名 values(值1,值2，值3);
		commit;
		
		注意： 数字类型可以直接写 字符串类型的值应该使用单引号 值如果可以为null 则可以使用null
		insert into student100 values(1,'zhangsan','201700011',null);
		insert into student100 values(2,'lisi','201700012',sysdate);
		
###
		1.12 选择部分字段插入
		没有选择到的字段 默认值是null 这就要求大家必须包含所有的非空字段
		insert into  表名(字段名1,字段名3)  values(值1,值3);
		commit;
		------------------------------------------------------------------------------------------
		insert  into  s_dept(id,name) values(101,'test101');
		commit;

### 1.2 删除语法
	
		delete  from  表名  where 条件;
		commit;

### 1.3 修改数据
		
		update  表名  set 字段名=值  where 条件;
		update  表名   set 字段名=值,字段名2=值2  where 条件;
		commit;
		------------------------------------------------------------------------------------------
		例如：
		把s_emp 表中 id = 5 的员工 first_name 改成 dengziqi salary改成1234567
		
		select first_name,salary from s_emp where id=5;
		update s_emp set first_name='dengziqi',salary=1234567 where id=5;
