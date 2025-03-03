---
layout: main_guide
title: Rails Girls on anynines
permalink: anynines
---

# Put your app online with anynines

*Created by Floor Drees, [@floordrees](https://twitter.com/floordrees)*

{% coach %}
Talk about the benefits of deploying to anynines vs utilising US data centers.
{% endcoach %}

### Get yourself some anynines

1. [Create an anynines account](https://anynines.com/).

2. [Download and install the Command Line Interface](https://anynines.zendesk.com/entries/60241846-How-to-install-the-CLI-v6) to interact with anynines.

3. Now select the anynines api endpoint as target and authenticate using your user credentials:

{% highlight sh %}
cf api https://api.de.a9s.eu  
cf login -u [your@email] -p [yourpassword]  
{% endhighlight %}


Or if that doesn't work for you, use:

{% highlight sh %}
cf login
{% endhighlight %}

... which will prompt you for your email address and password.

Wonder what that `cf` stands for? It's short for [Cloud Foundry](https://www.cloudfoundry.com/), a system anynines is using behind the scenes.

### Push your app online

Let's push this source code from your local machine to anynines:
{% highlight sh %}
$> cf push [application-name-of-your-choosing]
{% endhighlight %}

This will fail miserably since the example application needs a MySQL database to start. So, lets create one! The command below will create a MySQl service with a free service plan. After the plan name you have to specify a name for the service instance. This name will be used for further commands to refer to this service instance:

$> cf create-service mysql Pluto-free [service-name-you-can-choose]

(Really, you can use any name. Make it count!)

Next, binding the MySQL service instance to the application, to grant the application access to the MySQL instance, type:
{% highlight sh %}
$> cf bind-service [app-name-you-have-chosen-above] [service-name-you-have-chosen-above]
{% endhighlight %}

Finally we have to restart the application to make sure the service binding takes effect:
{% highlight sh %}
$> cf restart [app-name-you-have-chosen-above]
{% endhighlight %}

You will see this:  
{% highlight sh %}
Creating service postgresql-d2197... OK  
Binding postgresql-d2197 to railsgirls... OK  
{% endhighlight %}

Ending with... `Push successful! App 'railsgirls' available at railsgirls.de.a9sapp.eu`. Score!

### Version Control

We need to add our new code to version control. You can do this by running the following in the terminal:

{% highlight sh %}
git status
git add .
git commit -m "add anynines deployment"
{% endhighlight %}

{% coach %}
This would be a great time to talk about version control systems and git, if you haven't already.
{% endcoach %}

### Help

You can check all available cf sub-commands by typing `cf help`.
In case your terminal does not have all the answers, the anynines team probably does. Just shoot them a mail at support@anynines.com.

Happy deploying!
