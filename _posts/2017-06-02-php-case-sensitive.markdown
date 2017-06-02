
---
layout: post
title: "php中大小写敏感以及常量变量的问题"
subtitle: "个人的一些总结"
date: 2017-06-02 13:11:00
author: "pengwei"
tags:
    - 后台语言
    - PHP
    - 总结
---
> 这篇文章是从我的个人公众号转载的，这里是[原文](https://mp.weixin.qq.com/s?__biz=MzI3MjM4MDY3Mw==&mid=2247483659&idx=1&sn=a81889f3dca5d38100f3ed8a4ab88ba8&chksm=eb322765dc45ae7376aa89073f1736296c496ce1e577c94d5a0bda9067361e8edfd941e77434#rd)
# 大小写敏感问题
## 背景
    昨天梳理之前做的OC项目，着重看了一下框架的实现代码。review到Action.php的时候，看到里面有一句$class = 'Controller' . preg_replace('/[^a-zA-Z0-9]/','', $this->route)；因为对preg_replace函数不太熟，所以写了一个测试文件专门测试了这个函数，发现当route为’common/home’时，class是Controllercommonhome，但实际上这个类名叫ControllerCommonHome，当时挺惊奇的，所以就想到，PHP对类名不区分大小写。然后就写测试文件，结果也验证了我的想法：PHP对类名和方法名都不区分大小写。
## 发现
    1、所有变量均区分大小写，包括普通变量以及超全局变量，诸如$_GET, $_POST, $_REQUEST, $_COOKIE,$_SESSION, $GLOBALS, $_SERVER, $_FILES, $_ENV；
    2、常量名是否大小写敏感，取决于你如何定义该常量。PHP中定义常量的方法有两种：分别是define函数和const关键词。参考PHP官方手册，define函数有三个参数，第三个参数就是控制大小写敏感的，可以不填，不填则默认大小写敏感，设置为true，则大小写不敏感。而通过const定义的常量是大小写敏感的；
    3、魔术常量，也叫预定义常量，__LINE__、__FILE__、__DIR__、__FUNCTION__、__CLASS__、__METHOD__、__TRAIT__、__NAMESPACE__都是不区分大小写的；
    4、类型强制转换的时候，也是不区分大小写的，比如，(int)'345'和(InT)'345'是一样的。

# 常量、变量
## PHP中定义常量有两种方法，分别是define和const，它们的区别如下：
    1、define是一个函数，而const只是一种语言结构；
    2、define函数只能在类的外面使用，在类中使用，会报错，但报错内容因PHP版本的不同而不一样。经过测试发现，PHP版本为5.2.17时，报错内容为：Parseerror: syntax error, unexpected T_STRING, expecting T_FUNCTION in，而当PHP版本为5.3.0-nts时，报错内容为：Parse error: parse error, expecting `T_FUNCTION' in。const在5.3.0之前只能在类里面使用，在5.3.0以及以后的版本中也可以在类外面使用；
    3、const在编译时比define的速度快；
    4、不能在条件语句、方法以及函数中用const定义常量；
    5、const只能采用普通的常量名称，define则可以采用表达式作为名称，比如'NAM' . 'E'；
    6、const只能接受静态的标量，而define可以采用任何表达式。
## 也顺便整理一下常量和变量的区别，这些内容虽然取自网上，但经过本人测试验证：
    1、常量前面没有美元符号($)
    2、常量只能通过define()函数定义或者const，而不能通过赋值语句
    3、常量可以不用理会变量的作用域在任何地方定义和访问
    4、常量一旦定义就不能重新定义或取消定义
    5、常量只能包含标量数据（boolean、integer、float和string）