---
title: PYTHON-Class
cover: ''
date: 2023-09-17 01:09:23
updated: 2023-09-17 01:09:23
tags:
categories:
---

## 面向对象

类与实例的数据，都保存在一个名为 __dict__ 的字典属性中
灵活利用 __dict__ 属性，能帮你做到常规做法难以完成的一些事情
使用 @classmethod 可以定义类方法，类方法常用作工厂方法
使用 @staticmethod 可以定义静态方法，静态方法不依赖实例状态，是一种无状态方法
使用 @property 可以定义动态属性对象，该属性对象的获取、设置和删除行为都支持自定义

### @property

无描述符时，实现属性校验功能

在下面的代码里，我实现了一个 Person 类：
```
class Person:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```        
Person 是个特别简单的数据类，没有任何约束，因此人们很容易创建出一些不合理的数据，比如年龄为 1000、年龄不是合法数字的 Person 对象等。为了确保对象数据的合法性，我需要给 Person 的年龄属性加上一些正确性校验。

使用 @property 把 age 定义为 property 对象后，我可以很方便地增加校验逻辑：
```
class Person:
    ...

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        """设置年龄，只允许 0～150 之间的数值"""
        try:
            value = int(value)
        except (TypeError, ValueError):
            raise ValueError('value is not a valid integer!')

        if not (0 < value < 150):
            raise ValueError('value must between 0 and 150!')
        self._age = value
```        
通过在 age 属性的 setter 方法里增加校验，我最终实现了想要的效果：
```
>>> p = Person('piglei', 'invalid_age') ➊
...
ValueError: value is not a valid integer!
>>> p = Person('piglei', '200') ➋
...
ValueError: value must between 0 and 150!

>>> p = Person('piglei', 18) ➌
>>> p.age
18
```

❶ age 值不能转换为整型

❷ age 值不在合法的年龄范围内

❸ age 值符合要求，对象创建成功

粗看上去，上面使用 @property 的方案还挺不错的，但实际上有许多不如人意的地方。

使用属性对象最大的缺点是：很难复用。假如我现在开发了一个长方形类 Rectangle，想对长方形的边长做一些与 Person.age 类似的整型校验，那么我根本无法很好地复用上面的校验逻辑，只能手动为长方形的边长创建多个 @property 对象，然后在每个 setter 方法里做重复工作：
```
class Rectangle:

    @property
    def width(self): ...

    @width.setter
    def width(self): ...

    @property
    def height(self): ...

    @height.setter
    def height(self): ...
```    
如果非得基于 @property 来实现复用，我也可以继续用类装饰器（class decorator）或元类（metaclass）在创建类时介入处理，把普通属性自动替换为 property 对象来达到复用目的。但是，这种方案不但实现起来复杂，使用起来也不方便。

而使用描述符，我们可以更轻松地实现这类需求。不过在用描述符实现字段校验前，我们先了解一下描述符的基本工作原理。


## 面向对象高级特性

Python 使用 MRO 算法来确定多重继承时的方法优先级
super() 函数获取的并不是当前类的父类，而是当前 MRO 链条里的下一个类
Mixin 是一种基于多重继承的有效编程模式，用好 Mixin 需要精心的设计
元类的功能相当强大，但同时也相当复杂，除非开发一些框架类工具，否则你极少需要使用元类
元类有许多更简单的替代品，比如类装饰器、子类化钩子方法等
通过定义 __init_subclass__ 钩子方法，你可以在某个类被继承时执行自定义逻辑



## 抽象类
抽象类提供了一种更灵活的子类化机制，我们可以通过定义抽象类来改变 isinstance() 的行为
通过 @abstractmethod 装饰器，你可以要求抽象类的子类必须实现某个方法

## Mixin 的概念
Mixin 即 Mix-in，常被译为“混入”，是一种编程模式，在 Python 等面向对象语言中，通常它是实现了某种功能单元的类，用于被其他子类继承，将功能组合到子类中。

利用 Python 的多重继承，子类可以继承不同功能的 Mixin 类，按需动态组合使用。

当多个类都实现了同一种功能时，这时应该考虑将该功能抽离成 Mixin 类。

举个例子
定义一个简单的类：
```
class Person:
    def __init__(self, name, gender, age):
        self.name = name
        self.gender = gender
        self.age = age
```        
我们可以通过调用实例属性的方式来访问：
```  
p = Person("小陈", "男", 18)
print(p.name)  # "小陈"
```  
然后我们定义一个 Mixin 类：
```
class MappingMixin:
    def __getitem__(self, key):
        return self.__dict__.get(key)

    def __setitem__(self, key, value):
        return self.__dict__.set(key, value)
```        
这个类可以让子类拥有像 dict 一样调用属性的功能

我们将这个 Mixin 加入到 Person 类中：
```  
class Person(MappingMixin):
    def __init__(self, name, gender, age):
        self.name = name
        self.gender = gender
        self.age = age
```          
现在 Person 拥有另一种调用属性方式了：
```  
p = Person("小陈", "男", 18)
print(p['name'])  # "小陈"
print(p['age'])  # 18
```  
再定义一个 Mixin 类，这个类实现了 __repr__ 方法，能自动将属性与值拼接成字符串：
```  
class ReprMixin:
    def __repr__(self):
        s = self.__class__.__name__ + '('
        for k, v in self.__dict__.items():
            if not k.startswith('_'):
                s += '{}={}, '.format(k, v)
        s = s.rstrip(', ') + ')'  # 将最后一个逗号和空格换成括号
        return s
```          
利用 Python 的特性，一个类可以继承多个父类：
```  
class Person(MappingMixin, ReprMixin):
    def __init__(self, name, gender, age):
        self.name = name
        self.gender = gender
        self.age = age
```          
这样这个子类混入了两种功能：
```  
p = Person("小陈", "男", 18)
print(p['name'])  # "小陈"
print(p)  # Person(name=小陈, gender=男, age=18)
```  
- 总结
Mixin 实质上是利用语言特性，可以把它看作一种特殊的多重继承，所以它并不是 Python 独享，只要支持多重继承或者类似特性的都可以使用，比如 Ruby 中 include 语法，Vue 等前端领域也有 Mixin 的概念。

但 Mixin 终归不属于语言的语法，为了代码的可读性和可维护性，定义和使用 Mixin 类应该遵循几个原则：

Mixin 实现的功能需要是通用的，并且是单一的，比如上例中两个 Mixin 类都适用于大部分子类，每个 Mixin 只实现一种功能，可按需继承。
Mixin 只用于拓展子类的功能，不能影响子类的主要功能，子类也不能依赖 Mixin。比如上例中 Person 继承不同的 Mixin 只是增加了一些功能，并不影响自身的主要功能。如果是依赖关系，则是真正的基类，不应该用 Mixin 命名。
Mixin 类自身不能进行实例化，仅用于被子类继承。

## 元类

### __init_subclass__