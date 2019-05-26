熟悉工具可以让工具在工作中发挥出更大的作用。尽管江湖传言 JavaScript 很难调试，但如果你掌握了几个技巧，就能用很少的时间来解决错误和bug。

**文中已经列出了14个你可能不知道的调试技巧**，但是可能需要你牢记在心，以便在下次需要调试JavaScript代码时使用！

**一起来看**

大多数技巧都适用于Chrome控制台和Firefox, 尽管还有很多其他的调试工具，但大部分也适用。

## **1. debugger**

除了`console.log`, `debugger`是我们最喜欢、快速且肮脏的调试工具。执行代码后，Chrome会在执行时自动停止。你甚至可以把它封装成条件，只在需要时才运行。

```
if (thisThing) {
   debugger;
}
```

## **2. 用表格显示对象**

有时， 有一组复杂的对象要查看。可以通过`console.log`查看并滚动浏览，亦或者使用`console.table`展开，更容易看到正在处理的内容！

```
var animals = [
   { animal: 'Horse', name: 'Henry', age: 43 },
   { animal: 'Dog', name: 'Fred', age: 13 },
   { animal: 'Cat', name: 'Frodo', age: 18 }
];

console.table(animals);
```

输出：

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJQR341V4eEMuqn7MkeuxXY5AibwDLibJWM6SrvsYo3WNk5WiaRLP1VcMcg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## **3. 使用不同屏幕尺寸**

在桌面上安装不同移动设备模拟器非常棒，但现实确是不可行的。如何调整窗口大小呢？Chrome提供了所需的一切。跳到控制台并点击**‘切换设备模式’**按钮。观察窗口变化即可!

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJS45b3R4AQo1AibaqHToEiasT7BeqtqvKZXjAtkCgNMYD6UIGHMAGmwTg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## **4. 如何快速找到DOM元素**

在Elements面板中标记一个DOM元素，并在控制台中使用它。Chrome控制台会保留选择历史的最后五个元素，最终选择的首个元素被标记为`$0`，第二个选择的元素为`$1`，依此类推。

如果您按照“item-4”，“item-3”，“item-2”，“item-1”，“item-0”的顺序选择以下标签，则可以在控制台中访问DOM节点：

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJBktH0nobWXkJaMpRdnJ5FMdyU2yqOxaB4wticrGic7f3wBxffHvIRdBA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## **5. 使用** **console.time() 和 console.timeEnd() 测试循环**

要得知某些代码的执行时间，特别是调试缓慢循环时，非常有用。 甚至可以通过给方法传入不同参数，来设置多个定时器。来看看它是怎么运行的：

```
console.time('Timer1');

var items = [];

for(var i = 0; i < 100000; i++){
  items.push({index: i});
}

console.timeEnd('Timer1');
```

运行产生了一下结果：

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJKkjhFTkzc9DAYKEbdl6cS8GfPzXxOgiaOUYsiaGlZ4OPBX9NYAPK4q9A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## **6. 获取函数的堆栈跟踪信息**

使用JavaScript框架，会引入大量代码。

创建视图并触发事件，最后你想了解函数调用的过程。

由于JavaScript不是一个很结构化的语言, 有时候很难知道什么时候发生了什么。使用console.trace (仅仅只是在控制台中跟踪) 可以方便地调试JavaScript.

想象一下，要查看第24行`car`实例调用函数`funcZ`的整个堆栈跟踪信息：

```
var car;
var func1 = function() {
 func2();
}

var func2 = function() {
 func4();
}
var func3 = function() {
}

var func4 = function() {
 car = new Car();
 car.funcX();
}
var Car = function() {
 this.brand = ‘volvo’;
 this.color = ‘red’;
 this.funcX = function() {
   this.funcY();
 }

 this.funcY = function() {
   this.funcZ();
 }

 this.funcZ = function() {
   console.trace(‘trace car’)
 }
}
func1();
```

24行将输出：

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJ5C3dWCngj87OYpblYI4uZXTKia4uNuclBX3Sasda4LIgLCwWgDG5N2g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

可以看到 **func1** 调用 **func2**， **func2** 调用 **func4**。 **Func4** 创建了一个 **Car** 的实例，然后调用函数 **car.funcX**，依此类推。

即使你认为自己的代码写的非常好，这依然很有用。假如你想改进自己的代码。获取跟踪信息和所有涉及的函数，每一项都可以点击，可以在他们之间来回切换。就像是给你提供了一个调用堆栈的选择列表。

## **7. 将代码格式化后再调试JavaScript**

有时代码会在生产环境出问题，但是你的source maps没有部署在生产环境上。**不要怕**。Chrome可以将您的JavaScript文件格式化。格式化后的代码虽然不像真实代码那样有用，但至少可以看到发生了什么。点击 Chrome控制台中的源代码查看器中的`{}`按钮即可。

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJkrNb7hbvyJXszxaTuwFmLaYqOtEYBibfJCXFbq5RVEzeumms4ibklSFQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## **8. 快速查找要调试的函数**

假设你要在函数中打断点，最常用的两种方式是：

1. **在控制台查找行并添加断点**
2. **在代码中添加`debugger`**

在这两个解决方案中，您必须在文件中单击以调试特定行。

使用控制台打断点可能不太常见。在控制台中使用`debug(funcName)`，当到达传入的函数时，代码将停止。

这个调试方法很快, 但缺点是不适用于私有或匿名函数。但除了私有和匿名函数, 这可能是找到调试函数的最快方法。（注意：这个函数和`console.debug`函数不是同一个东西。）

```
var func1 = function() {
 func2();
};

var Car = function() {
 this.funcX = function() {
   this.funcY();
 }

 this.funcY = function() {
   this.funcZ();
 }
}

var car = new Car();
```

在控制台中输入`debug(car.funcY)`，当调用`car.funcY`时，将以调试模式停止：

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJ754a2hJ3ibhZEoXVSqdyD0ICvXjnHOzt2UcQOL7q7Q0ibghuOExjl3TA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## **9. 屏蔽不相关代码**

现在，我们经常在应用中引入几个库或框架。其中大多数都经过良好的测试且相对没有缺陷。 但是，调试器仍然会进入与调试任务无关的文件。解决方案是屏蔽不需要调试的脚本。当然可以包括你自己的脚本。在这篇文章中阅读更多关于调试不相关代码（https://raygun.com/blog/javascript-debugging-with-black-box/）

 ![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJiahQ0Tvz6kIDNhSXPkp9O3nH1M2m1tkXELSaWfSYVES2elGTE8gWrzA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## **10. 在复杂的调试过程中寻找重点**

在更复杂的调试中，我们有时希望输出很多行。可以做的就是保持良好输出结构，使用更多控制台函数，例如：

console.log, console.debug, console.warn, console.info, console.error等等。然后，可以在控制台中快速浏览。但有时候，某些JavaScrip调试信息并不是你需要的。现在，可以自己美化调试信息了。在调试JavaScript时，可以使用CSS并自定义控制台信息：

```
console.todo = function(msg) {
 console.log(‘ % c % s % s % s‘, ‘color: yellow; background - color: black;’, ‘–‘, msg, ‘–‘);
}

console.important = function(msg) {
 console.log(‘ % c % s % s % s’, ‘color: brown; font - weight: bold; text - decoration: underline;’, ‘–‘, msg, ‘–‘);
}

console.todo(“This is something that’ s need to be fixed”);
console.important(‘This is an important message’);
```

输出：

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJwDfdkOTreicABVBGRaw3SqeKLSYic9xPkibVKggBd1Chzk1srME9LDVYw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**例如：**

在`console.log()`中， 可以用`%s`设置字符串，`%i`设置数字，`%c`设置自定义样式等等，还有很多更好的`console.log()`使用方法。 如果使用的是单页应用框架，可以为视图（view）消息创建一个样式，为模型（models），集合（collections），控制器（controllers）等创建另一个样式。也许还可以像wlog，clog和mlog一样发挥想象力！

## **11. 观察特定函数的调用及参数**

在Chrome控制台中，可以观察特定的函数。每次调用该函数，就会打印出传入的参数。

```
var func1 = function(x, y, z) {
//....
};
```

输出：

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJibDCNcH0dUz9LyveqV6TQKJKogILoFUXoCIV3wVCh6tnxudibxCOGJzg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这是查看传入函数参数的好方法。但是，如果控制台提示我们形参的数目就更好了。在上面的例子中，func1期望3个参数，但是只有传入了2个参数。如果在代码中没有处理这个参数，就很可能出错。

## **12. 在控制台中快速访问元素**

控制台中比`querySelector`更快的方法是使用美元符号，`$('css-selector')`将返回CSS选择器的第一个匹配项。`$$('css-selector')`将返回所有匹配项。如果多次使用一个元素，可以把它保存为一个变量。

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJLt7DKBuOTLXh6W8sPqh8DnM4QdnARnDcgy4fQzn4RVafvFytwl4icoA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## **13. Postman 很棒（但Firefox更快）**

许多开发人员使用Postman查看ajax请求。Postman真的很优秀。但打开一个新的窗口，写入请求对象，然后再来测试它们，显得很麻烦。

有时使用浏览器更容易。

当你使用浏览器查看时，如果请求一个密码验证页面，不需要担心身份验证的cookie。下面看，在Firefox中如何编辑并重新发送请求。

打开控制台并切换到network选项卡。右击所需的请求，然后选择编辑并重新发送。现在可以改变任何想要的改的。更改标题并编辑参数，然后点击重新发送。

下面我用不同的属性发起的两次请求：

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJIS6wNc7v2fXTlAXmuaSFibpzCcIKO1NjjQFNGbXxGSyUbBDe0kH0nLw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## **14. 中断节点更改**

DOM是一个有趣的东西。有时候它会改变，你并不知道为什么。 但是，当您调试JavaScript时，Chrome可以在DOM元素发生更改时暂停。你甚至可以监视它的属性。在Chrome控制台中，右击该元素，然后在设置中选择中断：

![img](https://mmbiz.qpic.cn/mmbiz_png/1hReHaqafacZXBvFYgg4ib056ibOlU2iagJ6ztv8rcDnJkrSLxRJlxNwGvvr4AB0AB7uFDRraQzPiccNPpKct8cpibA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)