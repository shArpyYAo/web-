# web-safe
web安全相关的读书笔记，做个Research Review

# 《XSS跨站脚本攻击剖析与防御》

### 什么是XSS跨站脚本
比较简洁的一句话就是，xss漏洞是web应用程序对用户的输入过滤不足而产生的。
举个例子，某个网站具有留言功能，这个功能直接把用户写的东西写进HTML里
比如你写一段
    
      <script>alert('xss');</script>
那么任何一个用户进来这个页面浏览器就会执行这段代码。
XSS攻击就是将非法的js这一些脚本注入到用户浏览的网页上执行，
而web浏览器本身的设计是不安全的，它只负责解释和执行js等脚本语言，
而不会判断本身是否对用户有害

### xss的注入方式
个人认为，xss的攻击难度和防范难点在于注入的方式，只要注入成功，那么攻击就成功一大半。
#### 利用<>标记注射Html/Javascript
如果用户可以随心所欲的引入<>标记，那么他就可以添加一个标签
比如<script></script>、<img>、<table></table>等标签
例子：

    <script>alert('xss');</script>
    
所以首当其冲要过滤的就是<>、<script>
    
#### 利用html属性
很多html标记中的属性都支持javascript:[code]伪协议的形式，这个特殊的协议类型声明了URL的主体是任意的js代码，由js解释器运行（有局限性，只有在支持伪协议的浏览器上才可以。例如ie6）
例子：

    <img src=“javascript:alert('xss');”>

#### 空格回车Tab
先看一个例子

    <img src="javas cri pt : alert(/xss/)" >
    
为什么这样子也是有效果的呢？因为在js中通常以分号结束语句的。
    
    var a
    = 1;
    alert(a);
    
上面的代码在js中是可以运行的。（有局限性，只有在支持伪协议的浏览器上才可以。例如ie6）

#### 对标签属性进行转码（特别要注意转码，很灵活多变）
html的属性值是支持ASCII码的。

    <img src="javascrip&#116&#58alert('xss')" >
    
对应ASCII码可以看出，t的ASCII码是116，用&#116表示，：的ASCII码是58，用&#58表示。
同样的，全部转码也是可以生效的（有局限性，只有在支持伪协议的浏览器上才可以。例如ie6）
所以业务上没需要，也最好过滤&、#、/等字符。

#### 利用事件
先看一段代码

    <input type="button" onclick="alert('click')"/>
    
这是一句再正常不过的js代码了，我们就是通过事件来实现交互的。
但是我们看看这段代码

    <img src="#" onerror="alert(/xss/)">
    
onerror是img的事件之一只要发生错误，就会出发该事件。上面的代码中，src错误，触发了onerror事件。


    
