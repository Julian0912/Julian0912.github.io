---
layout: post
title: Web框架本质
categories: Programming
description: 
keywords: 
---

*在学习Django之前了解Web框架本质，有助于搞清楚Django框架中各文件的作用。*

<!--more-->

#### 一、客户端和服务端

在我们访问网页的时候，我们输入网址（URL），然后得到了一个页面。实际上对于浏览器和服务器来说，发生的是一个客户端和服务端交换信息的过程。浏览器就是客户端，服务器就是服务端。而它们**本质上都是socket**。所以当我们通过浏览器发送请求的时候，服务端那里**本质上**是这样一个状态：

```python
import socket

sock = socket.socket()  # 建立一个socket对象
sock.bind(('127.0.0.1', 8086))  # 绑定地址和端口
sock.listen(5)  # 同时允许5各客户访问

# 循环等待
while True:
    conn, addr = sock.accept()  # 如无访问则会卡在这里，有访问则建立连接
    data = conn.recv(8096)  # 接收到信息，最大8096bit
    conn.send(b'data received')  # 返回信息
    conn.close()  # 关闭连接
```

运行该代码后，经测试使用Edge无效，但IE可以接收到返回的数据。

但其实目前为止，接收的消息跟发送的消息无任何联系，因为无论给这个服务端发什么信息，它都只会回一个“data received”。

因此为了让客户端和服务端能够识别和处理不同的信息，我们规定了一些**协议**，这些协议实际就是数据的表示方式，它规定了发送和接收的数据要遵循什么样的格式，要包含哪些必要的内容。比如http协议。

http协议包含了**请求头**和**请求体**。在上段代码的第11行插入一句`print(data)`，我们可以知道浏览器给我们发送了什么信息。整理后如下：

```
GET / HTTP/1.1
Accept: text/html, application/xhtml+xml, image/jxr, */*
Accept-Language: zh-Hans-CN,zh-Hans;q=0.8,ja;q=0.6,en-US;q=0.4,en;q=0.2
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; rv:11.0) like Gecko
Accept-Encoding: gzip, deflate
Host: 127.0.0.1:8086
Connection: Keep-Alive


```

这段数据实际上是一段Bytes类型的字符串，在末端有两个`\r\n`，即windows下的换行。所以，在浏览器发送的数据中，**两个换行**前的数据都是请求头，后的都是请求体。

上述信息是访问`http://127.0.0.1:8086`返回的数据，若在地址后加一个`/index?p=12`，则该数据会显示在第一行的`GET /`后面，即`GET /index?p=12`。这就是GET请求。若访问为POST请求，则数据的第一行只显示`POST /index`，参数`p=12`会显示在请求体（即两个换行之后）中。

除了请求头和请求体外，还有**响应头**和**响应体**。浏览器给服务器发送了请求头和请求体，然后服务器根据这些信息返回了响应头和响应体。而响应体实际上就是我们看到的页面，它本质上是一串字符串，被浏览器解析后变成了漂亮的图文。

所以实际上上一段代码中我们的响应数据只有响应体（"data received"），缺少响应头，是不规范的。

在`conn.send(b"data received")`前加一句`conn.send(b"HTTP/1.1 200 OK\r\n\r\n")`，此时响应消息包含了响应头（"HTTP/1.1 200 OK"），Edge浏览器也可以接收到信息了。

#### 二、功能封装

现在来解决一下上面的遗留问题。上一段代码虽然包含了请求头、请求体，响应头、响应体，能够发送信息并接收信息，但并没什么卵用，因为无论URL后面的请求信息是什么，服务端只会义无反顾地返回一个“data received”。

所以，实际上我们可以在拿到客户端发送的请求信息后先解析一番，根据不同的请求信息做出不同的反应，即返回不同的响应信息。

以请求信息为例，先将请求头和请求体分割，分隔符是`\r\n\r\n`，然后观察请求头，以`\r\n`分割，再观察，发现除了第一行外，其余几行实际都是键值对，以`:`分隔。

此处我们当然可以拿到键值信息然后处理，但为了方便，我们暂不处理这些键值对了，只看第一行。

第一行很明显以空格分隔，所有可以用空格分割，拿到三个信息，分别是请求方式，url，和协议。此时我们可以通过判断url来返回不同的响应信息了。如下：

```python
import socket

sock = socket.socket()
sock.bind(('127.0.0.1', 8086))
sock.listen(5)

while True:
    conn, addr = sock.accept()

    data = str(conn.recv(8096), encoding='utf8')
    headers, body = data.split('\r\n\r\n')
    hd_ls = headers.split('\r\n')
    method, url, protocol = hd_ls[0].split(' ')

    conn.send(b'HTTP/1.1 200 OK\r\n\r\n')
    if url == '/test':
        conn.send(b'test successful')
    else:
        conn.send(b'404 not found')
    conn.close()
```

此处只处理了两种信息，如果是`http://127.0.0.1:8086/test`则返回`test successful`，如果不是，就返回`404 not found`。

注意，此时代码将所有功能，包括接收请求、处理请求、发送响应都放在了一起，如果url有很多种，则看上去会很乱，我们可以把不同的功能封装到不同的区域。

比如，对于不同的url，我们可以将不同的处理代码封装成不同的函数，然后设立一个列表，将url和函数名组合起来，如下：

```python
def f1(request):
    return b'visiting f1'
def f2(request):
    return b'visiting f2'
routers = [
    ('/xxx', f1),
    ('/ooo', f2)
]
```

此处函数的返回值可以是不同的页面信息，在此有所简化。但函数主体实际是要处理客户端发来的各种请求信息的，所以要加一个参数`request`，接收的就是各种请求数据，然后在函数主体内进行处理。

假设我们有一个HTML文件（"index.html"）：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>LOGIN</title>
</head>
<body>
<h1>用户登录</h1>
<form action="#">
    <p>用户名：<input type="text" /></p>
    <p>密　码：<input type="password" /></p>
</form>
</body>
</html>
```

我们就可以在某个函数里以二进制形式打开这个文件，（进行处理），然后返回给客户端。

```python
def f1(request):
    with open('index.html', 'rb') as file:
        d = file.read()
    return d
def f2(request):
    return b'visiting f2'
routers = [
    ('/xxx', f1),
    ('/ooo', f2)
]
```

这样同理，不同的函数可以根据需要打开处理相同的或者不同的文件。一个只有两个页面的“网站”就做好了。代码整体如下：

```python
import socket

def f1(request):
    with open('index.html', 'rb') as file:
        d = file.read()
    return d

def f2(request):
    return b'visiting f2'

routers = [
    ('/xxx', f1),
    ('/ooo', f2)
]

def run():
    sock = socket.socket()
    sock.bind(('127.0.0.1', 8086))
    sock.listen(5)

    while True:
        conn, addr = sock.accept()

        data = str(conn.recv(8096), encoding='utf8')
        headers, body = data.split('\r\n\r\n')
        hd_ls = headers.split('\r\n')
        method, url, protocol = hd_ls[0].split(' ')

        conn.send(b'HTTP/1.1 200 OK\r\n\r\n')

        func = None
        # 遍历routers，找到合适的那一个url后整个循环就可以关闭了
        for item in routers:
            if item[0] == url:
                func = item[1]
                break
        if func:
            response = func(data)
        else:
            response = b'404 not found'

        conn.send(response)
        conn.close()

if __name__ == '__main__':
    run()
```

#### 三、页面模板

由上一节我们可以看出，此处的`index.html`实际上起到的只是一个普通文件的作用，它可以是任何文件，任何后缀名，并不非得是`html`。

那么既然如此，我们在`index.html`中改一点东西，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>LOGIN</title>
</head>
<body>
<h1>用户登录</h1>
<form action="#">
    <p>用户名：<input type="text" /></p>
    <p>密　码：<input type="password" /></p>
    <p>时　间：[% time %]</p>
</form>
</body>
</html>
```

`[% time %]`的符号实际上可以随便定义。然后在函数部分做如下改动：

```python
def f1(request):
    # 注意打开方式改变了
    with open('index.html', 'r', encoding='utf8') as file:
        d = file.read()
    import time
    ct = time.time()
    d = d.replace('[% time %]', str(ct))
    return bytes(d, encoding='utf8')  # 将字符串转为字节
def f2(request):
    return b'visiting f2'
routers = [
    ('/xxx', f1),
    ('/ooo', f2)
]
```

此时间隔访问`http://127.0.0.1:8086/xxx`时，页面时间信息会产生变化了，当然显示方式可以更直观，但原理相同。

所以`index.html`充当了一个**模板**的作用，可以根据需要改变页面信息，这就是一个简单的**动态网页**了。

而且被替换的可以是任何信息，python处理的字符串中可以包含`tr` `td`等页面标签，通过格式化在页面里插入表格等结构，表格内容可以是数据库数据，这样每当数据库数据发生变化，页面表格内容就会发生变化。

#### 四、第三方渲染

在上一节中，我们在替换模板信息的时候，用到了自己定义的符号`[% time %]`，但我们要自己创建规则很麻烦，所幸有人已经造好了轮子。python的第三方库jinja2可以用来渲染网页，即替换信息。

我们新建一个网页文件`stulist.html`，如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>STUDENTS</title>
</head>
<body>
<table border="1">
    <tr>
        <th>Name</th>
        <th>Sex</th>
        <th>Age</th>
    </tr>
    \{\% for row in stu_ls \%\}
    <tr>
        <td>\{\{row.name\}\}</td>
        <td>\{\{row.sex\}\}</td>
        <td>\{\{row.age\}\}</td>
    </tr>
    \{\% endfor \%\}
</table>
<p>From \{\{user\}\}</p>
</body>
</html>
```

其中的符号要遵循既定的规则。再在python代码中修改如此：

```python
def f1(request):
    with open('index.html', 'r', encoding='utf8') as file:
        d = file.read()
    import time
    ct = time.time()
    d = d.replace('[% time %]', str(ct))
    return bytes(d, encoding='utf8')
def f2(request):
    with open('stulist.html', 'r', encoding='utf8') as file:
        df = file.read()
    # 该数据可以从数据库或者文件中获得
    students = [
        {'name': 'FMY', 'sex': 'male', 'age': 20},
        {'name': 'XLY', 'sex': 'female', 'age': 48},
        {'name': 'FZR', 'sex': 'male', 'age': 47}
    ]
    from jinja2 import Template
    template = Template(df)
    # 参数名即模板中的变量名
    dt = template.render(stu_ls=students, user='Nobody')
    return dt.encode('utf8')
routers = [
    ('/xxx', f1),
    ('/ooo', f2)
]
```

这样，我们就不用自己写代码渲染了。只要我们遵循了既定规则，第三方库会帮我们渲染。

但实际上在Django中我们也没用到jinja2去渲染，它使用了不同的渲染方法。但原理相同，只要我们使用规则写模板，就可以实现自动渲染。