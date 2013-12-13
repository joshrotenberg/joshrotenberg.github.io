---
layout: post
title: "Functions in Data.Maybe"
description: ""
category: haskell
tags: []
published: false
---


Lots of intros to monads in Haskell talk about `Maybe` type. Here are
some of the handy functions in that module for operating on the type.

## maybe 

{% highlight haskell %}
maybe :: b -> (a -> b) -> Maybe a -> b
{% endhighlight %}

* 'maybe' lets you check for 'Nothing' and return a default in that case, or accept
