---
layout: post
title:  "Finding duration between two times (in python)"
date:   2014-10-14 08:32:03
categories: python beginner coding funbutuseless
---

{% highlight python %}
	def()	
{% endhighlight %}

This works quite well and lets me use Hiera as a sort-of pseudo-ENC. My `hiera.yaml` is straightforward, as are my hierarchies. The issue I was having came from a misunderstanding of Automated Parameter Lookups in Puppet 3.x (3.6, if you want to be specific). Essentially, I was trying to convert code similar to this in my old site.pp:

{% highlight ruby %}
    class { 'postgresql::server':
        enable => true,
        postgres_password => 'whatever'
    }
{% endhighlight %}

into something akin to this in the YAML:

{% highlight ruby %}
   ---
    classes:
        - postgresql

    postgresql::server:
        enable: true
        postgres_password: 'whatever'
{% endhighlight %}

What caused me such great consternation (and delayed resolution of the problem for much longer than I'd like to admit) was simply the fact that PostgreSQL did, in fact, install fully. I had to eventually realize that the password was not being set correctly before I began investigating the possibility that I was not as savvy with automated parameter lookup as I had hoped.

For anyone in a similar situation, here's the correct format for the above example:

{% highlight ruby %}
    ----
    classes:
        - postgresql

    postgresql::server::enable: true
    postgresql::server::postgres_password: 'whatever'
{% endhighlight %}

Fixed!
