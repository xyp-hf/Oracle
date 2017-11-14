---
title: 22、PLSQL 变量
date: 2017-11-02 21:04:46
tags: PLSQL
---

## 1、变量

### 1.1 使用plsql 定义两个变量 分别和 s_emp 表中的 id 和 first_name 的类型保持一致,并且给 和 id 类型相同 的变量赋值成 10，
使用 sql 语句给 first_name类型相同的那个变量赋值 并 输出这个变量的值。

``` bash
declare
    /* 获取变量名 表名.字段名%type; */
    var_id s_emp.id%type;
    var_name s_emp.first_name%type;
begin
    var_id:=10;
    select  first_name   into  var_name  from  s_emp  where id=var_id;
    dbms_output.put_line(var_name); 
end;
/ 
set serveroutput on;
```

### 1.2 如果想把 id = 10 的员工的 first_name salary dept_id 这三个数据查询出来

``` bash
declare
       /* 获取变量名 表名.字段名%type */
       var_name s_emp.first_name%type;
       var_salary s_emp.salary%type;
       var_dept_id s_emp.dept_id%type;
begin
        select first_name,salary,dept_id into var_name,var_salary,var_dept_id from s_emp where id=10;
        dbms_output.put_line(var_name||':'||var_salary||':'||var_dept_id);
end;
/
```

### 1.3使用 recod 类型,可以组织多个变量
recode 类型的语法 ----- 定义一个recode 类型
``` bash
declare
	type 类型名 is record(
        字段名 类型,
        字段名 类型,
        字段名 类型
	);
    变量名 类型名;
begin
```

``` bash
declare
     type emptype is record(
      var_name    s_emp.first_name%type,
      var_salary   s_emp.salary%type,
      var_dept_id   s_emp.dept_id%type
     );
     /* 使用上面的emptype 定义一个变量 */
     var_emp smptype;
begin
	select first_name,salary,dept_id into var_emp
	      from s_emp where  id=10;
	dbms_output.put_line(var_emp.var_name||':'||var_emp.var_salary||':'||var_emp.var_dept_id);

end;
/

```
把s_dept 表中的所有字段 包装成一个record 类型,然后使用sql 把 id = 50 的信息存入这个变量中
打印这个变量的信息

``` bash
declare
        TYPE depttype  IS  RECORD(
                id     s_dept.id%type,
                name  s_dept.name%type,
                region_id  s_dept.region_id%type   
        );  
        var_dept   depttype;
begin 
        select  *  into  var_dept  from  s_dept where id=50;
        dbms_output.put_line(var_dept.id||':'||var_dept.name);         
end;
/ 
```

### 1.4 如何把 s_epm 表中所有的字段 包装成一个record类型 然后 把id=25 所有的员工信息放入这个类型对应的变量中,打印 其中的 id first_name salary
表名%rowtype 实际上获取的是 字段名 和 字段顺序 完全和表头相同的一个记录类型

``` bash
declare
     var_emp s_emp%rowtype;
begin
	select * into var_emp from s_emp where id=25;
	dbms_output.put_line(var_emp.id||':'||var_emp.first_name||':'||var_emp.salary);
end;
/
```

### 1.5 table类型
类似于java数组 或者Map<Integer,Object>

	1.5.1 语法
	TYPE 类型名 is table of 元素类型 index by binary_integer;
	变量名 类型名;
	变量名(下标) 下标任意
	变量名(下标):=值;
### 
	1.5.2 定一一个装 数字的 table类型 使用这个类型定义一个变量 用来存放 9 5 2 7 0 这个五个数字，最后显示下标为 1 的数据。

``` bash
declare
    type numstyle is table of number index by binary_integer;
	var_nums numstype;
begin
    var_nums(-1):=9;
    var_nums(0):=5;
    var_nums(1):=2;
    var_nums(2):=7;
    var_nums(3):=0;
	dbms_output.put_line(var_nums(1));

end;
/
```

###	
	1.5.3 下标连续时 如何遍历 table 类型的变量

``` bash
declare
	type numstype is table of number index by binary_integer;
	var_nums numstype;
	var_index binary_integer:=-1;
begin
	var_nums(-1):=9;
	var_nums(0):=5;
    var_nums(1):=2;
    var_nums(2):=7;
    var_nums(3):=0;
    dbms_output.put_line(var_nums(var_index));
    var_index:=var_index+1;
    dbms_output.put_line(var_nums(var_index));
    var_index:=var_index+1;
    dbms_output.put_line(var_nums(var_index));
    var_index:=var_index+1;
    dbms_output.put_line(var_nums(var_index));
    var_index:=var_index+1;
    dbms_output.put_line(var_nums(var_index));
end;
/
```
###
	1.5.4 下标不连续时 如何遍历?
	迭代器的核心思想
		可以根据上一个元素的信息 获取到 下一个元素相关的信息
	变量名.first()	获取最小的下标
	变量名.next(下标) 根据一个元素的下标,可以获取下一个元素的下标
	变量名.last()    获取最大的下标
	变量名.count()   获取元素个数

``` bash
declare
	type numstyle is table of number index by binary_integer;
	var_nums numstyle;
	var_index binary_interger:=-1;
begin
    var_nums(-1):=9;
    var_nums(0):=5;
    var_nums(11):=2;
    var_nums(2):=7;
    var_nums(3):=0;
    var_index:=var_nums.first();
    dbms_output.put_line(var_nums(var_index));
    var_index:=var_nums.next(var_index);
    dbms_output.put_line(var_nums(var_index));
    var_index:=var_nums.next(var_index);
    dbms_output.put_line(var_nums(var_index));
    var_index:=var_nums.next(var_index);
    dbms_output.put_line(var_nums(var_index));
    var_index:=var_nums.next(var_index);
    dbms_output.put_line(var_nums(var_index));
	dbms_output.put_line(var_nums.count());
end;
/
```

### 1.6、变量的作用域 和 可见性 
	
	全局不能访问 局部 局部可以访问全局
	declare
		var_m number:=1;
	begin
		var_n number:=100;
	end;
	/

### 如果需要全局访问局部可以使用标记
	<<abc>>
	declare
		var_m number:=1;
	begin
			declare
					var_m number:=100;
			begin
					dbms_output.put_line(var_m);
					dbms_output.put_line(abc.var_m);
			end;
	end;
	/

``` bash


```