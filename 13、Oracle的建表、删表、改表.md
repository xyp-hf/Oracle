---
title: 13、Oracle的建表、删表、改表
date: 2017-11-02 07:51:16
tags: Oracle
---

## DDL  （ data define  language）

### 1、建表

	  create  table  表名(
             字段名1    类型,
             字段名2    类型,
             字段名3    类型 
       );
	------------------------------------------------------------------------------------------
      类型:  number    数字类型  
             varchar2(n)    变长字符串类型
             char(n)     定长字符串  
             date        日期类型

### 2、
	  举例
	  建立一张学生表   叫 student100
      id     number
      name varchar2(30) 
      sno    char(30)
      sdate   date
	------------------------------------------------------------------------------------------
	  create  table  student100(
      	id     number，
      	name varchar2(30)， 
     	sno    char(30)，
      	sdate   date
      ); 

### 3、删除表
	 drop  table  表名;
	------------------------------------------------------------------------------------------
	 如：
	 drop  table  student100;

### 4、修改表
	删除某个字段
	alter  table  表名 drop  column  字段名
	如：
	alter   table  student100 drop  column sdate;

	增加一个字段
	alter  table  表名  add    字段名   类型;
	如：
	alter  table  student100 add  sdate  date;
	