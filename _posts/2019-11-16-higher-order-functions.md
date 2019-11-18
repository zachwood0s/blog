---
layout: post
title: "Higher Order Functions"
---

This is the second article in the functional programmin series.

- [Functional Programming]({% link _posts/2019-11-15-intro-to-functiona.md %})
- *Higher-Order Functions*
- [Laziness and Infinite Data Structures]({% link _posts/2019-11-16-laziness.md %})

# Higher-Order Functions

A higher-order function is a function that takes a function as an input or returns a function as output. In the previous post, I mentioned that the `map` function is a higher-order function.

## map

`map` is a function that applies one function to every element in a list, producing a new list. Say we had a list of numbers 1-4 and we wanted to multiple them each by two. `map` is the perfect function for doing this. 

```haskell
by2 x = x * 2

map by2 [1, 2, 3, 4]
-- Output: [2, 4, 6, 8]
```

Here we provided map with a function `by2` which was then applied to each element of the list. In other words it returns a new list that looks like:

```haskell
[by2 1, by2 2, by2 3, by2 4]
```

Our example could be rewritten to be quite a bit simpler by just doing

```haskell
map (*2) [1, 2, 3, 4]
``` 

but the other way makes it a little more obvious (I think) to explain what's going on.

### Why is this useful?

To see why this is useful, it's helpful to compare this to an imperative approach. In (imperative) C# we would have done something like

```csharp
List<int> input = new List<int>{1, 2, 3, 4};
List<int> result = new List<int>();

foreach(var a in input)
{
  result.Add(a * 2);
}
```

Notice how both of these solutions produce the same result, but the functional one is much more concise. In the imperative approach, we're once again describing how we want to produce the result.

> Loop through the list, multiply by 2 and add it to the result.

In the functional approach, we're describing what the result should be.

> Our result should be our original list with every element multiplied by two.

The distinction is subtle but it's an important one to make when looking at imperative style vs. functional (declarative) style.

## Other Common Higher-Order Functions

### filter

Filter takes in a predicate and a list and returns a new list consisting of elements that matched the predicate.

```haskell
filter (>10) [1, 10, 20, 6, 30]
-- Output: [20, 30]
```
Here we're keeping only the elements that are greater than 10

### foldl / foldr

Both of the fold functions take a list of elements and "fold" them into one result. Foldl is "fold from the left" whereas foldr is "fold from the right". This is done taking in a function that accepts an accumulator and the current element in the list. The sum example that we saw in the previous article can be written using these functions.

```haskell
sum xs = foldl (+) 0 xs
```

To see the distinction between `foldl` and `foldr` its useful to look at an example. If we fold a list using subtraction, we'll get different results depending on which direction we go. 

```haskell 
foldr (-) 0 [1, 2, 3, 4]
-- (1 - (2 - (3 - (4 - 0)))) = -2

foldl (-) 0 [1, 2, 3, 4]
-- ((((0 - 1) - 2) - 3) - 4) = -10
```

## Conclusion

Higher-order functions allow you to generalize a function so that it can work in various different scenarios. This allows you to avoid writing the same boiler plate code over and over again and instead allows you to focus on the logic that matters. There are many other higher-order functions that I haven't covered here but these are the most common ones. If you're interested in a more in depth look at higher-order functions, I highly recommend reading [this chapter](http://learnyouahaskell.com/higher-order-functions) in "Learn You A Haskell". 

In the next article, we'll look into [lazy evaluation and infinite data structures]({% link _posts/2019-11-16-laziness.md %})
