# Metaprogramming in Python
The magical world of python internals

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

```python

```

<!-- ```python
import builtins

builtin_types = [getattr(builtins, d) for d in dir(builtins) if isinstance(getattr(builtins, d), type)]


``` -->




---

## Everything happens in your terminal
Create slides and present them without ever leaving your terminal.

---

## Code execution
```go
package main

import "fmt"

func main() {
  fmt.Println("Execute code directly inside the slides")
}
```

You can execute code inside your slides by pressing `<C-e>`,
the output of your command will be displayed at the end of the current slide.

---

## Pre-process slides

You can add a code block with three tildes (`~`) and write a command to run *before* displaying
the slides, the text inside the code block will be passed as `stdin` to the command
and the code block will be replaced with the `stdout` of the command.

~~~graph-easy --as=boxart
[ A ] - to -> [ B ]
~~~

The above will be pre-processed to look like:

┌───┐  to   ┌───┐
│ A │ ────> │ B │
└───┘       └───┘

For security reasons, you must pass a file that has execution permissions
for the slides to be pre-processed. You can use `chmod` to add these permissions.

```bash
chmod +x file.md
```