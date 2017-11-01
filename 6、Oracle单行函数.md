---
title: 6、Oracle单行函数
date: 2017-11-01 23:42:41
tags: Oracle
---

1、概念
	
	单行函数：
		   针对sql语句影响的每一行 都做处理 并且针对每一行都会返回一个结果
		   sql语句影响多少行就返回多少个结果
	组函数:
		   针对sql语句影响的所有行 只返回一个结果
		   无论sql影响多少行 只返回一个结果

2、举例
	
	单行函数:
	select first_name,upper(first_name) name from s_emp where id=1;
	select first_name,upper(first_name) name from s_emp where id=1;
	select first_name,upper(first_name) name from s_emp where id=1;

	组函数：
	select count(first_name) cname from s_emp where id=1;
	select count(first_name) canme from s_emp where id>1;
	select  count(first_name) cname  from  s_emp where  id<1;

3、准备学习 oracle数据库中单行函数
	
	select upper(first_name) name,first_name from s_emp;
	为了方便学习 引入一个单行 单列的表 dual
	select * from dual;
	
4、Oracle数据库中 处理字符串的单行函数
	
	upper(par1) 变大写 如： select upper('hello') from dual;
	lower(par2) 变小写 如： select lower('HEllo') from dual;
	
	initcap(par1) 每个单词首字母变大写 如： select initcap('one world one dream') from dual;
	length(par1) 求长度 如：select length('hello') from dual;
	concat(par1,par2) 连接两个字符串 如: select concat('a','b') from dual;
	nvl(par1,par2) nvl 可以处理任何类型 要求par1 和 par2 类型保持一致
	substr(par1,par2,par3)
			par1	要处理的字符串或者字段
			par2	截取的位置 编号从1 开始        可是负数 -1 代表最后一个字符
			par3	截取的长度
			select substr('hello',0,2) from dual;
			select substr('hello',1,2) from dual;
			select  substr('hello',2,2)  from dual;

	------------------------------------------------------------------------------------------
	如：
	显示 s_emp 表中的first_name 和 first_name 对应的前三个字符
		select first_name,substr(first_name,1,3) from s_emp;
	
	显示 s_emp 表中的first_name 和 first_name 对应的后三个字符
		select first_name,substr(first_name,-3,3) from s_emp;

	replace(par1,par2,par3)
		     par1     要处理的字符串
			 par2     被替换的内容
			 par3     替换成什么
		如：select replace('one' world one dream','one','two') from dual;

5、处理数字的函数
	
	round(par1,par2)	四舍五入
			 par1      要处理的数字
			 par2      处理的精确 默认是0 可以省略
	保留小数点后一位 看第二位小数
		select round(9.58,1) from dual;
	对小数点前一位进行四舍五入
		select round(9.58,-1) from dual;
	------------------------------------------------------------------------------------------
	trunc(par1,par2) 截取函数
			  par1 	    要处理的数字
		 	  par2      处理的精度  默认是 0，可以省略
		select trunc(9.58) from dual;
		select trunc(12.88) from dual;
	保留小数点后一位 不看第二位
	select trunc(9.58,1) from dual;
	对小数点前一位 进行截取
	select trunc(9.58,-1) from dual;

6、格式显示函数
	
	to_char(par1,par2)
			 par1 要处理的数据，还可以处理日期
			 par2 是显示格式 以字符串形式出现 可以省略 如果省略代表把某个类型变成 字符串类型 
	
	FM       格式的开头 是format格式的缩写 可以省略
	￥       美元符号
	L        本地货币符号 ￥
	9        小数点前面 代表 0-9 的任意数字
             小数点后面 代表 1-9 的任意数字
	0        小数点前面代表 强制显示前导零 如 12345 => 012,234
	         小数点后面代表 0-9 的任意数字
	,	     千分位分隔符
	.		 小数点
	
	select salary,to_char(salary,'FM$099,999.99') from s_emp;
	select salary,to_char(salary,'FM$099,999.00') from s_emp;
	
	select to_char(12345.86,'$099,999.99') from dual;
	select to_char(12345.86,'$099,999.00') from dual;
	
	如果使用 L  则看的是 环境变量 NLS_LANG 
	select  to_char(12345.86,'L099,999.00') from dual;

3.7 函数嵌套
	
	把一个函数的返回值 作为另一个函数的参数
	如：显示s_emp 表中的first_name 和 first_name 前三个字符，然后把这三个字符变大写
	select first_name,upper(substr(first_name,1,3)) from s_emp;

	如：显示s_emp 表中的first_name 和 first_name 后三个字符 然后把这三个字符变大写
	select first_name,upper(substr(first_name,length(first_name)-2,3)) from s_emp;
	
	显示 s_emp 表中的 id first_name manager_id,如果manager_id是null,则显示成BOSS
	select id,first_name,nvl(to_char(manager_id),'BOSS')  mid  from  s_emp;