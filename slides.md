# Metaprogramming in Python
The magical world of python internals

> “Metaclasses are deeper magic than 99% of users should ever worry about. If you wonder whether you need them, you don’t (the people who actually need them know with certainty that they need them, and don’t need an explanation about why).”

-- Tim Peters

---

## Python is a dynamic language

```python
x = 5

print(type(x))

x = 'Five'

print(type(x))
```

Unlike in other languages, like typescript, where you declare a variable with it's type, python types are inferenced via the interpretter. 

---

## Introspectiong types in python
What happens when we introspect common types in python?

```python
print(type(int))
print(type(str))
print(type(float))
print(type(dict))
print(type(list))
```

```python
class CustomType:
    pass

print(type(CustomType))
```

---

## What is a type?

> A type is a signifier to the interpretter or compiler of the way a particular object is meant to be used in a program.

Because python is dynamically typed, different types define access and mutation patterns within their own definitions. 

This is why you can't add a string and an int -- the addition opertator (`__add__`) does not support cross type operations.

__**Python is dynamically typed, but also, strongly typed.**__[^1]

```python
print(type(type))
```

[^1]: https://wiki.python.org/moin/Why%20is%20Python%20a%20dynamic%20language%20and%20also%20a%20strongly%20typed%20language


---

## Metaclasses in Python

The default metaclass is `type`. 

As we've seen, `type` can tell you the type of an object, but it can also be used to create classes!

```python
MyClass = type('MyClass', (), {'__init__': lambda x: print('An instance of MyClass has been created!')})
print(MyClass)

class MyOtherClass:
    def __init__(self):
        print('An instance of MyOtherClass has been created!')

print(MyOtherClass)


x = MyClass()
y = MyOtherClass()
```

In some ways, the `class Name...` syntax is syntactic sugar over the first method of creating classes. 


**But how do we use Metaclasses in python?**

---
## Exploring Metaclasses in Python

Metaclasses are the class of a a class, and they inherit from `type`. 

```python
class MyMetaClass(type):
    pass

class MyClass(metaclass=MyMetaClass):
    pass

my_class_instance = MyClass()

print(type(MyClass))
print(MyClass.__class__)
print(MyClass.__mro__)
```

### So what's actually happening?

1. The first thing that happens is that method resolution order (MRO) is resolved.

2. The appropriate metaclass is determined.

3. The class namespace is prepared.

4. The class body is executed.

5. The class object is created.


---
## Resolving Method Resolution Order
This determines the order in which method names are resolved.

Specifically, which class's method should be used for what? 

Take the following code sample:

```python
class MyMetaClass(type):
    pass

class MyClass(metaclass=MyMetaClass):
    def helper_function(self):
        print('Calling from MyClass!')

class MySubClass(MyClass):
    def helper_function(self):
        print('Calling from MySubClass')

class MySubSubClass(MySubClass):
    pass

instance = MySubSubClass()
instance.helper_function()

print(MySubSubClass.__mro__)
```

---

## Determining the appropriate metaclass

### If no metaclass is given, just use `type` as the metaclass. (Remember, `type` is the default metaclass!)
```python
class MyCustomClass:
    pass

print(MyCustomClass.__class__)
```

### If an explicit metaclass is given and it is not an instance of `type`, then it is used directly as the metaclass.

In this case, we're passing a callable as the metaclass.

```python
class MyMetaClass(type):
    pass

def my_meta_callable(name, bases, namespace):
    return MyMetaClass(name, bases, namespace)

class MyCustomClass(metaclass=my_meta_callable):
    pass

class MyCustomDerivedClass(MyCustomClass):
    pass

print(type(my_meta_callable))

print(MyCustomClass.__class__)
print(type(MyCustomClass))

print(MyCustomDerivedClass.__class__)
print(type(MyCustomDerivedClass))
```


### If an instance of `type` is given as the explicit metaclass, or bases are defined, then the most derived metaclass is used.

All classes in python inherit from object. If you look at the MRO for any class you create, you'll see that the last entry is `object`.

```python
class MyCustomClass:
    pass

print(MyCustomClass.__mro__)

print(type(object))

print(isinstance(object, type))
```

`object` is an instance of type! 

All regular classes (i.e., non-metaclasses), are instances of `type`, which means if you try to specify a regular class as a metaclass, it will ignore that and try to use the "most dereived metaclass" -- if none of the base classes define a metaclass, then the most derived metaclass will be the default metaclass -- `type`!


---

## Preparing the class namespace

The class namespace is prepared via `__prepare__()`

The `__prepare__()` function returns a dictionary-like object to that contains all of the class's data. 

It doesn't necessarily need to be a dicitonary, but it could be. 

You could have retrun a custom implementation of `dict` that cares about ordering or has some other property.

## The class body is executed




---

## The class object is created

---

## Descriptors

---

## Conclusion

- Metaprogramming in Python is very powerful
- With great power, comes great responsibility


---
