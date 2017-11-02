---
title: 15、TCL （transaction  control  language）
date: 2017-11-03 01:16:53
tags: Oracle
---

### 1、事物的概念
	事物又叫做交易 开发中希望把多个sql操作 看成一个逻辑整体，这些sql 要求同时成功或者同时失败。
	
### 2、举例
	银行转账  
    操作要同时成功 或者要同时失败 
    update   bankAccount   set money=money-500000  where id=1;
    fa  
    update   bankAccount   set money=money+500000 where id=2;
    fb
    if(fa && fb){
    }else{
    }

### 3、可以完成一个事务的语句
	commit;  提交 确认
	rollback; 回滚  撤销

### 4、事务四大特性  (了解)
	原子性      事务中的语句是一个逻辑整体不可分割
	一致性      同时成功  同时失败 状态要保持一致 
    隔离性      一个事务中的操作 在没有提交之前 数据的变化 对另外一个事务而言不可见。     
    持久性      状态的持久 

### 5、保存点
	引入它就是为了打破事务的原子性
    insert into  student100 values(1,'test1','111',sysdate); 
    savepoint   a;         
    insert into  student100 values(2,'test2','222',sysdate); 
    savepoint   b;
    insert into  student100 values(3,'test3','3333',sysdate);
    rollback to b;
    commit;