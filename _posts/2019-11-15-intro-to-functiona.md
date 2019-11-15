---
layout: post
title: "Functional Programming"
---

## What is functional?

Traditional imperative programming can be broken down into three major parts: state, sequential, and assignment (Zuhud, 2013). Essentially, a series of sequential state modifications are used to express the program. This style of programming can be seen in the core of most of our modern computers; however, some researchers argue that additional abstractions can be made to write more expressive programs. One issue they see with imperative programming is that “state changes during the execution of the code before returning the final value rises unintended side effects, especially in critical mission and real-time software's[sic]” (Khanfor, 2017). This means that a developer cannot determine what effects a function call may have just by looking at its function signature. One solution to this is to use a style of programming called functional programming. In a purely functional language, no existing state can be modified, only new results can be generated. Because of this restriction, developers can ensure that calling a function will not affect the overall state of the program; such a guarantee cannot be made for imperative programming. (Khanfor, 2017). On top of “statelessness”, functional programming provides a set of very useful features that are worth mentioning: lazy evaluation, infinite data structures, increased software reuse, and intuitive ways of expressing non-determinism (Fischer, 2011). It is worth noting, however, that underneath the functional abstractions, an imperative machine will provide the desired solution (Zuhud, 2013). 

## Why functional?

Some researchers argue that the functional abstractions produce higher quality (and easier to reason about) code without sacrificing performance (Pankratius, 2012); however, others suggest that the conciseness of these abstractions can make programs more challenging to read (Lippart, 2010).

- Laziness
- Statelessness
- Higher order functions
- Conciseness

## Where functional excels

### Data Processing

Functional languages are typically very good at data processing. They provide easy ways of filtering, aggregating, sorting and maninpulating lists and list-like structures. A lot of this comes from the fact that they support functions as a first class data type. Because functions can be passed around as data, a sort function can be generalized to accept a “ordering” function. Or you apply a function onto each element of a list, also known as the “map” function. 

## Examples

### Summing a List

#### Haskell

```haskell
sum [] = 0            -- Empty list sums to 0
sum (x:xs) = x + sum xs    -- Recursively sum on the rest
```

#### C# (imperative)

```csharp
int sum = 0;
foreach(var x in list){
    sum += x
}
```


#### C# (functional without using .Sum())

```csharp
sum = list.Aggregate(0, (acc, x) => acc + x);
```

Talk about the example and the differences between the languages


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

## What's Next?

In the next view posts, we'll dive deeper into some of the features that functional languages have to offer. We'll start by exploring [higher order functions]({% link _posts/2019-11-16-higher-order-functions.md %})
