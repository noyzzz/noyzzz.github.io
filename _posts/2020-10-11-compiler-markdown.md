---
layout: post
title: Compiler Design 
subtitle: A self designed compiler for a new programming language
tags: [Project]
comments: true
---

### Brief introduction

{: .box-note}
**I**n this project we implemented a compiler for a new grammer. CUPS and LLVM are the tools that we used for implementation. Tests were also designed with JUnit.
This project had three main phases. The first phase was implementing a lexical analyser for the given grammer. After that we implemented the parser using CUPS in java. The last phase was implementing the code generator which was done in LLVM.


## Compiler Grammer
〈𝑝𝑟𝑜𝑔𝑟𝑎𝑚〉 → {〈𝑣𝑎𝑟\_𝑑𝑐𝑙〉∗ \| 〈𝑓𝑢𝑛𝑐\_𝑒𝑥𝑡𝑒𝑟𝑛〉∗ \|〈𝑠𝑡𝑟𝑢𝑐𝑡\_𝑑𝑒𝑐〉∗ } +

〈𝑓𝑢𝑛𝑐\_𝑒𝑥𝑡𝑒𝑟𝑛〉 → 〈𝑓𝑢𝑛𝑐\_𝑑𝑐𝑙〉 \| 〈𝑒𝑥𝑡𝑒𝑟𝑛\_𝑑𝑐𝑙〉

〈𝑓𝑢𝑛𝑐\_𝑑𝑐𝑙〉 → 〈𝑡𝑦𝑝𝑒〉𝒊𝒅 ([〈𝑎𝑟𝑔𝑢𝑚𝑒𝑛𝑡𝑠〉]) ; \| 〈𝑡𝑦𝑝𝑒〉𝒊𝒅 (〈𝑎𝑟𝑔𝑢𝑚𝑒𝑛𝑡𝑠〉]) 〈𝑏𝑙𝑜𝑐𝑘〉

〈𝑒𝑥𝑡𝑒𝑟𝑛\_𝑑𝑐𝑙〉 → 𝒆𝒙𝒕𝒆𝒓𝒏〈𝑡𝑦𝑝𝑒〉𝒊𝒅 ;

〈𝑎𝑟𝑔𝑢𝑚𝑒𝑛𝑡𝑠〉 → 〈𝑡𝑦𝑝𝑒〉 𝒊𝒅 [{ ′[′ ′]′ } + ] [, 〈𝑎𝑟𝑔𝑢𝑚𝑒𝑛𝑡𝑠〉]

〈𝑡𝑦𝑝𝑒〉 → 𝒊𝒏𝒕 \| 𝒃𝒐𝒐𝒍 \| 𝒇𝒍𝒐𝒂𝒕 \| 𝒍𝒐𝒏𝒈 \| 𝒄𝒉𝒂𝒓 \| 𝒅𝒐𝒖𝒃𝒍𝒆 \| 𝒊𝒅 \| 𝒔𝒕𝒓𝒊𝒏𝒈 \| 𝒗𝒐𝒊𝒅 \| 𝒂𝒖𝒕𝒐〈𝑠𝑡𝑟𝑢𝑐𝑡\_𝑑𝑒𝑐〉 → 𝒓𝒆𝒄𝒐𝒓𝒅𝒊𝒅𝒃𝒆𝒈𝒊𝒏〈𝑣𝑎𝑟\_𝑑𝑐𝑙〉 + 𝒆𝒏𝒅𝒓𝒆𝒄𝒐𝒓𝒅 ;

〈𝑣𝑎𝑟\_𝑑𝑐𝑙〉 → [𝒄𝒐𝒏𝒔𝒕] 〈𝑡𝑦𝑝𝑒〉〈𝑣𝑎𝑟\_𝑑𝑐𝑙\_𝑐𝑛𝑡〉 [,〈𝑣𝑎𝑟\_𝑑𝑐𝑙\_𝑐𝑛𝑡〉] ∗ ;

〈𝑣𝑎𝑟\_𝑑𝑐𝑙\_𝑐𝑛𝑡〉 → 〈𝑣𝑎𝑟𝑖𝑎𝑏𝑙𝑒〉 [= {〈𝑒𝑥𝑝𝑟〉}]

〈𝑏𝑙𝑜𝑐𝑘〉 → ′𝒃𝒆𝒈𝒊𝒏′ { 〈𝑣𝑎𝑟\_𝑑𝑐𝑙〉 \| 〈𝑠𝑡𝑎𝑡𝑒𝑚𝑒𝑛𝑡〉 } ∗ ′𝒆𝒏𝒅′

〈𝑠𝑡𝑎𝑡𝑒𝑚𝑒𝑛𝑡〉 → 〈𝑎𝑠𝑠𝑖𝑔𝑛𝑚𝑒𝑛𝑡〉 \| 〈𝑚𝑒𝑡ℎ𝑜𝑑\_𝑐𝑎𝑙𝑙〉 ; \| 〈𝑐𝑜𝑛𝑑\_𝑠𝑡𝑚𝑡〉 \| 〈𝑙𝑜𝑜𝑝\_𝑠𝑡𝑚𝑡〉 \| 𝒓𝒆𝒕𝒖𝒓𝒏 [〈𝑒𝑥𝑝𝑟〉]; \| 𝒃𝒓𝒆𝒂𝒌 ; \| 𝒄𝒐𝒏𝒕𝒊𝒏𝒖𝒆 ; \| 𝒔𝒊𝒛𝒆𝒐𝒇(〈𝑡𝑦𝑝𝑒〉)

〈𝑎𝑠𝑠𝑖𝑔𝑛𝑚𝑒𝑛𝑡〉 → 〈𝑣𝑎𝑟𝑖𝑎𝑏𝑙𝑒〉 = 〈𝑒𝑥𝑝𝑟〉

〈𝑚𝑒𝑡ℎ𝑜𝑑\_𝑐𝑎𝑙𝑙〉 → 𝒊𝒅 ([〈𝑝𝑎𝑟𝑎𝑚𝑒𝑡𝑒𝑟𝑠〉])

〈𝑝𝑎𝑟𝑎𝑚𝑒𝑡𝑒𝑟𝑠〉 → 〈𝑒𝑥𝑝𝑟〉 \| 〈𝑒𝑥𝑝𝑟〉, 〈𝑝𝑎𝑟𝑎𝑚𝑒𝑡𝑒𝑟𝑠〉

〈𝑐𝑜𝑛𝑑\_𝑠𝑡𝑚𝑡〉 → 𝒊𝒇 (〈𝑒𝑥𝑝𝑟〉〈𝑏𝑙𝑜𝑐𝑘〉 [𝒆𝒍𝒔𝒆〈𝑏𝑙𝑜𝑐𝑘〉] \| 𝒔𝒘𝒊𝒕𝒄𝒉 (𝒊𝒅) 𝒐𝒇∶′{′ [{𝒄𝒂𝒔𝒆𝒊𝒏𝒕\_𝒄𝒐𝒏𝒔𝒕∶〈𝑏𝑙𝑜𝑐𝑘〉} ∗] ∗𝒅𝒆𝒇𝒂𝒖𝒍𝒕: 〈𝑏𝑙𝑜𝑐𝑘〉 ′}′

〈𝑙𝑜𝑜𝑝\_𝑠𝑡𝑚𝑡〉 → 𝒇𝒐𝒓 ([〈𝑣𝑎𝑟\_𝑑𝑐𝑙〉] ; 〈𝑒𝑥𝑝𝑟〉 ; [〈𝑎𝑠𝑠𝑖𝑔𝑛𝑚𝑒𝑛𝑡〉 \| 〈𝑒𝑥𝑝𝑟〉]) 〈𝑏𝑙𝑜𝑐𝑘〉 | 𝒓𝒆𝒑𝒆𝒂𝒕〈𝑏𝑙𝑜𝑐𝑘〉𝒖𝒏𝒕𝒊𝒍 (〈𝑒𝑥𝑝𝑟〉); \| 𝒇𝒐𝒓𝒆𝒂𝒄𝒉(𝒊𝒅𝒊𝒏𝒊𝒅) 〈𝑏𝑙𝑜𝑐𝑘〉

〈𝑒𝑥𝑝𝑟〉 → 〈𝑒𝑥𝑝𝑟〉 〈𝑏𝑖𝑛𝑎𝑟𝑦\_𝑜𝑝〉〈𝑒𝑥𝑝𝑟〉 \| (〈𝑒𝑥𝑝𝑟〉) \| 〈𝑚𝑒𝑡ℎ𝑜𝑑\_𝑐𝑎𝑙𝑙〉 \| 〈𝑣𝑎𝑟𝑖𝑎𝑏𝑙𝑒〉 \| 〈𝑐𝑜𝑛𝑠𝑡\_𝑣𝑎𝑙〉 \| − 〈𝑒𝑥𝑝𝑟〉 \| ! 〈𝑒𝑥𝑝𝑟〉

〈𝑣𝑎𝑟𝑖𝑎𝑏𝑙𝑒〉 → 𝒊𝒅 [{ ′[′ 〈𝑒𝑥𝑝𝑟〉 ′]′ } +] \| ~〈𝑣𝑎𝑟𝑖𝑎𝑏𝑙𝑒〉 \| − −〈𝑣𝑎𝑟𝑖𝑎𝑏𝑙𝑒〉 \| + +〈𝑣𝑎𝑟𝑖𝑎𝑏𝑙𝑒〉 \| 〈𝑣𝑎𝑟𝑖𝑎𝑏𝑙𝑒〉 − − \| 〈𝑣𝑎𝑟𝑖𝑎𝑏𝑙𝑒〉 + +

〈𝑏𝑖𝑛𝑎𝑟𝑦\_𝑜𝑝〉 → 〈𝑎𝑟𝑖𝑡ℎ𝑚𝑎𝑡𝑖𝑐〉 \| 〈𝑐𝑜𝑛𝑑𝑖𝑡𝑖𝑜𝑛𝑎𝑙〉

〈𝑎𝑟𝑖𝑡ℎ𝑚𝑎𝑡𝑖𝑐〉 → + | − | ∗ | / | % | &amp; | ′|′ | ^ |

〈𝑐𝑜𝑛𝑑𝑖𝑡𝑖𝑜𝑛𝑎𝑙〉 → == | ! = | \&gt;= |\&lt;=| \&lt; | \&gt; | and | or | not

〈𝑐𝑜𝑛𝑠𝑡\_𝑣𝑎𝑙〉 → 𝒊𝒏𝒕\_𝒄𝒐𝒏𝒔𝒕 | 𝒓𝒆𝒂𝒍\_𝒄𝒐𝒏𝒔𝒕 | 𝒄𝒉𝒂𝒓\_𝒄𝒐𝒏𝒔𝒕 |𝒃𝒐𝒐𝒍\_𝒄𝒐𝒏𝒔𝒕 | 𝒔𝒕𝒓𝒊𝒏𝒈\_𝒄𝒐𝒏𝒔𝒕 | 𝒍𝒐𝒏𝒈\_𝒄𝒐𝒏𝒔t


This is a demo post to show you how to write blog posts with markdown.  I strongly encourage you to [take 5 minutes to learn how to write in markdown](https://markdowntutorial.com/) - it'll teach you how to transform regular text into bold/italics/headings/tables/etc.

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

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .mx-auto.d-block :}

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
