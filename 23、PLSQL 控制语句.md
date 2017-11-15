---
title: 23、PLSQL 控制语句
date: 2017-11-02 21:05:07
tags: PLSQL
---

## 1.控制语句 
### 1.1 分支语句 
  
	1.1.1  语法
	------------------------------------------------------------------------------------------
	if a > b then
	
	   end id;
	------------------------------------------------------------------------------------------
	if a > b then
	
	    else

	end if;
	------------------------------------------------------------------------------------------
	if a > b then

	elsif a > c then

	elsif a > c then

	elsif a > d then

	end if;
	------------------------------------------------------------------------------------------
	if a > b then

	elsif a > c then

	else

	end if;
	------------------------------------------------------------------------------------------

###
	1.1.2  练习
	定义两个整数变量并赋值,然后打印两个变量的最大值
	declare
		var_x number:=20;
		var_y number:=30;
	begin
		if var_x < var_y then
			dbms_output.put_line(var_y);
		else
			dbms_output.put_line(var_x);
		end if;
	end;
	/

###
	1.1.3 练习
	定义三个整数变量，并赋值，然后打印三个变量的最大值 
	declare
		var_x number:=20;
		var_y number:=30;
		var_z number:=40;
	begin
		if var_x < var_y then
			if var_y < var_z then
			else
			end if;
		else
			if var_x < var_z then
			else
			end if;
		end if;
	end;
	/
	------------------------------------------------------------------------------------------
	declare
			var_x number:=20;
			var_y number:=30;
			var_z number:=40;
			var_max number;
	begin
			var_max:=var_z;
			if var_max < var_x then
				var_max:=var_x;
			end if;
			if var_max < var_y then
				var_max:=var_y;
			end if;
			dbms_output.put_line(var_max);
	end;
	/


###
	1.1.4   null 值的判断
	declare
		var_x number;
		var_y number;
	begin
		if var_x < var_y then
			dbms_output.put_line('var_x < var_y');	
		elsif  var_x > var_y then
			dbms_output.put_line('var_x > var_y');
		elsif  var_x = var_y then
			dbms_output.put_line('var_x = var_y');
		elsif var_x is null and var_y is null then
			dbms_output.put_line('var_x is null and var_y is null');
		else
			dbms_output.put_line('other');
		end if;
	end;
	/
	------------------------------------------------------------------------------------------
	declare
          var_x   number;
          var_y   number;
  	begin
          if  var_x < var_y  then 
               dbms_output.put_line('var_x < var_y');
          end if;  
          if  var_x > var_y  then
               dbms_output.put_line('var_x > var_y');
          end if;
          if  var_x =  var_y  then 
                dbms_output.put_line('var_x = var_y');
          end if;
          if  var_x  is  null and var_y  is  null  then 
                dbms_output.put_line('var_x  is  null and var_y  is  null');  
          end if;    
   	end;
   	/


### 1.2 循环语句
	
	1.2.1  简单循环
	loop
		---循环的代码
	end loop;

###
	1.2.2  如何结束一个循环 循环中出现下面的语句之一可结束循环
	exit when 结束循环的条件;
	------------------------------------------------------------------------------------------
	if 结束循环的条件 then
	   exit;
	end if;

###
	1.2.3 使用简单循环 把一个变量的值 从1 输出到 10
	declare
		var_i number:=1;
	begin
		loop
			dbms_output.put_line(var_i);
			exit when var_i=10;
			var_i:=var_i+1;
		end loop;
	end;
	/
	------------------------------------------------------------------------------------------
   	declare
          var_i   number:=1;
   	begin
          loop
                 dbms_output.put_line(var_i);
                 if   var_i=10  then 
                         dbms_output.put_line('game over');
                         exit;
                 end if; 
                 var_i:=var_i+1;
          end loop;
   	end;
   	/
	
###
	1.2.4 while 循环
	while 循环条件 loop
		--- 循环语句
	end loop;
	------------------------------------------------------------------------------------------
	把一个变量的值 从 1 输出 到 10
	declare
		var_i number:=1;
	begin
		while var_i <= 10 loop
			dbms_output.put_line(var_i);
			var_i:=var_i+1;
		end loop;
	end;
	/
	------------------------------------------------------------------------------------------
	把一个变量的值 从 10 输出 到 1
	declare
		var_i number:=10;
	begin
		while var_i > 0 loop
			dbms_output.put_line(var_i);
			var_i:=var_i-1;
		end loop;
	end;

###
	1.2.5 for 循环
	for 变量 in 区间 loop

	end loop;
	闭区间的表达是 a..b
	注意： 不用对变量 进行加操作 这个变量其实只能读 不能写
	------------------------------------------------------------------------------------------
	举例：
	begin
		for var_i in 1..10 loop
			dbms_output.put_line(var_i);
		end loop;
	end/
	------------------------------------------------------------------------------------------
	反向输出 在in 后面加上reverse即可
	begin
		for var_i in reverse 1..10 loop
			dbms_output.put_line(var_i);
		end loop;
	end;
	/
	------------------------------------------------------------------------------------------
	for循环中自定义的变量只能读不嫩写
	begin
         for  var_i   in  reverse 1..10  loop
                dbms_output.put_line(var_i);
                -- var_i:=100;
                exit  when   var_i=5;
         end loop;
  	end;
  	/  

###
	1.2.6  循环的嵌套
	declare
		var_x number;
		var_y number;
	begin
		var_x:=1;
		while var_x < 4 loop
			var_y:=y;
				while var_y < 4 loop
					dbms_output.put_line(var_y);
					var_y:=var_y+1;
				end loop;
			var_x:=var_x+1;
		end loop;
	end;
	/
	因为 exit  退出的是当前循环 就是内层循环 所以 输出三次 1  2 
	------------------------------------------------------------------------------------------
	  declare
        var_x    number;
        var_y    number;
	 begin
	        var_x := 1;
	        while  var_x < 4  loop
	               var_y:=1;
	               while  var_y  <  4  loop
	                      dbms_output.put_line(var_y);
	                      if  var_y = 2  then
	                            exit;
	                      end if; 
	                      var_y:=var_y+1;
	               end loop;
	               var_x:=var_x+1;
	        end loop; 
	 end;
	 / 
	------------------------------------------------------------------------------------------
	 declare
	        var_x    number;
	        var_y    number;
	 begin
	        var_x := 1;
	        while  var_x < 4  loop
	               var_y:=1;
	               while  var_y  <  4  loop
	                      dbms_output.put_line(var_y);
	                      if  var_y = 2  then
	                            var_x:=4;
	                            exit;
	                      end if; 
	                      var_y:=var_y+1;
	               end loop;
	               dbms_output.put_line('sha che .....');
	               var_x:=var_x+1;
	        end loop; 
	 end;
	 / 
	------------------------------------------------------------------------------------------
	 declare
	        var_x    number;
	        var_y    number;
	 begin
	        var_x := 1;
	        <<outerloop>>
	        while  var_x < 4  loop
	               var_y:=1;
	               while  var_y  <  4  loop
	                      dbms_output.put_line(var_y);
	                      if  var_y = 2  then
	                            exit outerloop;
	                      end if; 
	                      var_y:=var_y+1;
	               end loop;
	               dbms_output.put_line('sha che .....');
	               var_x:=var_x+1;
	        end loop; 
	 end;
	 / 

### 1.3  控制语句  ------ goto 语句   (了解)
	1.3.1 语法
	goto 标签名;
	------------------------------------------------------------------------------------------
	1.3.2 练习
	declare
		var_x number;
	begin
		var_x:=-1;
		<<myloop>>
		if var_x < 11 then
			dbms_output.put_line(var_x);
			var_x:var_x+1;
			goto myloop;
		end if;
	end;
	/

###
	1.3.3  练习 使用goto结束外层循环
	begin
		<<outerloop>>
		for var_x in 1..3 loop
			for var_y in 1..3 loop
				dbms_output.put_line(var_y);
				if var_y = 2 then
					exit outerloop;
				end if;
			end loop;
		end loop;
	end;
	/
	------------------------------------------------------------------------------------------
	使用goto 结合标签 结束多重循环 必须放在代码块上，如果下面没有代码块补上null
	    begin
            for var_x  in 1..3 loop
                   for var_y in 1..3 loop
                          dbms_output.put_line(var_y);
                          if var_y = 2 then 
                                 goto  outerloop;
                          end if;
                   end loop;
            end loop;
             <<outerloop>>  
             dbms_output.put_line('game over');
    	end;
    	/
	------------------------------------------------------------------------------------------
	    begin
            for var_x  in 1..3 loop
                   for var_y in 1..3 loop
                          dbms_output.put_line(var_y);
                          if var_y = 2 then 
                                 goto  outerloop;
                          end if;
                   end loop;
            end loop;
             <<outerloop>>  
             null;
    	end;
   		/







