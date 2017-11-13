---
title: 21、PLSQL 过程化sql
date: 2017-11-03 21:27:50
tags: PLSQL
---

### 1、PLSQL(procedure language / struct query language)
	PLSQL 过程化sql,就是可以像写java程序一样，去完成数据库的操作
	jdbc 使用java程序访问数据库

### 2、plsql  简介
	plsql 是在标准sql的基础上 增加了 过程化处理语言
	它是在客户端操作服务端的一门语言

### 3、PLSQL的特点
	结构化 模块化编程
	良好的可扩展性
	良好的可维护性
	提高了程序的性能
	不便于向异构数据库移植

### 4、plsql扩展了sql那些内容
	变量和数据类型
	控制语句 --- 分支语句 循环语句 goto
	过程和函数
	对象和方法

### 5、plsql的程序结构

	   declare
          /* 申明区 
               用来定义变量  和  类型的   */
   		begin
          /* 执行区 
              用来执行plsql语句 和 sql语句的                
              */ 
   		exception
          -- 异常处理区   
          -- 当程序执行出错自动进入这个区处理  
	   	end;
	   	/

### 6、第一个plsql程序
	
	begin
			dbms_output.put_line('hello plsql!');
	end;
    /

### 7、执行 plsql程序
	命令行下的 sqlplus
	图形界面工具 plsqldeveloper sqldeveloper
	无论使用 哪种开发工具 必须设置 set serveroutput on;
	
### 8、标识符
	给变量 类型 过程 函数 游标等命名的

### 9、变量
	9.1 语法
	declare
		变量名 类型;
	begin
	类型:
		基本类型 number varchar2(n) char(n) date boolean binary_integer
		复合类型 record table cursor
		参考类型 ref
		大数据类型 BLOB CLOB FILE 一般使用，使用字符串存储路径即可

	------------------------------------------------------------------------------------------
	9.2 举例
	定义两个变量 分别是 数字类型 和 字符串类型
	declare
		var_id number;
		var_name varchar2(30);
	begin
		dbms_output.put_line(var_id||':'var_name);
	end;
	/
	变量没有赋值 则值一定是null
	------------------------------------------------------------------------------------------
	9.3 变量的赋值
	declare
			var_id number:=123;
			var_name varchar2(30):='zhangsan';
	begin
			dbms_output.put_line(var_id||':'||var_name);
	end;
	/
	------------------------------------------------------------------------------------------
	9.4 定义两个变量 分别 和 s_emp 表中的id 和first_name对应的类型保持一致
	然后分别给两个变量赋值成 1 testcarmen 最后打印这两个变量饿值
	declare
		id number(7);
		first_name varchar2(25);
	begin
		id:=1;
		first_name:='testcarmen';
		dbms_output.put_line(id||':'||first_name);
	end;
	/
	------------------------------------------------------------------------------------------
	9.5 可以使用 表名.字段名 % type 来获取表中字段对应的类型
	declare
           var_id        s_emp.id%type;
           first_name    s_emp.first_name%type;	
	begin
		var_id:=2;
		/* 使用sql 语句 给变量 first_name 赋值 */
		select first_name into first_name from s_emp where id=var_id;
	end;
	/
	------------------------------------------------------------------------------------------
	9.6 变量的修饰
	constant 代表修饰成常量
	not null 修饰成非空
	
	declare
		var_id constant s_emp.id%type:=1;
		first_name s_emp.first_name%type not null:='abc';
	begin
		-- var_id:=1;
		dbms_output.put_line(var_id||':'||first_name);
	end;
	/
	
	