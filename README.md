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

6. 假设可以不考虑计算机运行资源（如内存）的限制，以下 Python3 代码的预期运行结果是：

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
   【A】
   Hello World!
   __name__ value: print_func
   Done!
   【B】
   Hello World!
   __name__ value: print_module
   Done!
   【C】
   Hello World!
    __name__ value: __main__
   Done!
   【D】
   Hello World!
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

16. 对于下面的 Python3 函数，如果输入的参数 `n` 非常大，函数的返回值会趋近于以下哪一个值（选项中的值用 Python 表达式来表示）：

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

18. 在 Python3 中，以下字符串操作结果为：

    ```python
    strs = 'I like python'
    one = strs.find('a')
    print(one)
    two = strs.index('a')
    print(two)
    ```

    ```python
    【A】None, 报错
    【C】报错，报错
    【D】-1， None
    【E】-1， 报错
    ```

    正确答案：D

    ```python
    S.find(sub[, start[, end]]) -> int
    返回 S[start:end] 中出现 sub 的最小下标。S 中没有 sub 时返回 -1。
    
    S.index(sub[, start[, end]]) -> int
    返回 S[start:end] 中出现 sub 的最小下标。S 中没有 sub 时返回 ValueError
    ```

19. 在Python3中，下列程序结果为：

    ```python
    dict1 = {'one': 1, 'two': 2, 'three': 3}
    dict2 = {'one': 4, 'tmp': 5}
    dict1.update(dict2)
    print(dict1)
    ```
    ```python
    【A】{'one': 1, 'two': 2, 'three': 3, 'tmp': 5}
	【B】{'one': 4, 'two': 2, 'three': 3}
	【C】{'one': 1, 'two': 2, 'three': 3}
	【D】{'one': 4, 'two': 2, 'three': 3, 'tmp': 5}
    ```
    
    正确答案：D
    
    ```python
    D.update([E, ]**F) -> None
    根据 dict/iterable 类型的 E 和 F 更新 D。 
    
    当 E 拥有 .keys() 方法时：
    for k in E:
    	D[k] = E[k]
    
    当 E 缺少 .keys() 方法时：
    for k, v in D:
    	D[k] = v
    
    根据 K 对 D 进行更新：
    for k in F:
    	D[k] = F[k]
    ```

20. 在 Python3 中执行下列选项的程序，不会抛出异常的：

    ```python
    【A】
    b = 1
    def fn():
        nonlocal b
        b = b + 1
        print(b)
    fn()
    【B】
    tup = (('onion', 'apple'), ('tomato', 'pear'))
    for _, fruit in tup:
    	print(fruit)
    【C】
    a = [b for b in range(10) if b % 2 == 0]
    print(b)
    【D】
    lis = [1, 2, 'a', [1, 2]]
    set(lis)
    ```

21. 根据以下程序，下列选项中，说法正确的是：

    ```python
    class Vector:
    	__slots__='x', 'y'
        def __init__(self):
            pass
        
    class Vector3d(Vector):
        __slots__='x', 'z'
        def __init__(self):
            pass
    
    vector = Vector()
    vector3d = Vector3d()
    ```

    ```python
    【A】若子类没有定义 __slots__ 属性，则子类可以继承父类的 __slots__ 属性
    【B】Vector 类的实例对象 vector 会自动获得实例属性 x 和 y
    【C】Vector3d 类的实例对象 vector3d 最多只能允许属性 x 和 z
    【D】Vector3d 类的实例对象 vector3d 最多只能允许属性 x、y 和 z
    ```

    正确答案：D

    ```
    __slots__ 属性用于限制实例对象的属性，实例对象的属性被限制在 __slost__ 的范围内。
    如果子类没有定义 __slots__ 属性，则不会继承父类的 __slot__ 属性；
    如果子类定义了 __slots__ 属性，则子类对象允许的属性包括了子类的 __slots 和父类的 __slot__。
    
    参考：https://www.nowcoder.com/test/question/done?tid=60012666&qid=2225430#summary
    ```

22. 执行下列程序，输出结果为：

    ```python
    def fun(a, *, b):
        print(b)
        
    fun(1, 2, 3, 4)
    ```

    ```python
    【A】[2,3,4]
    【B】[3,4] 
    【C】报错 
    【D】4
    ```

    正确答案：C

    ```python
    当 * 单独作为参数时，表示它后面的参数必须使用关键字参数进行匹配。
    函数的参数顺序：位置参数 -> 默认参数 -> 位置不定长参数 -> 关键字不定长参数
    
    例如：
    def foo(a, b, c=1, d=2, *e, **f):
    	for x in [a, b, c, d, e, f]:
    		print(x)
    
    foo(1, 2, 3, 4, 5, 6, m=7, n=8)
    
    此时的参数赋值情况为：
    a = 1，b = 2，c = 3，d = 4， e = (5, 6)，f = {'m': 7, 'n': 8}
    ```

23. 在 Python3 中，有关字符串的运算结果为：

    ```python
    strs = 'I like python and java'
    one = strs.find('n')
    print(one)
    two = strs.rfind('n')
    print(two)
    ```

    ```python
    【A】12, 12
    【B】15, 15
    【C】12, 15
    【D】None, None
    ```

    正确答案：C

    ```python
    S.find(sub[, start[, end]]) -> int
    find 方法用于返回 S 中出现 sub 的最小下标。当 S 中没有 sub 时，返回 -1。
    
    S.rfind(sub[, start[, end]]) -> int
    rfind 方法用于返回 S 中出现 sub 的最大下标。当 S 中没有 sub 时，返回 -1。
    ```


24. 在 Python3 中，下列程序运行结果说明正确的是：

    ```python
    strs = 'abcd12efg'
    print(strs.upper().title())
    ```

    ```python
    【A】'ABCD12EFG'
    【B】'Abc12efg'
    【C】语法错误
    【D】'Abcd12Efg'
    ```

    正确答案：D

    ```
    str.title() 用于将 str 中所有单词的首字母转换成大写，每个单词首字母后的其他字母保持不变。
    例如：
    "abc def".title()  -> 'Abc Def'
    "h(e.l_Lo w_o)rld.".title()  ->  'H(E.L_Lo W_O)Rld.'
    ```

25. 执行以下程序，输出结果为：

    ```python
    def fun(a=(), b=[]):
        a += (1, )
        b.append(1)
        return a, b
    
    fun()
    print(fun())
    ```

    ```python
    【A】((1, ), [1, 1])
    【B】((1, 1), [1, 1])
    【C】((1, ), [1])
    【D】((1, 1), [1])
    ```

    正确答案：A

    ```
    在 Python 中，默认参数只在函数定义时被复制一次，即默认参数被保存在固定的内存地址中，而不会在每次调用时创建新的引用。
    如果默认参数的类型为不可变对象，则之后的调用会创建新的引用。
    如果默认参数的类型为可变对象，则之后的函数调用都会改变该可变对象。
    ```

26. 当使用 `import` 导入模块时，按 Python 查找模块的不同顺序可划分为以下几种：

    ①环境变量中的 `PYTHONPATH`

    ②内建模块

    ③`Python` 安装路径

    ④当前路径，即执行 `Python` 脚本文件所在的路径

    其中查找顺序正确的一组是：

    ```
    【A】① ④ ② ③
    【B】② ① ④ ③
    【C】② ④ ① ③
    【D】① ② ③ ④
    ```

    正确答案：C

    ```python
    1）导入内建模块。例如：abs(), print(), max(), min() 等函数。
    2）按照 sys.path 中的顺序进行寻找。sys.path 返回一个列表，包括了以下五个部分：
    	2.1）程序的根目录，即当前 Python 文件的目录
    	2.2）环境变量 PYHTONPATH
    	2.3）标准库（例如：os，sys和random）的目录
    	2.4）.pth 文件中的内容（.pth 文件用于存储 Python 的查找路径）
    	2.5）第三方扩展的 site-package/dist-package 目录
    
    例如，以下为 sys.path 的返回内容：
    ['', '/usr/lib/python310.zip', '/usr/lib/python3.10', '/usr/lib/python3.10/lib-dynload', '/usr/local/lib/python3.10/dist-packages', '/usr/lib/python3/dist-packages']
    其中，
    ''：程序的根目录
    '/usr/lib/python310.zip', '/usr/lib/python3.10', '/usr/lib/python3.10/lib-dynload'：标准库的目录
    '/usr/local/lib/python3.10/dist-packages', '/usr/lib/python3/dist-packages'：第三方扩展的 dist-package 目录
    
    参考：https://blog.csdn.net/qq_27825451/article/details/100552739
    ```


27. 当一个嵌套函数在其外部区域引用了一个值时，该嵌套函数就是一个闭包，以下代码输出值为：

    ```python
    def adder(x):
        def wrapper(y):
            return x + y
        return wrapper
    adder5 = adder(5)
    print(adder5(adder5(6)))
    ```

    ```python
    【A】10
    【B】12
    【C】14
    【D】16
    ```
    正确答案：D
    
    ```
    Python 函数闭包（Closure Function）：
    	1）函数嵌套，即在函数内部（外函数）定义了一个函数（内函数）
    	2）内函数引用了外函数的变量
    	3）内函数是外函数的返回值
    
    函数闭包在调用后，内部的值会被保存。本例中，在执行函数 adder(5) 后，函数 wrapper 中 变量 x 的值固定为 5。两次调用函数 adder5，等效于计算 6 + 5 + 5。
    
    何时使用函数闭包：
    Closures can avoid the use of global values and provides some form of data hiding. It can also provide an object oriented solution to the problem.
    When there are few methods (one method in most cases) to be implemented in a class, closures can provide an alternate and more elegant solution. But when the number of attributes and methods get larger, it's better to implement a class.
    
    查看函数闭包内保存的值：
    All function objects have a __closure__ attribute that returns a tuple of cell objects if it is a closure function. The cell object has the attribute cell_contents which stores the closed value.
    
    参考：https://www.programiz.com/python-programming/closure
    ```

28. 下列关于 Python `socket` 操作叙述正确的是：

    ```
    【A】使用 recvfrom() 接收 TCP 数据
    【B】使用 getsockname() 获取连接套接字的远程地址
    【C】使用 connect() 初始化 TCP 服务器连接
    【D】服务端使用 listen() 开始 TCP 监听
    ```

    正确答案：C、D

    ```python
    sk.recvfrom(bufsize[.flag]) 返回值是 (data, address)。其中 data 是包含接收数据的字符串，address 是发送数据的套接字地址；
    sk.getsockname() 返回套接字自己的地址。通常是一个元组 (ipaddr, port)。
    ```


29. 下面关于 `return` 说法正确的是：

    ```python
    【A】Python 函数中必须有 return
    【B】return 可以返回多个值
    【C】return 没有返回值时，函数自动返回 Null
    【D】执行到 return 时，程序将停止函数内 return 后面的语句
    ```

    正确答案：D

    ```
    在 Python 中，通过逗号分割的值被视为元组。元组的括号为可选项，只有在语法需要时才必要。
    通过函数返回多个值时，可以采用逗号分割的形式返回一个元组。
    
    参考：https://note.nkmk.me/en/python-function-return-multiple-values/
    ```

30. 根据以下程序，下列选项中，说法正确的是：

    ```python
    class Foo():
        def __init__(self):
            pass
        def __getitem__(self,pos):
            return range(0, 30, 10)[pos]
    
    foo = Foo()
    ```

    ```python
    【A】foo 对象表现得像个序列
    【B】可以使用 len(foo) 来查看对象 foo 的元素个数
    【C】可以使用 for i in foo: print(i) 来遍历 foo 的元素
    【D】不能使用 foo[0] 来访问对象 foo 的第一个元素
    ```

    正确答案：C

    ```
    【A】序列必须满足两个方法：__len__ 和 __getitem__。
    【B】类 Foo 没有定义方法 __len__，因此无法查看元素数量。
    【C】对象迭代需要调用方法 __iter__。如果没有定义该方法，Python 则调用方法 __getitem__，让迭代和运算符 in 可用。
    【D】通过索引访问元素对象时，调用方法 __getitem__。
    
    参考：https://www.nowcoder.com/test/question/done?tid=60470599&qid=2225432#summary
    https://www.programiz.com/python-programming/iterator
    ```

    
