---
layout: post
title: "Functional Programming"
---

## What is functional?

Traditional imperative programming can be broken down into three major parts: state, sequential, and assignment. Essentially, a series of sequential state modifications are used to express the program. This style of programming can be seen in the core of most of our modern computers; however, some researchers argue that additional abstractions can be made to write more expressive programs. 

> State changes during the execution of the code before returning the final value rises unintended side effects, especially in critical mission and real-time software's[sic].
>
> -- <cite>Kanfor (full name)</cite>

This means that a developer cannot determine what effects a function call may have just by looking at its function signature. One solution to this is to use a style of programming called functional programming. In a purely functional language, no existing state can be modified, only new results can be generated. Because of this restriction, developers can ensure that calling a function will not affect the overall state of the program; such a guarantee cannot be made for imperative programming. On top of “statelessness”, functional programming provides a set of very useful features that are worth mentioning: lazy evaluation, infinite data structures, increased software reuse, and intuitive ways of expressing non-determinism. It is worth noting, however, that underneath the functional abstractions, an imperative machine will provide the desired solution. 

## Why functional?

Some researchers argue that the functional abstractions produce higher quality (and easier to reason about) code without sacrificing performance; however, others suggest that the conciseness of these abstractions can make programs more challenging to read. Functional programming concepts can be applied in almost every situation but there are a few areas where it really excels.

### Data Processing

Functional languages are typically very good at data processing. They provide easy ways of filtering, aggregating, sorting and maninpulating lists and list-like structures. A lot of this comes from the fact that they support functions as a first class data type. Because of this, a function can be generalized to accept a function as an argument. These are called higher-order functions (we'll look into those later). A sort function can then be generalized to accept a “ordering” function. Or you could apply a function onto each element of a list, also known as the `map` function. 

### Parallelism

Because functions cannot produce side effects in a pure language, this makes them very good at dealing with parallelism. Functions can all be started in parellel while guaranteeing that they can't effect the overall state of the program. This eliminates a whole class of errors that are typically dealt with in parallel programming. Also, because of something called [referential transparency](https://softwareengineering.stackexchange.com/questions/254304/what-is-referential-transparency), it doesn't matter what order they are evaluated in. Meaning that every sub-expression can be evaluated in parallel.

### Expressivness

Typically functional code is very concise. Programs typically describe "what to do" rather than "how to do". Because of this, a lot of the boilerplate code is lost and you're left with a more concise, more expressive result. This does make functional languages less beginner friendly, however. Functions may be only one line long but they'll have a lot of meaning packed into that one line. Imperative languages, on the other hand, might produce quite a bit longer code but will typically "spell out" what they are doing. We'll see examples of this in the later articles.

## Quick Example

Before we jump into the more complex examples, I want to give one simpler example where I compare the two styles. I'm going to use C# as the imperative language for these examples. C# is a multi-paradigm language that supports both functional and imperative styles. I'm going to use Haskell as the functional language. Haskell is a pure functional language.

I think summing the elements in a list is a good way to start. In a traditional imperative language, we would probably keep some counter and then add each element in that list to the counter. 

```csharp
int sum = 0;                // Create out counter
foreach(var x in list){     // Loop through the list
    sum += x                // Add to the counter
}
```

This is fairly straight forward and you can essentially read the code as words. 

> Start sum at 0 and for each element in the list, add that element to the sum

In a Haskell, however, there is no such thing as a `for` loop. Instead we use recursion like so:

```haskell
sum [] = 0                 -- Empty list sums to 0
sum (x:xs) = x + sum xs    -- Add current element to the sum of the rest
```

> The sum is defined as the first element plus the sum of the rest
> *Note: the `(x:xs)` is taking a list and capturing the first element as `x` and the rest as `xs`.*

Notice how the C# version describes more ***how to*** sum a list, whereas, the functional style describe ***what*** the sum of a list is. The C# version states that we need a counter, we need to loop through every element in the list, and that we need to add those to the counter. It doesn't really describe what the sum is, only how to compute it. The functional on the other hand, says that the sum of a list is the first element in the list plus the sum of the rest of the elements. This describes what a sum is more than how to do it.

### Note about C#

It should be noted that C# does support functional concepts. For example, summing a list could be written like this in C#:

```csharp
// Aggregate takes an initial accumulator and a function 
// that takes the previous accumulator and the element on the list.
// Our function takes the previous acc and adds it to the current element. 
sum = list.Aggregate(0, (acc, x) => acc + x);
```

Most modern languages today support multiple paradigms, but for the sake of simplicity, we'll only focus on imperative in C# and functional in Haskell.


## What's Next?

In the next view posts, we'll dive deeper into some of the features that functional languages have to offer. 

- Higher order functions
- Laziness and Infinite Data Structures
- Representing Non-Determinism
(I'll add links to the articles on this)

We'll start by exploring [higher order functions]({% link _posts/2019-11-16-higher-order-functions.md %})


## Examples

These are just some examples that I plan on using throughout the blog posts. They'll go in the other posts where they're more specific I think. Maybe one or two will be used on this first article.


### Fibanocci

#### Haskell (Naive)
```haskell
fib :: Integer -> Integer
fib 0 = 0
fib 1 = 1
fib n = fib (n-1) + fib (n-2)
```

#### Haskell (Cool but complicated, utilizes infinite data structures)

```haskell
fibs = 1 : 1 : zipWith (+) fibs (tail fibs)
```

#### C#  (imperative)

```csharp
public static int GetNthFibonacci_Ite(int n)  
{  
    int number = n - 1; //Need to decrement by 1 since we are starting from 0  
    int[] Fib = new int[number + 1];  
    Fib[0]= 0;  
    Fib[1]= 1;  
    for (int i = 2; i <= number;i++)  
    {  
       Fib[i] = Fib[i - 2] + Fib[i - 1];  
    }  
    return Fib[number];  
} 
```

#### C# (recursive)

```csharp
public static int GetNthFibonacci_Rec(int n)  
{  
    if ((n == 0) || (n == 1))  
    {  
        return n;  
    }  
    else  
        return GetNthFibonacci_Rec(n - 1) + GetNthFibonacci_Rec(n - 2);  
}
```

Talk about the examples here

### Merge Sort
#### Haskell
```haskell
mergesort :: [String] -> [String]
mergesort = mergesort' . map wrap

mergesort' :: [[String]] -> [String]
mergesort' [] = []
mergesort' [xs] = xs
mergesort' xss = mergesort' (merge_pairs xss)

merge_pairs :: [[String]] -> [[String]]
merge_pairs [] = []
merge_pairs [xs] = [xs]
merge_pairs (xs:ys:xss) = merge xs ys : merge_pairs xss

merge :: [String] -> [String] -> [String]
merge [] ys = ys
merge xs [] = xs
merge (x:xs) (y:ys)
 = if x > y
        then y : merge (x:xs)  ys
        else x : merge  xs    (y:ys)

wrap :: String -> [String]
wrap x = [x]
```

#### C# 

```csharp
private void Merge(int[] input, int left, int middle, int right)
{
    int[] leftArray = new int[middle - left + 1];
    int[] rightArray = new int[right - middle];
 
    Array.Copy(input, left, leftArray, 0, middle - left + 1);
    Array.Copy(input, middle + 1, rightArray, 0, right - middle);
 
    int i = 0;
    int j = 0;
    for (int k = left; k < right + 1; k++)
    {
        if (i == leftArray.Length)
        {
            input[k] = rightArray[j];
            j++;
        }
        else if (j == rightArray.Length)
        {
            input[k] = leftArray[i];
            i++;
        }
        else if (leftArray[i] <= rightArray[j])
        {
            input[k] = leftArray[i];
            i++;
        }
        else
        {
            input[k] = rightArray[j];
            j++;
        }
    }
}
 
private void MergeSort(int[] input, int left, int right)
{
    if (left < right)
    {
        int middle = (left + right) / 2;
 
        MergeSort(input, left, middle);
        MergeSort(input, middle + 1, right);
 
        Merge(input, left, middle, right);
    }
} 
```
Talk about the examples here
