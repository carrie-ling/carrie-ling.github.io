---
layout: postjs
title:  "编写可维护的javascript!"
date:   2014-09-25 17:46:13
tags: [postjs]
categories: 读"编写可维护的javascript!"一书有感

---
# 编写可维护的javascript #

## 第一部分 ##

### 编程风格 ###

1、代码质量检查工具，JsLint和JsHint

jsLint只是简单查找不符合Javascritpt模式的，错误的小工具。经过数年的变化，jslint已经成为一个有用的工具，不仅仅可以找出代码中潜在的错误，而且能针对你的代码贵出编码风格上的警告。

Crockford将他对javascript风格的观点分成了三个不同的部分。

>“javascript风格的组成部分（第一部分）”（http://javascritp.crockford.com/style1/html），包含基本的模式和语法。
>”javascript风格的组成部分（第二部分）“（http://javascript.crockfrod.com/style2.html），包含一般性的javascript惯用法。
>”javascript编程语言的编码规范“（http://javascript.crockford.com/code.html）这个规范更加全面，从前两部分中提炼出了编程风格的精华部分，同时增补了少量的编程风格的指引；


{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
