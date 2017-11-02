---
title: 17、Oracle的约束
date: 2017-11-03 01:43:53
tags: Oracle
---

## 约束

### 1、约束是 对数据库表中字段加的限制，有了约束之后 可以让我们的数据更加准确 和 完整。


### 2、约束种类 
       主键约束     primary   key          pk
              如果给表的某个字段加了主键约束 则这个字段的值 不能重复,并且字段的值不能为 null.
              一张表只能有一个主键。
       唯一性约束    unique                  uk         
               如果给表的字段加了唯一性约束 则 这个字段的值不能重复。
               注意 唯一性约束 无法区分两个 null 值 
       非空约束       not null                nn
               如果给表的字段增加了 非空约束  则这个字段的值不能是 null 值 
       检查约束       check                   ck
               如果给表的字段增加了 检查约束  则这个字段必须符合检查条件  符合检查条件的数据
                      才能进入数据库。               
       外键约束       references         foreign key        fk 

### 3、约束的实现
	  列级约束:在定义表的某一列时  直接在这一列的后面加约束限制 叫列级约束。
      表级约束:在定义完表的所有列之后 再选择某些列加约束限制。

### 4、主键的列级约束实现
      建立一张 订单表  myorder   
      id      number    pk
      oname  varchar2(30)
      create   table  myorder(
             id    number   primary  key,
             name varchar2(30) 
       );
      insert into myorder  values(1,'test1');
      insert into myorder  values(1,'test1')
      *
     ERROR at line 1:
     ORA-00001: unique constraint (SYSTEM.SYS_C007619) violated
     如果不给约束起名字  则系统会自动给约束进行一个命名  保证约束的名字不重复
     但是这个名字 不方便记忆  并且 不方便排错 以及以后的约束管理。

### 5、如何给约束命名
	
	constraint    约束的名字     约束对应的英文
	约束的名字构成:  表名_字段名_约束简写
	------------------------------------------------------------------------------------------
    drop    table   myorder;
    create   table  myorder(
           id    number   constraint myorder_id_pk primary  key,
           name varchar2(30) 
     );
    insert into myorder  values(1,'test1');
    insert into myorder  values(1,'test1');
	
### 6、练习
     建立一张 订单表 叫 order100  
     id     number     pk
     ono    varchar2(30)  uk
     odate  date       nn 
     create  table  order100(
            id     number  constraint  order100_id_pk  primary key,
            ono  varchar2(30)  constraint  order100_ono_uk  unique,
            odate  date   constraint  order100_odate_nn  not null
     );
     insert into  order100 values(1,null,sysdate);
     insert into  order100 values(2,null,sysdate);

### 7、检查约束 需要把检查约束的条件写入() 
	在上面的表的基础上 加一个字段 omoney number 要求omoney 大于35 
	------------------------------------------------------------------------------------------
    drop   table    order100;
    create  table  order100(
            id     number  constraint  order100_id_pk  primary key,
            ono  varchar2(30)  constraint  order100_ono_uk  unique,
            odate  date   constraint  order100_odate_nn  not null,
            omoney   number   constraint  order100_omoney_ck  check(omoney > 35)        
     );

### 8、主键的表级约束实现
    create   table    order200(
           id    number,
           ono   varchar2(30),
           odate  date,
           omoney   number , constraint   order200_id_pk   primary  key(id)
     );

	表级约束最大的优势就在于可以做联合约束
    create   table    order300(
           id    number,
           ono   varchar2(30),
           odate  date,
           omoney   number , constraint   order300_idono_pk   primary  key(id,ono)
     );    
    1    'a'
    1    'b'

### 9、练习
     建立一张 订单表 叫 order1001  
     id     number     pk
     ono    varchar2(30)  uk
     odate  date       nn     列级约束 (因为不支持表级)
     omoney   number    ck   要求大于35 
     除了  odate之外 其它的字段需要使用 表级约束   
     create table   order1001(
            id   number,
            ono    varchar2(30),
            odate   date   not null,
            omoney   number, 
            constraint   order1001_id_pk  primary  key(id),
            constraint   order1001_ono_uk  unique(ono),
            constraint   order1001_omoney_ck  check(omoney>35)
      );