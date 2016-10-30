Track 2
=======

## Clojure as a functional language

Clojure is a functional language - that means that functions are first class citizens just like
any other value type you have already encountered (ints/floats/objects etc.).
This means that we can pass functions as arguments to other functions, returning
functions from functions, and even manipulating functions.

## Syntax

Compared to other languages, Clojure does function calls slightly differently than the majority of common programming languages. 

```java
myFunction("hi", "bye")
```

In Clojure this might look like the following:

```clojure
(my-function "hi" "bye")
```

There are a couple differences here, first thing to note is that we are using a different
case style. In Clojure we don't use camel case but instead kebab case which is dash separated.

To prove we didn't just make this up: [link](http://c2.com/cgi/wiki?KebabCase)

Another thing is that we've placed the function inside the parenthesis. In Clojure a function
call is always an open paren followed by the function we want to call, followed by any arguments
we are passing to the function, followed by a closing paren.

We also aren't separating the arguments with a comma anymore. In Clojure
commas are simply treated as whitespace so we don't need them.

## Koans

Periodically we'll mention exercises from the Clojure Koans. These are a series of exercises
designed to teach Clojure.

Each one will be an assertion such as the following:

```clojure
(= true __)
```

The purpose of these exercises is to alter the `__` to make the assertion pass.

```clojure
(= true true)
```

The koans are done incrementally, as you complete each one it will then move on to the next koan.
Feel free to go through these at your own pace, or you can follow along this document which will periodically reference the koans.

To get the koans all setup and running visit the official [koans page](http://clojurekoans.com/).

### Common functions

#### Math

Unlike common languages, such as JavaScript, in which `+` is an operation that is used in arithmetic expressions, and square root `sqrt` is a function, in Clojure every action is a function, there is no notion of operators. Additionally all of the
math operators take a variable number of arguments.

```clojure
user=> (+ 1 1)
2
user=> (+ 1 2 3)
6
user=> (- 1 1)
0
user=> (- 5)
-5
user=> (* 2 3)
6
user=> (/ 6 2)
3
```

#### Equality

We can use the `=` function to test whether its arguments are all equal to each other.
Like the math functions the `=` function also takes multiple arguments.

```clojure
user=> (= "hello" "hello")
true
user=> (= 1 1 1)
true
user=> (= 1 2)
false
```

## `nil`

`nil` is a special value in Clojure, it means "nothing". It is commonly used when a desired value is not found. We will see examples of its use later. For now, just note that it is not equal to other values:

```clojure
user=> (= nil false)
false
user=> (= nil true)
false
user=> (= nil 0)
false
```
### Common types

Clojure mostly just uses Java classes under the hood. Lets examine some common types using
the `type` function.

```clojure
user=> (type 2015)
java.lang.Long
user=> (type 3.14)
java.lang.Double
user=> (type "clojure")
java.lang.String
user=> (type true)
java.lang.Boolean
```

### Keywords

Periodically you'll see things in Clojure programs that have a colon in front of it: `:red` `:some-key`. These are keywords.

From the Clojure docs:
`Keywords are symbolic identifiers that evaluate to themselves.
They provide very fast equality tests.`
So at a high level a keyword is a constant literal, just like the integer `1234` or
the string constant `"hello"`. Keywords look and often behave a lot like strings
with a couple exceptions.

* They begin with a `:` so `:hi` is an example keyword, while `"hi"` is an example string.
* They can't have spaces in them. `"hi there"` may be a valid string but it's not a valid
keyword.
* Keywords get special treatment in Clojure: they act like a function when looking up a value
in a hash-map - we'll introduce hash-maps later.

```clojure
user=> :red
:red
user=> (= :red :blue)
false
user=> (= :red :red)
true
```

### Exercises

Try to complete the first set of Koans `01_equalities.clj`

## Common data structures

### Lists

A list in Clojure is just a linked list. There are a couple of ways we can create them, the
first one being the `list` function:

```clojure
user=> (list)
()
```

This gave us an empty list, but we can also supply it with a variable number of arguments to
initialize the list with:

```clojure
user=> (list 1 2 3)
(1 2 3)
```

We can efficiently add to the beginning of a linked list using the `conj` function:

```clojure
user=> (conj (list 1 2 3) 0)
(0 1 2 3)
```

Accessing the head or tail of the list is possible using the `first` and `rest` functions.
Note that while `first` returns an element, `rest` returns a list.
We can also access a specific element using the `nth` function.

```clojure
user=> (first (list 1 2 3))
1
user=> (rest (list 1 2 3))
(2 3)
user=> (nth (list 1 2 3) 0)
1
```

Finally Clojure has a shorthand for defining lists; instead of calling the list function we can
use the following:

```clojure
user=> '(1 2 3)
(1 2 3)
user=> '()
()
```

#### Exercises

Try to complete the second set of Koans `02_lists.clj`.

### Vectors

A vector in Clojure is a lot like an array in other languages. That means we can efficiently access
the index of a vector compared to a list, but adding elements might be a bit more expensive than on
a list.

To create a vector we can use the vector function:

```clojure
user=> (vector 1 2 3)
[1 2 3]
user=> (vector)
[]
```

We can perform a lot of the same operations on vectors as we can on lists:

```clojure
user=> (first (vector 1 2 3))
1
user=> (rest (vector 1 2 3))
(2 3)
user=> (nth (vector 1 2 3) 0)
1
```

Let's try using the `conj` function from earlier:

```clojure
user=> (conj (vector 1 2 3) 0)
[1 2 3 0]
```

Wait, this isn't what we expect. With a list using `conj` added the new element to the front,
not to the back. This is because the `conj` function adds elements where it is most efficient
for the data structure.

In a list it is cheapest to add an element to the front, we don't
need to traverse the whole list to get to the end to add the element.

In a vector it is cheapest to add an element to the end. Because vectors are arrays if we were to
add to the front, we would need to move every element that was already there down a slot to make
room for the new element.

Finally like lists vectors have a shorthand way to create them quickly:

```clojure
user=> [1 2 3]
[1 2 3]
user=> []
[]
```

#### Exercises

Try to complete the third set of Koans `03_vectors.clj`.

### Sets

Sets are a data-structure that can only contain unique elements. We construct a set with
the `set` function which takes in some other sequence.

```clojure
user=> (set [1 2 3 4])
#{1 4 3 2}
user=> (set [1 1 2 3])
#{1 3 2}
```

So using this method of constructing a set will guarantee us a set of unique elements if we happen
to have duplicates they just get thrown away.

Finally Clojure has a shorthand way to create sets.

```clojure
user=> #{1 2 3}
#{1 3 2}
```

But using the shorthand we can't initialize it with duplicate elements.

```clojure
user=> #{1 1 2 3}

IllegalArgumentException Duplicate key: 1  clojure.lang.PersistentHashSet.createWithCheck (PersistentHashSet.java:68)
```

#### Exercises

Try to complete the fourth set of Koans `04_sets.clj`.

### Hash Maps

Hash-maps are just like maps or dictionaries in other languages. They store key/value pairs
and we can efficiently access values in the hash-map by their keys.

We can create hash-maps with the `hash-map` function. This function takes an even number
of arguments, which is of the format key followed by pair.

```clojure
(hash-map "blue" 30 "red" 100)
```

So this will create a map, with keys "blue" and "red" each with an integer as its value.

While you can use any Clojure elements as keys, most commonly keywords are used for this purpose since lookup by keywords is very fast:

```clojure
(hash-map :blue 30 :red 100)
```

To access values from the keys we can use the `get` function which takes a map as its first
argument and the key for the value we want to retrieve as the second.

```clojure
user-> (get (hash-map :blue 30 :red 100) :blue)
30
```

Note that a map is itself a function that can be applied to look up a value for a key:

```clojure
user-> ((hash-map :blue 30 :red 100) :blue)
30
```

A keyword is also a function that takes a map as a parameter and returns a value associated with this key:

```clojure
user-> (:blue (hash-map :blue 30 :red 100))
30
```

If a key doesn't appear in a map, all three ways of lookup will return `nil`:

```clojure
user=>  (get (hash-map :blue 30 :red 100) :green)
nil
user=> ((hash-map :blue 30 :red 100) :green)
nil
user=> (:green (hash-map :blue 30 :red 100))
nil
```

We can also build a new map from an old one using the `assoc` function

```clojure
user=> (assoc (hash-map :blue 30 :red 100) :green 20)
{:blue 30, :green 20, :red 100}
```
If the key already exists in the map, its value will be replaced by the new one in the resulting map:

```clojure
user=> (assoc (hash-map :blue 30 :red 100) :blue 20)
{:blue 20, :red 100}
```

Finally we have a shorthand to build maps, we don't need to use the hash-map function.

```clojure
user=> {:red 100, :blue 30}
{:red 100, :blue 30}
```

#### Exercises

Try to complete the fifth set of Koans `05_maps.clj`.

## Saving values into names

So far we've just been nesting our function calls, what if we wanted to be able to store
the result of a call into a name so we can easily access it? We can use the `def` function
for this.

```clojure
user=> (def PI 3.14)
#'user/PI
user=> (def some-list (list 1 2 3 4))
#'user/some-list
user=> some-list
(1 2 3 4)
user=> PI
3.14
```

## Conditional computation: `if` statement
It is very common that the result of a computation depends on a condition. The most straightforward conditional statement in Clojure is `if`. It has three parts: the condition, the result when the condition is true, and the result when the condition is false. Using it, we can compute expressions such as the absolute value of a number:
```clojure
user=> (def x -5)
#'user/x
user=> (if (neg? x) (- x) x)
5
user=> (def y 7)
#'user/y
user=> (if (neg? y) (- y) y)
7
```


## Defining your own functions

So far we've seen how we can use builtin functions, but how do we define our own?

We can use the `fn` function to create new functions, we can use the `def` function from the
previous section to associate the new function with a name. Lets write a simple square function.

`fn` takes two arguments, the first argument is a vector of the arguments and the second is the
result we want to return.

```clojure
user=> (def square (fn [number] (* number number)))
#'user/square
user=> (square 5)
25
```

Notice that we didn't need to specify a return statement like we often have to do in other languages.
In Clojure a function call will return the last expression it evaluates.

We can also use a shorthand to combine `def` and `fn` which is `defn`.

```clojure
(defn square [number] (* number number))
```

## Thinking functionally

Functional approach to programming means that a solution is constructed as a composition of functions. Each function returns a new entity that's one step closer to the desired results. This is different from the more common imperative approach that keeps changing data and variables in memory (often using loops) until the result is constructed or determined.

For example, consider determining if a string is a palindrome. In a traditional approach one would have a loop in which an index is changing as the string is being traversed that compares string characters to each other. In a functional approach one would just compare the string to its reverse and return the result:

```clojure
user=> (defn is-palindrome? [s] (= s (clojure.string/reverse s)))
#'user/is-palindrome?
user=> (is-palindrome? "anna")
true
user=> (is-palindrome? "ann")
false
```
Here the function `is-palindrome?` uses functions = and `clojure.string/reverse` as its elements. It does not directly iterate over the string in a loop.

Note that functions that return a boolean traditionally have names that end with a question mark. Such functions are called *predicates*. Note how the question mark helps you understand what the function call is doing.

More interesting examples involve working with functions at all levels of the language. Functions are what's called *first class citizens* in functional languages, so you can pass them as parameters to other functions, or even construct them "on the fly" like you would calculate numbers.

Below is a simple, somewhat artificial example of passing a function to a function: suppose we have a vector of at least two elements, and we want to check that both elements satisfy a given condition, but we don't know ahead of time what the condition is. Here is the function and some examples of its usage:
```clojure
user=> (defn condition-holds? [f v] (and (f (first v)) (f (second v))))
#'user/holds-condition?
user=> (condition-holds? odd? [3 4])
false
user=> (condition-holds? is-palindrome? ["eye" "bob"])
true
```

#### Anonymous functions
In functional languages functions are first class citizens, just like numbers, so you can put them together at any point, they don't need to be defined ahead of time.

In this example we put together a function right in the call to `condition-holds?` using the `fn` syntax that we have introduced earlier, and it doesn't even need a name: it's an *anonymous function*. In this case we are checking if the elements of a vector are less than 10:
```clojure
user=> (condition-holds? (fn [n] (< n 10)) [4 5])
true
user=> (condition-holds? (fn [n] (< n 10)) [4 15])
false
```

There is an even shorter notation for anonymous functions known as a *function literal*. Here the `#( )` denotes the body of the function, and the arguments are referred to as `%1, %2`, etc., or just `%` if there is only one. Here is the same example as above, only with the function literal:
```clojure
user=> (condition-holds? #(< % 10) [4 5])
true
user=> (condition-holds? #(< % 10) [4 15])
false
```
#### Exercises

Try to complete the sixth set of Koans `06_functions.clj`.

#### Recursion

But what if we want to check that a condition holds for all elements of a vector, but we don't know how long the vector is? There are several ways of accomplishing it.

Here we will look at the approach called *recursion* which breaks the vector into the first element and the rest of the elements, does some work on the element (in our case checks if it satisfies a given condition), and then *calls the same function* again *on the rest* of the vector if needed.

Developing recursive functions is a bit more involved process than what we have done so far, so let's build it step-by-step.

When developing a function, it is useful to start by writing down how you expect it to work:
```clojure
user=> (holds-for-all? odd? [1 3 -1])
true
user=> (holds-for-all? odd? [1 3 0 -1])
false
user=> (holds-for-all? #(< % 5) [5])
false
user=> (holds-for-all? #(<= % 5) [5])
true
```
Obviously, this doesn't work yet because the function `holds-for-all?` doesn't exist yet. But these cases help us understand how the function works:

1. If there is only one element, it returns the result of the predicate on that one element (see the last two cases).
2. If there is more than one element then it returns true only if the predicate is true on the first element and on the rest of them.

The first case (one element) is called the *base* cases: the function immediately returns the answer, there is no "rest of the vector" to look at. Let's sketch out this case in code, assuming that 'f' is the predicate and 'v' is the vector: `(if (empty? (rest v)) (f (first v)) ....` (we don't know yet what gets returned in the case when the predicate is false).

Note that the base case happens when there is only one element in the vector, and we check it by checking if the rest of the elements is empty.

The second case is the so-called *recursive step*: it combines the result for the first element with the result of the same computation on the rest of them. This is where we will be calling the function recursively to determine if the predicate holds for all elements in the rest of the vector. Since our function works on any sequence of elements (by design), it will work on the rest of the elements of `v`.

Although this seems a bit weird, let's write down what the recursive step looks like, almost literally translating the second case from English to code: '(and (f (first v)) (holds-for-all? f (rest v)))`. One thing that may be a bit tricky here is that we need to pass not only the rest of the vector, but also the predicate to the recursive function call. This is simply because our function *does* take two parameters and will give an error when called with just one.

When we combine the two cases above and add the necessary syntax, we get:

```clojure
(defn holds-for-all? [f v]
  (if (empty? (rest v)) (f (first v))
    (and (f (first v)) (holds-for-all? f (rest v)))))
```
Now let's write out step-by-step what happens when this function is called.
Suppose our predicate is `odd?` and the vector is `[1 3 4]`:
```clojure
(holds-for-all? odd? [1 3 4])
```
We will go through each recursive call, one at a time:

###### First call `(holds-for-all? odd? [1 3 4])`

The condition `(empty? (rest [1 3 4])` returns false, so the function will go to the recursive step, not the base case. It will be evaluating the expression
```clojure
(and (odd? (first [1 3 4])) (holds-for-all? odd? (rest [1 3 4])))
```
Note that the `and` cannot be evaluated until the recursive call returns, so it will be sitting in computer memory waiting for the result of the second call to `holds-for-all?`.

###### Second call `(holds-for-all? odd? [3 4])`

Now let's see what happens in the second call.
In order to determine the result of the `and` in the first call, we need to compute the result of the second part, which is
```clojure
(holds-for-all? odd? [3 4])
```
Once again, the rest of the vector is not empty, the function goes into the recursive step:
```clojure
(and (odd? (first [3 4])) (holds-for-all? odd? (rest [3 4])))
```
The first part of `and` is `(odd? (first [3 4]))` and returns true. The second part requires another recursive call:
```clojure
(holds-for-all? odd? [4])
```
Now we have a second call waiting for the result of the third call in order to find out what its `and` returns.

###### Third call `(holds-for-all? odd? [4])` and recursive returns

In the third call we check the condition `(empty? [4])`, and it's now true. This means that, according to the `if` statement, we just return the result of `(odd? (first [4])`. This result is `false`.

The third call to the function is now done and returns `false` to the second call that's waiting for it in order to compute its `and`. Its computation now becomes `(and true false)` which evaluates to `false`, the second function call is done, and returns to the first call in the recursive sequence which is still waiting to finish its computation of `and`. Once again, the computation is  `(and true false)` which produces `false`, and that's what gets returned from the entire sequence of calls, which is what we expected since the vector `[1 3 4]` does not have only odd elements.

Walking through a recursive function helps you understand how it works. However, you don't have to do it every time you write a recursive function: typically just breaking down the problem into a base case and a recursive step and constructing the results in both cases is enough.

###### Details

There are a few details that we have skipped over in order to emphasize the main ideas. Feel free to read about these details now, or come back to them later.

1. You may be wondering what happens if the first even element is not in the last position of the vector: will the function go all the way to the end, or start returning earlier? The answer is: it will return earlier because `and` is what's called *short-circuiting*: it evaluates left to right, and stops and returns as soon as it knows the answer. Thus `(and false <anything>)` returns `false` immediately. If the first element of a vector is even, the function will return without going into the recursive call since `and` already knows that the answer is `false`. In general, however, you need to be careful since many ways of using the result of a recursive call are not short-circuiting.
2. Our base case for the function is a one-element vector (its rest is empty). However, typically such functions are written with the base case being just an empty vector. You may be wondering what should be returned for an empty vector: do all its elements satisfy the condition? For instance, are they all odd? The answer is, yes. If there are no elements in a vector, all its elements satisfy any property whatsoever (they are odd, even, blue, tasty....) since there are no elements that fail the condition. On your own, try to rewrite the function with an empty vector base case.
3. We have also simplified one important thing: we keep referring to the argument of the function as a vector, but in fact any sequence of elements will be fine (a list will do, for instance). Moreover, taking the rest of a vector gives you a sequence of elements, but not a vector, so after the first call we will be passing a sequence, not a vector to all subsequent ones! This is not important for understanding how recursion works in this case, but will be useful to know for the future.
4. Finally, you may be wondering: isn't recursion very inefficient? We have a bunch of `and` expressions waiting for results of computations, doesn't it take memory and time to manage? The answer is, it may be inefficient if one isn't careful. However, most functional languages use a mechanism known as *tail recursion* (and a few other tricks) to implement recursion efficiently. We are not going into the details of this here.

### Higher-order functions

A higher-order function is one that either takes a function as input or returns a function as
output. In Clojure we can treat functions as values just like any other type. This is one
of the really powerful things of functional programming in general.

To explore this further we'll go over 3 built-in higher order functions that are integral
to a functional programmers toolbox.

#### Map

`map` is a function that takes a function as its first argument and a sequence as its second
and applies the function to each element building a new list from that.

```clojure
user=> (map square [1 2 3])
(1 4 9)
user=> (map square '(1 2 3))
(1 4 9)
```

So we passed square as a value, and either a list or a vector and got a new list built.

#### Filter

`filter` works like `map` in the sense that it also takes a function as its first argument
and a sequence as its second. The function passed to `filter` must return a boolean.
Like `map` we will walk through each element of the sequence and apply the function to it,
if the function we passed in evaluates to `true` on the element we will keep it, if it's `false`
we will drop the element from the sequence.

Lets use a built-in function `even?` which returns `true` if the number we pass it is even, otherwise
`false`.

```clojure
user=> (even? 3)
false
user=> (even? 2)
true
```

Now lets pass `even?` to `filter`.

```clojure
user=> (filter even? [1 2 3 4 5 6])
(2 4 6)
```

#### Reduce

`reduce` also takes two arguments, the first one is a function and the second is a sequence just
like `map` and `filter`. The function we pass in must take two arguments instead of one.

`reduce` applies the function to the first element of the sequence and the second. It then
applies the result of the first call and applies it to the third element of the sequence.
It then walks through the rest of the sequence.

A quick example is we can reduce `+` over a sequence of numbers to add them all together.

```clojure
user=> (reduce + [1 2 3 4])
10
```

### Exercise on higher-order functions

Write a function that computes the number of palindrome numbers in a given range. A number is a palindrome if its digits form the same number if reversed. For instance, `21512` is a palindrome. The function that you are writing should take two non-negative integer numbers `n` and `m` (such that `n <= m`) and return the number of palindromes in the range from `n` to `m` (including `n`, not including `m`). For instance, given 5 and 15, it should return 6 since there are 6 palindromes in this range: 5, 6, 7, 8, 9, 11.

Use higher-order functions (`map, filter, reduce`).

Some helpful functions:

`str` converts numbers into strings. See [its documentation](https://clojuredocs.org/clojure.core/str) for more details.

`range` produces a sequence of all integers in a given range. Once again, consult [its documentation](https://clojuredocs.org/clojure.core/range) for details.

Note that your function just need to return the number of palindromes, not the palindromes themselves.
