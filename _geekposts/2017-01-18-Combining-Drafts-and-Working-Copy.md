---
layout: post
title: Combining Drafts and Working Copy
date: 2017-01-18
---
Both [Drafts](http://agiletortoise.com/drafts/) and [Working Copy](https://workingcopyapp.com) form the core of my workflow in publishing posts to my Github hosted website. One of the requirements of a Jekyll hosted Github blog is that posts must be named in a particular way i.e. yyy-mm-dd-title-of-post.md. In order to automate the naming of posts from Drafts I wrote the following Javascript to run as an action in Drafts:

{% highlight java %}
var str = draft.processTemplate("[[title]]");
var res = str.replace(/\s/g, "-");
draft.defineTag("hyphentitle", res);
{% endhighlight %}


In order to send the post from Drafts to Working Copy the Draft action has, as a second step, the following x-callback-url:

{% raw %}
```
working-copy://x-callback-url/write/?repo={{name.of.github.repo}}&path={{_posts/}}[[date]]-[[hyphentitle]].md&text=---%0a{{layout: post}}%0a{{title: }}[[title]]%0a---%0a[[body]]&key={{key}}
```
{% endraw %}
With one action I can automate the naming and posting of my blog post.