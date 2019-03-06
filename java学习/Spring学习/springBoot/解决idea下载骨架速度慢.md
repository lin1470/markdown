折腾了一段时间终于解决了，

可能是因为网络问题

xml一直没法访问

maven 骨架生成项目速度慢的令人发指，都在Generating project in Batch mode等待，Idea状态显示栏还在不行runing，并没有卡死。查看debug信息发现，是maven获取archetype-catalog.xml导致。（用游览器打开[http://repo1.maven.org/maven2/archetype-catalog.xml，需要等待很长时间才能获取到。）](http://repo1.maven.org/maven2/archetype-catalog.xml%EF%BC%8C%E9%9C%80%E8%A6%81%E7%AD%89%E5%BE%85%E5%BE%88%E9%95%BF%E6%97%B6%E9%97%B4%E6%89%8D%E8%83%BD%E8%8E%B7%E5%8F%96%E5%88%B0%E3%80%82%EF%BC%89)

 

xml下载不了于是

第一步   我在其他地方下载了这个骨架xml，放进本地（你的目录默认.m2）\repository\org\apache\maven\archetype\archetype-catalog\2.4

第二步   修改项目默认设置加入命令-DarchetypeCatalog=local

 

![img](https://images2015.cnblogs.com/blog/639200/201608/639200-20160821001117156-2011174371.png)

 

然后xml问题解决了就是下载速度问题了，

我特意连上了vpn

在cmd 里面执行了一次 mvn archetype:generate -DarchetypeCatalog=local

下载速度还不错，几分钟过去了下载完成了，

然后再去创建项目，下载了几个包之后，提示构建成功！