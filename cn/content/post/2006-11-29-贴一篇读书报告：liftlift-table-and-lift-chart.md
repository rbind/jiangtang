---
id: 9512
title: '贴一篇读书报告：Lift,Lift Table,  and Lift Chart'
date: 2006-11-29T22:32:00+00:00
author: jiang
layout: post
guid: http://li-and-jiang.com/blog/2006/11/29/%e8%b4%b4%e4%b8%80%e7%af%87%e8%af%bb%e4%b9%a6%e6%8a%a5%e5%91%8a%ef%bc%9aliftlift-table-and-lift-chart/
permalink: /2006/11/29/%e8%b4%b4%e4%b8%80%e7%af%87%e8%af%bb%e4%b9%a6%e6%8a%a5%e5%91%8a%ef%bc%9aliftlift-table-and-lift-chart/
categories:
  - 生活
---
前些日子屋里的两个哥们，[小裴](http://bbs.ss.pku.edu.cn/ss/index.php/6798)和[Crazy Hou](http://leihou.spaces.live.com/)纷纷开博，立志要写技术博客。同在软院混，不提些技术于情于理都说不过去，最近我倒是想贴些SAS的学习笔记，太花时间，先贴一篇前几周写的一个读书笔记，是关于提升表的，说是信用评分中的关键技术之一。把它贴出来也是因为当时google和baidu材料时，就没有找到可以用的中文材料，接下来的朋友有幸了，因为他能够baidu到这篇。我不打算要这篇的版权，只要不是商业用途，Feel free to cite, copy, post, or distribute any parts that you find helpful. 

Lift, Lift Table, and Lift Chart 

提升指数、提升表和提升图（草稿） 

2006-11-5 

1. 什么是Lift？ 

I) Lift（提升指数）是评估一个预测模型是否有效的一个度量；这个比值由运用和不运用这个模型所得来的结果计算而来。 

II) 一个简单的数字例子： 

i. 比如说你要向选定的1000人邮寄调查问卷。以往的经验告诉你大概20%的人会把填好的问卷寄回给你，即1000人中有200人会对你的问卷作出回应（response），用统计学的术语，我们说baseline response rate是20%； 

ii. 如果你现在就邮寄问卷，1000份你期望能收回200份，这可能达不到一次问卷调查所要求的回收率，比如说工作手册规定邮寄问卷回收率要在25%以上； 

iii. 通过以前的问卷调查，你收集了关于问卷采访对象的相关资料，比如说年龄、教育程度之类。利用这些数据，你确定了哪类被访问者对问卷反应积极。假设你已经利用这些过去的数据建立了模型，这个模型把这1000人分了类，现在你可以从你的千人名单中挑选出反应最积极的100人来，这10%的人的反应率(response rate)为60%。那么，对这100人的群体（我们称之为Top 10%），通过运用我们的模型，相对的提升(gain or lift value)就为60%/20%=3；换句话说，与不运用模型而随机选择相比，运用模型而挑选有3倍的好处； 

iv. 类似地，对占总样本的任何比例的人群，我们都可以计算出相应的提升指数，比如说我们可以计算Top 20%的群体的提升指数。 

III) 一个结论就是，提升指数越大，模型的运行效果越好。 

2. 建立Lift Table 的步骤（并画出Lift Chart），以验证信用评分模型为例： 

I) 利用已经建立的评分模型，对我们要验证的样本进行评分。样本下的每一个个体都将得到一个分数，或者是违约概率，或者是一个分值； 

II) 对样本按照上面计算好的分数进行降序排序； 

III) 把已经排好序的样本依次分成10个数量相同的群体，我们就建立了一个叫decile的变量，它依次取10个值，1、2、3、4、5、6、7、8、9、10，diclie1包括违约概率值最高的10%的个体，diclie2包括下一个10%的群体，以此类推； 

IV) 帐户总数是每个decile下的样本数，它是整个样本数的10%； 

V) 边际坏账数是每个decile内违约的人数，就是说，利用我们的评分模型，在decile1，有25个人违约，以此类推； 

VI) 累计坏账数，45表明前两个decile内共有45个人违约，以此类推； 

VII) 边际坏账率是每个decile内坏账的比率。对decile1，边际坏账率由25/100得来； 

VIII) 对每一个加总的decile，都计算一个累计坏账率，比如说，对前两个decile，也就是整个样本的20%，累计坏账率等于（25+20）/（100+100）； 

IX) 在每个decile里，提升指数（Lift）就是相应的累计坏账率与平均坏账率的偏离程度，计算公式是（累计坏账率-平均坏账率）/平均坏账率，习惯上还会乘上一个100。 

X) 注：在一些处理中，提升指数直接由每个decile的累计坏账率除以平均坏账率得来，它们之间就相差1，一个是相对偏离，一个是绝对偏离。 

XI) 就我们考察的信用评分模型，它的目的就是尽可能把人群区别来开来，比如说“好”的顾客、 “坏”的顾客。提升指数越大，表明模型运作效果越好。 

表1：Lift Table 

 <img height="246" src="http://tk4.storage.msn.com/x1pxOYwqu4SjF7Mdf9RgGBcEhe8YtwqytmMWawZwtb70n61ajEifZotw6hWP4lEUcksoO3CXr3IaJlwdb_RdajfjgabHgXahKyL5dLDdHDRceVO5biKLoQP0vq4z1NXMc78ztQfFWPm815edJ9WZP3CHPoa63ye1LqY" width="626" />

（注：该表内数字纯粹为了演示，没有任何实际背景） 

图1：Lift Chart 

 <img height="499" src="http://tk4.storage.msn.com/x1pxOYwqu4SjF7Mdf9RgGBcEhe8YtwqytmMWawZwtb70n7WkUnN3K8fmuMnQW7EDuie0wQpd-hXf8yPcVDSIDzM7xk8H7xqYVGS5w1uQ3-tjAD_F3VdNYnQe6t3Homo-XxfgA3uR3zIyreOY05mnRcPsg" width="499" />

3. 参考资料 

I) Bruce Ratner, Decile Analysis Primer: Cum Lift for Response Model. 

<http://www.dmstat1.com/res/DecileAnalysisPrimer.html> 

II) Howard J. Hamilton. Cumulative Gains and Lift Charts 

[http://www2.cs.uregina.ca/~hamilton/courses/831/notes/lift\_chart/lift\_chart.html](http://www2.cs.uregina.ca/~hamilton/courses/831/notes/lift_chart/lift_chart.html) 

III) David S. Coppock. Data Modeling and Mining: Why Lift? 

<http://www.dmreview.com/article_sub.cfm?articleId=5329> 

IV) Lift Chart. _See_ Thomas Hill, Paul Lewicki. Statistics: Methods and Applications. 

<http://www.statsoft.com/textbook/glosl.html>