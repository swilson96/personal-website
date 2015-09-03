---
layout: post
title:  "The specified named connection is either not found in the configuration, not intended to be used with the EntityClient provider, or not valid."
date:   2015-09-04
categories: blog

cover_image: FullEntityConnectionError.PNG

excerpt: "There are many possible causes of this .NET Entity Framework error, but here's one that caught me out."
---
"The specified named connection is either not found in the configuration, not intended to be used with the EntityClient provider, or not valid."

There are many possible causes of this error, but here's one that caught me out.

A client recently had their hosting provider move their database for reasons I won't go into, and they tried to keep the site working by replacing the connection strings in web.config with the config they thought would connect to the new database. There are two connection strings, one for the ASP.NET Membership stuff like logging in and managing users, and one for the application data, which is accessed through Entity Framework. This is what the config looked like when I was called in (sensitive bits changed, of course):

{% highlight xml %}
<add name="ApplicationServices" connectionString="Server=SQL-07; Database=db1; User Id=user_0001; Password=XXXX" providerName="System.Data.SqlClient" />
<add name="ApplicationDBEntities" connectionString="metadata=res://*/Models.ApplicationDBModel.csdl|res://*/Models.ApplicationDBModel.ssdl|res://*/Models.ApplicationDBModel.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=SQL-07; Database=db1; User Id=user_0001; Password=XXXX; MultipleActiveResultSets=True&quot;" providerName="System.Data.SqlClient" />
{% endhighlight %}

Interestingly, you could go to the homepage, log in, and go to the user management screens. But if you tried to do anything else, e.g. view and of the EF entities, you would see this error:

![This error could mean a lot of things](/images/SmallEntityConnectionError.PNG)

So clearly the membership connection worked and the entity framework connection didn't. I checked and double checked for differences but the server, username, password were all identical. I checked everything else in the connection string and it was all identical to the web.config when the app worked. I even got so desperate I tried messing around with the escaped quotes, replacing the escaped &quot; with unescaped single quotes and trying various combinations!

The culprit was the one thing I didn't initially think to check: the 'providerName' element. Since it was the same in the broken connection as in the working one, it hardly looked suspicious! But an entity framework connection string element needs the provider name to be set as 'System.Data.EntityClient', and that's why it thought the connection string was invalid.

So here's the working config; the only thing changed is the 'providerName' at the end:

{% highlight xml %}
<add name="ApplicationDBEntities" connectionString="metadata=res://*/Models.ApplicationDBModel.csdl|res://*/Models.ApplicationDBModel.ssdl|res://*/Models.ApplicationDBModel.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=SQL-07; Database=db1; User Id=user_0001; Password=XXXX; MultipleActiveResultSets=True&quot;" providerName="System.Data.EntityClient" />
{% endhighlight %}

In the end I spotted the problem by taking a step back, digging up the old web.config and taking a close look at everything that had changed, where previously I'd been focussing on just the DB connection details.