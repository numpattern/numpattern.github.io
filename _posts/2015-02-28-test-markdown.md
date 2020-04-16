---
layout: post
title: How to start using Anaconda
subtitle: First steps from zero
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
---

**Well, it works on your command line interface such as Anaconda prompt on Windows**. In this occasion, we will go on windows for our exercise purposes.

In our case we will first state that we can use the Anaconda Navigator to get into the Spyder IDE or the Jupyter notebook to wirte a python program an execute. There are differences between both of them. On the other hand, we also can use the Anaconda prompt where we type python and the >>> means that we already are on python (to exit press Ctrl+z and Enter). Finally we can launch spyder or Jupyter from command line with Anaconda prompt. All of this is made in Windows. In this case, in particular, I will choose Jupyter Notebooks since they are powerful way to write and iterate on Python code for data analysis. I mean you are able to write lines of c ode and run them one at a time. A short-key we can use in Jupyter-Notebook is the Shift-Enter to run one line. Said all this, I think we can go into the python code to start creating out SVM python code.

**Here is some bold text**

## Here is a secondary heading

Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .center-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
