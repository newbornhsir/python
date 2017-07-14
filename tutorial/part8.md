<h1 id="class">8.类</h1>

Compared with other programming languages, Python’s class mechanism adds classes with a minimum of new syntax and semantics. It is a mixture of the class mechanisms found in C++ and Modula-3. Python classes provide all the standard features of Object Oriented Programming: the class inheritance mechanism allows multiple base classes, a derived class can override any methods of its base class or classes, and a method can call the method of a base class with the same name. Objects can contain arbitrary amounts and kinds of data. As is true for modules, classes partake of the dynamic nature of Python: they are created at runtime, and can be modified further after creation.

In C++ terminology, normally class members (including the data members) are public (except see below Private Variables), and all member functions are virtual. As in Modula-3, there are no shorthands for referencing the object’s members from its methods: the method function is declared with an explicit first argument representing the object, which is provided implicitly by the call. As in Smalltalk, classes themselves are objects. This provides semantics for importing and renaming. Unlike C++ and Modula-3, built-in types can be used as base classes for extension by the user. Also, like in C++, most built-in operators with special syntax (arithmetic operators, subscripting etc.) can be redefined for class instances.

(Lacking universally accepted terminology to talk about classes, I will make occasional use of Smalltalk and C++ terms. I would use Modula-3 terms, since its object-oriented semantics are closer to those of Python than C++, but I expect that few readers have heard of it.)


<h1 id="name">8.1. Names and Objects</h1>

对象可以用于个人的和多个名字---别名的使用。通常在第一次使用python的时候不会喜欢这种做法，并且在处理不变类型的时候忽略了安全性。然而，别名在python语意中对于处理可变类型有很大帮助，比如列表，字典等。由于别名在某些方面的表现类似于指针，这有利于编程。比如，传递一个对象只需要传递一个指针，如果函数修改了一个作为参数传递的对象，调用者也会发现对象的变化。

<h1 id="scope">8.2. python作用域和命名空间</h1>

再介绍class之前，先简单介绍python作用域规则。

命名空间是名称到对象的映射。许多命名空间的实现类似于python的字典。命名空间的例子有：内置的名称集（如abs()）、模块内的全局名称、函数调用中的本地名称。在某种意义上，设置对象的属性也形成了一个命名空间。关于命名空间最重要的是不同命名空间的名称没有关联。比如：不同命名空间定义了同一个变量，在调用这个变量的时候需要加上模块前缀。

属性--类似z.attribute的形式。严格的说，在模块中引用名称是属性的引用：在表达式中modlename.funcname,modlename是模块对象，funcname是模块的属性。在这种情况下，模块的属性和模块中定义的全局名称之间恰好是一个简单的映射，它们共享同一个命名空间。

属性是可读或者可写的，在可写的情况下，可以对属性重新赋值。模块的属性是可写的，可写的属性可以使用del删除

命名空间在不同的条件下创建并且拥有不同的生命周期。命名空间包含内置的在解释器启动时创建的名称，并且不会删除。当读取模块的时候创建一个模块的全局命名空间，通常，模块的命名空间持续到解释器退出。The statements executed by the top-level invocation of the interpreter, either read from a script file or interactively, are considered part of a module called __main__, so they have their own global namespace. (The built-in names actually also live in a module; this is called builtins.)


函数的本地命名空间是在函数调用时创建的，当函数返回或引发异常时被删除。

作用域是python程序的一个文字区域，在这个区域中，可以直接访问命名空间。这里的“直接访问”表示对名称的不合格引用尝试在命名空间中查找名称。

虽然作用域的定义是静态的，但是使用是动态的。在执行过程中，至少有三个嵌套作用域可以直接访问命名空间：

- 首先搜索的最内层的包含本地名称的作用域
- the scopes of any enclosing functions, which are searched starting with the nearest enclosing scope, contains non-local, but also non-global names
- the next-to-last scope contains the current module’s global names
- the outermost scope (searched last) is the namespace containing built-in names

If a name is declared global, then all references and assignments go directly to the middle scope containing the module’s global names. To rebind variables found outside of the innermost scope, the nonlocal statement can be used; if not declared nonlocal, those variables are read-only (an attempt to write to such a variable will simply create a new local variable in the innermost scope, leaving the identically named outer variable unchanged).

Usually, the local scope references the local names of the (textually) current function. Outside functions, the local scope references the same namespace as the global scope: the module’s namespace. Class definitions place yet another namespace in the local scope.

It is important to realize that scopes are determined textually: the global scope of a function defined in a module is that module’s namespace, no matter from where or by what alias the function is called. On the other hand, the actual search for names is done dynamically, at run time — however, the language definition is evolving towards static name resolution, at “compile” time, so don’t rely on dynamic name resolution! (In fact, local variables are already determined statically.)

A special quirk of Python is that – if no global statement is in effect – assignments to names always go into the innermost scope. Assignments do not copy data — they just bind names to objects. The same is true for deletions: the statement del x removes the binding of x from the namespace referenced by the local scope. In fact, all operations that introduce new names use the local scope: in particular, import statements and function definitions bind the module or function name in the local scope.

The global statement can be used to indicate that particular variables live in the global scope and should be rebound there; the nonlocal statement indicates that particular variables live in an enclosing scope and should be rebound there.

example scope and nonlocal and global:

```
>>> def scope():
...     def local():
...         span = 'local span'
...     def nonlocals():
...         nonlocal span
...         span = 'nonlocal span'
...     def globals():
...         global span
...         span = 'global span'
...     span = 'test span'
...     local()
...     print('after  local assignment',span)
...     nonlocals()
...     print('after nonlocal assignement:',span)
...     globals()
...     print('after global assignment:',span)
... 
>>> scope()
after  local assignment test span
after nonlocal assignement: nonlocal span
after global assignment: nonlocal span
>>> print(span)
global span
>>> 

```

<h1 id="introduce">8.3.类的简单介绍</h1>

定义语法

```
calss ClassName:
    statement1
    ...
    ...
    statementN

```
在实际操作中，class中的语句经常是函数定义，也可以是其它语句。class中的函数定义有着特定的参数形式

当进入一个定义的类，将创建一个新的命名空间，使用本地作用域（我认为应该可以叫做局部作用域）。因此，所有的赋值到本地作用域都在新的命名空间中完成。

When a class definition is left normally (via the end), a class object is created. This is basically a wrapper around the contents of the namespace created by the class definition; we’ll learn more about class objects in the next section. The original local scope (the one in effect just before the class definition was entered) is reinstated, and the class object is bound here to the class name given in the class definition header (ClassName in the example).

### class 对象

Class 对象支持两种操作： 属性的引用和实例化

属性的引用使用python的标准语法：`obj.name`。合法的属性名称是那些在创建class object的时候所有的在class的命名空间中的名字。example：

```

class MyClass:
   """ a simple example class"""

   i = 1

   def f(self):
       pass

```

在这个例子中,Myclass.i/Myclass.f是合法的属性引用。属性可以重新分配值，`__doc__`返回类的文档字符串‘a simple example class‘

类的实例化通过以下方式

`x = MyClass()`

创建了一个类的实例化对象，并将该对象分配给本地变量x

这个实例化操作创建了一个空对象。许多class习惯域创建带有定制的初始状态的对象。需要使用`__init__`方法：

```
def __init__(self):
    pass

```

当一个class定义了一个__init__方法，class实例化会为新创建的class实例自动调用`__init__`方法。`__init__`方法可以通过传递参数获取更大的灵活性

```
>>> class P:
...     def __init__(self,name):
...         self.name = name
... 
>>> p1 = P('zhangsan')
>>> p2 = P('lisi')
>>> p1.name
'zhangsan'
>>> p2.name
'lisi'
>>> 
```

### 实例对象

实例对象唯一理解的操作是属性引用，有两种合法的属性：数据属性和方法。example:

```
class My:
 
    i = 1
    def say(self):
        pass

x = My()
x.i # data attribute
x.say() # method

```

method 是一个属于对象的函数（在python中，方法不是类的实例独有的，其他的数据类型也可以拥有方法。比如列表拥有append,insert等方法。）

实例对象的合法的方法名称依赖他的class。在定义中，class的所有属性中是函数对象的部分对应实例对象的方法。 所以x.say is method,My.f is a function

在实例对象的方法中，实例对象被当作第一个参数传入，x.say() equal My.say(x)


### class and instance variables

class : 所有实例对象共享的属性和方法
insturance: class 实例化后的独一无二的数据

example:

```

>>> class N:
...     l =[]
...     def __init__(self,name):
...         self.name = name
...     def add(self):
...         self.l.append('append')
... 
>>> n1 = N('a')
>>> n2 = N('v')
>>> n1.name
'a'
>>> n2.name
'v'
>>> n1.add()
>>> n1.l
['append']
>>> n2.add()
>>> n2.l
['append', 'append']
>>> n1.l
['append', 'append']
>>> 

```
在这里，我们不希望l是共享的，因此可以将l定义在`__init__`方法中

```
def __init__(self,name):
    self.l = []
    self.name = name

```

<h1 id="random">8.4.随机备注</h1>

数据属性覆盖具有相同名称的方法属性;
为了避免意外的名称冲突，这可能会导致大型程序中难以找到的错误，使用某种最小化冲突机会的约定是明智的。
可能的约定包括大写方法名称，前缀具有小的唯一字符串（可能只是下划线）的数据属性名称，或使用动词用于数据属性的方法和名词。

Data attributes may be referenced by methods as well as by ordinary users (“clients”) of an object. In other words, classes are not usable to implement pure abstract data types. In fact, nothing in Python makes it possible to enforce data hiding — it is all based upon convention. (On the other hand, the Python implementation, written in C, can completely hide implementation details and control access to an object if necessary; this can be used by extensions to Python written in C.)

Clients should use data attributes with care — clients may mess up invariants maintained by the methods by stamping on their data attributes. Note that clients may add data attributes of their own to an instance object without affecting the validity of the methods, as long as name conflicts are avoided — again, a naming convention can save a lot of headaches here.

There is no shorthand for referencing data attributes (or other methods!) from within methods. I find that this actually increases the readability of methods: there is no chance of confusing local variables and instance variables when glancing through a method.

Often, the first argument of a method is called self. This is nothing more than a convention: the name self has absolutely no special meaning to Python. Note, however, that by not following the convention your code may be less readable to other Python programmers, and it is also conceivable that a class browser program might be written that relies upon such a convention.

Any function object that is a class attribute defines a method for instances of that class. It is not necessary that the function definition is textually enclosed in the class definition: assigning a function object to a local variable in the class is also ok. For example:

```
# Function defined outside the class
def f1(self, x, y):
    return min(x, x+y)

class C:
    f = f1

    def g(self):
        return 'hello world'

    h = g

```

Now f, g and h are all attributes of class C that refer to function objects, and consequently they are all methods of instances of C — h being exactly equivalent to g. Note that this practice usually only serves to confuse the reader of a program.

Methods may call other methods by using method attributes of the self argument:

```
class Bag:
    def __init__(self):
        self.data = []

    def add(self, x):
        self.data.append(x)

    def addtwice(self, x):
        self.add(x)
        self.add(x)

```

Methods may reference global names in the same way as ordinary functions. The global scope associated with a method is the module containing its definition. (A class is never used as a global scope.) While one rarely encounters a good reason for using global data in a method, there are many legitimate uses of the global scope: for one thing, functions and modules imported into the global scope can be used by methods, as well as functions and classes defined in it. Usually, the class containing the method is itself defined in this global scope, and in the next section we’ll find some good reasons why a method would want to reference its own class.

Each value is an object, and therefore has a class (also called its type). It is stored as `object.__class__.`

<h1 id="inher">8.5.继承</h1>


继承语法

```
class DerivedClass(BaseClass):
    
    statement1
    ...
    ...
    statementN

```
BaseClass的定义必须在包含DeriverdClass的作用域中。派生类继承基类，派生类中没找到属性将会查找基类。如果基类继承其它基类并且在基类中没查找到这个属性，它会查找其它基类。

There’s nothing special about instantiation of derived classes: `DerivedClassName()` creates a new instance of the class. Method references are resolved as follows: the corresponding class attribute is searched, descending down the chain of base classes if necessary, and the method reference is valid if this yields a function object.

Derived classes may override methods of their base classes. Because methods have no special privileges when calling other methods of the same object, a method of a base class that calls another method defined in the same base class may end up calling a method of a derived class that overrides it. (For C++ programmers: all methods in Python are effectively `virtual`.)

An overriding method in a derived class may in fact want to extend rather than simply replace the base class method of the same name. There is a simple way to call the base class method directly: just call `BaseClassName.methodname(self, arguments)`. This is occasionally useful to clients as well. (Note that this only works if the base class is accessible as `BaseClassName` in the global scope.)

Python has two built-in functions that work with inheritance:

- Use `isinstance()` to check an instance’s type: `isinstance(obj, int)` will be `True` only if `obj.__class__ `is int or some class derived from `int`.
- Use `issubclass()` to check class inheritance: `issubclass(bool, int)` is `True` since `bool` is a subclass of `int`. However, `issubclass(float, int)` is `False` since `float` is not a subclass of `int`.

### Multiple Inheritance

Python supports a form of multiple inheritance as well. A class definition with multiple base classes looks like this:

```
class DerivedClassName(Base1, Base2, Base3):
    statement-1
    .
    .
    .
    statement-N

```

For most purposes, in the simplest cases, you can think of the search for attributes inherited from a parent class as depth-first, left-to-right, not searching twice in the same class where there is an overlap in the hierarchy. Thus, if an attribute is not found in DerivedClassName, it is searched for in Base1, then (recursively) in the base classes of Base1, and if it was not found there, it was searched for in Base2, and so on.

In fact, it is slightly more complex than that; the method resolution order changes dynamically to support cooperative calls to `super()`. This approach is known in some other multiple-inheritance languages as call-next-method and is more powerful than the super call found in single-inheritance languages.

Dynamic ordering is necessary because all cases of multiple inheritance exhibit one or more diamond relationships (where at least one of the parent classes can be accessed through multiple paths from the bottommost class). For example, all classes inherit from `object`, so any case of multiple inheritance provides more than one path to reach `object`. To keep the base classes from being accessed more than once, the dynamic algorithm linearizes the search order in a way that preserves the left-to-right ordering specified in each class, that calls each parent only once, and that is monotonic (meaning that a class can be subclassed without affecting the precedence order of its parents). Taken together, these properties make it possible to design reliable and extensible classes with multiple inheritance. For more detail, see <a href="https://www.python.org/download/releases/2.3/mro/">https://www.python.org/download/releases/2.3/mro/</a>.

<h1 id="private">8.6.私有变量</h1>

Python中不存在除在对象内部无法访问的“私有”实例变量。然而，大多数Python代码还有一个约定：以下划线为前缀的名称（例如_spam）应被视为API的非公开部分（无论是函数，方法还是数据成员）

Since there is a valid use-case for class-private members (namely to avoid name clashes of names with names defined by subclasses), there is limited support for such a mechanism, called name mangling. Any identifier of the form `__spam` (at least two leading underscores, at most one trailing underscore) is textually replaced with `_classname__spam`, where classname is the current class name with leading underscore(s) stripped. This mangling is done without regard to the syntactic position of the identifier, as long as it occurs within the definition of a class.

Name mangling is helpful for letting subclasses override methods without breaking intraclass method calls. For example:

```
class Mapping:
    def __init__(self, iterable):
        self.items_list = []
        self.__update(iterable)

    def update(self, iterable):
        for item in iterable:
            self.items_list.append(item)

    __update = update   # private copy of original update() method

class MappingSubclass(Mapping):

    def update(self, keys, values):
        # provides new signature for update()
        # but does not break __init__()
        for item in zip(keys, values):
            self.items_list.append(item)
```

Note that the mangling rules are designed mostly to avoid accidents; it still is possible to access or modify a variable that is considered private. This can even be useful in special circumstances, such as in the debugger.

Notice that code passed to `exec()` or `eval()` does not consider the classname of the invoking class to be the current class; this is similar to the effect of the global statement, the effect of which is likewise restricted to code that is byte-compiled together. The same restriction applies to `getattr()`, `setattr()` and `delattr()`, as well as when referencing `__dict__` directly.


<h1 id="odds">8.7. Odds and Ends</h1>

Sometimes it is useful to have a data type similar to the Pascal “record” or C “struct”, bundling together a few named data items. An empty class definition will do nicely:

```
class Employee:
    pass

john = Employee()  # Create an empty employee record

# Fill the fields of the record
john.name = 'John Doe'
john.dept = 'computer lab'
john.salary = 1000

```

A piece of Python code that expects a particular abstract data type can often be passed a class that emulates the methods of that data type instead. For instance, if you have a function that formats some data from a file object, you can define a class with methods `read()` and `readline()` that get the data from a string buffer instead, and pass it as an argument.

Instance method objects have attributes, too: `m.__self__ `is the instance object with the method `m()`, and `m.__func__` is the function object corresponding to the method.

<h1 id="iter">8.8. Iterators</h1>

你可能注意到许多对象可以使用for迭代

```
for element in [1, 2, 3]:
    print(element)
for element in (1, 2, 3):
    print(element)
for key in {'one':1, 'two':2}:
    print(key)
for char in "123":
    print(char)
for line in open("myfile.txt"):
    print(line, end='')

```
 
 for 语句在容器对象上调用 `iter()`. 这个函数返回了一个定义了`__next__`方法的迭代器对象。 当容器对象没有更多元素的时候，` __next__()` 升起`StopIteration` 异常终止for循环。 可以使用内建的`next()`方法调用`__next__()`。example:

```
>>>
>>> s = 'abc'
>>> it = iter(s)
>>> it
<iterator object at 0x00A1DB50>
>>> next(it)
'a'
>>> next(it)
'b'
>>> next(it)
'c'
>>> next(it)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    next(it)
StopIteration

```
在自己的类中定义迭代器行为。 定义`__iter__()` 方法返回一个带有`__next__()` 方法的对象. 如果 the class 定义 `__next__()`, 然后 `__iter__()` 只能返回自身:

```
>>> class i:
...     def __init__(self,data):
...         self.data = data
...         self.index = len(self.data)
...     def __iter__(self):
...         return self
...     def __next__(self):
...         if self.index == 0:
...             raise StopIteration
...         self.index -= 1
...         return self.data[self.index]
... 
>>> s = i('asdf')
>>> l = iter(s)
>>> next(l)
'f'
>>> next(l)
'd'
>>> next(l)
's'
>>> next(l)
'a'
>>> next(l)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 9, in __next__
StopIteration
>>> next(s)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 9, in __next__
StopIteration
>>> s = i('asdfg')
>>> next(s)
'g'
>>> next(s)
'f'
>>> next(s)
'd'
>>> 
```

<h1 id="generator">8.9. Generators</h1>

生成器是创建迭代器简单强大的工具。 、它们像常规函数一样编写但是使用`yield`语句返回数据。 每次调用`next()`， the generator resumes where it left off (it remembers all the data values and which statement was last executed). example:

```
>>> def f(data):
...     for n in range(len(data)):
...         yield data[n]
... 
>>> it = f('abcd')
>>> next(it)
'a'
>>> next(it)
'b'
>>> next(it)
'c'
>>> next(it)
'd'
>>> next(it)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>> 

```

Anything that can be done with generators can also be done with class-based iterators as described in the previous section. What makes generators so compact is that the `__iter__()` and `__next__()` methods are created automatically.

另一个关键特征是局部变量和执行状态在调用之间自动保存。这使得该功能比使用诸如self.index和self.data之类的实例变量的方法更容易编写和更清晰

In addition to automatic method creation and saving program state, when generators terminate, they automatically raise StopIteration. In combination, these features make it easy to create iterators with no more effort than writing a regular function.

<h1 id="expression">8.10. Generator Expressions</h1>

Some simple generators can be coded succinctly as expressions using a syntax similar to list comprehensions but with parentheses instead of brackets. These expressions are designed for situations where the generator is used right away by an enclosing function. Generator expressions are more compact but less versatile than full generator definitions and tend to be more memory friendly than equivalent list comprehensions.

Examples:

```
>>>
>>> sum(i*i for i in range(10))                 # sum of squares
285

>>> xvec = [10, 20, 30]
>>> yvec = [7, 5, 3]
>>> sum(x*y for x,y in zip(xvec, yvec))         # dot product
260

>>> from math import pi, sin
>>> sine_table = {x: sin(x*pi/180) for x in range(0, 91)}

>>> unique_words = set(word  for line in page  for word in line.split())

>>> valedictorian = max((student.gpa, student.name) for student in graduates)

>>> data = 'golf'
>>> list(data[i] for i in range(len(data)-1, -1, -1))
['f', 'l', 'o', 'g']

```



