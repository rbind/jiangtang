---
id: 10087
title: 博客搬家：从Windows Live Space到WordPress（图解）
date: 2009-03-30T15:47:15+00:00
author: jiang
layout: post
guid: http://li-and-jiang.com/blog/2009/03/30/windows-live-space-move-wordpress/
permalink: /2009/03/30/windows-live-space-move-wordpress/
categories:
  - 技术
  - 网络
tags:
  - Python
  - SQL
  - Windows Live Space
  - Wordpress
  - 博客搬家
---
独立开博，在Hostmonster上用Wordpress上搭建完自个的空间，第一件事就是把以前发布在Windows Live Space上的博客导进来。其中，下载WindowsLiveSpace的全部文章和评论所用的Python脚本，来自[Broom](http://b2.broom9.com/?page_id=519#note1)，剩下的一些优化工作，比如删除垃圾评论，用了一些简单的SQL脚本。从WindowsLiveSpace搬家，向来最为麻烦，以下把刚做的功课记下来，鼓励大伙及时退出WindowsLiveSpace。我06年开博，至今在WindowsLiveSpace上积攒下近六百篇博文，所谓积重难返路径依赖，这次全部顺畅导出，兀的不高兴死我也么哥。

<!--more-->

对不熟悉Python的朋友来说，以下的内容也一样没有没有障碍：只需要按着相应的步骤，复制粘贴再回车即可。这是我的生活博客，里面所有涉及所谓技术的内容，也是中文系高材生Li也能看懂的。

> 1.准备工作

**1.1. 设置Windows Live Space和Wordpress的日期格式**

重要的是让WindowsLiveSpace和Wordpress的日期格式一致，这里我两个设置的是2009/03/30。在WindowsLiveSpace中，我的时间格式是10:24:05，设置的路径是“选项”->“常规”->“日期和时间格式”：

[<img style="border-top-width: 0px; display: inline; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" title="space_date" src="http://li-and-jiang.com/blog/wp-content/uploads/2009/03/space-date-thumb.png" border="0" alt="space_date" width="233" height="121" />](http://li-and-jiang.com/blog/wp-content/uploads/2009/03/space-date.png)

另外，在“选项”->“日志”->“显示日志日期”选项中，勾选“在页首出显示日志发布日期”。在Wordpress中，相应的路径是“设置”->“常规”->”日期格式”。

**1.2.下载安装Python 2.5.2**

[Broom](http://b2.broom9.com/?page_id=519#note1)测试了两种组合，Python 2.5.2+Beautiful Soup 3.0.6和Python 2.5.1+Beautiful Soup 3.0.4。但后面的低版本组合需要打些补丁，我们就不必麻烦了，我是直接用的高版本。Python 2.5.2的官方下载地址在：

<http://www.python.org/download/releases/2.5.2/>

我用的是Windows平台，选择的是X86 processors python-2.5.2.msi。下载完毕，一路安装过去就是，不妨依着它默认的安装路径c:Python25。接下来看看Python好不好使：

开始->运行->在冒出来的框里输入 <span style="color: #ff0000;">cmd</span> 回车->在冒出来的命令行输入 <span style="color: #ff0000;">python </span>再回车

顺利的话，你将看到类似的回应：

[<img style="border-top-width: 0px; display: inline; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" title="cmd" src="http://li-and-jiang.com/blog/wp-content/uploads/2009/03/cmd-thumb.png" border="0" alt="cmd" width="558" height="125" />](http://li-and-jiang.com/blog/wp-content/uploads/2009/03/cmd.png)

如果提示说python不可识别，就需要为Python设置环境变量，如果不明白什么是“环境变量”，依着下面的操作就是。

“我的电脑”->右键“属性”->“高级”->“环境变量”->“系统变量”->点中一个叫Path的系统变量->“编辑”->在Path的变量值框的末尾，加上一个分号<span style="color: #ff0000;">;</span>（英文状态下），然后跟着是填上Python的安装目录，比如<span style="color: #ff0000;">c:Python25</span>。一路确定后，再回刚才的命令行试试python这个命令。

[<img style="border-top-width: 0px; display: inline; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" title="path" src="http://li-and-jiang.com/blog/wp-content/uploads/2009/03/path-thumb.png" border="0" alt="path" width="452" height="325" />](http://li-and-jiang.com/blog/wp-content/uploads/2009/03/path.png)

**1.3.下载Beautiful Soup 3.0.6**

Beautiful Soup 3.0.6是一个解析HTML页面的类库，接下来那个下载博客文章的脚本需要用它。下载地址：

<http://www.crummy.com/software/BeautifulSoup/download/3.x/BeautifulSoup-3.0.6.tar.gz>

解压到一个文件夹，比如D:downloadBeautifulSoup-3.0.6。你将看到BeautifulSoup.py等几个脚本。

**1.4.下载脚本Live-space-mover**

Live-space-mover这个脚本，能够把WindowsLiveSpace中的所有文章和评论等，生成一个Wordpress能够识别的XML文件，以实现博客搬家的目的。下载地址：

<http://code.google.com/p/live-space-mover/downloads/list>

现在的最新版本是live-space-mover.1.7.5.zip。解压到D:downloadBeautifulSoup-3.0.6。在D:downloadBeautifulSoup-3.0.6目录下，你应该看到脚本live-space-mover.py。

> 2.运行脚本，下载博客，生成XML文件

在刚才提到过的cmd命令行，转到D:downloadBeautifulSoup-3.0.6文件夹(先敲入<span style="color: #ff0000;">d:</span> 回车，然后敲入 <span style="color: #ff0000;">cd D:downloadBeautifulSoup-3.0.6</span> 回车)，敲入下面的**一行**语句并回车：

python live-space-mover.py -s [http://<span style="color: #ff0000;">yourSpaceName</span>.spaces.live.com/](http://yourSpaceName.spaces.live.com/) -t &#8220;%m/%d/%Y %I:%M:%S %p&#8221;

其中，<span style="color: #ff0000;">yourSpaceName</span>是你Windows Live Space的名字。

[<img style="border-top-width: 0px; display: inline; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" title="Space-mover" src="http://li-and-jiang.com/blog/wp-content/uploads/2009/03/spacemover-thumb.png" border="0" alt="Space-mover" width="425" height="149" />](http://li-and-jiang.com/blog/wp-content/uploads/2009/03/spacemover.png)

这个运行时间视你博客文章的多少而定，如果在屏幕上看到博客标题显示为乱码，不必在意。成功之后，在D:downloadBeautifulSoup-3.0.6文件夹，将生成一个类似export_03292009-2238.xml的文件，这就是你整个Space的文章、评论以及类别之类的了。

又，如果在准备工作1.1中，你Windows Live Space的时间格式是10:24而不是10:24:05，那么，以上命令就将更简洁些：

python live-space-mover.py -s [http://<span style="color: #ff0000;">yourSpaceName</span>.spaces.live.com/](http://yourSpaceName.spaces.live.com/)

> 3.把XML文件导入到Wordpress

这个就相对简单了。Wordpress后台->“工具”->“导入”->选择WordPress，上传那个export_03292009-2238.xml文件，再指定一个作者就行。这个速度就快多了。

一个问题是，系统只允许导入最大为2MB的文件，对超过2MB的文件（像我这次导入的近六百篇博文），只好手动分割文件或想其他办法了。如果一次导入不漂亮，可以把所有的文章及评论先删除了再试试，两个简单的SQL语句可以参考：

DELETE from wp\_posts WHERE post\_author=1;

DELETE from wp\_comments WHERE comment\_ID>1;

作者ID(post\_author)和评论ID(comment\_ID)你可以在phpMyAdmin中找到。

[<img style="border-top-width: 0px; display: inline; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" title="python_space_over" src="http://li-and-jiang.com/blog/wp-content/uploads/2009/03/python-space-over-thumb.png" border="0" alt="python_space_over" width="363" height="103" />](http://li-and-jiang.com/blog/wp-content/uploads/2009/03/python-space-over.png)

待看到“导入完毕。好好享受吧！”，心情当真是无比舒畅，按着赖哥哥宁的说法，是比大热天吃了冰水还舒服。

> 4.一些优化工作

对全盘导入的博文，难免泥沙俱下，以前大量垃圾评论也跟着过来了。垃圾评论的一个特点是姓名为空（“没有名称”或者No name），下面的SQL语句就可以把它们清除：

DELETE FROM wp\_comments WHERE CONVERT(\`wp\_comments\`.\`comment_author\` USING utf8) = &#8216;（没有名称）nwrote:&#8217; ;
  
DELETE FROM \`wp\_comments\` WHERE CONVERT(\`wp\_comments\`.\`comment_author\` USING utf8) = &#8216;No name&#8217; ;

&#8212;-by [Jiang<at>li-and-jiang.com](mailto:Jiang@li-and-jiang.com)&#8212;&#8212;-

<div id="scid:0767317B-992E-4b12-91E0-4F059A8CECA8:bb94781d-fd3f-4efb-a2fc-66a7650a0578" class="wlWriterEditableSmartContent" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; padding-top: 0px">
  Technorati 标签: <a rel="tag" href="http://technorati.com/tags/Windows+Live+Space">Windows Live Space</a>,<a rel="tag" href="http://technorati.com/tags/Wordpress">WordPress</a>,<a rel="tag" href="http://technorati.com/tags/%e5%8d%9a%e5%ae%a2%e6%90%ac%e5%ae%b6">博客搬家</a>,<a rel="tag" href="http://technorati.com/tags/SQL">SQL</a>,<a rel="tag" href="http://technorati.com/tags/Python">Python</a>
</div>