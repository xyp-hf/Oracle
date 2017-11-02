---
title: 18、Oracle的外键
date: 2017-11-03 01:58:19
tags: Oracle
---

## 外键

### 1.1 概念
	 涉及到两张表  一张叫 父表(主表)   另一张叫子表(从表)
	 子表:定义了外键的表 一定是子表
	 子表中外键字段的取值  要受限于父表中某个字段的取值,要么取父表字段对应的值 要么取null值。

### 1.2 语法  考虑外键
1.2.1 建表 

     先建父表   后建子表  除非先不考虑主外键关系(先建表 后加外键)
     create table   parent100(
             id        number    constraint  parent100_id_pk   primary  key,
             name   varchar2(30) 
     );       
     create  table   child100(
             id         number   constraint  child100_id_pk    primary key,
             name   varchar2(30),
             fid        number   constraint   child100_fid_fk   references  parent100(id) 
     );
	 ------------------------------------------------------------------------------------------
     1.2.2  插入数据 
     先插入父表对应的数据  后插入子表对应的数据    除非子表的外键值 使用 null 值 
     insert  into  parent100 values(1,'p1');
     insert  into   parent100 values(2,'p2');
     commit;
     insert into  child100  values(1,'c1',1);    
     commit;
	 ------------------------------------------------------------------------------------------
     1.2.3  删除数据 
     先删除子表中对应的数据  再删除父表中的数据   除非你使用级联 
     delete   from  parent100 where id=1; 
     delete   from  parent100 where id=2; 
	 ------------------------------------------------------------------------------------------
     1.2.4 删除表 
     先删子表 后删父表 除非使用 cascade constraints
     drop  table  parent100；
	 ------------------------------------------------------------------------------------------
     ERROR at line 1:
     ORA-02449: unique/primary keys in table referenced by foreign keys
     cascade  constraints   表示先解除主外键关系  这样可以删表时不考虑顺序 
     drop  table  parent100  cascade  constraints; 
	
### 1.3  建表一张 部门表  dept100   字段有  id      number  pk       name     varchar2(30)

     插入两条数据  数据是   1    test1            2      test2    注意提交数据
     再建立一张 员工表  emp100  字段有 id    number  pk      name   varchar2(30)   
     salary    number        dept_id   number    fk  插入的数据如下 
      1     ea    10000       1                  2     eb           8000   1          3   ec      12000     1 
      4     ed    28000      2                   5     ee           36000  2    提交数据
      要求建表之前  先删除表    要求给约束命名 .
          
      drop  table   emp100  cascade  constraints;
      drop  table   dept100  cascade  constraints;
      create  table  dept100(
             id   number  constraint  dept100_id_pk   primary  key,
             name  varchar2(30)
       );        
       insert into  dept100 values(1,'test1');
       insert into  dept100 values(2,'test2');
       commit;
       create    table   emp100(
              id       number   constraint  emp100_id_pk   primary key,
              name   varchar2(30),
              salary   number,
              dept_id    number   constraint  emp100_dept_id_fk  references  dept100(id)
        );
        insert  into  emp100 values(1,'ea',10000,1);
        insert  into  emp100 values(2,'eb',8000,1);
        insert  into  emp100 values(3,'ec',12000,1);
        insert  into  emp100 values(4,'ed',28000,2);
        insert  into  emp100 values(5,'ee',36000,2);
        commit; 	
	
### 1.4 级联
       on delete  cascade    级联删除  
       on  delete set null     级联置空
       把上面的操作 加在外键上 

### 1.5 表级约束   ----  外键
     create    table   emp100(
         id       number   constraint  emp100_id_pk   primary key,
         name   varchar2(30),
         salary   number,
         dept_id    number ,
         constraint  emp100_dept_id_fk   foreign  key(dept_id) references  dept100(id)
      ); 

### 1.6 先建表 后加约束  (外键)
      create    table   emp100(
         id       number   constraint  emp100_id_pk   primary key,
         name   varchar2(30),
         salary   number,
         dept_id    number
       ); 
 
     alter table   emp100  add  constraint   emp100_dept_id_fk   foreign  
           key(dept_id) references    dept100(id);