---
layout: post
title: JavaScript学习笔记
categories: Programming
description: JavaScript学习示例
keywords: JavaScript, Notes
---

在学习前端的时候记录的笔记，跟HTML笔记是一个系列的，大概也不会更新了。
<!--more-->

##### 例1：var和let的区别

```javascript
{
    var a = 1;
    let b = 2;
    const c = 3;
}
console.log('a = ' + a);
// console.log('b = ' + b);  //变量不存在
/*之前JS只有全局作用域和函数作用域，后来加入了块级作用域，let和const声明的变量/常量只在对应块中起作用，而var不受限制，因此建议用let和const*/
```

##### 例2：交换变量的两种方法
```javascript
let a = 1, b = 2;
console.log('Before: a = ' + a + '; b = ' + b);
//第一种方法：
let temp;
temp = a;
a = b;
b = temp;
console.log('After: a = ' + a + '; b = ' + b);
//第二种方法：适合数字
a = a + b;
b = a - b;
a = a - b;
console.log('After2: a = ' + a + '; b = ' + b);
```

##### 例3：js中的数据类型
```javascript
//JS的原始数据类型有number/string/boolean/null/undefined/object
//当一个变量只声明了没有初始化，即没有值，则其类型为undefined；当函数没有明确返回值时，返回值类型为undefined
let a = null;  //想让变量为null，必须手动设置
let t = typeof a;  //特殊的，null的类型是object
console.log(t);
```

##### 例4：进制的表示方式
```javascript
let bin = 0b10;  //二进制的表示方法
let dec = 10;
let oct = 0o10;  //八进制的表示方法
let hex = 0x10;  //十六进制的表示方法
console.log('bin = ', bin);
console.log('dec = ', dec);
console.log('oct = ', oct);
console.log('hex = ', hex);
```

##### 例5：number类型
```javascript
console.log(Number.MAX_VALUE);  //js中支持的数字最大值
console.log(Number.MIN_VALUE);  //js中支持的数字最小值
let n;
console.log(n+10);  //反馈NaN，NaN与任何值都不想等，包括它本身
console.log('n: ', isNaN(n));  //反馈true，但n的类型其实是undefined
let m = "10";
console.log('m: ', isNaN(m));  //反馈false，因为"10"也被认为是数字
```

##### 例6：string类型
```javascript
let str = "Hello World";
let string = 'Whatever, I\'m tired.';
console.log(str.length);  //获取字符串长度
console.log(string.length);
let n = 10;
let s = '20';
console.log(n + s);  //只要有一方是字符串，则+就是字符串拼接功能
console.log(n - s);  //若是-或者*或者/等，就按数字算，这是js的隐式转换（数据类型的自动互换）
```

##### 例7：类型转换
```javascript
//其它类型转数字类型：
//1. parseInt()
console.log(parseInt('10'));  //10
console.log(parseInt('10a'));  //10
console.log(parseInt('a10'));  //NaN
console.log(parseInt('1a0'));  //1
console.log(parseInt('10.2'));  //10
console.log(parseInt('10.2a'));  //10
//2. parseFloat()
console.log(parseFloat('10'));  //10
console.log(parseFloat('10a'));  //10
console.log(parseFloat('a10'));  //NaN
console.log(parseFloat('1a0'));  //1
console.log(parseFloat('10.2'));  //10.2
console.log(parseFloat('10.2a'));  //10.2
//3. Number()
console.log(Number('10'));  //10
console.log(Number('10a'));  //NaN
console.log(Number('a10'));  //NaN
console.log(Number('1a0'));  //NaN
console.log(Number('10.2'));  //10.2
console.log(Number('10.2a'));  //NaN
//其它类型转字符串类型：
//toString()或String()
let num = 10;
console.log(num.toString());  //变量有意义时用toString()方法
console.log(String(num));  //变量没有意义时用String()方法，比如变量没有定义(undefined)，或者为null时
//其它类型转布尔类型
//Boolean()
console.log(Boolean(1));  //非0数字为true
console.log(Boolean(0));  //0为false
console.log(Boolean(''));  //空字符串为false
console.log(Boolean('F'));  //非空字符串为true
console.log(Boolean(null));  //没有意义的类型都是false
console.log(Boolean(undefined));
```

##### 例8：逻辑运算符
```javascript
//expr1 && expr2  //逻辑与
//expr3 || expr4  //逻辑或
//! expr5  //逻辑非
```

##### 例9：一元运算符
```javascript
let n = 10;
let m = n++;  //先运算再自增
console.log('n=', n);
console.log('m=', m);
let p = 10;
let q = ++p;  //先自增再运算
console.log('p=', p);
console.log('q=', q);
```

##### 例10：if语句注意点
```javascript
let n = 10;
if (n > 0) {  //注意此处的“{”不可以换到下一行
    console.log(1);
}else if (n < 0) {
    console.log(-1);
}else{
    console.log(0);
}
```

##### 例11：prompt语句
```javascript
let s = prompt('Please enter something: ');  //弹框提醒输入
console.log('s=', s);
console.log('typeof(s)=', typeof s);
```

##### 例12：三元表达式
```javascript
let x = 10;
let y = 20;
let max = x > y ? x : y;  //三元运算符
console.log(max);
```

##### 例13：switch语句
```javascript
let n = parseInt(prompt('Please enter a number between 0 and 7:'));
switch (n) {
    case 1: console.log('Monday');break;  //注意不要漏掉break
    case 2: console.log('Tuesday');break;
    case 3: console.log('Wednesday');break;
    case 4: console.log('Thursday');break;
    case 5: console.log('Friday');break;
    case 6: console.log('Saturday');break;
    case 7: console.log('Sunday');break;
    default: console.log('Sorry, I cannot understand you');
}
```

##### 例14：switch的注意点
```javascript
let m = parseInt(prompt('Please choose a month:'));
switch (m) {
    case 1:
    case 3:
    case 5:
    case 7:
    case 8:
    case 10:
    case 12: console.log('31 days');break;  //但是尽量不要这么用
    case 4:
    case 6:
    case 9:
    case 11: console.log('30 days');break;
    case 2: console.log('28 days');break;
    default: console.log('Sorry, that does not exist');
}
```

##### 例15：document.write()
```javascript
document.write("<a href='https://www.baidu.com'>百度</a>");  //一般用于在页面上写超链接
```

##### 例16：一元运算符的注意点
```javascript
let i = 1;
let j = i++ + i++;  //j = 3
//第一步，i=1，所以j=1+i++
//第二部，i自增，此时i=2
//第三步，j=1+2
//第四步，i自增，此时i=3
//所以j=3
let k = i++ + ++i;  //k = 8
let l = ++i + i++;  //l = 12
let m = ++i + ++i;  //m = 17
```

##### 例17：数组
```javascript
//创建数组的两种方式
let a = [];
let b = new Array(5);  //可以设定数组长度，每个元素初始值为undefined
let c = [1, 2, 3, 4, 5, 6];
let d = new Array(1, 2, 3, 4, 5, 6);  //同上
let l = c.length;
let e = [1, 'a', "hello", 5.2, true, null];  //数组内数据类型可以不一样
c[6] = 7;  //数组可以这样赋值
c[c.length] = 7;  //这样可以理解为在数组最后追加值
c[8] = 9;  //此处也是可以的，但这样之后默认c[7]=undefined
```

##### 例18：arguments伪数组
```javascript
function fun(x, y) {
    console.log(arguments);  //伪数组，包含了函数传入的全部参数(实参)
    console.log(arguments.length);  //获取参数(实参)个数
}
fun(1, 2, 3, 4, 5, '6');
```

##### 例19：函数表达式
```javascript
//函数的另一种方式：函数表达式
let f = function () {  //不需要给函数名了
    console.log(1)
};  //注意有分号
f();  //调用
```

##### 例20：匿名函数一次性自调用
```javascript
//匿名函数的调用
(function () {
    console.log(1);
})();  //声明的同时直接调用，是一次性的
```

##### 例21：变量作用域
```javascript
var a = 1;  //声明一个全局变量，已不建议这样声明
let b = 2;  //声明一个块级作用域的局部变量
const c = 3;  //声明一个块级作用域的局部常量
d = 4;  //声明一个隐式全局变量
delete d;  //delete只能删除隐式全局变量
let x = y = z = 1;  //此处x是块级作用域的局部变量，但y和z都是隐式全局变量
```

##### 例22：预解析
```javascript
//var num;  //预解析后的实际声明位置（变量的提升只会提前到变量作用域的最前面）
console.log(num);  //此处输出undefined
var num = 10;  //运行前变量被预解析，将声明（var num;）提前，赋值依然在这个位置；但换做let声明就会报错……
fun();  //运行前函数被预解析（也被提前，但变量更靠前），此处正常执行不报错
function fun() {
    console.log(1);
}  //let声明的变量貌似不会提升，所以这里只注意函数的预解析就好了
```

##### 例23：利用系统构造函数创建对象
```javascript
//利用系统构造函数创建对象（实例化）
let person = new Object();
person.name = 'Jack';  //添加属性
person.age = 19;
person.eat = function () {  //添加方法（注意是匿名函数）
    console.log(this.name+' is eating...')  //调用当前对象的属性，用this
};
console.log(person instanceof Object);  //判断person是不是Object类型
```

##### 例24：工厂模式创建对象
```javascript
//工厂模式创建对象（有缺陷）
function createPerson(name, age) {
    let aPerson = new Object();
    aPerson.name = name;
    aPerson.age = age;
    aPerson.eat = function () {
        console.log(this.name+' is eating...');
    };
    return aPerson;
}
let p1 = createPerson('Jack', 19);
let p2 = createPerson('Jerry', 20);
```

##### 例25：自定义构造函数创建对象
```javascript
//自定义构造函数创建对象
function Person(name, age) {  //变量名首字母大写，标识这是一个构造函数（本质上与函数并无不同）
    this.name = name;  //设置属性方法用this
    this.age = age;
    this.eat = function () {
        console.log(this.name + ' is eating...');
    };
}
let p1 = new Person('Jack', 19);  //实例化
let p2 = new Person('Tom', 20);
console.log(p1.name);
console.log(p2['name']);  //访问属性的另一种方法
```

##### 例26：字面量的方式创建对象
```javascript
//字面量的方式创建对象
//缺陷：这是一次性对象，不能传值（像自定义构造函数那样）
let person = {
    name: 'Jack',
    age: 19,
    eat: function () {
        console.log(this.name+' is eating...');
    },
};
person.eat();
```

##### 例27：json数据及遍历方法
```javascript
let json = {  //花括号内键值对都是用双引号引起来的，则认为是json数据类型
    "name": "Jack",
    "age": "19",
    "sex": "male",
};
for (let key in json) {  //json的遍历方法（注意json里的数据是无序的）
    console.log(json[key]);
}
```

##### 例28：内置对象Math
```javascript
console.log(Math.PI);  //更多关于内置对象Math的属性和方法查阅MDN
console.log(Math.random());
```

##### 例29：String常见方法
```javascript
//string是一个值类型，String是一个引用类型
//let s = new String(...);  //创建字符串，s是object类型
//let s = String(...);  //这样更好，s是string类型
//let s = "...";  //通常这样创建，s是string类型
let str = 'hello';
console.log(str.length);
console.log(String.fromCharCode(65, 66));  //根据UTF-16代码单元数字查字符(可以一次查好几个)
console.log(str.concat(' world,', ' emm'));  //拼接字符串
console.log(str.indexOf('e'));  //搜索第一次出现指定字符(串)的位置(从左往右)
console.log(str.lastIndexOf('e'));  //搜索第一次出现指定字符(串)的位置(从右往左)
console.log(str.replace('e', 'E'));  //返回新字符串，原字符串不变
console.log(str.slice(1, 3));  //切片，支持负数，见MDN文档
console.log(str.split('e'));  //字符串分割，返回一个数组
console.log(str.toLocaleUpperCase());  //转大写，转小写见MDN文档
console.log('  middle   '.trim());  //删除两端空格
```

##### 例30：Array常见方法
```javascript
let a = [1, 2, 3];
let b = [4, 5, 6];
console.log(Array.isArray(a));  //判断是不是一个数组
console.log(Array.from('hello'));  //将一个可迭代对象转成数组
console.log(a.concat(b));  //拼接数组，可以拼好几个
a.forEach(function (i) {  //匿名函数内部可以是其他操作
    console.log(i);
});
console.log(a.join('|'));  //拼接数组元素
console.log(a.map(function (i) {  //对每个元素操作后存到新数组中
    return i + 1;
}));
console.log(a.reverse());  //返回新数组，原数组也被改变
//sort方法不稳定，详见MDN文档
console.log(a.slice(1, 2)); //数组切片
//splice方法见MDN文档，一般用于插入或替换元素
```

##### 例31：Array的every方法和filter方法
```javascript
function arrTest(a) {
    return a < 10;
}
let a = [8,9,10,11,12];
let b = [1,2,3,4,5,6];
//every方法可以测试数组中元素的值是否都能通过测试（函数），更多见MDN文档
console.log(a.every(arrTest));  //false
console.log(b.every(arrTest));  //true
//filter方法创建一个新数组，返回通过测试（函数）的元素
console.log(a.filter(arrTest));  //[8,9]
//一般把函数写在方法内，用匿名函数
```

##### 例32：基本包装类型
```javascript
//变量不能调用属性和方法
//对象可以调用属性和方法
//如果一个基本类型变量在执行时调用了属性和方法，那这个变量就不是一个普通变量了，而是一个基本包装类型对象
//这个变量的类型也不是基本类型了，而是基本包装类型
//基本包装类型有number/string/boolean
let a = false;  //a是一个基本类型
let b = Boolean(false);  //转换，b是一个基本类型
let c = new Boolean(false);  //创建对象，c是一个（基本包装类型）对象
//特别的（恶心的）：
//true&&对象===>对象
//对象&&true===>true
```

##### 例33：数组清空的方法
```javascript
let arr = [10, 24, 2, 42, 30, 65, 64];
arr.length = 0;
console.log(arr); //此时数组被清空了
//还可以循环调用arr.pop()，直到数组被清空
```

##### 例34：XML和HTML的不同
```xml
<?xml version="1.0" encoding="UTF-8" ?> <!--这一行必须有，标识为XML文件-->
<school>
    <!--必须有一个根标签，名字任意，内嵌标签名字任意，属性任意-->
    <class name="E1">
        <numbers>35</numbers>
    </class>
    <class name="E2">
        <numbers>37</numbers>
    </class>
</school>
<!--HTML：侧重于展示信息，展示数据-->
<!--XML：侧重于存储数据-->
<!--至于怎么存储，还不会……-->
```

##### 例35：相关概念
```javascript
//文档（document）：一个页面就是一个文档
//元素（element）：页面的所有标签都是元素，元素可以看成对象
//结点（node）：页面的所有内容都是结点，标签，属性，文本，都是结点
//范围：文档 > 结点 > 元素
//DOM树就是各级标签嵌套形成的树状图
```

##### 例36：点击注册事件
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
</head>
<body>
<input type="button" value="点我试试" id="btn" />
<!--点击操作是一个事件-->
<!--是一个事件就有事件源，触发和响应-->
<!--此处：按钮是事件源，点击按钮是触发事件，弹框是响应事件-->
<script>
    document.getElementById("btn").onclick = function () {
        alert("让你点你还真点！");
    }
    /*document.getElementById("btn")返回的是一个元素，即一个标签，一个对象，标签/属性有一个属性叫onclick
    * 将一个函数（匿名函数，避免重名）赋值给它，这样当点击事件（onclick）发生时，就会执行函数*/
</script>
</body>
</html>
```

##### 例37：点击修改图片
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        img {
            display: block;
        }
    </style>
</head>
<body>
<input type="button" value="更换图片" id="btn_change_pic" />
<input type="button" value="设置宽高" id="btn_set_wh" />
<img src="images/firefox_logo.png" alt="firefox_logo" id="image" />
<script>
    document.getElementById("btn_change_pic").onclick = function () {
        document.getElementById("image").src = "images/firefox_logo.jpg";
    };
    document.getElementById("btn_set_wh").onclick = function () {
        document.getElementById("image").width = "116"; /*注意没有px*/
        document.getElementById("image").height = "100";
    };
</script>
</body>
</html>
```

##### 例38：修改标签内文本
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
</head>
<body>
<input type="button" value="更改文本" id="btn_change_text" />
<p id="p_text">一个好消息，一个坏消息</p>
<script>
    document.getElementById("btn_change_text").onclick = function () {
        document.getElementById("p_text").innerText = "哈哈哈骗你的，两个都是坏消息";
        /*规律：但凡双标签内的文本，都可以用innerText属性*/
    };
</script>
</body>
</html>
```

##### 例39：修改链接
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
</head>
<body>
<input type="button" value="更改链接" id="btn_change_link" />
<a href="#" id="link">必应搜索</a>
<script>
    document.getElementById("btn_change_link").onclick = function () {
        let linkElement = document.getElementById("link");
        linkElement.innerText = "哈哈哈骗你的，这次才是真的链接";
        linkElement.href = "https://cn.bing.com";
        /*理论上可以这样，但不知道为什么WebStorm不给提示*/
    };
</script>
</body>
</html>
```

##### 例40：批量修改文本
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
</head>
<body>
<input type="button" value="批量更改文本" id="btn_change_texts" />
<p>我是一个无情的复读机</p>
<p>我是一个无情的复读机</p>
<input type="button" value="批量修改指定文本" id="btn_chg_specific_texts" />
<div id="first_div">
    <span>一组一</span>
    <span>一组二</span>
</div>
<div id="second_div">
    <span>二组一</span>
    <span>二组二</span>
</div>
<script>
    document.getElementById("btn_change_texts").onclick = function () {
        let tags = document.getElementsByTagName("p");
        for (let i = 0; i < tags.length; i++) {
            tags[i].innerText = "我是一个修改后的复读机";
        }
    };
    document.getElementById("btn_chg_specific_texts").onclick = function () {
        let specificDiv = document.getElementById("first_div");
        let specificTags = specificDiv.getElementsByTagName("span"); /*返回一个伪数组*/
        for (let j = 0; j < specificTags.length; j++) {
            specificTags[j].innerText = "被改了";
        }
    };
</script>
</body>
</html>
```

##### 例41：修改文本框的值
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
</head>
<body>
<input type="button" value="一键修改文本框" id="btn_chg_textboxes" /><br />
<input type="text" value="" /><br />
<input type="text" value="" /><br />
<input type="text" value="" /><br />
<script>
    document.getElementById("btn_chg_textboxes").onclick = function () {
        let inputs = document.getElementsByTagName("input");
        for (let i = 0; i < inputs.length; i++) {
            if (inputs[i].type === "text") { /*选定文本框*/
                inputs[i].value = "被改了";
            }
        }
    };
</script>
</body>
</html>
```

##### 例42：修改自身属性
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
</head>
<body>
<input type="button" value="我是按钮" id="btn" />
<script>
    let input = document.getElementById("btn");
    input.onclick = function () {
        this.type = "text"; /*修改自身属性可以用this*/
        this.value = "我是文本框";
        this.id = "txt";
    };
</script>
</body>
</html>
```

##### 例43：按钮排他性
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
</head>
<body>
<input type="button" value="没被点"/>
<input type="button" value="没被点"/>
<input type="button" value="没被点"/>
<script>
    let inputs = document.getElementsByTagName("input");
    for (let i = 0; i < inputs.length; i++) {
        inputs[i].onclick = function () {
            for (let j = 0; j < inputs.length; j++) {
                inputs[j].value = "没被点";
            }//end for
            inputs[i].value = "被点了"; /*这里用inputs[i]有没有bug呢？还是用this好呢？*/
        };
    }//end for
</script>
</body>
</html>
```

##### 例44：小小的封装
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
</head>
<body>
<input type="button" value="更改图片" id="btn" />
<img src="images/logo_firefox.png" alt="" id="image" />
<script>
    function my$(id) {
        return document.getElementById(id);
    } /*封装功能*/
    my$("btn").onclick = function () {
        my$("image").src = "images/logo_firefox.jpg";
    };
</script>
</body>
</html>
```

##### 例45：一个规律
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
</head>
<body>
<input type="button" value="恢复默认" id="btn" />
<input type="radio" value="male" name="sex" id="sex_male" /><label for="sex_male">男</label>
<input type="radio" value="female" name="sex" id="sex_female" /><label for="sex_female">女</label>
<input type="radio" value="unknown" name="sex" id="sex_unknown" checked="checked" /><label for="sex_unknown">保密</label>
<script>
    function my$(id) {
        return document.getElementById(id);
    }
    my$("btn").onclick = function () {
        my$("sex_unknown").checked = true;
        /*如果一个标签内的某个属性只有一个值，且这个值就是属性本身，
        * 那么在写js代码进行DOM操作时，这个属性值，是布尔类型就可以了
        * 例如：checked/selected/disabled/readonly等*/
    };
</script>
</body>
</html>
```

##### 例46：关于只有一个值的属性
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
</head>
<body>
<textarea name="texts" id="txt" cols="30" rows="5" readonly>
    试试看能不能删掉我，
    删不掉吧，臭弟弟。
</textarea>
<!--对于只有一个值且与属性名相同的属性，可以只写属性-->
<input type="button" value="修改文本" id="btn_chg_txt" disabled/>
<input type="button" value="允许修改" id="btn_alw_chg"/>
<script>
    function my$(id) {
        return document.getElementById(id);
    }
    my$("btn_alw_chg").onclick = function () {
        my$("btn_chg_txt").disabled = false;
    };
    my$("btn_chg_txt").onclick = function () {
        // my$("txt").readonly = false; /*这里貌似不行，不知道为什么*/
        my$("txt").value = "改了";
    };
</script>
</body>
</html>
```

##### 例47：设置样式
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        #dv {
            width: 300px;
            height: 200px;
            background-color: red;
        }
    </style>
</head>
<body>
<input type="button" value="设置样式" id="btn" />
<div id="dv"></div>
<script>
    function my$(id) {
        return document.getElementById(id);
    }
    my$("btn").onclick = function () {
        my$("dv").style.width = "400px";
        my$("dv").style.height = "300px";
        my$("dv").style.backgroundColor = "green";
        /*规律：css中的多单词属性在js的DOM操作中统一换成驼峰命名法*/
    }
</script>
</body>
</html>
```

##### 例48：一键控制显示和隐藏
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        #dv {
            width: 300px;
            height: 200px;
            background-color: red;
        }
    </style>
</head>
<body>
<input type="button" value="显示/隐藏" id="btn"/>
<div id="dv" hidden></div>
<script>
    function my$(id) {
        return document.getElementById(id);
    }
    my$("btn").onclick = function () {
        my$("dv").hidden = my$("dv").hidden !== true;
        /*一键控制显示和隐藏*/
    };
</script>
</body>
</html>
```

##### 例49：设置样式2
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        .cls {
            width: 300px;
            height: 200px;
            background-color: red;
            border: 2px solid purple;
        }
    </style>
</head>
<body>
<input type="button" value="显示" id="btn"/>
<div id="dv"></div>
<script>
    function my$(id) {
        return document.getElementById(id);
    }
    my$("btn").onclick = function () {
        my$("dv").className = 'cls';
        /*在js进行DOM操作时，若设置一个标签的class值，不用class关键字，而用className属性
        * 一般要设置的样式多于三个时就可以采取这种方式了（见例47）*/
    };
</script>
</body>
</html>
```


