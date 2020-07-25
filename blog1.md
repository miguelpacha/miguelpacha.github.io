layout: page
title: "Approaching functional programming"
permalink: /functional-1/

Approaching functional programming: solving Project Euler's Problem 1
===

This post aims to introduce some functional programming concepts by solving an easy problem using Python and Haskell.

Here is the statement of the problem
>If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.
>Find the sum of all the multiples of 3 or 5 below 1000.

Please try solving the problem on your own before reading the rest of this post.

It's possible to solve this without any programming, by summing all the multiples of three and all the multiples of five, and subtracting all the multiples of 15 (which would otherwise be counted twice). All the necessary math can be found here: https://en.wikipedia.org/wiki/Arithmetic_progression

Solving it in an imperative style is straightforward, for example using Python:
```python
total = 0
for n in range(1,1000):
    if n%3 == 0 or n%5 == 0:
        total += n
print(total)
```

Although the code above is perfectly fine, using functional programming idioms can yield shorter and simpler code that is easier to debug. Let's take baby steps.

We can begin by defining a lambda function that tests if a given number is a multiple of 3 or 5, returning a boolean. Then, we define a variable containing all the numbers from 1 to 1000 (not inclusive) using `range`. Now, we can use Python's `filter` function to pick only the numbers that pass the test we just defined. Finally, we use the `sum` function to add them all up:
```python
test = lambda n: n%3 == 0 or n%5 == 0
all_numbers = range(1,1000)
multiples = filter(test, all_numbers)
total = sum(multiples)
print(total)
```
Note: if you try to print the variables `all_numbers` or `multiples` you'll get the following result:
```python
print(all_numbers) # prints "range(1, 1000)"
print(multiples) # prints "<filter object at 0x32c4eb530>"
```
This does not print the actual numbers because these variables are generators – they are lazy, so if we want to force the computer to print the numbers, we must put them into lists, like this:
```python
print(list(all_numbers)) # prints "[1, 2, 3 ...]"
print(list(multiples)) # prints "[3, 5, 6, 9, 10 ...]"
```
Back to our problem, we can rewrite the functional solution in a terser style, doing away with the intermediate variables:
```python
print(sum(filter(lambda n: n%3==0 or n%5==0, range(1,1000))))
```
Although Python has some functional features, it is still primarily an imperative language. A purely functional language Haskell promises to offer prettier, more readable syntax (once you get used to it). Let us try and port the code above to Haskell:
```haskell
total = sum [ n | n<-[1..999], mod n 5 == 0 || mod n 3 == 0 ]
```
The code above sums all the x coming from the range [1..999] that satisfy the condition mod x 5 == 0 || mod x 3 == 0. Note the different syntax for the `mod` function, the lack of parentheses after `sum` and that the range is inclusive.

To sum up, Haskell can yield some nicer-looking code, but it may be hard to write at first.
