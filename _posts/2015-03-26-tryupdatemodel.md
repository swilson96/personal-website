---
layout: post
title:  "Using TryUpdateModel in .NET MVC"
date:   2015-03-26
categories: blog

cover_image: airfixfail.png

excerpt: "In some legacy code I came across a controller that uses the built-in .NET method TryUpdateModel(). Is this method good practice, and how you you unit test a controller that uses it?"
---

In some legacy code I came across a controller that uses the built-in .NET controller method [TryUpdateModel()](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.tryupdatemodel(v=vs.100).aspx). It rings a bell from some old MVC 2 tutorials but I don't think I've ever seen it used in anger. So it raises two questions:

1. Is it good practice, or to be avoided?
2. How do you unit test a controller that uses it? My early attempts were thwarted by errors such as "System.ArgumentNullException: Value cannot be null. Parameter name: controllerContext".

Searching the internet and asking around the team didn't provide any answers to either. Eventually I decided

1. It's probably bad practice: if you are using TryUpdateModel or a similar method, then you're likely to be exposing business objects or persisted entities in the view, and on a project of any size it's a better practice to use a dedicated viewmodel (the Model-View-ViewModel pattern or [MVVM](http://en.wikipedia.org/wiki/Model_View_ViewModel)) then map that to your model explicitly. If must use reflection to map between viewmodels and other objects without lots of boilerplate code you could try [AutoMapper](https://github.com/AutoMapper/AutoMapper) or similar.

   There are security concerns as well: if you don't give it a whitelist you are risking a "mass assignment" style vulnerability, a bit like the one that [github famously suffered in 2012](http://blog.erratasec.com/2012/03/rubygithub-hack-translated.html). So if you do use it, at least use an overload that takes a whitelist of allowed parameters!

2. If, like me, you have this in legacy code and you need to create unit tests for it, then you need to do some mocking. This is what I came up with using Moq, based on a couple of StackOverflow posts:

{% highlight csharp %}
// Value provider, for methods like TryUpdateModel
// http://stackoverflow.com/questions/530133/mocking-requirements-for-tryupdatemodel-in-asp-net-rc1
Controller.ValueProvider = new FormCollection().ToValueProvider();

// Mock controllerContext, for methods like TryUpdateModel
// http://stackoverflow.com/questions/32640/mocking-asp-net-mvc-controller-context
var request = new Mock<HttpRequestBase>();
request.Setup(r => r.HttpMethod).Returns("POST");
var mockHttpContext = new Mock<HttpContextBase>();
mockHttpContext.Setup(c => c.Request).Returns(request.Object);
var controllerContext = new ControllerContext(mockHttpContext.Object, new RouteData(), new Mock<ControllerBase>().Object);
Controller.ControllerContext = controllerContext;
{% endhighlight %}

If you know of a cleaner solution, please let me know!
