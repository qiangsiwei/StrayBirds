---
layout: post
title: Python
category: Coding Language
comments: true
---

# Python (Standard Libaray)

------

Python’s standard library is very extensive, offering a wide range of facilities as indicated by the long table of contents listed below.

## Built-in Functions

![此处输入图片的描述][1]

basestring()   
This abstract type is the superclass for str and unicode. It cannot be called or instantiated, but it can be used to test whether an object is an instance of str or unicode. isinstance(obj, basestring) is equivalent to isinstance(obj, (str, unicode)).

chr(i)   
Return a string of one character whose ASCII code is the integer i.

ord(c)   
Given a string of length one, return an integer representing the Unicode code point of the character when the argument is a unicode object, or the value of the byte when the argument is an 8-bit string. 

cmp(x, y)   
Compare the two objects x and y and return an integer according to the outcome. The return value is negative if x < y, zero if x == y and strictly positive if x > y.

compile(source, filename, mode[, flags[, dont_inherit]])   
Compile the source into a code or AST object. Code objects can be executed by an exec statement or evaluated by a call to eval().

dir([object])   
Without arguments, return the list of names in the current local scope. With an argument, attempt to return a list of valid attributes for that object.

```
>>> import struct
>>> dir()   # show the names in the module namespace
['__builtins__', '__doc__', '__name__', 'struct']
>>> dir(struct)   # show the names in the struct module
['Struct', '__builtins__', '__doc__', '__file__', '__name__',
 '__package__', '_clearcache', 'calcsize', 'error', 'pack', 'pack_into',
 'unpack', 'unpack_from']
>>> class Shape(object):
        def __dir__(self):
            return ['area', 'perimeter', 'location']
>>> s = Shape()
>>> dir(s)
['area', 'perimeter', 'location']
```

eval(expression[, globals[, locals]])   
The arguments are a Unicode or Latin-1 encoded string and optional globals and locals. If provided, globals must be a dictionary. If provided, locals can be any mapping object.

```
>>> x = 1
>>> print eval('x+1')
2
```

execfile(filename[, globals[, locals]])   
This function is similar to the exec statement, but parses a file instead of a string. It is different from the import statement in that it does not use the module administration — it reads the file unconditionally and does not create a new module. [1]

globals()   
Return a dictionary representing the current global symbol table. This is always the dictionary of the current module (inside a function or method, this is the module where it is defined, not the module from which it is called).

locals()   
Update and return a dictionary representing the current local symbol table. Free variables are returned by locals() when it is called in function blocks, but not in class blocks.

filter(function, iterable)   
Construct a list from those elements of iterable for which function returns true.

map(function, iterable, ...)   
Apply function to every item of iterable and return a list of the results.

reduce(function, iterable[, initializer])   
Apply function of two arguments cumulatively to the items of iterable, from left to right, so as to reduce the iterable to a single value.

classmethod(function)   
Return a class method for function.

```
class C(object):
    @classmethod
    def f(cls, arg1, arg2, ...):
        ...
```

staticmethod(function)   
Return a static method for function.

```
class C(object):
    @staticmethod
    def f(arg1, arg2, ...):
        ...
```

class property([fget[, fset[, fdel[, doc]]]])   
Return a property attribute for new-style classes (classes that derive from object).

```
class Parrot(object):
    def __init__(self):
        self._voltage = 100000

    @property
    def voltage(self):
        """Get the current voltage."""
        return self._voltage
```

super(type[, object-or-type])   
Return a proxy object that delegates method calls to a parent or sibling class of type. This is useful for accessing inherited methods that have been overridden in a class. The search order is same as that used by getattr() except that the type itself is skipped.

```
class C(B):
    def method(self, arg):
        super(C, self).method(arg)
```

reload(module)   
Reload a previously imported module. The argument must be a module object, so it must have been successfully imported before.

sorted(iterable[, cmp[, key[, reverse]]])   
Return a new sorted list from the items in iterable.

zip([iterable, ...])   

```
>>> x = [1, 2, 3]
>>> y = [4, 5, 6]
>>> zipped = zip(x, y)
>>> zipped
[(1, 4), (2, 5), (3, 6)]
>>> x2, y2 = zip(*zipped)
>>> x == list(x2) and y == list(y2)
True
```

## Built-in Types

### Sequence Types — str, unicode, list, tuple, bytearray, buffer, xrange

String: String Formatting Operations

![此处输入图片的描述][2]

![此处输入图片的描述][3]

Mutable Sequence Types

![此处输入图片的描述][4]


### Set Types — set, frozenset

There are currently two built-in set types, set and frozenset. The set type is mutable — the contents can be changed using methods like add() and remove(). Since it is mutable, it has no hash value and cannot be used as either a dictionary key or as an element of another set. The frozenset type is immutable and hashable — its contents cannot be altered after it is created; it can therefore be used as a dictionary key or as an element of another set.

### Built-in Exceptions

The class hierarchy for built-in exceptions is:






***相关连接***

 - https://docs.python.org/2.7/library/index.html

 [1]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2015-01-01-python_documents/2015-01-01-python_documents_1.png
 [2]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2015-01-01-python_documents/2015-01-01-python_documents_2.png
 [3]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2015-01-01-python_documents/2015-01-01-python_documents_3.png
 [4]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2015-01-01-python_documents/2015-01-01-python_documents_4.png
 [5]: https://raw.githubusercontent.com/qiangsiwei/blog/gh-pages/_figures/2015-01-01-python_documents/2015-01-01-python_documents_5.png

