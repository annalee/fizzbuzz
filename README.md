# What's FizzBuzz?

FizzBuzz is a popular brain-teaser for programmers. It's sometimes used as a coding exercise during technical interviews.

It goes like this:

```
Write a program that prints the numbers 1-100.
If the number is divisible by 3, print "fizz" instead of the number.
If the number is divisible by 5, print "buzz" instead of the number.
If the number is divisible by both 3 and 5, print "fizzbuzz" instead of the number.
```

There are a ton of ways to solve fizzbuzz, ranging from the straightforward to the gordian.

# FizzBuzz in Python

So how can you solve FizzBuzz in Python3? Like this:

```python
for x in ["fizz"*(x%3==0)+"buzz"*(x%5==0)+str(x)*(x%3 > 0 and x%5 > 0) for x in range(1,101)]: print(x)
```

## Wait, What?

Kinda looks like your python interpreter threw up on itself, right?

Yeah. Don't solve FizzBuzz like that.

In fact, don't solve _anything_ like that.

That answer is technically correct--it does solve fizzbuzz. But let this be a lesson to ye junior devs: just because you _can_ cram a bunch of logic into one line doesn't mean you _should_. You should write code that's easy for you and others to read and follow.

_But_ this solution does show off a few neat things about python, so let's dig into what it's doing and unpack it into something a little more readable.

I'm starting with basic concepts for new devs here. If you already know how modulo and comparisons work, you can skip ahead to step 4.

## 1. Modulo

The modulo operator (`%`) gives you the remainder of one number divided by another. For example:

```
>>> 5 % 3
2
>>> 
```

When the first number can be evenly divided by the second, the modulo operator will return zero:

```
>>> 9 % 3
0
>>> 
```

In Fizzbuzzenstein's monster up there, the modulo operator appears four times: twice to check the remainder of a number divided by 3, and twice to check the remainder if divided by 5.

## 2. Comparison Operations

We're using two _comparison operations_ to test if our remainders are equal to (`==`) or greater than (`>`) zero. Comparison operations always evaluate to the boolean values `True` or `False`.

For instance:

```
>>> 1 < 2
True
>>> 
```

```
>>> 1 == 2
False
>>> 
```

If `x % 3 == 0` returns `True`, then x is divisible by 3, and we'll need to print "fizz." If `x % 5 == 0` returns `True`, then x is divisible by 5, and we need to print "buzz."

If both remainders are greater than 0, then x is not divisible by either 3 or 5, and we need to print the number itself. We're checking this last condition with a compound statement: `x%3 > 0 and x%5 > 0`. The `and` operator has a lower priority in python than the `>` operator, so this is equivalent to `(x%3 > 0) and (x%5 > 0)`. Or, in plain English, `The remainder of x divided by three is greater than zero, and the remainder of x divided by 5 is greater than zero.` This compound statement will only evaluate `True` if both of its clauses are `True`. If x is divisible by either 3 or 5, it'll return `False.`

These are the tools you'll need for any straightforward solution to FizzBuzz. There are ways to solve it without using modulo and comparing the remainder, but if anyone ever asks you to, they're evaluating your skill at math, not programming.

## 3. String Concatenation

In python, you add strings to other strings like this:

```
>>> "fizz" + "buzz"
'fizzbuzz'
>>>
```

You can also add strings to empty strings:

```
>>> "" + "buzz"
'buzz'
>>> 
```

We're using concatenation to construct the string we need to print for each number.

## 4. String Multiplication

You can also use multiplication on a string to repeat it. For example:

```
>>> print("The cake is a lie! "*3)
The cake is a lie! The cake is a lie! The cake is a lie! 
>>> 
```

This will work with any integer. And yes, I know what you're thinking: _But what about_ negative _integers, Ms. Smartypants?_

Well, what about them?

```
>>> print("The cake is a lie! "*-3)

>>> 
```

You can't print something 'negative' times, so it returns an empty string. Same thing happens if you try to multiply by 0: it'll print it 0 times.

```
>>> print("The cake is a lie! "*0)

>>>
```

Neat, right? Hang onto that thought for a sec. We'll come back to it.

## 5. Booleans as Integers

Up in Step 2, we used comparison operators to test if a number was divisible by 3 and/or 5. We also established that comparisons always return boolean values: `True` or `False`.

This is the secret sauce behind our solution to FizzBuzz--the lightning that animates Fizzbuzzenstein's monster: In python, _booleans are a type of integer_.

From [the Python 3 docs](https://docs.python.org/3/reference/datamodel.html#the-standard-type-hierarchy):

>These represent the truth values `False` and `True`. The two objects representing the values `False` and `True` are the only Boolean objects. The Boolean type is a subtype of the integer type, and Boolean values behave like the values 0 and 1, respectively, in almost all contexts, the exception being that when converted to a string, the strings "False" or "True" are returned, respectively.

So, Boolean values can be used as numbers, and numbers can be used to multiply strings.

With me so far? Good. Let's take a look at Fizzbuzzenstein's Monster again. Specifically, this part:

```python
"fizz"*(x%3==0)+"buzz"*(x%5==0)+str(x)*(x%3 > 0 and x%5 > 0)
```

What we're doing here is assembling a string using concatenation and multiplication.

`x%3==0`, `x%5==0`, and `x%3 > 0 and x%5 > 0` are all comparisons, and will all evaluate as either `True` or `False`. `True` and `False`, in python, are 1 and 0, respectively.

So `"fizz"*(x%3==0)` will write "fizz" 1 time if x is divisible by 3, and 0 times if it is not. `"buzz"*(x%5==0)` will write "buzz" one time if x is divisible by 5, and 0 times if it is not. `str(x)*(x%3 > 0 and x%5 > 0)` will print x (converted to a string) 0 times if x is divisible by 3 and/or 5, and 1 time if x is divisible by neither.

These strings are concatenated together to create one string, which will contain either "fizz," "buzz," "fizzbuzz," or the value of x.

This string-building engine is the powerhouse behind Fizzbuzzenstein's monster. For the sake of readability, let's pull it out into its own function, and separate it out into its constituent steps:

```python
def stringbuilder(x):
    fizz = "fizz"*(x%3==0)
    buzz = "buzz"*(x%5==0)
    num  = str(x)*(x%3 > 0 and x%5 > 0)
    fizzbuzz = fizz + buzz + num
    return fizzbuzz
```
If we plug that function into our fizzbuzz solution, it looks like this:
```python
for x in [stringbuilder(x) for x in range(1,101)]: print(x)
```
That's a lot less messy. But what's this stuff in the brackets doing?

## 6. List Comprehensions

List Comprehensions are my favorite part of python--they're a way to operate on items in a list in a compact and efficient way.

You can read all about them in [the Python 3 docs](https://docs.python.org/3/howto/functional.html?#generator-expressions-and-list-comprehensions), but for now, we just need to understand their general format so we can unpack this list comprehension into something that's easier to read.

The most basic list comprehension looks like this: `[x for x in [5,6,7,8]]`. That says "make a list that contains each item in the list `[5,6,7,8]`." Now, that's not very useful, because you've already got all those items in a list. But list comprehensions let you operate on the items in a list--either by changing them before adding them to a new list, or checking if they meet a condition before adding them to a new list.

We can subtract two from each item in the list:
```
>>> [x - 2 for x in [5,6,7,8]]
[3, 4, 5, 6]
>>> 
```
Or we can only grab items of the list that are divisible by 2:
```
>>> [x for x in [5,6,7,8] if x %2 ==0]
[6, 8]
>>> 
```
What we're doing in our fizzbuzz solution, however, is using the numbers in the list to change _something else_. Namely, it's changing the string we'll need to print for that number.

Here is our list comprehension, using our stringbuilder: `[stringbuilder(x) for x in range(1,101)]`

It's taking each number from the list and plugging it into our string-builder as x. Then it's storing the string-builder's string for that number in a new list.

## 7. Breaking it all out (Whitespace is our friend):

List comprehensions are slick and powerful, but they're not as readable as normal `for loop`s. Here's what our list comprehension looks like as a normal loop that breaks all the steps out on their own lines:

```python
fizzbuzzlist = []
for x in range(1,101):
    fizzbuzzstring = stringbuilder(x)
    fizzbuzzlist.append(fizzbuzzstring)
```    
Now we have our new list, `fizzbuzzlist`, that contains everything we need to print to complete the fizzbuzz exercise.

The outermost layer of our fizzbuzz solution is another `for loop` that prints everything in that list:

```python
for x in [fizzbuzzlist]:
    print(x)
```
Fizzbuzzenstein's monster slammed that all together on one line, but loops are easier to read when broken out.

Now, we're only using `fizzbuzzlist` to store things we're going to print. We can save ourselves a step by just printing each string directly, instead of storing it:

```python
for x in range(1,101):
    print(stringbuilder(x))
```

Now, our full solution looks like this:

```python
def stringbuilder(x):
    fizz = "fizz"*(x%3==0)
    buzz = "buzz"*(x%5==0)
    num  = str(x)*(x%3 > 0 and x%5 > 0)
    fizzbuzz = fizz + buzz + num
    return fizzbuzz
    
for x in range(1,101):
    print(stringbuilder(x))
```

Wrapping up chunks of code into functions is a good idea if you're going to reuse the same code in several places, but in this case, using a function is actually making the script longer, and interrupting the clear chronological flow. We can get the same solution without the function by putting our string-building engine back in the for loop:

```python
for x in range(1,101):
    fizz = "fizz" * (x%3==0)
    buzz = "buzz" * (x%5==0)
    num  = str(x) * (x%3 > 0 and x%5 > 0)
    
    print(fizz + buzz + num)
```

And here we have an elegant, readable solution to fizzbuzz in python3.
