---
title: 16、Oracle中的日期类型
date: 2017-11-03 01:25:26
tags: Oracle
---

### 1、默认的日期类型
	dd-MON-yy
	表现默认是两年 实际上存储了四位

### 2、显示 s_emp 表中的数据  按照入职日期排序  显示 id  first_name  start_date
		select   id,first_name,start_date from s_emp  order by start_date; 

### 3、 如何按照指定格式 显示日期
		to_char(par1,par2)   par1是要处理的日期 
            par2 日期格式    日期格式 
            yyyy      四位年          mm   月               dd    日 
            hh        12小时制        hh24  24小时制         mi   分钟     		ss  秒 
            day       星期几          MON  英文月的缩写      month  英文月的全写 
	------------------------------------------------------------------------------------------
	select   id,first_name,to_char(start_date,'yyyy-mm-dd hh24:mi:ss')  start_date from s_emp  order by start_date;

### 4、如何插入一个日期
	之前学过  null    和 sysdate   sysdate 代表系统时间 
	如何插入未来的时间  或者 过去的时间点
	2008-08-08 20:08:10
	insert into  student100(id,sdate) values(3,'08-AUG-08');
	默认只能放入 年 月 日,时分秒信息是  0 
	如果需要时分秒信息 则使用  to_date 就可以了
	to_date(par1,par2)   par1 是一个日期字符串   par2 是日期的格式 
	insert into  student100(id,sdate) values(4,to_date('2008-08-08 20:08:08','yyyy-mm-dd hh24:mi:ss'));
	select  id,to_char(sdate,'yyyy-mm-dd hh24:mi:ss') from  student100;
	2012-12-22  23:59:59

### 5、日期的调整
	
	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss') from dual;
	调整一天   默认按照天为单位
	
	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
            to_char(sysdate+1,'yyyy-mm-dd hh24:mi:ss') from dual;

	调整一个小时的一半   默认按照天为单位
	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
            to_char(sysdate+1/(24*2),'yyyy-mm-dd hh24:mi:ss') from dual;
	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
            to_char(sysdate-1/(24*2),'yyyy-mm-dd hh24:mi:ss') from dual;

### 6、特殊的调整
	按照月为单位 调整 
  	add_months(par1,par2)   par1 要调整的日期    par2 调整的月数  可以是负数
	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
            to_char(add_months(sysdate,1),'yyyy-mm-dd hh24:mi:ss') from dual;

	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
            to_char(add_months(sysdate,-1),'yyyy-mm-dd hh24:mi:ss') from dual;

	对日期进行 截取     默认以天为单位截取 
	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
            to_char(trunc(sysdate),'yyyy-mm-dd hh24:mi:ss') from dual; 
	 
	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
            to_char(round(sysdate),'yyyy-mm-dd hh24:mi:ss') from dual;  

	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
            to_char(trunc(sysdate,'hh'),'yyyy-mm-dd hh24:mi:ss') from dual; 
	
	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
            to_char(trunc(sysdate,'mm'),'yyyy-mm-dd hh24:mi:ss') from dual; 

	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
            to_char(trunc(sysdate,'yy'),'yyyy-mm-dd hh24:mi:ss') from dual;

### 7、给你一个时间  获取到这个时间对应的月的最后的一天的最后一秒对应的时间点
	sysdate
    
	select  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
            to_char(add_months(trunc(sysdate,'mm'),1)-1/(24*60*60),
                     'yyyy-mm-dd hh24:mi:ss') from dual; 
