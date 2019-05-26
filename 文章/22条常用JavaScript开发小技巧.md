**1、使用var声明变量**

如果给一个没有声明的变量赋值，默认会作为一个全局变量（即使在函数内赋值）。要尽量避免不必要的全局变量。



**2、行尾使用分号**

虽然JavaScript允许省略行尾的分号，但是有时不注意的省略，会导致不必要的错误。建议在可用可不用行尾分号的地方加上分号。



**3、获取指定范围内的随机数**

```
var getRandom = function(max, min) {min = arguments[1] || 0;return Math.floor(Math.random() * (max - min + 1) + min);};
```

上面的函数接受一个你希望的随机最大数和一个你希望的随机最小数。



**4、打乱数字数组的顺序**

```
var sortArray = array.sort(function(){  return Math.random() - 0.5;});
```



**5、取出数组中的随机项**

```
var ran = array[Math.floor(Math.random() * array.length)];
```



**6、去除字符串的首尾空格**

```
var s = string.trim();
```



**7、类数组对象转为数组**

比如：类数组对象遍历：

```
Array.prototype.forEach.call(argumens,function(value){})
```

DOM的NodeList和HTMLCollection也是类数组对象



**8、获取数组中的最大值和最小值**

```
var max = Math.max.apply(Math, array);
var min = Math.min.apply(Math, array);
```



**9、清空数组**

array.length = 0;
array = [];



**10、保留指定小数位**

```
var num = num.toFixed(2);
```

返回字符串，保留两位小数



**11、使用for-in循环来遍历对象的属性**

```
for(var key in object) {   // object[key]}
```

不要用for-in来遍历数据



**12、获取某月天数**

```
function getMonthDay(date){  date = date || new Date();  if(typeof date === 'string') {      date = new Date(date);  };  date.setDate(32);  return 32 - date.getDate();}
```

传入date参数，可以是字符串、日期对象实例；为空表示当月天数



**13、浮点数问题**

```
0.1 + 0.2 = 0.30000000000000004 != 0.3
```

JavaScript的数字都遵循IEEE 754标准构建，在内部都是64位浮点小数表示



**1****4、JSON序列化和反序列化**

使用`JSON.stringify()`来将JavaScript对象序列化为有效的字符串。
使用`JSON.parse()`来将有效的字符串转换为JavaScript对象。

在AJAX传输数据时很有用



**15、使用“===”替换“==”**

相等运算符（==）在比较时会将操作数进行相应的类型转换，而全等运算符（===）不会进行类型转换。



**16、避免使用with()**

使用with()可以把变量加入到全局作用域中，因此，如果有其它的同名变量，一来容易混淆，二来值也会被覆盖。



**17、不要使用eval()或函数构造器**

eval()和函数构造器（Function consturctor）的开销较大，每次调用，JavaScript引擎都要将源代码转换为可执行的代码。



**18、简化if语句**

```
if (condition) {  fn();}
```

可替换成：

```
condition && fn();
```



**19、给可能省略的参数赋默认值**

```
function test(a, b){  a = a || '1';}
```



**20、给数组循环中缓存length的值**

如果你确定循环中数组的长度不会变化，那么你可以这样：

```
var length = array.length;for(var i = 0; i < length; i++) {}
```

可以避免在每次迭代都将会重新计算数组的大小，提高效率



**21、合并数组**

对于小数组，我们可以这样：

```
var arr1 = [1,2,3];var arr2 = [4,5,6];
var arr3 = arr1.concat(arr2);  // [1,2,3,4,5,6]
```

不过，concat()这个函数并不适合用来合并两个大型的数组，因为其将消耗大量的内存来存储新创建的数组。在这种情况之个，可以使用`Array.prototype.push.apply(arr1,arr2)`来替代创建一个新数组。
这种方法不是用来创建一个新的数组，其只是将第一个第二个数组合并在一起，同时减少内存的使用：

```
Array.prototype.push.apply(arr1, arr2); 
console.log(arr1); // [1,2,3,4,5,6]
```

**22 枚举对象“自身”的属性**

for...in除了枚举对象“自身”的属性外，还会枚举出继承过来的属性。

```
var hasOwn = Object.prototype.hasOwnProperty;
var obj = {name: 'tg', age: 24};for(var name in obj) {  if (hasOwn.call(obj, name)) {    console.log(name + '：' + obj[name]);  }}// name：tg// age：24
```