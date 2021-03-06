# **第一章：Web应用框架与渗透测试环境搭建**
****

## 1.分别尝试安装一个Windows和Linux操作系统

![win7](http://wx4.sinaimg.cn/mw690/c4428b0bgy1g0wsr2y9w6j211y0lcka4.jpg)

![kail](http://wx1.sinaimg.cn/mw690/c4428b0bgy1g0wsr251ysj21hc0u01kx.jpg)

过程略.....

## 2.自己搭建一个网站

>**[猫窝](http://blog.yejiah.com)**
（悄悄偷个懒）

## 3.分析网站和浏览器之前通信的HTTP协议

>HTTP是一个客户端终端（用户）和服务器端（网站）请求和应答的标准（TCP）。通过使用网页浏览器、网络爬虫或者其它的工具，客户端发起一个HTTP请求到服务器上指定端口（默认端口为80）。我们称这个客户端为用户代理程序（user agent）。应答的服务器上存储着一些资源，比如HTML文件和图像。我们称这个应答服务器为源服务器（origin server）。在用户代理和源服务器中间可能存在多个“中间层”，比如代理服务器、网关或者隧道（tunnel）。{来自wikipedia}

下面这张图片完整表示了HTTP请求和响应的7个步骤
![http相应过程图解](http://wx3.sinaimg.cn/mw690/c4428b0bgy1g0wtvd6tsjj20eo0i3dgf.jpg)

***Http四个基于：***

  **1.请求与响应：** 客户端发送请求，服务器端响应数据

**2.无状态的：** 协议对于事务处理没有记忆能力，客户端第一次与服务器建立连接发送请求时需要进行一系列的安全认证匹配等，因此增加页面等待时间，当客户端向服务器端发送请求，服务器端响应完毕后，两者断开连接，也不保存连接状态，一刀两断！恩断义绝！从此路人！下一次客户端向同样的服务器发送请求时，由于他们之前已经遗忘了彼此，所以需要重新建立连接。针对无状态的一些解决策略：

有时需要对用户之前的HTTP通信状态进行保存，比如执行一次登陆操作，在30分钟内所有的请求都不需要再次登陆。于是引入了Cookie技术。

HTTP/1.1想出了持久连接（HTTP keep-alive）方法。其特点是，只要任意一端没有明确提出断开连接，则保持TCP连接状态，在请求首部字段中的Connection: keep-alive即为表明使用了持久连接。
等等还有很多。。。。。。

**3.应用层：** Http是属于应用层的协议，配合TCP/IP使用。

**4.TCP/IP：** Http使用TCP作为它的支撑运输协议。HTTP客户机发起一个与服务器的TCP连接，一旦连接建立，浏览器（客户机）和服务器进程就可以通过套接字接口访问TCP。



（参考了[这篇jobbole文章](http://android.jobbole.com/85218/)及[这篇csdn文章](https://blog.csdn.net/yezitoo/article/details/78193794)）

## 4.思考虚拟机的NAT模式与桥接模式的区别

![vnet](http://wx1.sinaimg.cn/mw690/c4428b0bgy1g0wungsuk4j20ge0ep0t3.jpg)

NET模式，简单来说就是在你的**宿主电脑**的网络基础上，再生成一个子网络（图中为VMnet8），ip的前两位默认是192.168（改是能改，但是子网一般是192.168.xx.xx或者10.xx.xx.xx），然后第三位第四位是自己可以手动设置的。使用这种模式唯一的一个缺点就是你的虚拟机只有**宿主电脑**可以访问，其他电脑不管通过什么方式都是访问不了的，然后在**宿主电脑**的同一子网（VMnet8）上创建多台虚拟机，这些虚拟机和**宿主电脑**都可以相互连通。

桥接模式则是让你的虚拟机的ip和**宿主电脑**的ip在同一个网段（我家用192.168.66.xx），这样的好处就是：只要**A宿主电脑（192.1168.66.20）** 和**B宿主电脑（192.168.66.21）** 在同一个网段（192.168.66.xx）当中（连了同一个路由器，或者插着同一个交换机，理论上就叫在同一个网段当中），这样**A宿主电脑**上的虚拟机，**B宿主电脑** 也能访问得到，这样就可以使用几台物理**宿主电脑**每台都配置多个虚拟机，让这些虚拟机组成一个大的计算集群（莫名其妙的讲起了数据中心VCenter的配置架构.....）。

## 5.如何保存虚拟机的多个运行状态？

![kuaizhao](http://wx2.sinaimg.cn/mw690/c4428b0bgy1g0wv6x1siaj20o90jidg0.jpg)

## 6.思考HTTP代理的作用
可以隐藏自己的真实IP和信息

过滤远程网站的内容（去广告）

逃避内容审查
