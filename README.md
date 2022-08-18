# Python 编程语言专项练习

1. Python 变量的查找顺序为：

   ```
   【A】局部作用域>外部嵌套作用域>全局作用域>内置模块作用域
   【B】外部嵌套作用域>局部作用域>全局作用域>内置模块作用域
   【C】内置模块作用域>局部作用域>外部嵌套作用域>全局作用域
   【D】置模块作用域>外部嵌套作用域>局部作用域>全局作用域
   ```

   正确答案：D
   
   ```
   Python 中的四种作用域查找顺序——LEGB原则
   L（Local），即局部作用域，例如在函数中定义的变量；
   E（Enclosing），即外部嵌套作用域，例如上级函数的局部作用域；
   G（Global），即全局作用域，例如模块级别的变量；
   B（Built-in），即内置模块作用域，例如int和print。
   ```

2. 对于 Python 中 `__new__` 和 `__init__` 的区别，下列说法正确的是：

   ```
   【A】__new__是一个静态方法，而__init__是一个实例方法
   【B】__new__方法会返回一个创建的实例，而__init__什么都不返回
   【C】只有在__new__返回一个cls的实例时，后面的__init__才能被调用
   【D】当创建一个新实例时调用__new__，初始化一个实例时用__init__
   ```

   正确答案：A、B、C、D

   ```
   __new__ 为构造函数，属于静态方法。在实例创建之前被调用，在创建实例后返回该实例。
   __init__ 为初始化方法，属于非静态方法。在 __new__ 实例创建之后被调用，对实例进行初始化。
   
   静态方法属于类所有，只能访问类中的静态变量和其他静态方法，静态方法在类实例化前就可以使用。
   非静态方法可以访问类中的任何成员，非静态变量必须在实例化之后才能分配内存。
   静态方法和静态变量在创建后始终占用同一个内存空间，而不同的实例则会占用多个内存空间。
   静态方法效率较高，但不能自动销毁；非静态方法效率较低，可以自行销毁。
   ```

3. 执行以下程序，结果输出为：

   ```python
   a = [1]
   b = 2
   c = 1
   
   def fn(lis, obj):
       lis.append(b)
   	obj = obj + 1
       return lis, obj
   
   fn(a, c)
   print(fn(a, c))
   ```

   ```python
   【A】([1, 2, 2], 2)
   【B】([1, 2, 2], 3)
   【C】([1, 2], 2)
   【D】([1, 2], 3)
   ```

   正确答案：A

   ```
   当函数参数为可变对象时，采用引用传递。函数中对变量的修改会对实参造成影响；
   当函数参数为不可变对象时，采用值传递。函数中对变量的修改不会影响到实参。
   
   Python 中的不可变对象：元组、字符串、数值
   Python 中的可变对象：列表、字典、集合
   
   参考：https://www.jb51.net/article/223840.htm
   ```

4. 执行下列程序，输出结果为：

   ```python
   def fn():
     t = []
     i = 0
     while i < 2:
       t.append(lambda x: print(i*x,end=","))
   	i += 1
       return t
   
   for f in fn():
       f(2)
   ```

   ```python
   【A】4,4,
   【B】2,2,
   【C】0,1,
   【D】0,2,
   ```

   正确答案：A

   ```
   函数 fn 中包含了一个匿名函数，其内部对外部作用域的变量进行了引用，且该匿名函数充当其外部函数 fn 的返回值。此时，该匿名函数形成了一个闭包（closure）。闭包常用于保存当前的运行环境。
   
   在本题中，外部作用域的自由变量 i 被匿名函数引用。在执行时，Python 对闭包采用了延迟绑定策略，即在通过 f(2) 调用匿名函数时，i 保存了 while 循环的退出条件（i == 2），所以 f(2) 返回结果为 2 * 2 和 2 * 2。
   
   参考：https://zhuanlan.zhihu.com/p/22229197
   ```

5. 下面有关计算机基本原理的说法中，正确的一项是：

   ```
   【A】堆栈（stack）在内存中总是由高地址向低地址方向增长的。
   【B】发生函数调用时，函数的参数总是通过压入堆栈（stack）的方式来传递的。
   【C】在 64 位计算机上，Python3 代码中的 int 类型可以表示的最大数值是 2^64-1。
   【D】在任何计算机上，Python3 代码中的 float 类型都没有办法直接表示 [0, 1] 区间内的所有实数。
   ```

   正确答案：D

   ```
   【A】在 Intel 架构中，栈由高地址向低地址增长，堆由低地址向高地址增长；
    		 在 ARM 架构中，可以自由选择增长方向。
   【B】在 x64 系统中，函数调用的前 4 个参数总是存放在寄存器中，剩余参数则压入堆栈中；
   		 在 x86 系统中，函数调用的所有参数都被压入堆栈中，fastcall 方式除外。
   【C】在 32 位机器中，取值范围为 -2^31~2^31-1；
   		 在 64 位机器中，取值范围为 -2^63~2^63-1。
   【D】计算机中的浮点类型通过离散方式近似表示小数，并且区间 [0, 1] 内的实数无限多。
   
   参考：https://www.nowcoder.com/test/question/done?tid=59682202&qid=345275#summary
   ```

6. 假设可以不考虑计算机运行资源（如内存）的限制，以下 python3 代码的预期运行结果是：

   ```python
   import math
   def sieve(size):
       sieve = [True] * size
       sieve[0] = False
       sieve[1] = False
       for i in range(2, int(math.sqrt(size)) + 1):
           k = i * 2
           while k < size:
              sieve[k] = False
              k += i
       return sum(1 for x in sieve if x)
   print(sieve(10000000000))
   ```

   ```python
   【A】455052510
   【B】455052511
   【C】455052512
   【D】455052513
   ```

   正确答案：B

   ```
   题意为求 0~10000000000 以内所有质数的数量。
   
   在初始化时，函数 sieve 假设 [0, 10000000000] 以内的所有整数均为非质数（sieve = [True] * size）。
   循环 range(2, int(math.sqrt(size)) + 1) 用于遍历 10000000000 的所有可能的因数，并通过循环 while k < size 排除所有因素的乘数（sieve[k] = False）。
   
   Tip：寻找某个数 x 的最大公因数时，寻找范围可缩小至 [2, sqrt(x) + 1]。
   ```

7. 在 Python3 中，以下程序结果为：

   ```python
   one = (1, 2, 3)
   two = ('a', 'b')
   print(one + two)
   ```

   ```python
   【A】None
   【B】报错
   【C】(1, 2, 3, 'a', 'b')
   【D】[1, 2, 3, 'a', 'b']
   ```

   正确答案：C

   ```
   元组中，+ 运算符表示连接两个元组并生成一个新的元组。
   ```

8. 已知 `print_func.py` 的代码如下：

   ```python
   print('Hello World!')
   print('__name__value: ', __name__)
   def main():
       print('This message is from main function')
   if __name__ == '__main__':
       main()
   ```

   `print_module.py` 的代码如下：

   ```python
   import print_func
   print("Done!")
   ```

   运行 `print_module.py` 程序，结果是：

   ```python
   【A】Hello World!
   		 __name__ value: print_func
   		 Done!
   【B】Hello World!
   		 __name__ value: print_module
   		 Done!
   【C】Hello World!
   	 	 __name__ value: __main__
   		 Done!
   【D】Hello World!
   		 __name__ value:
   		 Done!
   ```

   正确答案：A

   ```
   __name__ 是一个系统变量，用于表示模块的名字。
   当直接运行该模块时，即作为主线运行程序时，__name__ 被赋值为 __main__；
   当该模块被其他模块导入时，__name__ 被赋值为自己的文件名，例如 print_func。
   ```

9. 执行以下程序，输出结果为：

   ```python
   def outer(fn):
       print('outer')
       def inner():
           print('inner')
           return fn
       return inner
   
   @outer
   def fun():
       print('fun')
   ```

   ```python
   【A】outer
   【B】inner
   【C】fun
   【D】因为没有调用任何函数，所以没有输出结果
   ```

   正确答案：A

   ```
   函数 fun 之上的 @outer 为装饰器，等同于将函数 fun 作为参数传入了函数 outer，即 outer(fun)。
   函数 outer 返回值为函数 inner 的内存地址，并非调用了该函数（返回值为 inner()）。
   此外，在装饰器对函数完成装饰后，装饰器会立即执行。
   ```

10. 以下程序输出为：

    ```python
    info = {'name':'班长', 'id':100, 'sex':'f', 'address':'北京'}
    age = info.get('age')
    print(age)
    age = info.get('age',18)
    print(age)
    ```

    ```python
    【A】None 18
    【B】None None
    【C】编译错误
    【D】运行错误
    ```

    正确答案：A

    ```
    dict.get(key, default=None)
    当字典中没有匹配的 key 时，返回值默认为空。
    ```

11. 在 Python2 中，以下程序输出为：

    ```python
    print type(1/2)
    ```

    ```python
    【A】<type 'int'>
    【B】<type 'number'>
    【C】<type 'float'>
    【D】<type 'double'>
    【E】<type 'tuple'>
    ```

    正确答案：A

    ```
    在 Python2 中，除法运算默认向下取整，因此 1/2 = 0 的计算结果为整形。
    ```

12. 在 Python3 中，程序语句结果为：

    ```python
    strs = 'abbacabb'
    print(strs.strip('ab'))
    ```

    ```python
    【A】'ab'
    【B】语法错误
    【C】'c'
    【D】'ca'
    ```

    正确答案：C

    ```python
    str.strip(self, chars=None)
    当 chars 为空时，移除首位中的空格；当 chars 不为空时，移除首尾出现的 chars 中的字符。
    ```

13. 执行以下程序，输出结果为：

    ```python
    a = [['1', '2'] for i in range(2)]
    b = [['1', '2']] * 2
    a[0][1] = '3'
    b[0][0] = '4'
    print(a, b) 
    ```

    ```python
    【A】[['1', '3'], ['1', '3']] [['4', '2'], ['4', '2']]
    【B】[['1', '3'], ['1', '2']] [['4', '2'], ['4', '2']]
    【C】[['1', '3'], ['1', '2']] [['4', '2'], ['1', '2']]
    【D】[['1', '3'], ['1', '3']] [['4', '2'], ['1', '2']]
    ```

    正确答案：B

    ```
    在列表 a 中，['1', '2'] 发生了深拷贝，即 a[0] 和 a[1] 分别引用了不同的元素，id(a[0]) 和 id(a[1]) 不同；
    在列表 b 中，['1', '2'] 发生了浅拷贝，即 a[0] 和 a[1] 都引用了相同的元素，id(a[0]) 和 id(a[1]) 相同。
    ```

14. 关于 Python 中的复数，下列说法错误的是：

    ```
    【A】表示复数的语法是 real + image j
    【B】实部和虚部都是浮点数
    【C】虚部必须后缀 j，且必须小写
    【D】方法 conjugate 返回复数的共轭复数
    ```

    正确答案：C

    ```
    在 Python 中，实部和虚部均为 float。
    虚部的后缀可以是 j 或者 J，赋值后 J 会 自动转化为 j。
    复数 a 的共轭复数可以通过 a.conjugate() 来计算。
    ```

15. 执行以下程序，输出结果为：

    ```python
    a = 0 or 1 and True
    print(a)
    ```

    ```python
    【A】0
    【B】1
    【C】False
    【D】True
    ```

    正确答案：D

    ```
    逻辑运算符优先级：not > and > or。本题中，1 and True 返回 True，0 or True 返回 True。
    
    and 使用右侧匹配，从左至右返回最后一个判断为 True 的元素。
    例如：1 and 2 and 3，返回 3；1 and True and 'a'，返回 'a'。
    当判断某一元素为 False 时，直接返回该元素。
    例如：1 and False and 2，返回 False；1 and 0 and 'a'，返回 0。
    
    or 使用短路匹配：从左至右返回第一个判断为 True 的元素。
    例如：0 or 1 or 2，返回 1；'a' or 0 or False，返回 'a'。
    当判断所有元素均为 False 时，返回最后一个元素。
    例如：0 or False or ''，返回 ''；0 or False 返回 False。
    ```

16. 对于下面的 Python3 函数，如果输入的参数 n 非常大，函数的返回值会趋近于以下哪一个值（选项中的值用 Python 表达式来表示）：

    ```python
    import random 
    def foo(n):   
            random.seed()
         c1 = 0
         c2 = 0
         for i in range(n):
            x = random.random()
            y = random.random()
            r1 = x * x + y * y
            r2 = (1 - x) * (1 - x) + (1 - y) * (1 - y)
            if r1 <= 1 and r2 <= 1:
               c1 += 1
             else:
               c2 += 1
        return   c1 / c2
    ```

    ```python
    【A】4 / 3
    【B】(math.pi - 2) / (4 - math.pi)
    【C】math.e ** (6 / 21)
    【D】math.tan(53 / 180 * math.pi)
    ```

    正确答案：A

    ![solution](https://uploadfiles.nowcoder.com/images/20190403/7732482_1554284927216_D91C814D196D43F5FC5B6592A1F9CA95)

    ```
    参考：https://www.nowcoder.com/test/question/done?tid=59837531&qid=345294#summary
    ```

17. 执行以下程序，下列选项中，说法正确的是：

    ```python
    tup = (1 ,2, [3, 4]) # ①
    tup[2]+=[5, 6]  # ②
    ```

    ```python
    【A】执行代码 ② 后，变量 tup[2] 的 id 发生改变
    【B】① 和 ② 均可以执行而不会抛出异常
    【C】执行代码 ② 时会抛出异常，最终 tup 的值为 (1, 2, [3, 4, 5, 6])
    【D】执行代码 ② 时会抛出异常，最终 tup 的值为 (1, 2, [3, 4])
    ```
    正确答案：C
    
    ```
    元组 tup 属于不可变类型， 而元素 tup[2] 为列表，属于可变类型。
    在 += 运算时，[3, 4] 在原地址的起初上先被修改为 [3, 4, [5, 6]，然后再赋值给 tup[2]。对元组赋值操作引发了错误，但此时元组内的元素已经被修改。
    list.extend() 通用在原地修改列表，但没有重新赋值的行为，故不会引发元组的异常。
    ```
