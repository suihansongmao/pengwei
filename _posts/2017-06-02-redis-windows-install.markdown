---
layout:     post
title:      "记录一下曲折的redis安装历程"
subtitle:   ""
date:       2017-06-02 14:13:00
author:     "Pengwei"
header-img: "img/post-bg-e2e-ux.jpg"
tags:
    - redis安装
    - windows
    - 总结
---

> 对新事物保持一种好奇的状态，对于处在快速发展环境中的我们是非常重要的。

## 写在前面
    对于redis，我之前理解停留在，它是一个很火的NoSQL数据库，也可以作为缓存使用。其实，redis出来也很久了，但由于各种原因，直到昨天才开始比较系统的了解，昨天是初步了解，今天完成了安装和在php中的初步使用，现把安装过程记录如下： 
## 开始安装
    首先，redis和memcache都是CS架构的，所以无论在哪里使用，都需要安装Server端。redis虽然官方不支持windows安装，但是官网上面可以找到windows版本的链接，如下截图所示：
![](/img/in-post/post-redis-windows-install/redis-io.jpg)
点击**Learn more**，进入的应该是这样一个页面：
![](/img/in-post/post-redis-windows-install/redis-windows.jpg)
然后，继续往下滚，找到这句话：
![](/img/in-post/post-redis-windows-install/redis-windows-more.jpg)
就是release page，点击进入到：https://github.com/MSOpenTech/redis/releases，应该是这样的一个页面：
![](/img/in-post/post-redis-windows-install/redis-windows-release.jpg)
到这里，安装过程就进入了收尾阶段。这里发个小牢骚，由于某些你懂得原因，下载速度简直感人，所以建议最好用VPN，如果没有，我只能说多尝试几次吧。
这里我下载的是MSI，直接安装，安装好后就是以服务的形式运行，比较省事，接下来就是去下载phpredis了，这是一个PHP版本的API，用于与redis服务端通信，下载地址：
http://windows.php.net/downloads/pecl/releases/redis/2.2.7/，找一个与本地安装的PHP版本相匹配的文件，下载后放到PHP目录下的ext文件夹下，然后记得在php.ini文件中加上extension=php_redis.dll，最后别忘了，重启apache或者nginx服务器哦，重启后就可以愉快的玩耍了。
## 关于redis教程
直接看phpredis包里的REDEME.markdown文件，上面讲的非常详细，而且是官方的版本。如果要深入了解redis，直接看官网上的Docs就行了，上面都是最权威的。唯一的不足就是都是英文的。
## 经验教训
本人在安装过程中走了很多弯路，记录下来，虽然是弯路，应该也有一些意义。首先，其实我看到了红框中的那句话，但我当时压根没在意，毕竟它是unsigned的，所以，我直接定位到倒数第二行。先尝试了NuGet，点击进入到https://www.nuget.org/packages/Redis-64/，找到Download：
然后下载，下载后发现文件是nupkg格式的包，然后又去nupkg的官网：https://docs.nuget.org/ndocs/guides/install-nuget#nuget-cli，最后到https://dist.nuget.org/index.html，下载好NuGet CLI工具软件，开始静静地安装，就在我满心欢喜，以为终于快要完成的时候，系统提示我，此应用无法在你的系统上运行，我再仔细看刚才的下载页面，才发现这是个X86的CLI，而除了这个之外，就是Visual Studio版的，根本就没有64位的版本啊。OMG，当时的我几乎是崩溃的。可是还是要继续下去啊，于是，我开始尝试另一个，即chocolatey，进入到：https://chocolatey.org/packages/redis-64，找到了download，然后，开始静静地下载，等我下载好后，感人的发现，这TM不就是刚刚下载的nupkg包嘛，简直如假包换，于是我再度崩溃，然后，我又看到了这个：
意识到这应该要先安装chocolatey，于是，我又找到了这篇指南：https://chocolatey.org/install。本来想顺着指南安装，结果突然灵光一现，想再搜索看看，结果就给我发现了这个，http://blog.csdn.net/renfufei/article/details/38474435/，
正是这个，让我发现了隐藏了很久的release。非常感谢该篇博文的博主。
嗯，今天就是这样，明晚见，明晚准备讲memcache

***注：楼主环境配置如下，Win10家庭中文版，8G内存，64位操作系统。目前，只有windows 64位的redis，所以32系统的小伙伴就不要折腾了，换个系统再战吧。。。***