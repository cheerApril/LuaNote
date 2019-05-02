# LuaNote
# LuaNote
<br>  warning: !! 这个文档是基于lua 5.1下的笔记 (具体来说是Lua程序设计中文第二版的书籍相关笔记)
<br> lua 8种类型: nil, boolean, number, string, function, thread, userdata, table
<br> 可以使用type函数来返回一个值得类型名称
<br> nil 主要功能是用于区别其他任何值,将nil赋予一个全局变量等同于删除它
<br> boolean nil 跟false都视为假, 在条件判断(if) 将数字0和空字符串也都视为"真"
<br> string lua的字符窜是不可变值,字符串可以用单引号活着双引号来界定(个人觉得双引号比较好), 可以在字符窜前面放置操作符"#" 来获得该字符串的长度.
<br> a = "string"; print(#a); -->6
<br> table 是lua主要的(事实上也是仅有的)数据结构机制,基于table可以一种简单,统一和高效的方式来表示普通数据，符号表，集合，记录，对列和其他的数据结构.
可以将table想象成一种动态分配的对象,程序仅持有一个队它们的引用(或指针),Luab不会暗中为table的副本或者创建新的table.br
<br> e.g  table 只是获得里面元素的引用
<br> a = {}; --创建一个table a,并将它的引用存储到a
<br>k = "x";
<br>a[k] = 10;
<br>a[20] = "great";
<br>print(a["x"]);  -- print 10
<br>k = 20;
<br>print(a[k]);    -- print great
<br>a["x"] = a["x"] + 1;
<br>print(a["x"]);  -- print 11
<eg> eg b与啊引用同一个table , 如果a =nil 而且 b = nil 再也没有对一个table的引用的话,Lua的垃圾收集器最终会删除这个table,并复用它的内存
<br>a = {};
<br>a["k"] = 10;
<br>b = a;
<br>print(b["k"]); --- print 10
<br>a["k"] = 11;
<br>print(b["k"]);  ---- print 11
<br>a = nil;
<br>print(b["k"]); ---- print 11
<br> print(b["v"]) --- print nil 当table某个元素没有初始化，它的内容为nil
<br> e.g 使用#获得数组的长度 #只能获取从索引"1" 开始到的没有nil的数值的长度,如果table[1] 为nil 那后面的返回的长度都是0
<br>  a = {};  a[2] = 2; print(#a) -->print 0; a[1] = 2; print(#a) ---> print 2
<br> ---------- 表达式------------------
<br> 关系表达式 使用  ~= 用于不等于测试  == 用于等于测试 ！！注意假如判断的两个值类型不相同会判断不相同 ！！nil只与自身相等
<br> 对于table跟userdata,Lua是作引用比较,只有它们引用同一个对象的时候,它们才会相等. a = {}; b= {}; a.x = 1; b.x = 1; c = a; print(a==b); print(a == c); 第一个输出是false,第二个输出是true
<br> a = 1; b = "1"; print(a>b) 会报错,lua不允许number跟string作比较处理
<br> 逻辑操作符(and or not) a and b 如果a 为true的话就返回b,否则返回a ; a or b 如果a为true就返回a，否则返回b; not a 只返回true or false 
print(4 and 5);  print(3 or 5);  print(3 > 6 and 1 or 5); 结果分别是 5,3,5 最后的可以当做?: 来理解

      
