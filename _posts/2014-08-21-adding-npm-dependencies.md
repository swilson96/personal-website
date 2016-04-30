---
layout: post
title:  "Quick tip: Adding NPM dependencies"
date:   2014-08-21
categories: blog

cover_image: blog-cover.jpg

excerpt: "Use the --save option when installing node modules"

---
Just a quick tip. If you're learning node you don't usually see this on the tutorials, so you might not know it. When you install a new NPM dependency for your project, like this

{% highlight bash %}
$ npm install module
{% endhighlight %}

Then you also need to manually add it to package.json, so that anyone else can run npm install and run your code. You need to manually figure out which version(s) your project depends on. Instead try

{% highlight bash %}
$ npm install module --save 
{% endhighlight %}

Which adds the dependency to package.json automatically. Nice.
