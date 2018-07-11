---
layout: post
title: Python, First Impressions
date: 2017-08-11 19:39:12 -0400
comments: true
categories: python
---

#### Debugging
- Write the following code to use an interactive debugger in Python.

```python
import pdb
pdb.set_trace()
```
#### Self
- Passing a reference to `self` seems repetitive.

```python
class Foo:
  def bar(self)
    print('Hello')
```

#### Testing
- You need to pass in the -s to `print` test output. I expected `print` to log output regardless.

#### Modules
- You can conditionally implement different behavior when importing modules in Python.
- Taken from [this](https://stackoverflow.com/questions/419163/what-does-if-name-main-do) StackOverflow example.

```python one.py
def func():
    print("func() in one.py")

print("top-level in one.py")

if __name__ == "__main__":
    print("one.py is being run directly")
else:
    print("one.py is being imported into another module")
```

```python two.py
import one

print("top-level in two.py")
one.func()

if __name__ == "__main__":
    print("two.py is being run directly")
else:
    print("two.py is being imported into another module")
```

Running `python one.py` returns:
`
top-level in one.py
one.py is being run directly
`

While running `python two.py` returns:
`
top-level in one.py
one.py is being imported into another module
top-level in two.py
func() in one.py
two.py is being run directly
`

