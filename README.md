# LuaNote
<br>  warning: !! 这个文档是基于lua 5.1下的笔记 (具体来说是Lua程序设计中文第二版的书籍相关笔记)
<br> ------------------lua 8种类型: nil, boolean, number, string, function, thread, userdata, table
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
<br> ---------- -----------------表达式------------------
<br> 关系表达式 使用  ~= 用于不等于测试  == 用于等于测试 ！！注意假如判断的两个值类型不相同会判断不相同 ！！nil只与自身相等
<br> 对于table跟userdata,Lua是作引用比较,只有它们引用同一个对象的时候,它们才会相等. a = {}; b= {}; a.x = 1; b.x = 1; c = a; print(a==b); print(a == c); 第一个输出是false,第二个输出是true
<br> a = 1; b = "1"; print(a>b) 会报错,lua不允许number跟string作比较处理
<br> 逻辑操作符(and or not) a and b 如果a 为true的话就返回b,否则返回a ; a or b 如果a为true就返回a，否则返回b; not a 只返回true or false 
print(4 and 5);  print(3 or 5);  print(3 > 6 and 1 or 5); 结果分别是 5,3,5 最后的可以当做?: 来理解（and 的优先级于 or 所以先使用and 再 or操作）
<br> 字符串连接(..) 要在lua中连接两个字符串,可以使用操作符"..",lua中的字符串是不可修改的,所以使用字符串连接只会创建一个新的字符串 
<br> ----------------------------------- 语句
<br> 可以多重赋值 a, b , c = 0; print(a, b, c) --> 0, nil, nil
<br> 局部变量 local 
<br>local a = 1;
<br>if(a ~= b) then
<br>    print(a);  // 1
<br>    local a = 10; 
<br>    print(a);  // 10
<br>else
<br>    print(b);
<br>endms -- a , 20
<br>print(foo2(), 10); -- a 10
<br>print(foo2()); -- a, b
<br> 变长参数   
<br> function a ( ... )
<br>     x ,y, z = ...;
<br>     print(x, y , z);
<br> end
<br> a(1, 2); -- 1, 2, nill
<br> a(1, 2, 3, 4); -- 1 , 2, 3
<br> a(1, 2, 3); --1, 2, 3
<br> 判断语句 if condition then 执行代码 elseif codition then 执行代码 end
<br> 
    
    
    
    i=0,10, i++ do 执行代码 end
<br> for key, value in iparis(table = {1, 2, 3}) do 执行代码 end  ipairs方法只适用于打印数组类型的,其他都不会显示出来.
<br> for key, value in paris(table = { a = 1, 2, 3}) do 执行代码 end
    
<br> loadfile loadstring load loadlib
<br> if not<condition> then error(“errer msg”) ---> 等价于 a = assert(functon(xx), "error msg");如果function没有报错就把结果赋值给n,或者就会引起一个错误并返回error msg
<br> pcall(fn)  pcall 会用一种"保护模式",来调用它的第一个参数，因此PCall可以捕获函数执行中的任何错误，如果没有发生错误,pcall会返回true以及函数的返回值,否则返回false以及错误信息.
    
<br>    <---->协同函数
<br> a = coroutine.create(function ()
<br>    print(1);
<br>     coroutine.yield(); //让该协同函数从running改变成suspended状态
<br>     print(2);
<br> end);
<br> 一个协同程序可以处于4种不同的状态: 挂起(suspended),运行(running),死亡(dead),正常normal 用coroutine.status(co)可以看到一个协同函数的对象
<br> coroutine.resume 用于启动或再次启动一个协同程序的执行(在协同函数里面可以用关键字 让一个运行中(running)的函数挂起(suspended);
<br> -------
<br> local co = coroutine.create(function (a, b, c)
<br>	print(1);
<br>	coroutine.yield(1);
<br> end)
<br>print(coroutine.resume(co, 1, 2, 3)); --- 1
<br> ----使用table 来实现相关的数据结构  table的key是无序的!!!!
<br>  数组: a = {2,  3 , 4} //记住lua都是从1开始做索引的
<br>  矩阵: 
<br> mt = {}; --矩阵
<br> for i = 1, 5 do
     for j = 1, 3 do
        mt[(i-1)* 3 + j] = 0;
     end
<br> end
<br> 队伍跟双向对列 table.insert ; table.delete
<br>List = {};
<br>function List.new()
<br>    return {first = 0, last = -1}
<br>end

<br>function List.pushFirst(list, value)
<br>    local first = list.first - 1;
<br>    list.first = first;
<br>    list[first] = value
<br>end

<br>function List.pushLast(list, value)
<br>    local last = list.last + 1;
<br>    list.last = last;
<br>   list[last] = value;
<br>end
<br> 集合set的实现方式 if(table[key]) then table end;
<br> 字符串缓冲,因为lua的字符串是不可更改所有创建字符串会增加内存 table.concat();
<br> 图
<br> -------元表
<br> 可以通过元表来修改一个值得行为,使其在面对一个非预定义的操作是执行一个指定的操作，例如两个table a跟table b 做相加的操作的时候,可以根据他们元表里面的字段__add 做相加操作 ，Lua 在床架耐心的table时候不会创建元表.
<br> 如果table a 跟 table b 有元表的时候 ， 做 a+b 操作的时候 优先使用table a元表的方法,如果没有__add 的方法的时候查找table b 元表的方法
<br> 访问table的元方法
<br> 1.__index方法:当方法一个table不存在的字段是,得到的结果为nil。实际上,这些访问会促使解析器去查找一个叫__index的元方法.如果没有这个元方法,那么结果如前述的为nil.否则,就由这个元方法来提供最终结果.
<br> 这个方法可以创建table的时候创建默认值
<br>a = {};
<br>mt = {};
<br>mt.prototype = {x = 10, y =10};
<br>setmetatable(a, mt);
<br>mt.__index = mt.prototype;

<br>print(a.x);
<br> 若Lua检测到w中没有某字段,但在其元表却有一个__index字段,那么Lua就会以元表上__index上的字段来查找相关的值
<br> 2__newindex 元方法与__index类似,不同之处在于前者用于table的更新,而后者用于table的查询.当对一个table中不存在的索引赋值，解析器就会查找__newindex元方法
<br> 只读table __newindex方法抛出一个错误,__index按照带默认值方法就行了。
<br> ---模块与包
<br> require函数 ---查找一个模块的时候会先检查该模块是否已经加载过，没有才重新查找这个模块，返回这个模块，但是没有这个模块的话会报错.

<br> lua的面向对象---------------------
<br> self 定义了这项操作的"接受者";
<br> 使用self参数是所有面向对象语言的一个核心,大多数面向对象语言都能对程序员隐藏部分self参数,Lua只需使用冒号,就能隐藏该参数,
<br> a = {
<br>     balance = 1.00;
<br>     withdraw  = function(self, v)
<br>         self.balance = self.balance + v;
<br>     end
<br> }

<br> function a: deposit(v)  --使用冒号,隐藏了函数里面的self参数
<br>     self.balance = self.balance - v;
<br> end
<br> print(a.balance); -- 1.00
<br> a:withdraw(19);
<br> print(a.balance); -- 20.00
<br> a.deposit(a,  11);
<br> print(a.balance); -- 9.00
<br> 创建类 使用__index的元方法,原理就是的新创建的table没有相应办法的时候就会去查找元表中的__index里面是否有对应的方法，有的话就使用元表中__index定义的方法,从而实现了类
<br> Account = {
<br>     balance = 0,
<br>     deposit = function(self, v)
<br>         self.balance = self.balance + v;
<br>     end
<br> }

<br> function Account:new(o)
<br>     o = o or {};
<br>     setmetatable(o, self);
<br>     self.__index = self;
<br>     return o;
<br> end

<br> c = Account.new(Account);
<br> print(c.balance); --0
<br> 继承就是上面的方法
<br> a = c:new(); --实现对account方法的继承
<br> print(a.balance); --如果在c找不到相应方法的话 就会上去上一级Account里面找到相应的方法或者对象 a.balance
<br> 私有方法定义
<br>function newAccount(init)
<br>    local self = {account = init}; ---私有方法,就算定义了一个新的变量也不会重新方法self
<br>    local getBalance = function()
<br>        return self.account;
<br>    end
<br>    return{
<br>        getBalance = getBalance;  ---只能访问暴露的方法
<br>    }
<br>end
<br>c = newAccount(1);
<br>print(c.getBalance());
<br> lua的数字库 Math https://blog.csdn.net/w18767104183/article/details/21171443
<br> lua的table库 table库是由一些赋值函数构成的,这些函数将table作为数组来做
<br> table.insert(list, pos, value);
<br> table.insert(list, value);
<br> table.remove(list); --不指定位置就会删除数组的最后一个元素
<br> table.remove(list, pos);
<br> table.sort(list); --不传入判断大少的函数默认少于判断
<br> table.sort(list, function () end);
<br> table.concat(list);
<br> table.concat(list, sep);
<br> table.concat(list, sep, i);
<br> table.concat(list, sep, i, j);
<br> string库 https://blog.csdn.net/nmn0317/article/details/4933207
<br> 操作系统库: os.time()//返回时间戳 os.date()//获取当前时间日期跟时间
<br>Lua 采用了自动内存管理。 这意味着你不用操心新创建的对象需要的内存如何分配出来， 也不用考虑在对象不再被使用后怎样释放它们所占用的内存。 Lua 运行了一个 垃圾收集器 来收集所有 死对象 （即在 Lua 中不可能再访问到的对象）来完成自动内存管理的工作。 Lua 中所有用到的内存，如：字符串、表、用户数据、函数、线程、 内部结构等，都服从自动管理。

Lua 实现了一个增量标记-扫描收集器。 它使用这两个数字来控制垃圾收集循环： 垃圾收集器间歇率 和 垃圾收集器步进倍率。 这两个数字都使用百分数为单位 （例如：值 100 在内部表示 1 ）。

垃圾收集器间歇率控制着收集器需要在开启新的循环前要等待多久。 增大这个值会减少收集器的积极性。 当这个值比 100 小的时候，收集器在开启新的循环前不会有等待。 设置这个值为 200 就会让收集器等到总内存使用量达到 之前的两倍时才开始新的循环。

垃圾收集器步进倍率控制着收集器运作速度相对于内存分配速度的倍率。 增大这个值不仅会让收集器更加积极，还会增加每个增量步骤的长度。 不要把这个值设得小于 100 ， 那样的话收集器就工作的太慢了以至于永远都干不完一个循环。 默认值是 200 ，这表示收集器以内存分配的“两倍”速工作。

如果你把步进倍率设为一个非常大的数字 （比你的程序可能用到的字节数还大 10% ）， 收集器的行为就像一个 stop-the-world 收集器。 接着你若把间歇率设为 200 ， 收集器的行为就和过去的 Lua 版本一样了： 每次 Lua 使用的内存翻倍时，就做一次完整的收集。

你可以通过在 C 中调用 lua_gc 或在 Lua 中调用 collectgarbage 来改变这俩数字。 这两个函数也可以用来直接控制收集器（例如停止它或重启它）。

2.5.1 – 垃圾收集元方法
你可以为表设定垃圾收集的元方法， 对于完全用户数据（参见 §2.4）， 则需要使用 C API 。 该元方法被称为 终结器。 终结器允许你配合 Lua 的垃圾收集器做一些额外的资源管理工作 （例如关闭文件、网络或数据库连接，或是释放一些你自己的内存）。

如果要让一个对象（表或用户数据）在收集过程中进入终结流程， 你必须 标记 它需要触发终结器。 当你为一个对象设置元表时，若此刻这张元表中用一个以字符串 "__gc" 为索引的域，那么就标记了这个对象需要触发终结器。 注意：如果你给对象设置了一个没有 __gc 域的元表，之后才给元表加上这个域， 那么这个对象是没有被标记成需要触发终结器的。 然而，一旦对象被标记， 你还是可以自由的改变其元表中的 __gc 域的。

当一个被标记的对象成为了垃圾后， 垃圾收集器并不会立刻回收它。 取而代之的是，Lua 会将其置入一个链表。 在收集完成后，Lua 将遍历这个链表。 Lua 会检查每个链表中的对象的 __gc 元方法：如果是一个函数，那么就以对象为唯一参数调用它； 否则直接忽略它。

在每次垃圾收集循环的最后阶段， 本次循环中检测到的需要被回收之对象， 其终结器的触发次序按当初给对象作需要触发终结器的标记之次序的逆序进行； 这就是说，第一个被调用的终结器是程序中最后一个被标记的对象所携的那个。 每个终结器的运行可能发生在执行常规代码过程中的任意一刻。

由于被回收的对象还需要被终结器使用， 该对象（以及仅能通过它访问到的其它对象）一定会被 Lua 复活。 通常，复活是短暂的，对象所属内存会在下一个垃圾收集循环释放。 然后，若终结器又将对象保存去一些全局的地方 （例如：放在一个全局变量里），这次复活就持续生效了。 此外，如果在终结器中对一个正进入终结流程的对象再次做一次标记让它触发终结器， 只要这个对象在下个循环中依旧不可达，它的终结函数还会再调用一次。 无论是哪种情况， 对象所属内存仅在垃圾收集循环中该对象不可达且 没有被标记成需要触发终结器才会被释放。

当你关闭一个状态机（参见 lua_close）， Lua 将调用所有被标记了需要触发终结器对象的终结过程， 其次序为标记次序的逆序。 在这个过程中，任何终结器再次标记对象的行为都不会生效。

2.5.2 – 弱表
弱表 指内部元素为 弱引用 的表。 垃圾收集器会忽略掉弱引用。 换句话说，如果一个对象只被弱引用引用到， 垃圾收集器就会回收这个对象。

一张弱表可以有弱键或是弱值，也可以键值都是弱引用。 含有弱值的表允许收集器回收它的值，但会阻止收集器回收它的键。 若一张表的键值均为弱引用， 那么收集器可以回收其中的任意键和值。 任何情况下，只要键或值的任意一项被回收， 相关联的键值对都会从表中移除。 一张表的元表中的 __mode 域控制着这张表的弱属性。 当 __mode 域是一个包含字符 'k' 的字符串时，这张表的所有键皆为弱引用。 当 __mode 域是一个包含字符 'v' 的字符串时，这张表的所有值皆为弱引用。

属性为弱键强值的表也被称为 暂时表。 对于一张暂时表， 它的值是否可达仅取决于其对应键是否可达。 特别注意，如果表内的一个键仅仅被其值所关联引用， 这个键值对将被表内移除。

对一张表的弱属性的修改仅在下次收集循环才生效。 尤其是当你把表由弱改强，Lua 还是有可能在修改生效前回收表内一些项目。

只有那些有显式构造过程的对象才会从弱表中移除。 值，例如数字和轻量 C 函数，不受垃圾收集器管辖， 因此不会从弱表中移除 （除非它们的关联项被回收）。 虽然字符串受垃圾回收器管辖， 但它们没有显式的构造过程，所以也不会从弱表中移除。

弱表针对复活的对象 （指那些正在走终结流程，仅能被终结器访问的对象） 有着特殊的行为。 弱值引用的对象，在运行它们的终结器前就被移除了， 而弱键引用的对象则要等到终结器运行完毕后，到下次收集当对象真的被释放时才被移除。 这个行为使得终结器运行时得以访问到由该对象在弱表中所关联的属性。

如果一张弱表在当次收集循环内的复活对象中， 那么在下个循环前这张表有可能未被正确地清理。

<br>--Lua 面试之后相关问题总结
<br> --面向对象 创建类 只读table
<br> ---userdata 
<br> ----弱table
<br> ----lua 跟C/C++的交互
<br> Lua解析器,即可执行程序"lua".这个解析器是一个简单的应用程序,它依靠Lua库来实现主要功能，这个程序会处理与用户的交互,它会将用户的文件或字符串输入Lua库,由Lua库来完成主要的工作 ---C代码--应用程序代码
<br> 一个使用了Lua的程序可以在Lua环境中注册C语言（或其他语言）实现的新函数,由此就可以向Lua添加某些无法直接用Lua编写的功能.这便使Lua称为一种'可扩展语言: ---C代码称为"库代码"
<br> Lua解析等类似的吧Lua当做一个库来使用,这种形式中的C代码称为"应用程序代码",第二种形式,Lua拥有控制权,C语言成为一个"库代码". 应用程序代码跟库代码都使用同样的API来与Lua通信,这些API称为CAPI.
<br> ----环境
<br> 环境: Lua将其所有的全局变量保存在一个常规的table中,这个table称为"环境",为了便于实施这种便利的操作,Lua将环境table自身保存在一个全局变量_G中，
eg: for n in pairs(_G) do print(n) end --可以打印当前环境所有的全局变量.
<br>--全局变量的声明: lua 的全局变量不用声明就能使用,可以通过元表来改变其访问全局变量时的行为
<br>-- 一种方法是简单地检测所有对全局table中不存在key的访问
<br>setmetatable(_G ,{
<br>    __newindex = function(_, n)
<br>        error('attempt to write to undeclared variable'..n, 2);
<br>    end,

<br>    __index = function(_, n)
<br>        error('attempt to read undeclared variable'..n, 2);
<br>    end
<br>})

<br>--定义之后访问一个不存在或者创建一个的全局变量就会报错
<br>--声明一个新的变量可以使用rawset,它可以绕过元表
<br>rawset(_G, a , 1);
<br>function declare(name, initval)
<br>    rawset(_G, name, initval or false)--如果不传initval 的话 创建的全局变量默认值就是nil.会把系统回收
<br>end

<br>local declaredNames = {}; --因为全局变量不允许nil,保存已声明变量的名称
<br>--另外一种只允许在主程序块中对全局变量进行赋值
<br>setmetatable(_G ,{
<br>    __newindex = function(t, n, v)
<br>        if not declaredNames[n] then
<br>            local w = debug.getinfo(2, "s").what; --what 字段表示主程序还是普通的Lua函数又或是C函数
<br>            if( w ~= "main" and w ~= "C" ) then
<br>                error('attempt to write to undeclared variable'..n, 2);
<br>            end
<br>            declaredNames[n] = true;
<br>        end
<br>        rawset(t, n, v);
<br>    end,

<br>    __index = function(_, n)
<br>        if not declaredNames[n] then
<br>            error('attempt to read undeclared variable'..n, 2);
<br>        else
<br>            return nil
<br>        end
<br>    end
<br>})

<br>--lua 5允许每个函数拥有一个自己的环境来查找全局变量,可以通过函数setfenv来改变一个函数的环境,该函数的参数是一个函数和一个新的环境table,第一个参数除了指定为函数本身以外,还可以指定一个数字,以表示当期当前函数调用栈中的层数,数字1表示当前函数
<br>function test()
<br>    a = 1;
<br>    local mt = {};
<br>setmetatable(mt, {__index = _G});
<br>    setfenv(1, mt); -- 1 表示当前函数,数字2表示调用函数当前函数的函数,依次类推.

<br>    print(a); -- 1;
<br>    a = 10;
<br>    print(a);
<br>    print(_G.a);
<br>    _G.a = 20;
<br>   print(_G.a);
<br>end

<br> ---内存管理
