### 1. 空(null, undefined)验证

当我们创建了一个新的变量，我们通常会去验证该变量的值是否为空(null)或则未定义(undefined)。这对于 JavaScript 编程来说，是一个经常要考虑到的验证。

如果直接写，那么像下面这样：

```js
`if (variable1 !== null || variable1 !== undefined || variable1 !== "") {    let variable2 = variable1;}`
```

我们可以使用一个更加简洁的版本：

```js
`let variable2 = variable1 || "";`
```

如果你不信，在谷歌浏览器开发者面板的控制台下试试！

### 2. 如何优雅的表示大数字

在 JavaScript 中，有一个简写数字的方法，也许你忽略了。`1e7`表示 10000000。

简化前：

```js
`for (let i = 0; i < 10000; i++) {`
```

简化后：

```js
`for (let i = 0; i < 1e7; i++) {`
```

### 3. 函数调用还可以更短

简化前：

```js
`function x() {    console.log("x");}function y() {    console.log("y");}let z = 3;if (z == 3) {    x();} else {    y();}`
```

简化后：

```js
`function x() {    console.log("x");}function y() {    console.log("y");}let z = 3;(z == 3 ? x : y)();`
```

你说四不四很短？

**3.1. 另外一种undefined**

```js
var data = void 0; // undefined
```

**4.论如何优雅的向下取整**

```js
var a = ~~2.33 //这种方法还可以将字符串转换成数字类型var b= 2.33 | 0var c= 2.33 >> 0
```

**5.如何装逼用代码骂别人SB**

```js
(!(~+[])+{})[--[~+""][+[]]*[~+[]] + ~~!+[]]+({}+[])[[~!+[]]*~+[]]
```

**6.如何用代码优雅的证明自己NB**

```js
([][[]]+[])[+!![]]+([]+{})[!+[]+!![]]
```

------


**二**以下我最近的一些收藏 `javascript`精简代码集合。它们都可以在你的开发控制台中运行，你可以从控制台中查看运行结果。

作者：megatron
https://juejin.im/post/5cc55eb5e51d456e577f93f0

### 1.日历

创建过去七天的数组，如果将代码中的减号换成加号，你将得到未来7天的数组集合

```js
[...Array(7).keys()].map(days => new Date(Date.now() - 86400000 * days));
```

### 2.生成随机ID

在原型设计时经常使用的创建ID功能。但是我在实际项目中看到有人使用它。其实这并不安全

```js
Math.random().toString(36).substring(2);
```

### 3.获取URL的查询参数

这个获取URL的查询参数代码，是我见过最精简的 `QAQ`

```js
?foo=bar&baz=bing => {foo: bar, baz: bing}
q={};location.search.replace(/([^?&=]+)=([^&]+)/g,(_,k,v)=>q[k]=v);q;
```

### 4.本地时间

通过一堆HTML，您可以创建一个本地时间，其中包含您可以一口气读出的源代码，它每秒都会用当前时间更新页面

```js
<body onload="setInterval(()=>document.body.innerHTML=new Date().toLocaleString().slice(10,19))"></body>
```

### 5.数组混淆

随机更改数组元素顺序，混淆数组

```js
(arr) => arr.slice().sort(() => Math.random() - 0.5)
```

### 6.生成随机十六进制代码（生成随机颜色）

使用JavaScript简洁代码生成随机十六进制代码

```js
'#' + Math.floor(Math.random() * 0xffffff).toString(16).padEnd(6, '0');
```

### 7.一个面试题

这是一个臭名昭著的面试题，让你写出他的运行结果，受不了~

```js
for(i=0;++i<101;console.log(i%5?f||i:f+'Buzz'))f=i%3?'':'Fizz'
```

### 8.数组去重

这是一个原生的JS函数但是非常简洁，Set接受任何可迭代对象，如数组[1,2,3,3]，并删除重复项

```js
[...new Set(arr)]
```

### 9.创建特定大小的数组

方便快捷创建特定大小的数组

```js
[...Array(3).keys()]
```

### 10.返回一个键盘（惊呆了）

这是一个很难看懂的简洁代码，但是运行后你会惊呆的，他竟然返回一个图形键盘

```js
(_=>[..."`1234567890-=~~QWERTYUIOP[]\\~ASDFGHJKL;'~~ZXCVBNM,./~"].map(x=>(o+=`/${b='_'.repeat(w=x<y?2:' 667699'[x=["BS","TAB","CAPS","ENTER"][p++]||'SHIFT',p])}\\|`,m+=y+(x+'    ').slice(0,w)+y+y,n+=y+b+y+y,l+=' __'+b)[73]&&(k.push(l,m,n,o),l='',m=n=o=y),m=n=o=y='|',p=l=k=[])&&k.join``)()
```

这是它的打印结果：

![img](https://mmbiz.qpic.cn/mmbiz_png/btsCOHx9LAMAPy0RScPZtem1sVqkJsRBuKCgDvjZxLicDib8dsVqxJjYagNL4sBHqOqYwe6GF3GcaypS37PLRJZg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

惊呆不，不得不说有才的人真多！