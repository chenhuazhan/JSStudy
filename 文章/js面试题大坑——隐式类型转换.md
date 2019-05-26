**1.1-隐式转换介绍**



- 在js中，当运算符在运算时，如果两边数据不统一，CPU就无法计算，这时我们编译器会自动将运算符两边的数据做一个数据类型转换，转成一样的数据类型再计算

- 这种无需程序员手动转换，而由编译器自动转换的方式就称为隐式转换

- 例如1 > "0"这行代码在js中并不会报错，编译器在运算符时会先把右边的"0"转成数字0`然后在比较大小



**1.2-隐式转换规则**





1. 转成string类型： +（字符串连接符） 2..转成number类型：++/--(自增自减运算符)+ - * / %(算术运算符) > < >= <= == != === !=== (关系运算符)
2.  转成boolean类型：!（逻辑非运算符）



**1.3-坑一：字符串连接符与算术运算符隐式转换规则混淆**



**• 常见面试题如下**


console.log ( 1 + "true" );// ‘1true‘’
   console.log ( 1 + true );//2
   console.log ( 1 + undefined );//   NaN
   console.log ( 1 + null );//1

**• 原理分析**


/*此类问题的坑： 将字符串连接符(+ : 只要+号两边有一边是字符串)与算术运算符(+:两边都是数字)的隐式转换搞混淆
  1.字符串连接符+：会把其他数据类型调用String()方法转成字符串然后拼接
  2.算术运算符+ ：会把其他数据类型调用Number（）方法转成数字然后做加法计算
    */

   //+是字符串连接符： String(1) + 'true' = '1true'
   console.log ( 1 + "true" );//1true
   //+是算术运算符 ： 1 + Number(true) = 1 + 1 = 2
   console.log ( 1 + true );//2
   // +是算术运算符 ： 1 + Number(undefined) = 1 + NaN = NaN
   console.log ( 1 + undefined );//   NaN
   // +是算术运算符 ： 1 + Number(null) = 1 + 0 = 1
   console.log ( 1 + null );//1



**1.4-坑二：关系运算符：会把其他数据类型转换成number之后再比较关系**

- **常见面试题如下**


console.log ( "2" > 10 );//false 
console.log ( "2" > "10" );//true 
console.log ( "abc" > "b" );//false
console.log ( "abc" >  "aad" );//true 
console.log ( NaN == NaN );//false
console.log ( undefined == null );//true

**• 原理分析**


//2.1 : 当关系运算符两边有一边是字符串的时候，会将其他数据类型使用Number()转换，然后比较关系
   console.log ( "2" > 10 );//false   Number('2') > 10   =           2 > 10           =       false

   /*2.2： 当关系运算符两边都是字符串的时候，此时同时转成number然后比较关系
  重点：此时并不是按照Number()的形式转成数字，而是按照字符串对应的unicode编码来转成数字
  使用这个方法可以查看字符的unicode编码： 字符串.charCodeAt(字符下标，默认为0)
    */
   console.log ( "2" > "10" );//true     '2'.charCodeAt() > '10'.charCodeAt() = 50 > 49 = true
   console.log ( "2".charCodeAt () );//数字50
   console.log ( "10".charCodeAt () );//数字49（默认返回第一个字符的编码，如果想要查询第二个字符可以传参下标）

   //多个字符从左往右依次比较
   console.log ( "abc" > "b" );//false     先比较'a' 和 'b'， 'a' 与 'b'不等，则直接得出结果
   console.log ( "abc" >  "aad" );//true     先比较'a'和'a'，两者相等，继续比较第二个字符 'b' 与 'a' ,得出结果
   console.log ( "a".charCodeAt () );//数字97
   console.log ( "b".charCodeAt () );//数字98

   //2.3 特殊情况(无视规则)：如果数据类型是undefined与null，，得出固定的结果
   console.log ( undefined == undefined );//true
   console.log ( undefined == null );//true
   console.log ( null == null );//true
   //2.4 特殊情况（无视规则）：NaN与任何数据比较都是NaN
   console.log ( NaN == NaN );//false



**1.5-坑三：复杂数据类型在隐式转换时会先转成String，然后再转成Number运算**



![img](https://mmbiz.qpic.cn/mmbiz_jpg/X36HLl2EicOebia2ltN2aNGXY5OdUK3BmlmvZsVYChto7kLzOHBaYsbCatgNB2cSE3XCEkYZOURKcchgGJcKZe6g/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![img](https://mmbiz.qpic.cn/mmbiz_jpg/X36HLl2EicOebia2ltN2aNGXY5OdUK3BmliaSLbiaxzRAdaen8Q0JFsOicBdvIJjYToshgKsQDib8ftribD2F9JXLDEeQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**• 原理分析**


   /*复杂数据类型转number顺序如下
  1.先使用valueOf()方法获取其原始值，如果原始值不是number类型，则使用 toString()方法转成string
  2.再将string转成number运算
    */

   console.log ( [ 1,2] == '1,2' );//true     先将左边数组转成string，然后右边也是string则转成unicode编码运算
   console.log ( [ 1, 2 ].valueOf () );// [1,2]
   console.log ( [ 1, 2 ].toString () );// '1,2'

   var a = {};
   console.log ( a == "[object Object]" );//true
   console.log ( a.valueOf ().toString() );//[object Object]

/*分析：逻辑与运算一假则假，要想if分支语句小括号条件成立，则必须要让a的值同时等于1 且 等于 2 且等于3
  乍看之下，好像根本不可能实现，但是复杂数据类型会先调用valueOf()方法,然后转成number运算
  而对象的valueOf（）方法是可以重写的
    */
   var a = {
       i : 0,//声明一个属性i
       valueOf:function ( ) {
           return ++a.i;//每调用一次，让对象a的i属性自增一次并且返回
      }
  }
   if (a == 1 && a == 2 && a == 3){//每一次运算时都会调用一次a的valueOf()方法
       console.log ( "1" );
  }





**1.6-坑四：逻辑非隐式转换与关系运算符隐式转换搞混淆**



*前方高能，请注意~*

- 空数组的toString()方法会得到空字符串，而空对象的toString()方法会得到字符串`[object Object]` （**注意第一个小写o，第二个大写O哟**）

-  **常见面试题**


//大坑
console.log ( [] == 0 );//true
console.log ( ! [] == 0 );//true

//神坑
console.log ( [] == ! [] );//true
console.log ( [] == [] );//false

//史诗级坑
console.log({} == !{});//false
console.log({} == {});//false

**• 原理分析**


/*1.关系运算符：将其他数据类型转成数字
2.逻辑非：将其他数据类型使用Boolean()转成布尔类型
      \* 以下八种情况转换为布尔类型会得到false
          \* 0 、-0、NaN、undefined、null、''(空字符串)、false、document.all()
      \* 除以上八种情况之外所有数据都会得到true
  */
   
   /*原理   
  （1）[].valueOf().toString() 得到空字符串
  （2）Number('') == 0 成立
  */
   console.log ( [] == 0 );//true
   
   /* 原理 :本质是 `![]` 逻辑非表达式结果 与   0 比较关系
      (1)逻辑非优先级高于关系运算符 ![] = false (空数组转布尔得到true，然后取反得到false)
      (2)false == 0 成立
    */
   console.log ( ! [] == 0 );//true


/*原理 ：本质其实是 `空对象{}`   与   `!{}`   这个逻辑非表达式结果做比较
    （1） {}.valueOf().toString() 得到字符串 '[object Object]'
      (2) !{} = false
      (3) Number('[object Object]') == Number(false)
    */
   console.log({} == !{});//false
   //引用类型数据存在堆中，栈中存储的是地址，所以他们的结果是false
   console.log({} == {});//false

   /*原理：本质是 `空数组[]`   与   `![]`   这个逻辑非表达式结果做比较
  (1) [].valueOf().toString() 得到空字符串 ''
  (2) ![] = false
  (3) Number('') == Number(false)   成立 都是0
    */
   console.log ( [] == ! [] );//true
   //引用类型数据存在堆中，栈中存储的是地址，所以他们的结果是false
   console.log ( [] == [] );//false


   console.log ( {}.valueOf ().toString () )//[object Object]
   console.log ( [].valueOf ().toString () );//'' 空字符串



![img](https://mmbiz.qpic.cn/mmbiz_jpg/X36HLl2EicOebia2ltN2aNGXY5OdUK3BmllhovcyIfYZpl2CAZuRunF14FZaly8aO4xG3gOgnWheUibs62qJf8tdA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![img](https://mmbiz.qpic.cn/mmbiz_gif/X36HLl2EicOebia2ltN2aNGXY5OdUK3Bmlvemdly9cT6QTuoqUwNyHmf4nQia85OHE0FOsha53SZ6Ym1oxhHkPlicA/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

![img](https://mmbiz.qpic.cn/mmbiz_gif/X36HLl2EicOebia2ltN2aNGXY5OdUK3BmlcXJicic7vdVmVT0AiaunuFddnGXWvxKxLYdWuiakf9NeWrdTALOMpGXGew/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)