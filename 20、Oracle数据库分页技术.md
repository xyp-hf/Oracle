---
title: 20、Oracle数据库分页技术
date: 2017-11-03 07:16:38
tags: Oracle
---

## 分页技术 

### 
	rownum      行编号   这个字段叫伪列
	------------------------------------------------------------------------------------------
	select  rownum,first_name,salary  from  s_emp;

	
	一页显示 11 条数据  显示第一页数据
	select  rownum,first_name,salary  from  s_emp  where  rownum < 12; 

	
	一页显示 11 条数据  显示第二页数据
	select  rownum,first_name,salary  from  s_emp  where  rownum < 23 and rownum > 11;


	通过子查询 把rownum  变成一个普通的字段
		select r,first_name,salary 
				from (select  rownum  r,first_name,salary  from  s_emp  
             			where  rownum < 23 ) where r > 11;	

	
	一页显示 11 条数据  显示第三页数据
		select r,first_name,salary 
				from (select  rownum  r,first_name,salary  from  s_emp  where  rownum < 3*11+1 ) 
								where r > (3-1)*11;

	按照某个字段排序之后 显示 第几页数据
	按照salary  排序  显示  first_name  salary 
	select   first_name,salary from  s_emp    order by salary;


	按照salary  排序 一页显示11 条数据 显示 第二页数据
	select * from(select rownum r,first_name,salary from 
        (select   first_name,salary from  s_emp    order by salary) where rownum < 2*11+1
    )where r > (2-1)*11 ;
	
	
	最内层:负责排序
	中间层:负责编号   和 起别名  以及部分数据的过滤
	最外层:负责使用别名进行过滤 

    按照salary  排序 一页显示11 条数据 显示 第三页数据   
    select * from(select rownum r,t.* from 
        (select   first_name,salary from  s_emp    order by salary) t where rownum < 3*11+1
     )where r > (3-1)*11 ;
	
	