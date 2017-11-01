---
title: 2、安装Oracle数据库
date: 2017-11-01 19:46:36
tags: Oracle
---

1、Oracle数据库的安装
	
	1. 解压Oracle软件安装包（不能有中文路径,安装位置也不要有中文）
	2. 关闭所有的杀毒软件
	3. 安装时使用默认安装

	安装完毕之后 按 win+R 键，输入 cmd 打开终端命令行
	输入 
	    sqlplus system/密码
	
	如果连接别人的电脑，可以输入
		sqlplus system/密码@别人的ip地址

2、操作Oracle 的命令行工具
	
	1、sqlplus 用户名/密码 暂且先使用 system 登录 
		SQL>路径/scott.sql
	2、执行完毕之后 输入 exit 退出
	3、使用 scott/TIGER 登录
	4、输入desc emp; 查询表的结构如下

	Name                                      Null?         Type
	-----------------------------------------        -------- ------------
	EMPNO                                     NOT NULL   NUMBER(4)
	ENAME                                                VARCHAR2(10)
	JOB                                                  VARCHAR2(9)
	MGR                                                  NUMBER(4)
	HIREDATE                                             DATE

	Name    这一列就是描述表头的 里面是一个个字段名
	Null?   字段是否可以为NULL , not null 代表本字段不可为空
	Type	代表字段的数据类型 NUMBER 数字类型 VARCHAR2(n) 字符串 Date 日期

3、修改语言环境
	
	select userenv('lang') from dual;
	US
	如果你是 ZHS
	那么你需要配置一个环境变量
	右击计算机>属性>高级>环境变量>新建环境变量
	新建环境变量	NLS_LANG
	环境变量值	AMERICAN_AMERICA.UTF8

	重新开启一个终端 执行脚本
	SQL>@位置/summit2_drop.sql
	
	
	