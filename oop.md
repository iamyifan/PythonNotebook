# Python OOP

## 1. 一切皆对象

**面向对象编程（Object Oriented Programming, OOP）**由**面向过程编程（Procedure Oriented Programming, POP）**发展而来，具有更强的灵活性和拓展性。以下[表格](https://www.upgrad.com/blog/oop-vs-pop/)展示了 OOP 和 POP 的不同：

| **Parameters** | **OOP** | **POP** |
| ------------------ | ----------| ---------- |
| **Basic Definition** | Object-oriented programming | Structure or procedure-oriented programming |
| **Program Division** | Objects | Functions |
| **Approach** | Bottom-up approach | Top-down approach |
| **Data Control** | Data in each object is controlled on its own. | Every function has different data, so there’s no control over it. |
| **Entity Linkage** | Object functions are linked through message passing. | Parts of a program are linked through parameter passing. |
| **Expansion** | Adding new data and functions is easy. | Expanding data and function is not easy. |
| **Inheritance**                  | Inheritance is supported in three modes: public, private & protected. |Not supported.|
| **Access control**               | Access control is done with access modifiers.                |Not supported.|
| **Data Hiding**                  | Data can be hidden using Encapsulation.                      |Not supported.|
| **Overloading or Polymorphism\***  | Overloading functions, constructors, and operators are done. |Not supported.|
| **Friend function**              | Classes or functions can be linked using the keyword *friend*, only in C++. |Not supported.|
| **Virtual classes or functions** | The virtual function appears during inheritance.             |Not supported.|
| **Code Reusability**             | The existing code can be reused.                             |Not supported.|
| **Problem Solving**              | Used for solving big problems.                               |Not suitable for solving big problems.|
| **Example**                      | C++, Java, VB.NET, C#.NET.                                |C, VB, FORTRAN, Pascal|

OOP 基于**封装（Encapsulation）**的思想，将用于描述**对象**的**数据**和**函数**封装到一起。在 Python 中一切皆对象，例如 `int`, `float`, `str` 和 `list` 等。

常见术语简述如下：

- **类（Class）**：构建对象的“蓝图”。
- **对象（Object）**：通过类创建出的实例，是属性和方法的集合。
- **属性（Attribute）**：类中的所有属性。
- **方法（Method）**：类中的所有函数。

## 2. 类的构建

在 Python 中可通过关键字 `class` 来定义类，基本语法如下：

```python
class ClassName:  # 类头
    """ Document...
    """
    # 类体
    attribute1 = ...
    attribute2 = ...
    def method1(self, *args, **kwargs):
        ...
    def method2(self, *args, **kwargs):
        ...
```

类名常采用首字母大写的 `CamelCase` 方式命名，而属性和方法采用 `snake_case` 方式命名。关于类的说明文档放置在类头之后。

在创建类时，可以为类手动添加一个**构造方法（构造函数）**：`__init__()`。每当类创建实例时，Python 解释器会自动调用该方法。语法如下：

```python
class ClassName:
    def __init__(self, *args, **kwargs):
        ...
```

构造方法 `__init__()` 必须包含参数 `self`，且必须作为第一个参数。当类中没有被手动添加构造方法时，Python 会自动为类添加一个仅包含参数 `self` 的构造方法，即默认构造方法。

## 3. 类的实例化

类的实例化指创建类对象的过程。语法如下：

```python
class ClassName:
    class_attr1 = ...
    class_attr2 = ...
    def __init__(self, attr1,attr2):
		self.obj_attr1 = attr1
        self.obj_attr2 = attr2
    def method(self, *args, **kwargs):
        ...
        
object_name = ClassName(attr1, attr2)
```

上述实例中，由于构造方法除了参数 `self` 外，另设有参数 `attr1` 和 `attr2`，因此在实例化时需要传入相应的参数值。在类方法外定义的变量 `class_attr1` 和 `class_attr2` 称为**类变量**，在类方法中定义的变量 `obj_attr1` 和 `obj_attr2` 称为**实例变量**。

实例化后的类对象可以执行以下操作：

- 访问或修改类对象的实例变量
- 添加或删除类对象的实例变量
- 调用类对象的方法
- 动态添加新的类对象方法

通过对象调用实例变量和实例方法的语法如下：

```python
object_name.obj_attr1
object_name.method()
```

为对象动态添加和删除实例变量的语法如下：

```python
object_name.new_obj_attr = ...
del object_name.new_obj_attr
```

Python 允许通过**动态绑定**的方式为对象添加新的实例方法，例如：

```python
def func_name(self, *args, **kwargs):
	...

object_name.new_obj_func = func_name
object_name.func_name(object_name)
```

或采用 `lambda` 表达式：

```python
object_name.new_obj_func = lambda self: ...
object_name.new_obj_func(object_name)
```

在动态绑定方法中，Python 不会自动将调用者（对象）绑定到参数 `self`，必须手动为参数 `self` 传值，即该方法的调用者。

除此之外，可以使用模块 **`types`** 中的 **`MethodType`** 来免去手动传参，例如：

```python
from types import MethodType

def func_name(self, *args, **kwargs):
	...

object_name.new_obj_func = MethodType(func_name, object_name)
object_name.new_obj_func()
```

`MethodType` 在包装实例方法 `func_name` 时，已经将参数 `self` 绑定为对象 `object_name`，因此在后续调用该方法时无需再手动传参。

## 4. 参数 `self` 详解

Python 规定，在构造方法和实例方法中，至少包含一个参数，并约定俗成称之为 `self`。

参数 `self` 的作用相当于 C++ 中的 `this` 指针。当对象调用类方法时，该对象会将自身的引用作为第一个参数传递给该方法，即 Python 自动将该方法的第一个参数指向了其调用者。由此，Python 解释器可以区分不同对象的间的方法调用过程。在构造函数中，参数 `self` 代表了当前正在实例化的类对象。

```python
class ClassName:
    def __init__(self):
        print(self.__repr__())

object_name1 = ClassName()
object_name2 = ClassName()
```

```python
<__main__.ClassName object at 0x000001A71C3C1580>
<__main__.ClassName object at 0x000001A71C3C17F0>
```

## 5. 类变量和实例变量



## 6. 类方法、实例方法和静态方法



## 参考

[Classes - Python documentation](https://docs.python.org/3/tutorial/classes.html)

[Python类和对象 - C语言中文网](http://c.biancheng.net/python/class_object/)

