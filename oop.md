# Python OOP

## 1. 一切皆对象

**面向对象编程（Object Oriented Programming, OOP）**由**面向过程编程（Procedure Oriented Programming, POP）**发展而来，具有更强的灵活性和拓展性。以下[表格](https://www.upgrad.com/blog/oop-vs-pop/)展示了 OOP 和 POP 的不同：

|  | **OOP** | **POP** |
| ------------------ | ----------| ---------- |
| **Basic Definition** | Object Oriented Programming | Structure or Procedure Oriented Programming |
| **Program Division** | Objects | Functions |
| **Approach** | Bottom-up approach | Top-down approach |
| **Data Control** | Data in each object is controlled on its own. | Every function has different data, so there's no control over it. |
| **Entity Linkage** | Object functions are linked through message passing. | Parts of a program are linked through parameter passing. |
| **Expansion** | Adding new data and functions is easy. | Expanding data and function is not easy. |
| **Inheritance**                  | Inheritance is supported in three modes: public, private & protected. |Not supported.|
| **Access control**               | Access control is done with access modifiers.                |Not supported.|
| **Data Hiding**                  | Data can be hidden using encapsulation.                     |Not supported.|
| **Overloading or Polymorphism** | Overloading functions, constructors, and operators are done. |Not supported.|
| **Friend function**              | Classes or functions can be linked using the keyword *friend*, only in C++. |Not supported.|
| **Virtual classes or functions** | The virtual function appears during inheritance.             |Not supported.|
| **Code Reusability**             | The existing code can be reused.                             |Not supported.|
| **Problem Solving**              | Used for solving big problems.                               |Not suitable for solving big problems.|
| **Example**                      | C++, Java, VB.NET, C#.NET.                                |C, VB, FORTRAN, Pascal|

OOP 基于**封装（Encapsulation）**的思想，将用于描述**对象**的**数据**和**函数**封装到一起。在 Python 中一切皆对象，例如 `int`, `float`, `str` 和 `list` 等。

常见术语简述如下：

- **类（Class）**：构建对象的模板，用于创建具体的对象
- **对象（Object）**：通过类创建出的实例（即实例化过程），是属性和方法的集合
- **属性（Attribute）**：类中的所有变量统称为属性
- **方法（Method）**：类中的所有函数统称为方法

## 2. 类的构建

在 Python 中可通过关键字 `class` 来定义类，基本语法如下：

```python
class MyCls:  # 类头
    """ Document...
    """
    # 类体
    cls_attr1 = None
    cls_attr2 = None
    def obj_ meth1(self, *args, **kwargs):
        pass
    def obj_meth2(self, *args, **kwargs):
        pass
```

类名常采用首字母大写的 `CamelCase` 方式命名，属性和方法的名称采用 `snake_case` 方式命名。关于类的说明文档放置在类头之后，类体之前。

在创建类时，可以为类手动添加一个**构造方法（构造函数）**：`__init__()`。每当类创建实例时，Python 解释器会自动调用该方法。语法如下：

```python
class MyCls:
    def __init__(self, *args, **kwargs):
        pass
```

构造方法 `__init__()` 必须包含参数 `self`，且必须作为第一个参数。当类中没有被手动添加构造方法时，Python 会自动为类添加一个仅包含参数 `self` 的构造方法，即默认构造方法。

## 3. 类的实例化

类的**实例化**指创建类对象的过程。语法如下：

```python
class MyCls:
    cls_attr1 = None
    cls_attr2 = None
    def __init__(self, attr1, attr2):
		self.obj_attr1 = attr1
        self.obj_attr2 = attr2
    def obj_meth(self, *args, **kwargs):
        pass
        
my_obj = MyCls(attr1, attr2)
```

上述实例中，由于构造方法除了参数 `self` 外，另设有参数 `attr1` 和 `attr2`，因此在实例化时需要传入相应的参数值。在类方法外定义的变量 `cls_attr1` 和 `cls_attr2` 称为**类属性**，在类方法中定义的变量 `obj_attr1` 和 `obj_attr2` 称为**实例属性**。

实例化后的对象可以执行以下操作：

- 访问或修改该对象的实例属性
- 添加或删除该对象的实例属性
- 调用该对象的实例方法
- 动态绑定新的实例方法

通过对象调用实例属性和实例方法的语法如下：

```python
my_obj.obj_attr1  # 实例属性
my_obj.obj_meth()  # 实例方法
```

为对象添加和删除实例属性的语法如下：

```python
my_obj.new_obj_attr = None  # 添加实例属性
del my_obj.new_obj_attr  # 删除实例属性
```

Python 允许通过**动态绑定**的方式为对象添加新的实例方法，例如：

```python
def new_obj_meth(self, *args, **kwargs):
	pass

my_obj.new_obj_meth = new_obj_meth  # 动态绑定新的实例方法
my_obj.new_obj_meth(my_obj)  # 调用新的实例方法，手动为参数 self 传参
```

或采用 `lambda` 表达式：

```python
my_obj.new_obj_meth = lambda self: pass  # 动态绑定新的实例方法（lambda 表达式）
my_obj.new_obj_meth(my_obj)  # 调用新的实例方法，手动为参数 self 传参
```

在动态绑定方法中，Python 不会自动将调用者（对象）绑定到参数 `self`，必须手动为参数 `self` 传值，即该方法的调用者。

除此之外，可以使用模块 **`types`** 中的 **`MethodType`** 来免去手动传参，例如：

```python
from types import MethodType

def new_obj_meth(self, *args, **kwargs):
	pass

my_obj.new_obj_meth = MethodType(new_obj_meth, my_obj)  # 通过 MethodType 动态添加新的实例方法
my_obj.new_obj_meth()  # 调用新的实例方法，无需为参数 self 手动传参
```

`MethodType` 在包装实例方法 `new_obj_meth` 时，已经将参数 `self` 绑定为对象 `my_obj`，因此在后续调用该方法时无需再手动传参。

## 4. 参数 `self` 

Python 规定，在构造方法和实例方法中，至少包含一个参数，并约定俗成称之为 `self`。

参数 `self` 的作用相当于 C++ 中的 `this` 指针。当对象调用实例方法时，该对象会将自身的引用作为第一个参数传递给该方法，即 Python 自动将该方法的第一个参数指向了其调用者。由此，Python 解释器可以区分不同对象的间的方法调用过程。在构造函数中，参数 `self` 代表了当前正在实例化的对象。

下例展示了不同对象在构造函数中，参数 `self` 被指向了不同的内存地址。

```python
class MyCls:
    def __init__(self):
        print(self.__repr__())

my_obj1 = MyCls()
my_obj2 = MyCls()
```

```python
<__main__.MyCls object at 0x000001A71C3C1580>
<__main__.MyCls object at 0x000001A71C3C17F0>
```

## 5. 类属性，实例属性和局部变量

在类体中，根据属性（变量）定义的不同位置以及定义的不同方式，可将属性分为以下三种：

- **类属性或类变量（Class Attribute）**：位于类体中，所有方法外
- **实例属性或实例变量（Instance Attribute）**：位于所有方法内部，常以 `self.obj_attr` 的方式出现
- **局部变量（Local Varibale）**：位于所有方法内部，常以 `local_var = local_var_val` 的方式出现

```python
class MyCls:
    cls_attr1 = "class attribute 1"  # 类属性
    cls_attr2 = "class attribute 2"  # 类属性
    def __init__(self):  # 构造函数
        self.obj_attr1 = "object attribute 1"  # 实例属性
    def obj_meth(self):  # 实例方法
        self.obj_attr2 = "object attribute 2"  # 实例属性
        local_var = "local variable"  # 局部变量
```

在上述示例中，类属性为 `cls_attr1` 和 `cls_attr2`，实例属性为 `obj_attr1` 和 `obj_attr2`，局部变量为 `local_var`。

```python
my_obj1 = MyCls()
print(MyCls.cls_attr1, my_obj1.cls_attr1)  # 类属性可通过类和对象调用

MyCls.cls_attr1 = "new class attribute 1"  # 通过类对类属性的修改会影响所有对象
print(MyCls.cls_attr1, my_obj1.cls_attr1)

my_obj2 = MyCls()
my_obj2.cls_attr1 = "NEW CLASS ATTRIBUTE 1"  # 通过对象对类属性的修改不会影响其他对象
print(MyCls.cls_attr1, my_obj1.cls_attr1, my_obj2.cls_attr1)
print(my_obj2.__dict__)  # 打印对象的所有实例属性，在 my_obj2 中等效于创建了新的同名实例属性 cls_attr1
```

```python
class attribute 1 class attribute 1
new class attribute 1 new class attribute 1
class attribute 1 class attribute 1 NEW CLASS ATTRIBUTE 1
{'obj_attr1': 'object attribute 1', 'obj_attr2': 'object attribute 2', 'cls_attr1': 'NEW CLASS ATTRIBUTE 1'}
```

通过类对类属性进行修改，新的类属性由所有对象共享；通过对象对类属性进行修改，对象会创建同名的实例属性，而类属性自身不受影响。在类中，实例属性和类属性可以同名，但会导致无法通过对象调用类属性，故不推荐使用对象调用类属性。

类属性，实例属性和局部变量的区别总结如下：

| | **类属性** | **实例属性** | **局部变量** |
| :-: | :----: | :------: | :---: |
| **定义位置** | 类体中，所有方法外 |任意方法内|任意方法内|
| **定义形式** | `cls_attr = cls_attr_val ` |`self.obj_attr = obj_attr_val`|`local_var = local_var_val`|
| **作用域** | 所有对象共享 |其所属的对象专属|函数内部，执行完成后销毁|
|**类调用**|例如 `MyCls.cls_attr`|不可调用|不可调用|
|**对象调用**|（不推荐）任意对象调用，例如 `my_obj.cls_attr`|其所属的对象调用，例如 `my_obj.obj_attr`|不可调用|
| **动态添加** | 允许动态添加类属性，添加后的类属性由所有变量共有 |允许动态添加实例属性，添加后的实例属性专属于该对象|不可动态添加|


## 6. 类方法、实例方法和静态方法

在语法上：

- **类方法（Class Method）**：采用函数装饰器 `@classmethod` 装饰的方法
- **实例方法（Instance Method）**：没有被任何函数装饰器所装饰的方法
- **静态方法（Static Method）**：采用函数装饰器 `@staticmethod` 装饰的方法

下例展示了对上述三种方法的定义和使用：

```python
class MyCls:
    cls_attr = "class attribute"
    def __init__(self):  # 构造方法，属于一种特殊的实例方法
        self.obj_attr = "object attribute"
    def obj_meth(self):  # 实例方法
        print("object method")
    @classmethod
    def cls_meth(cls):  # 类方法
        print("class method")
    @staticmethod
    def stc_meth(*args, **kwargs):  # 静态方法
        print(args, kwargs)
	
my_obj = MyCls()
my_obj.obj_meth()  # 通过对象调用实例方法
MyCls.obj_meth(my_obj)  # 通过类调用实例方法，需要为参数 self 手动传参

MyCls.cls_meth()  # 通过类调用类方法
my_obj.cls_meth()  # 通过对象调用类方法，不需要为参数 self 手动传参

MyCls.stc_meth(1, 2, a=1, b=2)  # 通过类调用静态方法
my_obj.stc_meth(3, 4, c=3, d=4)  # 通过对象调用静态方法
```

```python
object method
object method
class method
class method
(1, 2) {'a': 1, 'b': 2}
(3, 4) {'c': 3, 'd': 4}
```

**绑定方法**指通过对象调用类方法，Python 在调用自动将参数 `self` 与其调用者绑定，无需再手动传参；**非绑定方法**指通过类调用类方法，需要为参数 `self` 手动传参。

与参数 `self` 相似，类方法的第一个参数约定俗成命名为参数 `cls` 。Python 自动将类方法与其调用者（类）绑定，因此无需显式得为其传参。

静态方法与普通函数相似，静态方法定义在**类命名空间**中，而普通函数定义在程序所在的**全局命名空间**中。静态方法不需要绑定参数 `self` 和 `cls`。在 Python 中，所有通过关键字 `class` 修饰的代码块，都可视为独立的类命名空间。

类方法、实例方法和静态方法的区别总结如下：

|      | **类方法** | **实例方法** |  **静态方法**   |
| :------------: | :------------: | :----------: | :-------------: |
| **函数装饰器** | `@classmethod` |      无      | `@staticmethod` |
|       **参数 `self`**       | 无 |第一个参数 `self` 自动绑定为调用该方法的对象|无|
|       **参数 `cls`**       |           第一个参数 `cls` 自动绑定为调用该方法的类           |无|无|
|**类调用**|例如 `MyCls.cls_meth()`|（非绑定方法）由类调用时需要手动传入参数给 `self`，例如 `MyCls.obj_meth(my_obj)`|例如 `MyCls.stc_meth()`|
|**对象调用**|（不推荐）由对象调用时不需要手动传入参数，例如 `my_obj.cls_method()`|         （绑定方法）例如 `my_obj.obj_method()`          |例如 `my_obj.stc_meth()`|

## 7. 描述符

在 Python 中，**描述符（Descriptor）**用于自定义对象在查找、存储和删除时的操作。本质上，描述符是一个类，定义了另一个类中属性的访问方式，可以实现对于另一个类的属性管理。描述符是复杂属性访问的基础，在内部被用于实现函数装饰器 `@property`，方法和超类 `super`。

描述符基于以下三种特殊方法，统称为**描述符协议（Descriptor Protocol）**：

- **`descr.__set__(self, obj, value) -> None`**：用于设置属性
- **`descr.__get__(self, obj, type=None) -> value`**：用于读取属性
- **`descr.__delete__(self, obj) -> None`**：用于删除属性

根据实现的描述符协议的不同，可进一步划分为：

- **非数据描述符（Non-data Descriptor）**：仅定义了方法 `__get__()`
- **数据描述符（Data Descriptor）**：定义了方法 `__set__()` 或方法 `__delete__()`

下例展示了描述符的使用方法：

```python
class MyDescr:
    def __init__(self, value):
        self.descr_val = value
    def __get__(self, instance, owner):
        print("calling __get__ from descriptor")
        return self.descr_val
    def __set__(self, instance, value):
        print("calling __set__ from descriptor")
        print('changing value from "{}" to "{}"'.format(self.descr_val, value))
        self.descr_val = value
    def __delete__(self, instance):
        print("calling __delete__ from descriptor")
        print("deleting value")
        del self.descr_val

class MyCls:
    cls_attr = MyDescr("original class attribute value")

my_obj = MyCls()
print(my_obj.cls_attr)  # 调用方法 __get__

my_obj.cls_attr = "new class attribute value"  # 调用方法 __set__
print(my_obj.cls_attr)  # 调用方法 __get__

del my_obj.cls_attr  # 调用方法 __delete__
```

```python
calling __get__ from descriptor
original class attribute value
calling __set__ from descriptor
changing value from "original class attribute value" to "new class attribute value"
calling __get__ from descriptor
new class attribute value
calling __delete__ from descriptor
deleting value
```

## 8. 方法 `property()`

在上一章节中，通过 `my_obj.cls_attr` 调用类属性的方法破坏了类的封装原则。因此，在不破坏类封装原则的基础上，本章节将通过一种定义实例方法的方式，间接实现对类属性的访问和修改。

在 Python 中，函数 **`property(fget=None, fset=None, fdel=None, doc=None)`** 通过 `my_obj.obj_meth` 的形式，实现了对类属性的访问、修改和删除。参数详解如下：

- **`fget`**：访问指定类属性的类方法
- **`fset`**：修改指定类属性的类方法
- **`fdel`**：删除指定类属性的类方法
- **`doc`**：文档字符串，用于说明此函数的作用

以上四个参数的指定必须遵循以下规则：仅指定第一个；或仅指定前两个；或仅指定前三个；或全部指定。在函数 ` property(fget=None, fset=None, fdel=None, doc=None)` 中，如果只传入了参数 `fget`，则该属性只可读，不可写，不可删除；如果只传入了参数 `fget` 和 `fset`，则该属性可读，可写，不可删除；如果传入了参数 `fget`、 `fset` 和 `fdel`，则该属性可读，可写，可删除。

下例展示了对上一章节中描述符方法的优化：

```python
class MyCls:
    def __init__(self, obj_val):
        self.__obj_attr = obj_val  #设置 obj_attr 为私有属性
    def set_obj_attr(self, new_obj_val):  # 修改实例属性
        print("calling set_obj_attr")
        print("changing from {} to {}".format(self.__obj_attr, new_obj_val))
        self.__obj_attr = new_obj_val
    def get_obj_attr(self):  # 访问实例属性
        print("calling get_obj_attr")
        return self.__obj_attr
    def del_obj_attr(self):  # 删除实例属性
        print("calling del_obj_attr")
        print("deleting obj_attr")
        del self.__obj_attr
    obj_attr = property(fget=get_obj_attr, fset=set_obj_attr, fdel=del_obj_attr)

my_obj = MyCls("original object attribute")
print(my_obj.obj_attr)

my_obj.obj_attr = "new object attribute"
print(my_obj.obj_attr)

del my_obj.obj_attr
```

```python
calling get_obj_attr
original object attribute
calling set_obj_attr
changing from original object attribute to new object attribute
calling get_obj_attr
new object attribute
calling del_obj_attr
deleting obj_attr
```

在方法 `get_obj_attr` 中，如果返回值为 `self.obj_attr`，Python 会自动再一次调用方法 `get_obj_attr`，形成死循环。在初始化时，将实例属性 `obj_attr` 改为私有 `__obj_attr` 可以避免此类情况的发生。

## 9. 函数装饰器 `@property`






## 参考

- [9. Classes - Python documentation](https://docs.python.org/3/tutorial/classes.html)

- [Python类和对象 - C语言中文网](http://c.biancheng.net/python/class_object/)

