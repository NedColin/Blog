---
title: iOS中堆block与栈block
date: 2015-05-26 19:55:16
categories: jekyll testing
---


默认情况下，block是分配在栈上，他们出了他们被定义的范围内就会被销毁，这使得代码相当危险。

{% highlight python %}
void (^block)();

if ( /* some condition */ ) {
    block = ^{ /* do something */ };
} else {
    block = ^{ /* do something else */ };
}

block();
{% endhighlight %}

一个好的做法是将block分配在堆上，这样出了定义block的范围后，仍然可以使用，可以用copy实现
{% highlight python %}
void (^block)();

if ( /* some condition */ ) {
    block = [^{ /* do something */ } copy];
} else {
    block = [^{ /* do something else */ } copy];
}

block();
{% endhighlight %}
