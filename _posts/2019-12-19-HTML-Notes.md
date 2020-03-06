---
layout: post
title: HTML 学习笔记
categories: Programming
description: HTML5学习示例
keywords: HTML5, Notes
---

许久之前突然想搞前端的时候记录的学习笔记，虽然作为一个后端爱好者以后可能不会真的再去手写前端了，但该知道的，还是要知道一些的。

<!--more-->

##### 例1：HTML骨架

```html
<!DOCTYPE html>  <!--文档类型，声明html版本，此处是html5-->
<html lang="en">  <!--语言默认英语，可以不写-->
<head>
    <meta charset="UTF-8">  <!--字符集-->
    <title>Default</title>
</head>
<body>
</body>
</html>
```

##### 例2：单标签
```html
<body>
    <hr />  <!--水平线，单标签，习惯加一个空格-->
    <br />  <!--换行-->
</body>
```

##### 例3：排版标签
```html
<body>
    <div>This is the first division.</div>  <!--无语义标签，一个占一行-->
    <div>This is the second division.</div>
    <span>This is the first span.</span>  <!--无语义标签，一行可以有多个-->
    <span>This is the first span.</span>
</body>
```

##### 例4：文本格式化标签
```html
<body>
    <strong>This is strong.</strong>  <!--加粗，有强调的意思，语义强烈，在XHTML建议用strong-->
    <b>This is bold.</b>  <!--加粗，没有强调的意思-->
    <i>This is italic.</i>
    <em>This is also italic.</em>  <!--在XHTML建议用em-->
    <s>This is strikethrough.</s>  <!--删除线-->
    <del>This is deletion.</del>  <!--删除线，语义更强，在XHTML建议用del-->
    <u>This is underline.</u>  <!--下划线，不建议在XHTML中用u-->
    <ins>This is insert.</ins>  <!--下划线-->
    <!--b i s u无语义，strong em del ins有语义-->
</body>
```

##### 例5：图像标签
```html
<body>
    <img src="firefox_log.png" alt="firefox logo" title="firefox" width="51" />
    <!--src图片路径；alt图片不能显示时的提示文本；title鼠标悬停时的显示文本；只改width或height时图片会等比缩放-->
</body>
```

##### 例6：链接标签
```html
<body>
    <a href="http://www.baidu.com" target="_blank">百度</a>
    <!--超链接的标准模式：href跳转的网址；target跳转方式，“_blank”打开新窗口，“_self”覆盖本窗口-->
    <a href="#">链接暂无</a>
    <!--显示为超链接，但暂时没有链接地址-->
    <a href="https://home.firefoxchina.cn/"><img src="firefox_log.png" alt="firefox logo" /></a>
    <!--链接标签内除了文本也可以是图片，表格，视频，音频等-->
</body>
```

##### 例7：锚点定位
```html
<body>
    <a href="#target">当前位置</a>  <!--把href的属性值设为“#id”，可以在本页跳转到id是相应值的标签（页面可滚动）-->
    <!--此处省略一大堆内容-->
    <p id="target">目标位置</p>
</body>
```

##### 例8：base标签
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <base target="_blank" />  <!--使当前页面所有的链接都在新窗口打开-->
</head>
<body>
    <a href="http://www.baidu.com">百度</a>
    <a href="http://www.sina.com" target="_self">新浪</a>  <!--如果只想让个别的链接在当前窗口打开，可以单独加属性-->
    <a href="http://www.163.com">网易</a>
</body>
</html>
```

##### 例9：无序列表
```html
<body>
    <h3>水果：</h3>
    <ul>  <!--ul无序列表，内部只能有li标签，但li标签内可以有其他标签-->
        <li>苹果</li>
        <li>香蕉</li>
        <li>
            <h4>橙子</h4>
            <p>脐橙</p>
            <p>甘橙</p>
        </li>
    </ul>
</body>
```

##### 例10：有序列表
```html
<body>
    <h3>金牌榜：</h3>
    <ol>  <!--ol有序列表，内部只能由li标签，li标签内可以有其他标签-->
        <li>美国</li>
        <li>英国</li>
        <li>中国</li>
    </ol>
</body>
```

##### 例11：自定义列表
```html
<body>
    <dl>  <!--dl自定义列表，有dt和dd两个标签，dd是对dt的解释-->
        <dt>潍坊</dt>
        <dd>诸城</dd>
        <dd>青州</dd>
        <dd>安丘</dd>
        <dt>青岛</dt>
        <dd>崂山</dd>
        <dd>鱼山</dd>
        <dd>城阳</dd>
    </dl>
</body>
```

##### 例12:表格标签
```html
<body>
    <table width="300" border="1" align="center" cellspacing="10" cellpadding="10">  <!--整个表格，一个大盒子-->
    <!--width单元格宽度；border表格边框宽度；align表格对齐方式；cellspacing单元格外边距；cellpadding单元格内边距-->
    <!--但是建议 三参为0，即border/cellspacing/cellpadding-->
        <caption>个人信息表</caption>  <!--表格标题标签，紧跟在table之后-->
        <thead>        <!--表格头部-->
            <tr>  <!--表格内的一行-->
                <th>姓名</th>  <!--表头单元格，基本单元格的加强版，加粗居中-->
                <th>性别</th>
                <th>电话</th>
            </tr>
        </thead>
        <tbody>  <!--表格身体-->
            <tr>
                <td>小王</td>  <!--基础单元格-->
                <td>女</td>
                <td>110</td>
            </tr>
            <tr>
                <td>小李</td>
                <td>男</td>
                <td>120</td>
            </tr>
        </tbody>
    </table>
</body>
```

##### 例13：合并单元格
```html
<body>
    <table width="400" border="1">  <!--合并单元格原则，从上向下，从左向右-->
        <tr>
            <td rowspan="2">123</td>  <!--跨行合并11和21单元格-->
            <td>123</td>
            <td>123</td>
        </tr>
        <tr>
        <!--<td>123</td>-->  <!--21单元格消失-->
            <td>123</td>
            <td>123</td>
        </tr>
        <tr>
            <td>123</td>
            <td colspan="2">123</td>  <!--跨列合并32和33单元格-->
        <!--<td>123</td>-->  <!--33单元格消失-->
        </tr>
    </table>
</body>
```

##### 例14：input控件
```html
<body>
    <!--详细input控件属性见OneDrive/Programming/HTML+/input控件属性.png-->
    <div>  <!--将label标签的属性for和input标签的属性id对应-->
        <label for="usr">用户名：</label>
        <input id="usr" type="text" value="username" />
    </div>
    <div>  <!--如果是一组单选框，必须加name属性，并命名为同样的值，以保证单选-->
        男<input type="radio" name="sex" />
        女<input type="radio" name="sex" />
    </div>
    <div>  <!--如果是一组复选框，也建议加name属性，并命名为同样的值-->
        苹果<input type="checkbox" name="fruit" />
        香蕉<input type="checkbox" name="fruit" />
        橙子<input type="checkbox" name="fruit" />
    </div>
</body>
```

##### 例15：下拉菜单
```html
<body>
    <label for="year">选择年份</label>
    <select id="year">
        <option>1990年</option>
        <option selected="selected">1991年</option>
        <option>1993年</option>
    </select>
</body>
```

##### 例16：表单域
```html
<body>
    <form action="" method="post">
    <!--action接受表单数据服务器程序的url地址；method数据提交方式，有get/post两种-->
        <label for="usr">用户名：</label><input id="usr" type="text" /><br /><br />
        <label for="pwd">密　码：</label><input id="pwd" type="password" /><br /><br />
        <input type="submit" value="提交" />
        <input type="reset" value="重置" />
    </form>
</body>
```

##### 例17：行内样式表和内部样式表
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        h3 {  /*内部样式表格式：选择器 {属性1:值1;属性2:值2;}*/
            color: skyblue;
        }
    </style>
</head>
<body>
    <p style="color: red;">文本文本</p>  <!--行内样式表-->
    <h3>文本文本</h3>
</body>
</html>
```

##### 例18：外部样式表
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <link rel="stylesheet" href="demo.css" />  <!--外部样式表-->
<!--demo.css中的内容：-->
<!--div {
        color: violet;
    }-->
</head>
<body>
    <div>文本文本</div>
    <div>文本文本</div>
</body>
</html>
```

##### 例19：标签选择器/类选择器/id选择器/通配符选择器
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        p {  /*标签选择器*/
            color: blue;
        }
        .first-div {  /*类选择器，命名时建议用-而不是_*/
            color: grey;
        }
        #last {  /*id选择器*/
            color: pink;
        }
        * {  /*通配符选择器，选中所有标签，使用较少*/
            font-weight: bold;
        }
    </style>
</head>
<body>
    <p>文本文本</p>
    <div class="first-div">文本文本</div>  <!--可以多个标签有相同的类-->
    <div id="last">文本文本</div>
</body>
</html>
```

##### 例20：多类名选择器
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        .red {
            color: red;
        }
        .font20 {
            font-size: 20px;
        }
        .fontWeight {
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="red font20">文本文本</div>  <!--多类名选择器-->
    <div class="red fontWeight">文本文本</div>  <!--样式显示效果与HTML标签内的类名顺序无关，与CSS中样式书写的上下顺序有关，后来者居上原则-->
    <div>文本文本</div>
    <div class="red">文本文本</div>
</body>
</html>
```

##### 例21：字体font-weight
```html
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        * {
            font-family: "宋体", Arial;
            /*如果字体名称中有空格、“#”或“$”，需要用引号引起来，多个字体的执行顺序是从前往后找，如果找到该字体就不会使用后面的字体，如果都没找到则使用默认字体*/
        }
    </style>
</head>
```

##### 例22：字体font-style
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        em {
            font-style: normal;  /*normal用的最多，通常用于一些重要但又不要斜体的文字*/
        }
    </style>
</head>
<body>
    <em>把斜体摆正</em>
</body>
</html>
```

##### 例23：字体连写
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        h1 {
            font: italic 700 30px/40px "宋体";
            /*选择器 {font: font-style font-weight font-size/line-height font-family;}*/
            /*字体样式连写，顺序格式不能乱，中间用空格隔开，font-size和font-family不能省略，其他样式可以有省略*/
        }
    </style>
</head>
<body>
    <h1>字体连写</h1>
</body>
</html>
```

##### 例24：更多样式
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        h1 {
            text-align: center;  /*让h1内的 文字 居中水平对齐，标签本身占一行，没有所谓居中不局中*/
            line-height: 30px;  /*行与行之间的距离*/
            text-decoration: underline;  /*文本装饰，常用none和underline*/
        }
        p {
            text-indent: 2em;  /*首行缩进，如果是文字，2em代表缩进两个汉字*/
        }
    </style>
</head>
<body>
    <h1>更多样式</h1>
    <p>在昨日丽江嘉云昊队主帅李虎就缺席了赛前的新闻发布会，当时俱乐部给出的解释是李虎由于身体欠佳，去医院接受治疗。然而今日李虎出现在俱乐部时，向记者否认了这一说法，并且坦言已经向俱乐部提出了辞呈。</p>
</body>
</html>
```

##### 例25：后代选择器
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        div p {  /*后代选择器*/
            color: red;
        }
        ul li {  /*通常对于表格会使用这种格式，因为可能有多个表格，有多种li*/
            color: #daa520;
        }
    </style>
</head>
<body>
    <p>外层</p>
    <div>
        <p>内层</p>
    </div>
    <ul>
        <li>苹果</li>
        <li>梨子</li>
        <li>香蕉</li>
    </ul>
</body>
</html>
```

##### 例26：子代选择器
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        ul li > a {  /*子代选择器，只选择子一代，使用较少*/
            color: red;
        }
    </style>
</head>
<body>
    <ul>
        <li>
            <a href="#">一代</a>
            <div>
                <a href="#">二代</a>
            </div>
        </li>
    </ul>
</body>
</html>
```

##### 例27：交集和并集选择器
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        p.last {  /*交集选择器，用得较少*/
            color: green;
        }
        span, a {  /*并集选择器，用得较多*/
            color: blue;
        }
    </style>
</head>
<body>
    <div class="last">交集选择器</div>
    <p class="last">交集选择器</p>
    <span>并集选择器</span>
    <a href="#">并集选择器</a>
    <h3>并集选择器</h3>
</body>
</html>
```

##### 例28：伪类选择器
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>  /*伪类选择器，以:开头，以下四个属性顺序不能变*/
        a:link {  /*未访问的链接*/
            color: #666666;
            text-decoration: none;
            font-weight: 700;
        }
        a:visited {  /*访问过的链接*/
            color: #daa520;
        }
        a:hover {  /*鼠标经过触发，实际开发一般只用这个*/
            color: #f10215;
        }
        a:active {  /*点击时的链接*/
            color: green;;
        }
    </style>
</head>
<body>
    <a href="#">伪类选择器</a>
</body>
</html>
```

##### 例29：块级标签和行内标签/显示模式
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        div {  /*块级标签一个占一行，可以设宽度高度居中等格式*/
            width: 200px;
            height: 200px;
            background-color: pink;
            display: inline;  /*标签显示模式可改*/
        }
        span {  /*行内标签仅靠自身的字体大小和图像尺寸支撑结构，不能设置宽高居中等格式*/
            width: 100px;
            height: 100px;
            background-color: purple;
            display: block;
        }
    </style>
</head>
<body>
    <div>块级标签</div>
    <div>块级标签</div>
    <span>行内标签</span>  <!--行内标签内只能放文本或其他行内标签（a特殊，a里面可以放块级元素），但p、h1等文字类标签虽然是块级元素，也尽量只放文本-->
    <span>行内标签</span>
    <!--行内块元素：<img />、<input />、<td>是行内元素（inline-block），但可以设置宽高对齐等-->
</body>
</html>
```

##### 例30：导航栏案例
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Demo</title>
    <style>
        .nav {
            text-align: center;  /*行内元素和行内块元素可以看作文本，因此此处可以这么用*/
        }
        .nav a {
            width: 120px;
            height: 50px;  /*宽高要与图片宽高一致*/
            display: inline-block;  /*不要忘记这个，否则无法加背景图*/
            background-image: url(bg.png);  /*加入背景图的方法*/
            text-decoration: none;
            line-height: 50px;  /*令行高等于盒子高度，可以让单行文本垂直居中*/
        }
        .nav a:hover {
            background-image: url(bgc.png);
        }
    </style>
</head>
<body>
    <div class="nav">
        <a href="#">导航首页</a>
        <a href="#">导航首页</a>
    </div>
</body>
</html>
```

##### 例31：样式的层叠性和继承性
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        .cascade {
            color: red;
            font-size: 25px;
        }
        .cascade {  /*层叠性，此处的样式覆盖了之前的样式，此处没设置大小，因此大小没覆盖*/
            color: green;
        }
        .succession {  /*继承性，此处样式没有直接应用于p标签，但它是.succession的子标签，继承了父标签的样式；但不是所有的样式都继承，主要是文本样式，行高不会继承*/
            color: blue;
        }
    </style>
</head>
<body>
    <div class="cascade">文本文本</div>
    <div class="succession">
        <p>文本文本</p>
    </div>
</body>
</html>
```

##### 例32：样式优先级
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        * {
            color: hotpink!important;  /*优先级最高*/
        }
        .demo {  /*类选择器优先于标签选择器*/
            color: green;
        }
        div {
            color: red;
        }
    </style>
</head>
<body>
    <!--选择器优先级/权重：
    继承或*：0,0,0,0
    标签：0,0,0,1
    类或伪类：0,0,1,0
    id：0,1,0,0
    行内样式：1,0,0,0
    !important：∞
    注意：1、相同权重的按层叠性算
    2、权重可以叠加，例div p {}权重是0,0,0,2；特殊0,0,0,5+0,0,0,5=0,0,0,10
    3、继承的权重0，即使加了!important也是0
    -->
    <div class="demo">文本文本</div>
</body>
</html>
```

##### 例33：背景
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        div {
            width: 500px;
            height: 500px;
            background-color: pink;
            background-image: url(firefox_log.png);
            background-repeat: no-repeat;  /*详细的、其余的见手册，下同*/
            background-position: center top;
            /*背景样式也可以简写，顺序如下（非强制）
            background: 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置*/
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```

##### 例34：背景半透明
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        body {
            background-color: pink;
        }

        div {
            width: 300px;
            height: 200px;
            color: white;
            /*background-color: black;*/
            background-color: rgba(0, 0, 0, 0.5);  /*a代表alpha，指透明度，范围为0~1*/
        }
    </style>
</head>
<body>
<div>背景半透明</div>
</body>
</html>
```

##### 例35：边框
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        #fir {
            width: 200px;
            height: 200px;
            border-color: red;
            border-width: 2px;
            border-style: solid; /*dotted(点线)、dashed(虚线)*/
            /*border连写，没有顺序要求，但习惯上width style color*/
            /*border: 2px dashed red*/
        }

        #sec {
            width: 300px;
            height: 300px;
            border-top: 2px dashed pink; /*只显示上边框*/
            border-bottom: 2px dotted green; /*只显示下边框，左右同理*/
        }
    </style>
</head>
<body>
<div id="fir"></div>
<br/>
<div id="sec"></div>
</body>
</html>
```

##### 例36：相邻边框覆盖
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        table {
            width: 300px;
            height: 300px;
        }
        td {
            text-align: center;
        }
        table, td {
            border: 2px solid red;
            border-collapse: collapse; /*相邻边框覆盖而非紧贴*/
        }
    </style>
</head>
<body>
<table cellpadding="0" cellspacing="0">
    <tr>
        <td>合并</td>
        <td>合并</td>
        <td>合并</td>
    </tr>
    <tr>
        <td>相邻</td>
        <td>相邻</td>
        <td>相邻</td>
    </tr>
    <tr>
        <td>边框</td>
        <td>边框</td>
        <td>边框</td>
    </tr>
</table>
</body>
</html>
```

##### 例37：内边距
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        a {
            display: inline-block;
            height: 40px;
            line-height: 40px;
            background-color: #cccccc;
            text-decoration: none;
            /*padding: 10px; !*默认上下左右内边距都是10*!*/
            /*padding: 10px 20px; !*上下 左右*!*/
            /*padding: 10px 20px 30px; !*上 左右 下*!*/
            /*padding: 10px 20px 30px 40px; !*上 右 下 左（顺时针）*!*/
            padding-left: 10px; /*实现了不同数量的文字统一设置左右内边距*/
            padding-right: 10px;
            /*用padding会撑开盒子，所以要保证总盒子宽高不变，需要改原盒子宽高，border也会影响盒子大小*/
        }
    </style>
</head>
<body>
<a href="#">首页</a>
<a href="#">移动客户端</a>
</body>
</html>
```

##### 例38：新浪导航栏（外边距）
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        * { /*取消掉浏览器默认的内外边距；外边距margin的用法与padding相同(实际上喜欢把所有要消除内外边距的标签都列出来)*/
            margin: 0;
            padding: 0;
        }
        .nav {
            height: 44px;
            background-color: #fcfcfc;
            border-top: 3px solid #ff8500;
            border-bottom: 1px solid #F3F4F5;
        }
        .nav a {
            display: inline-block;
            height: 44px;
            line-height: 44px;
            text-decoration: none;
            color: #555555;
            font-size: 14px;
            padding: 0 10px;
        }
        .nav a:hover {
            background-color: #EDEEF0;
            color: #ff8500;
        }
    </style>
</head>
<body>
<div class="nav">
    <a href="#">设为首页</a>
    <a href="#">手机新浪网</a>
    <a href="#">移动客户端</a>
    <a href="#">网站导航</a>
</div>
</body>
</html>
```

##### 例39：水平居中
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        .test {
            width: 300px;
            height: 300px;
            text-align: center; /*文字（也可以是行内元素或行内块元素）居中对齐*/
            background-color: pink;
            margin: 0 auto; /*居中对齐，应用于块级元素，且指定了宽度（上下为0，左右自动铺满）*/
        }
    </style>
</head>
<body>
<div class="test">文字</div>
</body>
</html>
```

##### 例40：外边距合并
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        div {
            width: 300px;
            height: 300px;
            background-color: purple;
        }
        .sec {
            background-color: pink; /*利用优先级覆盖*/
            margin-top: 100px;
        }
        .fir {
            margin-bottom: 50px; /*两者不会叠加使外边距变为150px，而是取较大的那个，即100px*/
        }
    </style>
</head>
<body>
<div class="fir">1</div>
<div class="sec">2</div>
</body>
</html>
```

##### 例41：外边距塌陷
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        .outer {
            width: 300px;
            height: 300px;
            background-color: pink;
            /*border: 1px solid pink; !*Ⅱ*!*/
            /*padding-top: 1px; !*Ⅲ*!*/
            overflow: hidden; /*Ⅳ*/
        }
        .inner {
            width: 100px;
            height: 100px;
            background-color: purple;
            margin-top: 50px; /*Ⅰ*/
        }
    </style>
</head>
<body>
<div class="outer">
    <div class="inner"></div>
    <!--想让内盒子下移，有两种方法，一种是给外盒子加内边距，同时调整外盒子高度，防止撑大，另一种是给内盒子加外边距-->
    <!--但给内盒子直接加外边距会把外盒子也带下来，见Ⅰ。这种情况只会出现在嵌套元素的上下移动过程中-->
    <!--解决方法之一是给外盒子加一个1px的边距，以隔开内外盒子，见ⅠⅡ-->
    <!--解决方法之二是给外盒子加一个1px的上内边距，以隔开盒子，见ⅠⅢ-->
    <!--解决方法之三是给外盒子加overflow:hidden，见ⅠⅣ，后面会讲-->
</div>
</body>
</html>
```

##### 例42：padding不会撑开盒子的情况
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        .outer {
            height: 300px;
            width: 300px;
            background-color: pink;
        }
        .inner {
            height: 100px;
            /*width: 300px;*/
            padding-left: 30px; /*当不指定宽度时，内盒子默认跟外盒子一样宽，这时padding不会撑开内盒子，指定之后，就会撑开了*/
            /*所以，只有指定了width，padding才会撑开盒子（此处撑开的还是内盒子，外盒子不变）*/
            /*margin不会撑开盒子*/
        }
    </style>
</head>
<body>
<div class="outer">
    <div class="inner">文本</div>
</div>
</body>
</html>
```

##### 例43：盒子圆角
```css
div {
    width: 300px;
    height: 300px;
    background-color: pink;
    margin: 0 auto;
    /*border-radius: 10px; !*设定圆角半径*!*/
    /*border-radius: 50%; !*不管宽高多少，圆角半径为其50%*!*/
    border-radius: 20px 0; /*规则与padding相同，顺时针*/
}
```

##### 例44：盒子阴影
```css
div {
    width: 300px;
    height: 300px;
    margin: 20px auto;
    box-shadow: 8px 8px 18px 2px rgba(0, 0, 0, 0.2);
    /*参数：水平阴影(必须) 垂直阴影(必须) 模糊度 阴影范围 阴影颜色（可以在浏览器中边调边观察）*/
}
```

##### 例45:浮动
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        div {
            width: 200px;
            height: 200px;
        }
        .fir {
            background-color: red;
            float: left; /*float可以让多个块级元素在同一行排列，可以靠左或靠右*/
        }
        .sec {
            background-color: blue;
            float: right;
        }
    </style>
</head>
<body>
<div class="fir"></div>
<div class="sec"></div>
</body>
</html>
```

##### 例46：浮动不会超过边框
```css
.outer { /*无特殊说明，遇到outer和inner就是嵌套的div（无内容）*/
    width: 600px;
    height: 600px;
    background-color: pink;
    padding: 50px;
}
.inner {
    width: 200px;
    height: 200px;
    background-color: purple;
    float: right; /*浮动不会超过边框*/
}
```

##### 例47：浮动注意点
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        div {
            width: 200px;
            height: 200px;
        }
        .one {
            background-color: pink;
            float: left;
        }
        .two {
            width: 250px;
            height: 250px;
            background-color: purple;
        }
        .three {
            background-color: skyblue;
            float: left; /*它不会跑到第一行上去，可以理解为浮动只能在当前行向上浮，不能拐到别的行上*/
        }
    </style>
</head>
<body>
<div class="one"></div>
<div class="two"></div>
<div class="three"></div>
</body>
</html>
```

##### 例48：浮动会把元素转为行内块元素
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Default</title>
    <style>
        div {
            height: 100px;
            background-color: pink;
            float: left; /*float会默认把元素（块级和行内）转换为行内块元素*/
        }
        span {
            width: 200px;
            height: 200px;
            background-color: purple;
            float: left; /*不需要用display转换也可以有宽高了，因为float默认把span转为行内块元素了*/
        }
    </style>
</head>
<body>
<div>文本</div>
<span>文本</span>
</body>
</html>
```


