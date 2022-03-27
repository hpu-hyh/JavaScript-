# 01-变量

## 1. var 

### var 声明作用域

var 在函数内部声明变量会产生局部变量，同时也意味着，在函数退出时，该变量将会被销毁；

```js
function test () {
    var message = 'hi';//局部变量
}
test();
console.log (message);//出错！
```

在函数中可以省略var从而声明一个全局变量；

```js
function test () {
    message = 'hi';//全局变量
}
test();
console.log (message);//'hi'
//但是在实际开发中并不支持这么做，全局变量会给代码维护带来困难。
```

同时声明多个变量时可以在一条语句中用逗号分隔开每一个变量：

```js
var message = 'hi',
    found = false,
    age = 29;
```

var 在全局作用域中声明变量会成为windows对象的属性。

### var  声明提升

在使用var时，存在变量提升，所以下面的代码不会报错：

``` js
function foo () {
   console.log(age);
   age = 26;
}
foo();//undefined
```

相当于执行了下列代码：

```js
function foo () { 
    var age;
    console.log(age); 
    age = 26;
}
foo();//undefined
```

即将当前作用域的变量声明提升至当前作用域的最前面，但并不提升变量赋值。

## 2. let

### let 声明作用域

let和var的声明差别在于let的声明是块作用域，而var是函数作用域；

详见：[](https://juejin.cn/post/6924234619621474318)

```js
if (ture) {
	var name = 'Matt';
	console.log(name);//'Matt'
}
console.log(name);//'Matt'


if (ture) {
	let name = 'Matt';
	console.log(name);//'Matt'
}
console.log(name);//ReferenceError:没有定义；
```

同时let也不允许在同一个块作用域内重复声明，

即使let和var混用，声明了相同名称的变量依然会报错，因为两者声明的变量本质上没有区别。

let 在全局作用域中声明变量不会成为windows对象的属性。



### let条件声明

```js

//1号作用域
<script>
    let name = 'Nicholas';
    let age = 36;
</script>
//2号作用域
<Script>
    if (typeof name === 'undefined'){
        let name;
    }
	name = 'Matt';
	//因为let被限制在if{}的作用域内，所以最后的声明相当于全局赋值。
</Script>
```

为此可知，let这个 ES6 声明关键字，不能依赖条件声明模式；

### for循环中的let声明

在let出现之前，for循环中的迭代变量会渗透到循环体外部：

```javascript
for (var i = 1;i<5;i++){
 	//循环逻辑
}
conlose,log(i);//5
//而在改成let之后这个问题就消失了，因为这是迭代变量的作用域仅限于for循环块环境；
```

## 3. const

### const声明

const和let的行为基本相同:

- 不允许重复声明
- 声明的作用域也是块

唯一的重要区别在于const在声明变量是必须同时初始化变量，并且尝试修改const声明的变量会导致运行时报错。

```js
const age = 26;
age = 36;//typeError 
// 因此，const常常用来给常数和匿名函数声明。
```

*注意：在用const声明对象时，可以对对象的属性做出修改。

## 4.声明风格及最佳实践

在es6版本中，基本已经舍弃了var的使用，但在平常做题时还会遇见。

实际开发中，我们通常：

- 不使用var
- const 优先，let次之



------



# 02-数据类型

## 1. Undefined 类型

一般来说，增加 Undefined 这个值目的就是为了正式明确空对象指针(null)和未初始化变量的区别。

```js
let message ;//变量被声明了，但是没有初始化，值为Undefined

//确保没有声明过这个变量
//let age ;
console.log(message);//'Undefined'
console.log(age);//报错
```

```js
let message ;//声明了但是没有初始化
//未声明age
if (message) {
    //这个快不会被执行
}
if (!message) {
    //这个快会被执行
}
if (age) {
    //报错
}
```

## 2. Null 类型

一般来说，Null 值表示一个空【对象】指针，这也是给一个 type of 传一个null会返回'Object'的原因。

undefined是null的一个派生值，虽然两者在表意上相等，但两者在用途上截然不同:

```JS
let message ;//合法声明，但永远不必显性的将一个变量值设置为undefined。
let age = null;//合法声明，且在实际开发中运用广泛，可以用来声明一个空的对象。
```

```js
let message = null;
let age;
if (message) {
    //这个块不会被执行
}
if (!message) {
    //这个块会执行
}
if (age) {
    //这个块不会执行
}
if (!age) {
    //这个块会执行
}
```

## 3. Boolean 类型

布朗值字面量true和false是区分大小写的，且true不等于1，false不等于0。

虽然布尔值只有两个，但是其他ES类型的值都有相对应的布尔值的等价形式。【使用Boolean()函数】

| 数据类型  | 转换为true的值 | 转换为false的值 |
| :-------: | :------------: | :-------------: |
|  Boolean  |      true      |      false      |
|  String   |   非空字符串   |    空字符串     |
|  Number   |    非零数值    |   0，  N a N    |
|  Object   |    任意对象    |      null       |
| Undefined |  N/A(不存在)   |    undefined    |

理解上列转换十分重要，因为 if 等控制流语句会自动执行其他类型到布尔值的转换，例如：

```js
let message = 'Holle world!';
if (message) {
    conlose.log("Value is true");
}
//会输出字符串"Value is true"，因为字符串message会被自动转换为等价的true。
```

## 4. Number类型

### 进制的表示

八进制的字面量第一个值必须是0，且数值只能在0~7范围内，如果出现不在范围内的数字，系统则会忽略字面量0，当成十进制处理。

```js
let num1 = 070; //八进制的56
let num1 = 079; //无效的八进制，当成79处理
let num1 = 07; //无效的八进制，当成8处理
```



要创建十六进制数值，需要在字面量前加上 0x ，十六进制数字（0~9   A~F）。

### 浮点值

要定义浮点数，数值中必须含有小数点，且小数点后必须有至少一个整数值。

对于十分大的数，可以使用科学计数法来表示：

```js
let floatNum = 3.125e7;//等于31250000
//以3.125为系数，乘以10对的七次方
//e后面也可以承接负数，用来表示十分小的数值
```

浮点数的精确值高达17位小数，满足日常运算但远不日整数精度。

例如，0.1+0.2得到的值不是0.3，而是0.30000000000004。

### 数值转换

- Number()
- parseInt()
- parseFloat()

#### number()

number是转型函数，常用于将字符串转换为数字，可用于任何数据类型

number函数基于以下规则进行转换：

- 布尔值，true转换为1，false转换为0
- 数值直接返回
- null返回0
- undefined返回NaN
- 字符串遵循以下规则
  - ***number('1')返回1，number('123')返回123，number('001')返回1 【忽略前面的0】***
  - ***如果字符串中包含浮点，则会返回浮点数，同样会忽略字面量前面的0***
  - ***如果包含十六进制格式如"0xf" ，则会返回十六进制对应的十进制数，八进制同理***
  - ***空字符串则返回0***
  - ***对象，先调用valueof()方法，并按照上述规则返回的值，如果返回NaN,则调用 toString() 方法，再按照上述方法转换。***

具体实例：

```js
let num1 = Number("Hello world");  //NaN
let num2 = Number(''); //0
let num3 = Number("0000011"); //11
let num4 = Number(true); //1
```



#### parseInt()

通常在需要得到整数时使用，该函数更专注于字符串是否含有数值模式

parseInt()函数基于以下规则进行转换：

- ***字符串前空格会被省略***
- ***如果第一个字符不是数值字符，加号或者减号，立即返回NaN，包括空字符串***
- ***如果第一个字符串是数值字符，加号或者减号，则依次检测每一个字符直到字符串尾末***
- ***在字符串检测中1，如果遇到非数值字符则会立即返回，如"1234blue"会返回1234，blue省略***
- ***浮点数，例如"22.5"，则会返回22***
- ***parseInt()可以识别不同的整数格式，十六进制的返回值并不会转换为八进制***

具体实例：

```js
let num = praseInt("1234blue"); //1234
let num = praseInt(""); //NaN
let num = praseInt("0xA"); //10,解释为十六进制数值
let num = praseInt("22.5"); //22
let num = praseInt("70"); //解释为十进制数值
let num = praseInt("0xf"); //解释为十六进制数值
```

让函数自己解析如何转换数值很容易出错，所以parseInt()提供了第二个参数用于指定底数(进制数)

```js
let num = parseInt("10",2); //2,按二进制来解析
let num = parseInt("10",8); //8,按八进制来解析
let num = parseInt("10",10); //10,按十进制来解析
let num = parseInt("10",16); //16,按十六进制来解析
//为避免出错，始终建议传入第二个参数
```



#### parseFloat()

parseFloat()函数和parseInt()函数的工作方式差不多相同，但是parseFloat()函数可以识别浮点数，这意味着第一次出现的小数点有效但第二次出现的小数点就无效了，会立即返回。

两者另一个不同之处在于parseFloat()始终忽略字符串开头的0，这个函数能够识别所有格式的浮点数，以及十进制格式，，且职能解析十进制格式，其他进制的数值会被返回0。

具体实例：



```js
let num = parseFloat("1234blue"); //1234,按整数解析
let num = parseFloat("0xA"); //0,不解析十六进制
let num = parseFloat("22.5"); //22.5
let num = parseFloat("22.3.4"); //22.3 ,第二个小数点无效
let num = parseFloat("0908.7"); //908.7
let num = parseFloat("3.125e7"); //31250000
```

## 5. string类型

### 字符字面量

以下列举重要的几个字符字面量：

| 字面量 | 含义 |
| :----: | :--: |
|   \n   | 换行 |
|   \t   | 制表 |
|   \b   | 退格 |
|   \r   | 回车 |
|   \f   | 换页 |

这些字符字面量可以出现在字符串中的任何位置，且可以当作单个字符被解释。

### 转换为字符串

有两种方式把一个值转换为字符串： toString（）     ；      String（）；

#### toString()

该函数几乎对所有值都适用，这个方法的唯一用途就是返回当前值的等价字符串，例如：

```js
let age = 11;
let ageAsstring = age.toString();  //字符串 “11”
```

该方法常用于数值，对象，字符串，和对象。

null和undefined没有这个方法。

多数情况下，toString不接受任何参数，但是在对数值调用这个方法时，可以接收一个底数参数来决定输出数值以什么样的进制返回。

```js
let num = 10;
console.log(num.toString(2));  //"1010"
console.log(num.toString(8));  //"12"
console.log(num.toString(16));  //"a"
```

#### String()

String算是对于toString的一个补充，String调用时遵循以下如下规则：

- 如果传入的值有toString的方法，则调用该方法
- 如果传入的值是null，返回null
- 如果传入的值是undefined，返回undefined

总的来说，String算是对toString的一个补充，一般在不确定所传入的值是否是null或者undefined时使用。

### 模板字面量

es6中新增了使用模拟板字面量定义字符串的功能，与单双引号不同的地方是，模板字面量可以保留字符串中的空格以及换行，可以跨行定义字符。使用反引号进行定义，这里不做过多解释。

### 字符串插值

字符串插值是模板字面量一个常用的特性，可以在一个连续定义中插入一个或者多个值。

```js
//以前字符串插值是这样实现的
let num = 5;
let str = 'second';
let newStr = num + 'to the' + str + 'power is' +  (num*num);
//复杂且结构不清晰
```

```js
//在es6中。使用模板字面量特性实现字符串插值需要使用  ${}
let num = 5;
let str = 'second';
let newStr = `${num} to the ${str} power is ${num*num}`;
//最终结构清晰且保留了空格等结构
```

字符串插值中支持调用方法和函数

