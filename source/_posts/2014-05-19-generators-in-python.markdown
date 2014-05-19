---
layout: post
title: "Generators in Python"
date: 2014-05-19 11:01:58 +0530
comments: true
author: Yogeswaran
categories: python
---

One of the few obscure feature of python (for the beginners) is **Generators**.
In this post I would like to share few naive questions I had about generators
and the answers I got after understanding them.

>**Question 1:** Are generators something like static variables in C?  Say, generateFibonacciNumber() is a generator.   First time I call generateFibonacciNumber() and iterate upto value 5, the next time I call  generateFibonacciNumber() will it start returning from value 8 when iterated?

This could sound like most dumb question on earth, but honestly I had this
question having come from C background.

**Answer:**  No. Generators should not be confused with static variables in C.
Every time a generator is called it will return a generator object and each has
their own state variables.  So iterating one generator will not affect the other.

>**Question 2:** Creating multiple generators holding their reference and not iterating through them could potentially lead to memory exhaustion, correct?

Phew.  Yet another dumb question.

**Answer:** Obviously if you are going to hold the reference it is going to eat memory.
But, that is not the problem of generators.  Let's take for example, to process
a million lines of code we use a _generator_ and a _normal function_.  At any point
of time single generator object will hold only state variables and not the memory
needed to hold all million lines.  But holding the value returned by a normal
function is like holding million lines in the memory.  Compare holding multiple
instances of state variables and multiple instance of million lines - the answer is obvious.

### So, what is actually is a Generator?

To understand generator we'll have to understand what is __iterator__ in python.

In simple terms, __iterator__ is a object having two methods `__iter__()` and `next()`.
When iterators are using along with looping constructs like `for`, the `__iter__` and
`next` methods are called implicitly.

You can find more information on iterators [here](https://docs.python.org/2/library/stdtypes.html#iterator-types).
>\_\_iter__() : Returing itself

>next()  : Returning next item, or __StopIteration__ exception on no further items.

```python
class Iterator():
  def __init__(self):
    self.i = 0

  def __iter__(self):
    return self

  def next(self):
    if self.i < 10:
      self.i += 1
      return self.i
    else:
      raise StopIteration()

def main():
  iterator = Iterator()
  print type(iterator)
  for i in iterator:
    print i,

if __name__ == '__main__':
  main()
```

Executing this code will give us output as shown below,

```
<type 'instance'>
1 2 3 4 5 6 7 8 9 10
```

**Generator** is a special iterator.  We will not have to write a class with
these methods, instead `yield` keyword can do all the **magic** for us.

Now, let us rewrite the above code with generator.

```python
def generator():
  i = 0
  while i < 10:
    i += 1
    yield i

def main():
  gen = generator()
  print type(gen)
  print dir(gen)
  for i in gen:
    print i,

if __name__ == '__main__':
  main()
```

Executing this code will give us output as shown below,

```
<type 'generator'>
['__class__', '__delattr__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__iter__', '__name__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'close', 'gi_code', 'gi_frame', 'gi_running', 'next', 'send', 'throw']
1 2 3 4 5 6 7 8 9 10
```

> Is that all?  Is generator just another method to create iterator?

>But according to the zen of python,

>_"There should be one-- and preferably only one --obvious way to do it."_

Hey look, `dir(generator)` is giving something more than what we would expect from a `dir(function)`.

### Generator is more than just an iterator

Let us now dwell deep into generator, for which we'll have to understand `yield` keyword.

`yield` as just the literal meaning - relinquishes the control temporarily.

Whenever a generator function is called, the actual code
inside the function does not get executed.

For example, the same code without iterating through items of generator.

```python
def generator():
  print 'First line of generator'
  i = 0
  while i < 10:
    i += 1
    yield i

def main():
  print 'Before calling generator'
  gen = generator()
  print 'After calling generator'

if __name__ == '__main__':
  main()
```

Output of this code will look like,

```
Before calling generator
After calling generator
```

You can notice something.  _"First line of generator"_ is
not printed when generator is invoked.

>This is how, generator is different from other functions.  Calling a generator
>function does not execute any code in the function, instead returns a generator
>object.

>So, when does actual code gets executed?
>
>__The actual code gets executed when the next() method is called.__

```python
def generator():
  print 'In generator() first line'
  i = 0
  while i < 10:
    i += 1
    print 'In generator() before yield'
    yield i
    print 'In generator() after yield'

def main():
  print 'In main() before calling generator()'
  gen = generator()
  print 'In main() after calling generator()'
  print 'In main() before calling next()'
  gen.next()
  print 'In main() after calling next()'

if __name__ == '__main__':
  main()
```

Output of this code will look like this,

```
In main() before calling generator()
In main() after calling generator()
In main() before calling next()
In generator() first line
In generator() before yield
In main() after calling next()
```

>It is clear from the example above, that the code inside generator
>actually gets executed when next() method is called.

>One more thing to be noticed here is, the last line of generator
>__'In generator() after yield'__ is not getting printed.

>The execution resumes from this point when next() method of the generator
> object is called the next time.

The code snippet below explains this control flow.

```python
def generator():
  print 'In generator() first line'
  i = 0
  while i < 10:
    i += 1
    print 'In generator() before yield'
    yield i
    print 'In generator() after yield'

def main():
  print 'In main() before calling generator()'
  gen = generator()
  print 'In main() after calling generator()'
  print 'In main() before calling next()'
  gen.next()
  print 'In main() after calling next()'
  print 'In main() before calling next() second time'
  gen.next()
  print 'In main() after calling next() second time'

if __name__ == '__main__':
  main()
```

Output of this code will be,

```
In main() before calling generator()
In main() after calling generator()
In main() before calling next()
In generator() first line
In generator() before yield
In main() after calling next()
In main() before calling next() second time
In generator() after yield
In generator() before yield
In main() after calling next() second time
```

### So, when to use generator and when to use iterable?

Any iterable can be replaced with a generator but converse is not true.

Generators are preferred for two reasons,

1. When dealing with large sequence
2. When the end point of the sequence is not known beforehand

Thats all about generators in Python.  Happy hacking.

