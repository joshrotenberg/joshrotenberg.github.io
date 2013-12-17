---
layout: post
title: "Functions in Data.Maybe"
description: ""
category: haskell
tags: []
---

Most introductions to monads in Haskell talk about `Maybe`. Here are
some of the handy functions in that module for operating on the type.
`maybe` (the first function below) is should be available in ghci
right away in Prelude, but the others require that you load the
Data.Maybe module in ghci:

{% highlight haskell %}
λ> :m +Data.Maybe
{% endhighlight %}

`maybe` either applies a function to a `Maybe` or returns a default
value. It's type signature looks like this:

{% highlight haskell %}
maybe :: b -> (a -> b) -> Maybe a -> b
{% endhighlight %}
---
{% highlight haskell %}
λ> let x = Just 40
λ> let y = Nothing
λ> maybe 30 (*2) x -- x is Just 40, so it gets applied to (*2)
80
λ> maybe 30 (*2) y -- y is Nothing, we end up with the default value
30
{% endhighlight %}


`isJust` is straightforward. Given a `Maybe` argument, it returns true if it's `Just`:

{% highlight haskell %}
isJust :: Maybe a -> Bool
{% endhighlight %}
---
{% highlight haskell %}
λ> isJust x
True
λ> isJust y
False
{% endhighlight %}


`isNothing`: not hard to figure what this does:

{% highlight haskell %}
isNothing :: Maybe a -> Bool
{% endhighlight %}
---
{% highlight haskell %}
λ> isNothing x
False
λ> isNothing y
True
{% endhighlight %}

`fromJust` is a relatively unsafe way to get at the value of a `Maybe`:

{% highlight haskell %}
fromJust :: Maybe a -> a
{% endhighlight %}
---
{% highlight haskell %}
λ> fromJust x
40
λ> fromJust y
*** Exception: Maybe.fromJust: Nothing -- woops. use a case expression in real code
{% endhighlight %}

`fromMaybe` is similar to `maybe`, but instead of a function argument
being applied to the value, you simply get the value in your `Maybe`
or the default you pass in:

{% highlight haskell %}
fromMaybe :: a -> Maybe a -> a
{% endhighlight %}
---
{% highlight haskell %}
λ> fromMaybe 30 x
40
λ> fromMaybe 30 y
30
{% endhighlight %}

`maybeToList` will plop your `Maybe`'s value into a list and return it:

{% highlight haskell %}
maybeToList :: Maybe a -> [a]
{% endhighlight %}
---
{% highlight haskell %}
λ> maybeToList x
[40]
λ> maybeToList y
[] -- always get a value, it just happens to be an empty list if we pass in Nothing
{% endhighlight %}

`listToMaybe` goes the other way:

{% highlight haskell %}
listToMaybe :: [a] -> Maybe a
{% endhighlight %}
---
{% highlight haskell %}
λ> listToMaybe [2]
Just 2
λ> listToMaybe $ maybeToList x
Just 40
λ> listToMaybe $ maybeToList y
Nothing
λ> listToMaybe [20,40,60] -- listToMaybe will drop all but the first element
Just 20
λ> listToMaybe "foo"
Just 'f'
{% endhighlight %}

`catMaybes` takes a list of `Maybe`s and returns a list of the values
for any `Just` value, but drops out your `Nothing`s:

{% highlight haskell %}
catMaybes :: [Maybe a] -> [a]
{% endhighlight %}

{% highlight haskell %}
λ> catMaybes [x, y, (Just 21), Nothing, Just(22)]
[40,21,22]
{% endhighlight %}

`mapMaybe` lets you apply a function that accepts and returns a
`Maybe` to a list of `Maybe`s, but returns a list of the values (and
skips the `Nothing`s):

{% highlight haskell %}
mapMaybe :: (a -> Maybe b) -> [a] -> [b]
{% endhighlight %}

{% highlight haskell %}
λ> let f x = (+2) <$> x -- or fmap (+2) x
λ> f (Just 30)
Just 32
λ> mapMaybe f [(Just 20), Nothing, x]
[22,42]
{% endhighlight %}
