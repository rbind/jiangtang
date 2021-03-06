---
id: 136
title: 'SAS Algorithmically(1): Newton-Raphson method'
date: 2010-10-21T21:39:42+00:00
author: Jiangtang Hu
layout: post
guid: http://www.jiangtanghu.com/blog/2010/10/21/sas-algorithmically1-newton-raphson-method/
permalink: /2010/10/21/sas-algorithmically1-newton-raphson-method/
categories:
  - SAS
tags:
  - algorithms
  - Newthon-Raphson
  - numeric precision
  - SAS
---
A good reference for the basic algorithms of Newton-Raphson method to calculate the square root of a number, _see_

> <http://mathforum.org/library/drmath/view/52644.html>

And the SAS codes(self-explanatory):

> <font face="Courier New">data root; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; /*question: find the square root of 4*/ <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; x=4;&#160; </font>
> 
> <font face="Courier New">&#160;&#160;&#160;&#160;&#160;&#160;&#160; /*first choose a rough approximation of sqrt(4); <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; actually, you can start with any numbers*/ <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; y0=1;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; </font>
> 
> <font face="Courier New">&#160;&#160;&#160;&#160;&#160;&#160;&#160; count=0;/*init count number*/ </font>
> 
> <font face="Courier New">&#160;&#160;&#160;&#160;&#160;&#160;&#160; do until (w<1e-8); /*set a small tolerance error*/ <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; count=count+1;&#160;&#160; /*accumulate count number*/ <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; y=(y0+x/y0)/2;&#160;&#160; /*Newton&#8217;s formula*/ <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; w=abs(y-y0); /*if close, exit;*/ <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; y0=y;&#160;&#160;&#160;&#160;&#160;&#160;&#160; /* otherwise, keep the new one*/ <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; end; </font>
> 
> <font face="Courier New">&#160;&#160;&#160;&#160;&#160;&#160;&#160; output; <br />run;</font>

The outputs:&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; 

> x&#160;&#160;&#160; y0&#160;&#160;&#160; count&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; w&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; y 
> 
> 4&#160;&#160;&#160;&#160; 2&#160;&#160;&#160;&#160;&#160; 6&#160;&#160;&#160;&#160;&#160; 2.2204E-15&#160;&#160;&#160; 2 

After 6 iterations, Newton-Raphson(also called **divide-and-average**) gets an approximated square root. See what happed during each iteration compared the output generated by SAS function,sqrt():

> <font face="Courier New">data root; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; x=4;&#160; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; y0=1;&#160;&#160;&#160;&#160;&#160;&#160;&#160; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; count=0; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; do until (w<1e-8);&#160;&#160; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; count=count+1;&#160;&#160; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; y=(y0+x/y0)/2;&#160; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; w=abs(y-y0); <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; y0=y; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; if y =sqrt(x) then is_eq_sqrt="YES"; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; else is_eq_sqrt="NO"; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; output; <br />&#160;&#160;&#160;&#160;&#160;&#160;&#160; end; <br />run;</font>

Outputs:

> x&#160;&#160;&#160;&#160;&#160;&#160; y0&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; count&#160;&#160;&#160;&#160;&#160;&#160;&#160; w&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; y&#160;&#160;&#160;&#160; is\_eq\_sqrt 
> 
> 4&#160;&#160;&#160; 2.50000&#160;&#160;&#160;&#160;&#160; 1&#160;&#160;&#160;&#160;&#160; 1.50000&#160;&#160;&#160; 2.50000&#160;&#160;&#160;&#160; NO   
> 4&#160;&#160;&#160; 2.05000&#160;&#160;&#160;&#160;&#160; 2&#160;&#160;&#160;&#160;&#160; 0.45000&#160;&#160;&#160; 2.05000&#160;&#160;&#160;&#160; NO   
> 4&#160;&#160;&#160; 2.00061&#160;&#160;&#160;&#160;&#160; 3&#160;&#160;&#160;&#160;&#160; 0.04939&#160;&#160;&#160; 2.00061&#160;&#160;&#160;&#160; NO   
> 4&#160;&#160;&#160; 2.00000&#160;&#160;&#160;&#160;&#160; 4&#160;&#160;&#160;&#160;&#160; 0.00061&#160;&#160;&#160; 2.00000&#160;&#160;&#160;&#160; NO   
> 4&#160;&#160;&#160; 2.00000&#160;&#160;&#160;&#160;&#160; 5&#160;&#160;&#160;&#160;&#160; 0.00000&#160;&#160;&#160; <font color="#ff0000">2.00000&#160;&#160;&#160;&#160; NO</font>   
> 4&#160;&#160;&#160; 2.00000&#160;&#160;&#160;&#160;&#160; 6&#160;&#160;&#160;&#160;&#160; 0.00000&#160;&#160; <font color="#ff0000">2.00000</font>&#160;&#160;&#160;&#160; <font color="#ff0000">YES </font>&#160;&#160;&#160;&#160; 

What’s the difference between count 5 and 6 since their y values look the same? We reset the tolerance value to 1e-3 rather than 1e-8, and get the outputs:

> x&#160;&#160;&#160;&#160;&#160;&#160; y0&#160;&#160;&#160;&#160;&#160; count&#160;&#160;&#160;&#160;&#160;&#160; w&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; y&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; is\_eq\_sqrt 
> 
> 4&#160;&#160;&#160; 2.50000&#160;&#160;&#160;&#160;&#160; 1&#160;&#160;&#160;&#160;&#160; 1.50000&#160;&#160;&#160; 2.50000&#160;&#160;&#160;&#160;&#160; NO   
> 4&#160;&#160;&#160; 2.05000&#160;&#160;&#160;&#160;&#160; 2&#160;&#160;&#160;&#160;&#160; 0.45000&#160;&#160;&#160; 2.05000&#160;&#160;&#160;&#160;&#160; NO   
> 4&#160;&#160;&#160; 2.00061&#160;&#160;&#160;&#160;&#160; 3&#160;&#160;&#160;&#160;&#160; 0.04939&#160;&#160;&#160; 2.00061&#160;&#160;&#160;&#160;&#160; NO   
> 4&#160;&#160;&#160; 2.00000&#160;&#160;&#160;&#160;&#160; 4&#160;&#160;&#160;&#160;&#160; 0.00061&#160;&#160;&#160; 2.00000&#160;&#160;&#160;&#160;&#160; NO&#160;&#160;&#160;&#160;&#160;&#160; 

The system get a faster convergence at an higher error rate, with an approximated&#160; value little away from sqrt(4). 

We should have a deep understanding of how SAS stores numeric values, which deserves a full session to discuss, to unearth the mystery. Some basic references:

  * _[Numeric Precision in SAS Software](http://support.sas.com/documentation/cdl/en/lrcon/62955/HTML/default/a000695157.htm)_
  * _[Numeric Precision 101](http://support.sas.com/techsup/technote/ts654.pdf)_
  * _[Dealing with Numeric Representation Error in SAS Applications](http://support.sas.com/techsup/technote/ts230.html)_