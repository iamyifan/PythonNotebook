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

上述实例中，由于构造方法除了参数 `self` 外，另设有参数 `attr1` 和 `attr2`，因此在实例化时需要传入相应的参数值。在类方法外定义的变量 `class_attr1` 和 `class_attr2` 称为**类属性**，在类方法中定义的变量 `obj_attr1` 和 `obj_attr2` 称为**实例属性**。

实例化后的类对象可以执行以下操作：

- 访问或修改类对象的实例属性
- 添加或删除类对象的实例属性
- 调用类对象的方法
- 动态添加新的类对象方法

通过对象调用实例属性和实例方法的语法如下：

```python
object_name.obj_attr1
object_name.method()
```

为对象动态添加和删除实例属性的语法如下：

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

## 4. 参数 `self` 

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

## 5. 类属性，实例属性和局部变量

在类体中，根据属性（变量）定义的不同位置以及定义的不同方式，可将属性分为以下三种：

- **类属性或类变量（Class Attribute）**：位于类体中，所有方法外
- **实例属性或实例变量（Instance Attribute）**：位于所有方法内部，常以 `self.attr_name` 的方式出现
- **局部变量（Local Varibale）**：位于所有方法内部，常以 `attr_name = attr_value` 的方式出现

```python
class ClassName:
    cls_attr1 = "class attribute 1"
    cls_attr2 = "class attribute 2"
    def __init__(self):
        self.obj_attr1 = "object attribute 1"
    def func(self):
        self.obj_attr2 = "object attribute 2"
        local_var = "local variable"
```

在上述示例中，类属性为 `cls_attr1` 和 `cls_attr2`，实例属性为 `obj_attr1` 和 `obj_attr2`，局部变量为 `local_var`。

```python
object_name1 = ClassName()
print(ClassName.cls_attr1, object_name1.cls_attr1)  # 类属性可通过类和对象调用

ClassName.cls_attr1 = "new class attribute 1"  # 通过类对类属性的修改会影响所有对象
print(ClassName.cls_attr1, object_name1.cls_attr1)

object_name2 = ClassName()
object_name1.cls_attr1 = "NEW CLASS ATTRIBUTE 1"  # 通过对象对类属性的修改不会影响其他对象
print(ClassName.cls_attr1, object_name1.cls_attr1, object_name2.cls_attr1)
print(object_name1.__dict__)  # 打印对象的所有实例属性
```

```python
class attribute 1 class attribute 1
new class attribute 1 new class attribute 1
class attribute 1 NEW CLASS ATTRIBUTE 1 class attribute 1
{'obj_attr1': 'object attribute 1', 'obj_attr2': 'object attribute 2', 'cls_attr1': 'NEW CLASS ATTRIBUTE 1'}
```

通过类对类属性进行修改，新的类属性由所有对象共享；通过对象对类属性进行修改，对象会创建同名的实例属性，而类属性自身和其他对象不受影响。在类中，实例属性和类属性可以同名，但会导致无法通过对象调用类属性的情况，故不推荐使用对象调用类属性。

类属性，实例属性和局部变量的区别总结如下：

| | **类属性** | **实例属性** | **局部变量** |
| :-: | :----: | :------: | :---: |
| **定义位置** | 类体中，所有方法外 |任意方法内|任意方法内|
| **定义形式** | `cls_attr = cls_attr_val ` |`self.obj_attr = obj_attr_val`|`local_var = local_var_val`|
| **作用域** | 由所有对象共有 |其所属的对象|其定义的方法内部，方法执行完成后销毁|
|**类调用**|例如 `ClassName.cls_attr`|不可调用|不可调用|
|**对象调用**|（不推荐）由任意对象调用，例如 `object_name.cls_attr`|由其所属的对象调用，例如 `self.obj_attr`|不可调用|
| **动态添加** | 允许动态添加类属性，添加后的类属性由所有变量共有 |允许动态添加实例属性，添加后的实例属性归属于其所属对象|不可动态添加|


## 6. 类方法、实例方法和静态方法

在语法上：

- **类方法（Class Method）**：采用函数装饰器 `@classmethod` 装饰的方法
- **实例方法（Instance Method）**：没有被任何函数装饰器所装饰的方法
- **静态方法（Static Method）**：采用函数装饰器 `@staticmethod` 装饰的方法

下例展示了对上述三种方法的定义和使用：

```python
class ClassName:
    cls_attr = "class attribute"
    def __init__(self):  # 构造方法属于一种特殊的实例方法
        self.obj_attr = "object attribute"
    def obj_method(self):  # 实例方法
        print("object method")
    @classmethod
    def cls_method(cls):  # 类方法
        print("class method")
    @staticmethod
    def stc_method(*args, **kwargs):  # 静态方法
        print(args, kwargs)
	
object_name = ClassName()
object_name.obj_method()  # 通过对象调用实例方法
ClassName.obj_method(object_name)  # 通过类调用实例方法，需要手动传入参数作为self

ClassName.cls_method()  # 通过类调用类方法
object_name.cls_method()  # 通过对象调用类方法，不需要再手动传入参数

ClassName.stc_method(1, 2, a=1, b=2)  # 通过类调用静态方法
object_name.stc_method(3, 4, c=3, d=4)  # 通过对象调用静态方法
```

```python
object method
object method

class method
class method

(1, 2) {'a': 1, 'b': 2}
(3, 4) {'c': 3, 'd': 4}
```

**绑定方法**指通过对象访问类成员的方法，**非绑定方法**指通过类调用类成员的方法。

与参数 `self` 相似，类方法的第一个参数约定俗成命名为参数 `cls` 。Python 自动将类方法与其调用者（类）绑定，因此无需显式得为其传参。

静态方法与普通函数相似，静态方法定义在**类命名空间**中，而普通函数定义在程序所在的**全局命名空间**中。静态方法不需要绑定参数 `self` 和 `cls`。在 Python 中，所有通过关键字 `class` 修饰的代码块，都可视为独立的类命名空间。

类方法、实例方法和静态方法的区别总结如下：

|      | **类方法** | **实例方法** |  **静态方法**   |
| :------------: | :------------: | :----------: | :-------------: |
| **函数装饰器** | `@classmethod` |      无      | `@staticmethod` |
|       **参数 `self`**       | 无 |第一个参数 `self` 自动绑定调用该方法的对象|无|
|       **参数 `cls`**       |           第一个参数 `cls` 自动绑定调用该方法的类            |无|无|
|**类调用**|例如 `ClassName.cls_method()`|（非绑定方法）由类调用时需要手动传入参数给 `self`，例如 `ClassName.obj_method(obj_name)`|例如 `ClassName.stc_method()`|
|**对象调用**|（不推荐）由对象调用时不需要手动传入参数，例如 `object_name.cls_method()`|         （绑定方法）例如 `object_name.obj_method()`          |例如 `object_name.stc_method()`|

## 7. 描述符




## 参考

[9. Classes - Python documentation](https://docs.python.org/3/tutorial/classes.html)

[Python类和对象 - C语言中文网](http://c.biancheng.net/python/class_object/)

