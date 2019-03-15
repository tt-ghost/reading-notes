## python学习（进阶）
### 一、函数式编程
- 1、map() ，map方法对list中的每个元素都执行f方法，并返回新list，原list不变
```python
def f(x):
    return x*x
map(f,[1,2,3]) #  [1,4,9]
```
- 2、reduce() ，与map类似，一个函数f，一个list，不同的是f必须接受2个参数，反复调用f，可接受第三个参数，为初始值
```python
def f(x):
    return x*x
map(f,[1,2,3,4]) #  24，既((1*2)*3)*4
```
- 3、filter()接受一个函数f和一个list，f的作用是对每个元素进行判断，返回True或False，返回符合条件元素组成的新list
```python
def is_not_empty(s)
    return s add len(s.strip()) > 0
filter(is_not_empty,['test', None, '', 'str', ' ', 'END’]) #  [’test’,’str’.’END’]
# s.strip(rm) 删除s字符串中开头、结尾处的rm序列的字符，为空时默认删除空白符（’\n’,’\r’,’\t’,’'）
```
- 4、sorted()，接受一个比较函数f，传两个参数(x,y)，如果x在y前面返回-1，否则返回1，相等返回0
```python
def rcmp(x,y):
    if x>y:
        return -1
    if x<y:
        return 1
    return 0
sorted([36,5,12,9,21],rcmp) #  [36,21,12,9,5]
#  也可对字符串排序，更具ASCII比大小
``
- 5、返回函数
```python
def f():
    print ‘a’
    def g():
        print ‘gig'
    return g
# 可以执行，f()()才真的执行
```
- 6、闭包
内层函数引用了外层函数的变量，然后返回内层函数的情况，称为闭包
```python
def count():
    fs = []
    for i in range(1,4):
        def f():
            return i*i
        fs.append(f)
    return fs
# 接受返回
f1,f2,f3 = count() # 结果都为9
```
- 7、匿名函数
```python
map(lambda x: x*x,[1,2,3]) #  [1,4,9]，lambda为匿名函数的意思
m = lambda x:-x if x<0 else x #  返回函数的时候，也可以返回匿名函数
m(-1) #  1
```
- 8、无参数decorator，接受一个函数作为参数，返回新函数
```python
# 编写一个@performance，可以打印函数调用的时间
import time

def performance(f):
    def fn(*args,**kw):
        t1 = time.time()
        r = f(*args,**kw)
        t2 = time.time()
        print 'call %s() in %s' %(f.__name__,(t2-t1))
        return r
    return fn

@performance
def factorial(n):
    return reduce(lambda x,y: x*y, range(1, n+1))
print factorial(10)
```
- 9、完善decorator
  - 1）@decorator可以动态实现函数功能的增加，但经改造后与原函数相比，却有些不同
```python  
#  例1
def f1(x):
    pass
print f1.__name__ # f1
#  例2
def log(f):
    def wrapper(*args,**kw):
        print ‘call…'
        return f(*args,**kw)
    return wrapper
@log
def f2(x):
    pass
print f2.__name # wrapper
```
  - 2）python内置functools可以用来自动化完成内部属性的复制
```python  
# 例
import functools
def log(f):
    @functools.wrap(f)
    def wrapper(*args,**kw):
        print ‘call…'
        return f(*args,**kw)
    return wrapper
# 因为把原函数签名改成了(*args,**kw)，将无法获取原函数的原始参数信息
```
- 10、偏函数（参数过多时进行减少）
```python  
# 例int() 把字符串转整数，默认按十进制
int(’234’) #  234
int(‘123’,base=8) #  83
# functools.partial可以把一个参数多的函数变成一个参数少的新函数，需指定默认
import functools # 需要导入functors模块
int2 = functools.partial(int,base=2)# 重新定义新函数
int2(‘100101’) # 37
```

### 二、模块和包的概念
- 1、模块是xx.py的文件，包是文件夹名（文件夹中必须有一个__init__.py的文件），其中包可以嵌套
```python  
#  a/b/c.py中有个test()的方法，调用为a.b.c.test();
```
- 2、导入模块
```python  
# 常规导入
import math,logging #  导入math和logging模块
# 只导入math模块下某些函数
from math import pow, sin, log
#  如果不同模块之间方法重复，不会冲突
import math, logging
print math.log(10) # 调用math下log
print logging.log(10,’sass') # 调用logging下log
#  如果通过from … import 导入的方法就会冲突，可以起别名解决
from math import log
from logging import log as logger # 给logging模块下的log重新起名logger，之后通过logger()调用
# 调用系统相关模块
# import os
# inport os.path
# from os import path
from os.path import ids, isfile
print isdir(r’/data’) # True，判断参数给的是否为目录
print isfile(r’./test.sh')
```
- 3、动态导入模块，不存在模块会报ImportError
```python  
try:  # 尝试从cStringIO中导入，捕捉到错误时从StringIO中导入
    from cStringIO import StringIO
except ImportError:
    from StringIO import StringIO
    ```
- 4、使用__future__，python的新版本与老版本不兼容时，会将新特性导入到老版本的__future__模块中
```python  
10/3  #老版本结果为整数3，新版本为3.33333333333
10# 3 #新版本中为整数3，老版本不支持
#  导入__future__模块
from __future__ import division
print 10/3 #3.3333333333
# isinstance(’sss’,unicode) #判断第一个变量是否第二个参数的实例
```
- 5、python包安装工具，推荐pip
```python  
pip install web.py #安装pip模块，在python中即可直接导入 import web
```

### 三、面向对象编程
- 1、类的创建
```python  
class Test: #  或者 class Test(object)，表示Test类是从object类继承下来的
    def __init__(self,name):
        self.name = name
        ...
test = Test() # 实例化一个对象
test.name = ‘xiaoming’ #给实例添加属性
```
- 2、初始化实例属性（__init__()方法）
```python  
class Person(object):
    def __init__(self,name,age): #  第一个参数必须为self（习惯用法，为实例的引用），后面参数与定义函数一样
        self.name=name
        self.age=age
xiaohong = Person(‘xiaohong’,23)
## 初始化时接受任意关键字参数，并把它们都做作为属性赋值给实例（关键字定义用**kw）
class Person(object):
    def __init__(self,name,age,**kw)
        self.name = name
        self.age = age
        for k,v in kw.iteritems): ##取值
            setattr(self, k, v) ##设置属性
xiaowang = Person(‘xiaowang’,23,job=’Student')
```
- 3、访问权限，以双下划线开头的属性不能被外部访问（__），类内部可以访问，但（__xxx__）这样的属性可以被外部访问，属于特殊属性，但下划线的属性可被外界访问，按照习惯，不应该被外部访问
- 4、创建类属性
实例属性每个实例各自拥有，互相独立，而类属性有且只有一份
```python  
class Person(object):
    address = ‘Earth’
    def __init__(self,name):
        …
print Person.address # Earth，类可以直接访问类属性
p1 = Person(‘Jim’)
p2 = Person(’Tom’)
print p1.address #  Earth
Person.address = ‘Moon’
print p1.address #  Moon，因为python是动态语言，类属性只有一份
p2.address=’Sun’
print Person.address #  Moon
print p2.address #  Sun
#当实例属性与类属性重名时，实例属性优先级高（在实例上修改类属性，实际是给实例绑定一个实例属性，而类属性不受影响）
```
- 5、定义实例方法
```python  
# 一个实例的私有属性是以__开头，实例方法就是在类中定义的函数，第一个参数永远是self
class Person(object):
    def __init__(self,name):
        self.__name = name
    def get_name(self):
        return self.__name
p1 = Person(’Tom’)
print p1.get_name() #Tom
```
- 6、给实例绑定方法（方法也是属性），利用types.MethodType()，函数调用不需要传入self，而类的方法必须传入self
```python  
import types #需要导入types模块
def fn_get_grade(self):
    …
class Person(object):
    def __init__(self,name):
        self.name = name
p1 = Person(’Tom’)
p1.get_grade = types.MethodType(fn_get_grade, p1, Person) # 将方法绑定到实例属性上
p2 = Person(“Kat”)
print p2.get_grade() #会报错，没有get_grade()方法
```
- 7、定义类方法
```python  
# 在class中定义的全部是实例方法，实例方法第一参数self是实例本身
# 在class中定义的方法需要加 @classmethod 标记，第一个参数传如类本身，通常为cls，类方法无法获得任何实例的变量，只能获得类的引用
class Person(object):
    count = 0
    @classmethod #方法前引用
    def how_many(cls):
        return cls.count
    def __init__(self,name):
        self.name = name
        Person.count = Person.count+1
```

### 四、继承
- 1、利用super().__init__()
```python  
# 定义父类
class Person(object):
    def __init__(name):
        self.name = name
#定义子类
class Student(Person):
    def __init__(self,name,score):
        super(Student,self).__init__(name)
        self.score = score
# super(Student,self)通过当前类返回父类，然后__init__(name)初始化，init必须加name参数，否则之类没有name属性
# isinstance() 判断一个实例（第一个参数）是否是一个类（第二个参数）的实例
```
- 2、多态
```python  
# open()可打开磁盘文件，返回File对象，此对象有个read()方法
import json
f = open(‘/path/file.json’, ‘r’)  # 以只读方式打开
print json.load(f) #  f不一定为File对象，只要f对象有read()方法都可以使用 json.open()
```
- 3、多重继承
```python  
class A(object):
    pass
class B(A):
    pass
class C(A):
    pass
class D(B,C):
    pass
# 这样D拥有了ABC的全部功能，多重继承通过super()调用__init__()方法时，A被继承两次，但__init__()只调用一次
```
- 4、获取对象信息
```python  
# isinstance() 获取是否是对象实例
# type()获取变量类型，返回type对象
# dir()获取变量的所有属性，去掉__xxx__这类属性，可以配合使用filter()函数
# getattr(a,b) 获取a对象的b属性值
# setattr(a,b,c) 设置a对象b属性值为c
# getattr(s,’age’,20) 获取s对象的age属性，如果没有返回默认值20
```

### 五、定制类
- 1、特殊方法
```python  
# __str__() 把一个实例变成str，print时自动调用对象的这个方法
# __repr__() 同上，显示给开发者看的，不用print打印对象时默认调用此方法
class Person(object):
    def __init__(self,name,age):
        self.name = name
        self.age = age
    def __str__(self):
        retrurn ‘(Person: %s ,%s)’ %(self.name,self.age)
p = Person(’Tom’,23)
print p #(Person:Tom,23)
p # <main.Person object at 0x10c941890>
#想让直接调用p对象也是__str__()，可在Person类中申明变量
__repr__ = __str__
```
- 2、__cmp__
```python  
# 对int、str等内置数据类型排序时，sorted()按照默认的比较函数cmp排序，但一组Student类必须用我们自己的特殊方法__cmp__()
```
- 3、__len__
```python  
# 如果一个类表现的像一个list，要获取多少个元素就用len()，然而类需要添加一个__len__()方法
class Students(object):
    def __init__(self,*args):
        self.names = args
    def __len__(self):
        return len(self.names)
ss = Student(‘a’,’b’,’c’)
print len(ss) # 3
#斐波那契函数
class Fib(object):
    def __init__(self,num):
        a,b,L = 0,1,[]
        for n in range(num):
            L.append(a)
            a,b = b,a+b
        self.numbers = L
    def __str__(self):
        return str(self.numbers)
    __repr__ = __str__
def __len__(self):
    return len(self.numbers)
f = Fib(5)
print f # [0,1,1,2,3,5,8,13,21,34]
print len(f) # 10
```
- 4、数学运算
```python  
# 四则运算除int和float外，还有有理数、矩阵等，表示有理数用Rational类表示
# 内置运算 __add__() 加；__sub__() 减；__mul__() 乘；__div__() 除；
```
- 5、类型转换
```python  
# Rational类实现int及float运算
int(12.34) # 12
float(12) # 12.0
# 对于Rational类需要实现内部的__int__()及__float__()方法
class Rational(object):
    def __init__(self,p,q):
        self.p=p
        self.q=q
    def __int__(self):
        return self.p #  self.q
print int(Rational(7,2)) #3
```
- 6、@property装饰器封装方法（如get/set）成属性调用
```python  
class Student(object):
    def __init__(self,score):
        self.name = name
        self.__score = score
    @property
    def score(self):
        return self.__score
    @score.setter
    def score(self,score):
        if score<0 or score>100:
            raise ValueError(‘invalid score’)
        self.__score = score
# 第一个score(self)是get方法，用@property装饰，第二个score(score,self)是set方法，用@score.setter装饰
```
- 7、__slots__ #限制类添加属性，可以节约内存，Python是动态语言，任何实例在运行期间都可以动态添加属性
```python  
class Students(object):
    __slots__ = (’name’,’score’)
    def __init__(self,name,score):
        self.name = name
        self.score = score
s = Student(’Tom’,34)
s.name = ’Tim’ # OK
s.grade = ‘A’ # 报错
```
- 8、__call__() # Python中函数也是对象，实例可以变成一个可调用对象，需要特殊方法__call__()
```python  
class Person(object):
    def __init__(self,name,gender):
        self.name = name
        self.gender = gender
    def __call__(self,friend):
        print ‘My name is %s’ % self.name
        print ‘My friend is %s ‘ % friend
p. Person(’Tom’,’male’)
p(’Tim’)
# My name is Bob
# My friend is Tim
```
