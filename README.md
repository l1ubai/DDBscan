## 插件介绍：
DDB

前身链接https://github.com/l1ubai/GyScan

后根据https://github.com/Tsojan/TsojanScan 启发，舍弃了原本的配置文件，并且借鉴了其ui以及部分扫描逻辑

## 使用说明：

burp加载 出现下图所示即为成功！

![使用说明](https://github.com/l1ubai/DDBscan/blob/main/1.PNG)

## Config Panel

设置要扫描的漏洞 支持的如下图所示 需要注意！

如果测试后台功能可能会造成添加脏数据 登陆的时候可能会导致账号被封
![Config](https://github.com/l1ubai/DDBscan/blob/main/2.png)

## 说明一下几个扫描功能

### 正则匹配：

实现功能点在Nopoc类

通过正则匹配了回包中的敏感信息，包括js以及静态文件

这个功能在没有勾选插件激活的情况下也会开启，因为不发包所以不会被waf拦截

emm....这个功能有时候好用有时候又觉得很鸡肋，但是各位师傅可以diy

目前匹配的信息包括手机号、身份证号、password、osskey、ueditor等

### springboot env ：

其实就是扫springboot actuator未授权 下边的springboot cross 就是通过..;/的方式来扫，结果都一样

同时也会扫druid以及swagger 但是后两者的判断有待改良，现在的感觉有些时候还是不太稳

### Log4j Scan：

目前扫描的包括 所有参数 ,请求头中的ua Origin Referer Accept

如果想扫描其他请求头字段，可以在Log4jScan的HEAD_GUESS参数中添加其他字段

### Xss Scan:

垃圾....只是简单的判断了回包中是否有请求参数字段，而且会发很多包，慎用

### Spel Scan：

用的出网payload，所以需要配置dns，并没有回显payload 

### Shiro Scan：

只是通过rememberMe探测了是否使用shiro框架，并没有扫key

### Fastjson Scan：

借鉴了公网以及TsojanScan的部分payload，在dnslog中可以看到类似fjson1 fjson2...这样的域名，代表不同的paylaod，如果是高位payload，fastjson版本一般较高，也就只能做个判断用了

### weblogic Scan:

借鉴了TsojanScan，部分也需要配置dnslog

### Sqli Scan：

这个要重点说一下，现在判断的方式是两点，一个是单引号加上后页面的变化，一个是看页面有无报错。

之前觉得这样判断很low...确实很low，但是也会有效，在之前项目中有过帮助，不过误报会很多....这是缺点

也借鉴过TsojanScan，发现那样扫描漏洞点覆盖很全，但会发很多包，同时因为近段时间所有项目基本上都带waf，基本很少能用到，反而不如以前只是依靠单引号更实用（也因为我不会绕waf），所以继续改回了这种极low的判断方式

如果平时的渗透或者攻防不会遇到waf，那么TsojanScan的判断方式绝对更适合各位

如果有师傅有更好并且能不触发waf告警的方式，请务必带带我

## Custom Panel

![Custom](https://github.com/l1ubai/DDBscan/blob/main/3.png)

因为ui是借鉴的TsojanScan，我平时个人使用的时候基本上不会在意发包速度...反而很多时候希望他快一点，所以把超时阉割掉了，这里设置是没有作用的。

下边则是配置的不扫描的域名，匹配到就会直接跳过扫描

## Dnslog Panel

![Dnslog](https://github.com/l1ubai/DDBscan/blob/main/4.png)

dnslog配置

两种：第一种是ceye，第二种是自己搭的dns，搭建方式参见

https://github.com/yumusb/DNSLog-Platform-Golang

如果要使用自己的dns，要修改代码中MyDns类的playformUrl参数为自己的dnslog平台地址

## tips：

### 1、漏洞入库

代码中Addissuse方法中注释掉了把漏洞写入数据库的几行，如果需要漏洞存储的话，可以把注释去掉，同时在getConnection中添加自己的数据库连接

## 最后：

这个插件一开始开发是为了扫springboot actuator 那个时候这个洞太多了，而自己用手试则有点费心费力，刚写完gyscan的时候去某sdn试了下，基本上所有域名都有这个问题，但是这么长时间过去，这个洞肉眼可见的减少。

后来慢慢加上了其他功能，参考其他师傅的代码进行了优化，感觉还挺好用的，所以这里发出来，希望可以减少安全工程师简单操作反复进行的工作量，能把更多的精力投入到更"深"的研究中。

这个插件一开始写的时候真的很菜，所以很多结构代码都会显得臃肿，如果有师傅有增效或者更好的判断方式，请带带我。

欢迎各位提issuse！
