---
title: Walrus in Python :=
date: "2022-01-23T05:07:05Z"
description: Simplify the pattern of fetch a data, compare it to something else, and then use it
---

I was reading Effective Python by Brett Slatkin and saw this Pythonic way of thinking which I found very useful. 

A pretty common logic in programming is to fetch data, check for a basic condition and execute certain tasks based on the condition.

For instance, if I want to make a lemonade, given the number of fruits,
```python
# python

fresh_fruit = {
    'apple': 10,
    'banana': 5,
    'lemon' : 5,
}

def make_lemonade(lemon):
    # add lemonade making logic here
    ...

def out_of_stock():
    # add out of stock logic here
    ...

# fetch data
count = fresh_fruit.get('lemon', 0)

#check for a condition
if count:
    make_lemonade(count)
else:
    out_of_stock()

```

While this logic is simple and straightforward, it is a bit noisy and can be reduced by assigning the count value at the condition check time. This would shave off a line and make the code look succinct. 

This feature, however, is available for Python 3.8 and newer.

```python
# rewritten logic above with Walrus operator on Python 3.8+

fresh_fruit = {
    'apple': 10,
    'banana': 5,
    'lemon' : 5,
}

def make_lemonade(lemon):
    # add lemonade making logic here
    ...

def out_of_stock():
    # add out of stock logic here
    ...

# see Walrus here :=
if count := fresh_fruit.get('lemon',0):
    make_lemonade(count)
else:
    out_of_stock()

```
