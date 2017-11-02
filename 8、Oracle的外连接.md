---
title: 8、Oracle的外连接
date: 2017-11-02 06:43:44
tags: Oracle
---

### 1、外连接的特点
	
	外连接的结果集 等于 内连接的结果集  加上 匹配不上的记录
	一个也不能少

### 2、如何实现
	
	(+) (+)的意思是 (+)所在的字段 对面的表的数据全部被匹配出来。

	找出所有的普通员工?
		select distinct  m.id,m.first_name
	         from  s_emp  e,s_emp  m
	                 where  e.manager_id(+) = m.id and e.manager_id is  null;
	
### 3、显示每个部门的名字 和 对应的地区的名字
	
	   select d.name,r.name
          from   s_dept d,s_region r
                 where  d.region_id = r.id;

	公司为了发展 增加了新的部门  
    insert  into  s_dept values(100,'test100',null);
    commit; 
	----------------------------------------------------------------------
	显示每个部门的名字 和 对应的地区的名字   没有地区编号的部门也要显示
	    select d.name,r.name
          from   s_dept d,s_region r
                 where  d.region_id = r.id(+);

### 4、显示每个员工的 first_name salary  以及 工资对应的工资级别
		
		  select first_name,salary,grade    
        		from  s_emp,salgrade 
                	where  salary  between  losal  and hisal;

		给 id  大于 23 的人涨工资   涨成 12500
		update  s_emp  set salary=12500  where  id > 23;
  		commit;
	----------------------------------------------------------------------
		使用外连接  找回那些超出统计范围的员工
		select first_name,salary,grade    
        		from  s_emp,salgrade 
                		where  salary  between  losal(+)  and hisal(+); 

### 5、显示s_emp 表中所有的领导?
		select  distinct m.id,m.first_name      
            from  s_emp  e,s_emp m
                    where e.manager_id = m.id;
	----------------------------------------------------------------------
	显示s_emp  表中所有的普通员工
	     select  distinct m.id,m.first_name      
               from  s_emp  e,s_emp m
                     where e.manager_id(+) = m.id and  e.manager_id is null;

	(+)  的意思  (+) 所在字段的表的对面的表的数据全部被匹配出来 
	本质上 数据是通过 null 记录进行的匹配

### 6、表连接的总结
	
	内连接   符合连接条件的数据被选中 不符合的被过滤掉
            等值连接     员工和对应的部门   部门和对应的地区 
            非等值连接   工资 和工资级别
            自连接         谁是领导
                    
    外连接   外连接的结果集 等于 内连接的结果集 加上匹配不上的记录 
            (+)  (+)所在的字段的表的对面的表的数据全部被匹配出来  本质上使用null 记录     
            自连接     谁是普通员工
            等值连接   找出新增的部门
            非等值连接  找出超出工资统计范围的员工 
	
