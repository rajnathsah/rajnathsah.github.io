---
layout: post
title: Q&A for Python interview
date: 2020-01-02 00:00:00 +0000
description: Important questions for Python interview.
img: python.png # Add image post (optional)
tags: [Python]
---
Important topics in Q&A format is based on my experience. Feel free to update me in case of any observation or you want to add anything.

##### 1. How to debug pip installation error?
For debugging purpose, pip displays some installation message on console. It also offers ways to control console level log by -v,--verbose, -q and --quiet. Full log can be saved by providing --log option with file path, it will have complete installation log. There are cases where currupt download also creates issues at the time of installation, in that case try --no-cache-dir. If that also does not work then you can always try the installation from source file.

##### 2. Difference between list and dictionary?
###### List
* List in Python is a heterogeneous container for items.
* Elements present in List maintain their order.
* The elements present in list can be of any type (int, float, string, tuple etc.).
* Elements are accessed through their index values.
* If you have a collection of data that does not need random access, use List.
* Where you have to deal with values which can be changed, use List.
```python
mylist=[1,2,3,'4','sample']
```
###### Dictionary
* Dictionary is an unordered collection of key-value pairs.
* Dictionaries are used to handle large amount of data.
* Every element is having a key-value pair.
* Elements are accessed by using it’s key value.
* When you are dealing with unique keys and you are mapping values to the keys, use Dictionary.
```python
mydict = {1:'Python'}
```

##### 3. How to read 10 characters from a file?
```python
f = open('<file name>','r')
print(f.read(10))
```

##### 4. What are membership operators in Python? Write an example to explain both.
There are 2 types of membership operators in Python:  
in: If the value is found in a sequence, then the result becomes true else false  
not in: If the value is not found in a sequence, then the result becomes true else false  
```python
a=15
b=30
list = [3,6,15,20,25]

if (a in list):
	print('a is available in given list')
else:
	print('a is not available in given list')

 
if ( b not in list ):
	print('b is not available in given list')
else:
	print('b is available in given list')
```

##### 5. Command to get all keys from the dictionary.
```python
mydict = {1:'Raj',2:'Som'}
print(mydict.keys())
```

##### 6. What is monkey patching?
Monkey patching means adding a new variable or method to a class or module after it's been defined.  
Example: We define a class A as
```python
class A(object):
    def __init__(self, num):
        self.num = num

    def __add__(self, other):
        return A(self.num + other.num)
```
But now we want to add another function later in the code like below.
```python
def get_num(self):
    return self.num	
```
To do this as method of class A at runtime, we assign the function to class like below.  
```python
A.get_num = get_num
```
At runtime, assign the new method and use new methos like below.
```python
foo = A(42)
A.get_num = get_num
bar = A(6);
foo.get_num() # 42
bar.get_num() # 6
```

##### 7. What are \**args and \***kwargs?
The special syntax, *args and \**kwargs in function definitions is used to pass a variable number of arguments to a function. The single asterisk form (*args) is used to pass a non-keyworded, variable-length argument list, and the double asterisk form is used to pass a keyworded, variable-length argument list.  
*args example :  
```python
def test_var_args(farg, *args):
    print('formal arg:{}'.format(farg))
    for arg in args:
        print('another arg:{}'.format(arg)

test_var_args(1, "two", 3)
```
Output
```python
formal arg: 1
another arg: two
another arg: 3
```
\**kwargs example
```python
def test_var_kwargs(farg, **kwargs):
    print('formal arg:{}'.format(farg))
    for key in kwargs:
        print('another keyword arg: {}: {}'.format(key, kwargs[key]))

test_var_kwargs(farg=1, myarg2="two", myarg3=3)
```
```python
formal arg: 1
another keyword arg: myarg2: two
another keyword arg: myarg3: 3
```

##### 8. What is Iterables in python?
When you create a list, you can read its items one by one. Reading its items one by one is called iteration:  
```python
mylist = [1, 2, 3]
for i in mylist:
	print(i)
```
Output
```python
1
2
3
```
mylist is an iterable. When you use a list comprehension, you create a list, and so an iterable:  
```python
mylist = [x*x for x in range(3)]
for i in mylist:
	print(i)
```
Output
```python
0
1
4
```
Everything you can use "for... in..." on is an iterable; lists, strings, files...  
These iterables are handy because you can read them as much as you wish, but you store all the values in memory and this is not always what you want when you have a lot of values.  

##### 9. What is Generators in python?
Generators are iterators, a kind of iterable **you can only iterate over once**. Generators do not store all the values in memory, **they generate the values on the fly**:  
```python
mygenerator = (x*x for x in range(3))
for i in mygenerator:
	print(i)
```
Output
```python
0
1
4
```
It is just the same except you used () instead of []. BUT, you cannot perform for i in mygenerator a second time since generators can only be used once: they calculate 0, then forget about it and calculate 1, and end calculating 4, one by one.  

##### 10. What does the "yield" keyword do?
yield is a keyword that is used like return, except the function will return a generator.  
Example:  
```python
def createGenerator():
  mylist = range(3)
  for i in mylist:
    yield i*i

# create a generator
mygenerator = createGenerator()
# print generator object
print(mygenerator)
# print value
for i in mygenerator:
  print(i)
```
Output
```python
0
1
4
```

This is a simple example, but it's handy when you know your function will return a huge set of values that you will only need to read once. To master yield, you must understand that when you call the function, the code you have written in the function body does not run. The function only returns the generator object. Then, your code will continue from where it left off each time for uses the generator.

The first time the for calls the generator object created from your function, it will run the code in your function from the beginning until it hits yield, then it'll return the first value of the loop. Then, each other call will run the loop you have written in the function one more time, and return the next value until there is no value to return.

The generator is considered empty once the function runs, but does not hit yield anymore. It can be because the loop had come to an end, or because you do not satisfy an "if/else" anymore.  

##### 11. What is class in Python?
Like any other language classes in Python are also a piece of code that descibe how to produce an object. That is true for python as well.
```python
class ObjectCreator(object):
  pass

my_object = ObjectCreator()
print(my_object)
```
Output
```python
<__main__.ObjectCreator object at 0x7f3d86c186d0>
```
But classes are more than that in Python. Classes are objects too.
As soon as you use the keyword class, Python executes it and creates an OBJECT. The instruction
```python
class ObjectCreator(object):
  pass
```
creates in memory an object with the name "ObjectCreator".  
**This object (the class) is itself capable of creating objects (the instances), and this is why it's a class.**
But still, it's an object, and therefore:  
* you can assign it to a variable
* you can copy it
* you can add attributes to it
* you can pass it as a function parameter  
Example:  
```python
class ObjectCreator(object):
  pass

# you can print a class because it's an object
print(ObjectCreator) 

def echo(o):
  print(o)

# you can pass a class as a parameter
echo(ObjectCreator) 

print(hasattr(ObjectCreator, 'new_attribute'))

# you can add attributes to a class
ObjectCreator.new_attribute = 'foo'

print(hasattr(ObjectCreator, 'new_attribute'))

print(ObjectCreator.new_attribute)

# you can assign a class to a variable
ObjectCreatorMirror = ObjectCreator 
print(ObjectCreatorMirror.new_attribute)

print(ObjectCreatorMirror())
```
Output
```python
<class '__main__.ObjectCreator'>
<class '__main__.ObjectCreator'>
False
True
foo
foo
<__main__.ObjectCreator object at 0x7f455006b810>
```
Since classes are objects, you can create them on the fly, like any object. But it's not so dynamic, since you still have to write the whole class yourself. For more details, Please refer [link](https://stackoverflow.com/questions/100003/what-are-metaclasses-in-python/6581949#6581949).  

##### 12. What are metaclasses in Python?
Metaclasses are the 'stuff' that creates classes, i.e. metaclasses are what create these objects. They are the classes' classes.
```python
MyClass = MetaClass()
my_object = MyClass()
```
type is the metaclass Python uses to create all classes behind the scenes.  
Everything is an object in Python. That includes ints, strings, functions and classes. All of them are objects. And all of them have been created from a class:  
```python
>>> age = 35
>>> age.__class__
<type 'int'>
>>> name = 'bob'
>>> name.__class__
<type 'str'>
>>> def foo(): pass
>>> foo.__class__
<type 'function'>
>>> class Bar(object): pass
>>> b = Bar()
>>> b.__class__
<class '__main__.Bar'>
```
Now, what is the __class__ of any __class__ ?
```python
>>> age.__class__.__class__
<type 'type'>
>>> name.__class__.__class__
<type 'type'>
>>> foo.__class__.__class__
<type 'type'>
>>> b.__class__.__class__
<type 'type'>
```
type is the built-in metaclass Python uses, but there is a way to create own metaclasses by defining metaclass attribute.  
Python 2 syntax for creating metaclass:
```python
class Foo(object):
    __metaclass__ = something...
    [...]
```
Python 3 syntax for creating metaclass:  
```python
class Foo(object, metaclass=something):
    ...    
```
You write class Foo(object) first, but the class object Foo is not created in memory yet.  
Python will look for **metaclass** in the class definition. If it finds it, it will use it to create the object class Foo. If it doesn't, it will use type to create the class.  
For detail explaination please refer stackoverflow [link](https://stackoverflow.com/questions/100003/what-are-metaclasses-in-python/6581949#6581949).

##### 13. How to merge two dictionaries in single expression?
In Python 3.5 and above to mearge two dictionaries x and y below syntax is used.
```python
z = {**x, **y}
```
In Python 2, (or 3.4 or lower) write a function:
```python
def merge_two_dicts(x, y):
    z = x.copy()   # start with x's keys and values
    z.update(y)    # modifies z with y's keys and values & returns None
    return z
    
z = merge_two_dicts(x, y)   
```
For dictionaries x and y, z becomes a shallowly merged dictionary with values from y replacing those from x.

##### 14. How to print without new line?
```python
print('.', end='')
```

##### 15. What is Decorators in Python?
Decorators are functions which modify the functionality of other functions. In another word decorators wrap a function, modifying its behavior. Let us start with simple example:  
```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

def say_whee():
    print("Whee!")

say_whee = my_decorator(say_whee)

say_whee()
```
Output
```python
Something is happening before the function is called.
Whee!
Something is happening after the function is called.
```
In the above code, decoration happened at line. 
```python
say_whee = my_decorator(say_whee)
```
The way you decorated say_whee() above is a little clunky.Python allows you to use decorators in a simpler way with the @ symbol.  
Same code can be re-written.
```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

@my_decorator
def say_whee():
    print("Whee!")
```
Output
```python
Something is happening before the function is called.
Whee!
Something is happening after the function is called.
```
For more details, Please refer the [link](https://realpython.com/primer-on-python-decorators/#simple-decorators).

##### 16. What does if __name__ == "__main__": do in Python?
Whenever the Python interpreter reads a source file, it does two things:  
* it sets a few special variables like __name__, and then
* it executes all of the code found in the file.
For detailed explaination, please refer [link](https://stackoverflow.com/questions/419163/what-does-if-name-main-do/419185#419185)

##### 17. What is the meaning of a single and a double underscore before an object name?
* Single Underscore  
Names, in a class, with a leading underscore are simply to indicate to other programmers that the attribute or method is intended to be private.  
* Double Underscore  
Any identifier of the form \__spam (at least two leading underscores, at most one trailing underscore) is textually replaced with _classname__spam, where classname is the current class name with leading underscore(s) stripped. This mangling is done without regard to the syntactic position of the identifier, so it can be used to define class-private instance and class variables, methods, variables stored in globals, and even variables stored in instances. private to this class on instances of other classes.  
Example:
```python
>>> class MyClass():
...     def __init__(self):
...             self.__superprivate = "Hello"
...             self._semiprivate = ", world!"
...
>>> mc = MyClass()
>>> print mc.__superprivate
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: myClass instance has no attribute '__superprivate'
>>> print mc._semiprivate
, world!
>>> print mc.__dict__
{'_MyClass__superprivate': 'Hello', '_semiprivate': ', world!'}
```

##### 18. What is NumPy?
NumPy is the fundamental package for scientific computing in Python. It is a Python library that provides a multidimensional array object, various derived objects (such as masked arrays and matrices), and an assortment of routines for fast operations on arrays, including mathematical, logical, shape manipulation, sorting, selecting, I/O, discrete Fourier transforms, basic linear algebra, basic statistical operations, random simulation and much more. For more details please refer [numpy](https://docs.scipy.org/doc/numpy/user/whatisnumpy.html).

##### 19. What is pandas?
pandas is a Python package providing fast, flexible, and expressive data structures designed to make working with “relational” or “labeled” data both easy and intuitive. It aims to be the fundamental high-level building block for doing practical, real world data analysis in Python.  
pandas is well suited for many different kinds of data:  
* Tabular data with heterogeneously-typed columns, as in an SQL table or Excel spreadsheet
* Ordered and unordered (not necessarily fixed-frequency) time series data.
* Arbitrary matrix data (homogeneously typed or heterogeneous) with row and column labels
* Any other form of observational / statistical data sets. The data actually need not be labeled at all to be placed into a pandas data structure  

For getting started with basic pandas, Please refer [10 minutes to pandas](https://pandas.pydata.org/pandas-docs/stable/getting_started/10min.html)  

##### 20. How to iterate over pandas dataframe rows?
Using DataFrame.iterrows (generator) which yields both index and row.
```python
import pandas as pd

df = pd.DataFrame([{'c1':10, 'c2':100}, {'c1':11,'c2':110}, {'c1':12,'c2':120}])

for index, row in df.iterrows():
    print(row['c1'], row['c2'])

Output: 
   10 100
   11 110
   12 120
```

##### 21. How to handle large data files in pandas?
Data files which does not fit into memory can be ready in chunk sizes. Pandas has option to define chunk size while reading the data file.
* Option 1: Spefify chunksize to read_csv, doing so will return an iterable object of type TextFileReader
```python
reader = pd.read_csv('tmp.sv', sep='|', chunksize=4)

for chunk in reader:
	print(chunk)
```
* Option 2: Specify iterator=True to read_csv, it also return an iterable object of type TextFileReader.
```sql
reader = pd.read_csv('tmp.sv', sep='|', iterator=True)
reader.get_chunk(5)
```
