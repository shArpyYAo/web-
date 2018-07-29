# web-
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
个人认为，xss的攻击难度和防范难点在于注入的方式，只要注入成功，那么
攻击就成功一大半。
#### 利用<>标记注射Html/Javascript
如果用户可以随心所欲的引入<>标记，那么他就可以添加一个标签
比如<script></script>、<img>、<table></table>等标签
例子：
> <script>alert('xss');</script>
所以首当其冲要过滤的就是<>、<script>
#### 利用html属性
很多html标记中的属性都支持javascript:[code]伪协议的形式，这个特殊的协议类型声明了
URL的主体是任意的js代码，有js解释器运行
例子：
> <img src=“javascript:alert('xss');”>

未完待续。。。
