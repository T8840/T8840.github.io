---
title: Python入门
cover: 'https://source.unsplash.com/random'
tags: Python
categories: 编程语言
abbrlink: d9ef328
date: 2023-02-26 22:02:52
updated: 2023-02-26 22:02:52
---

Python是一种高级、解释型的编程语言，广泛用于Web开发、数据科学、人工智能、自动化等各个领域。下面分别介绍Python的入门和高级语法。
## Python基础语法介绍

### 变量和数据类型
在Python中，可以使用变量来存储值，并且不需要提前声明变量类型。Python支持多种数据类型，包括整数、浮点数、布尔值、字符串、列表、元组、字典等。

以下是一些变量和数据类型的示例：

```python

# 整数类型
x = 5
y = -10

# 浮点数类型
z = 3.14

# 布尔值类型
a = True
b = False

# 字符串类型
c = "Hello, World!"

# 列表类型
d = [1, 2, 3, 4, 5]

# 元组类型
e = (1, 2, 3)

# 字典类型
f = {"name": "Alice", "age": 30}
```
#### 字节字符串
在 Python 中，b"Hello, world!" 是一个字节字符串的表示方式。
在 Python 3 中，字符串有两种类型：文本字符串（str）和字节字符串（bytes）。
文本字符串用于表示文本数据，而字节字符串用于表示原始的字节数据。
b"Hello, world!" 使用 b 前缀来表示一个字节字符串。它表示由 ASCII 字符组成的字节序列，每个字符都用一个字节表示。

字节字符串是在处理二进制数据、网络通信、文件操作等场景中常用的数据类型。通过使用字节字符串，可以确保数据以原始字节的形式进行处理，而不会进行任何编码或字符集转换。

在特定的上下文中，data = b"Hello, world!" 将字符串 "Hello, world!" 转换为字节字符串，并将其赋值给变量 data。这样，变量 data 将持有这个字节字符串，可以在后续的代码中使用。

- 文本字符串与字节字符串的转化
在 Python 中，文本字符串（str）使用 Unicode 编码来表示和处理。Unicode 是一种字符集，它为世界上几乎所有的字符分配了唯一的标识符，包括各种语言的字符、符号、表情符号等。

在计算机内部存储文本字符串时，Python 使用一种称为 UTF-8 的编码方案。UTF-8 是一种可变长度的编码方式，它使用不同长度的字节序列来表示 Unicode 字符，根据字符的不同而变化。

UTF-8 编码的特点是：

ASCII 字符（U+0000 至 U+007F）使用单个字节表示，与 ASCII 编码兼容。
非 ASCII 字符使用多个字节表示，具体的字节数取决于字符的 Unicode 值。
当文本字符串被存储在计算机内存中时，它们以字节序列的形式存在。Python 解释器会根据使用的编码（如 UTF-8）将文本字符串转换为相应的字节序列。

在进行文本字符串的处理和操作时，Python 解释器会根据编码规则进行相应的解码和编码操作。这样，Python 可以正确地处理不同语言的字符，并提供字符串的各种操作和方法。
```
# 定义文本字符串
text = "Hello, world!"

# 将文本字符串转换为字节字符串
byte_string = text.encode("utf-8")
# print(byte_string) # b'Hello, world!'
# 打印字节字符串的二进制表示
# format(byte, "08b")：对于每个字节 byte，使用 format() 函数将其格式化为一个 8 位的二进制字符串。其中，"08b" 表示使用 8 位（补零至 8 位）的二进制表示
binary_representation = " ".join(format(byte, "08b") for byte in byte_string)
print(binary_representation) # 01001000 01100101 01101100 01101100 01101111 00101100 00100000 01110111 01101111 01110010 01101100 01100100 00100001
```

### 运算符
Python支持多种运算符，包括算术运算符、比较运算符、逻辑运算符、位运算符等。以下是一些运算符的示例：

```python

# 算术运算符
x = 5 + 3
y = 5 - 3
z = 5 * 3
a = 5 / 3
b = 5 % 3
c = 5 ** 3

# 比较运算符
d = 5 == 3
e = 5 != 3
f = 5 > 3
g = 5 < 3
h = 5 >= 3
i = 5 <= 3

# 逻辑运算符
j = True and False
k = True or False
l = not True

# 位运算符
m = 5 & 3
n = 5 | 3
o = 5 ^ 3
p = ~5
q = 5 << 3
r = 5 >> 3
```
### 控制流语句
Python支持多种控制流语句，包括if语句、for循环、while循环等。以下是一些控制流语句的示例：

```python

# if语句
x = 5
if x > 0:
    print("x is positive")
elif x < 0:
    print("x is negative")
else:
    print("x is zero")

# for循环
y = [1, 2, 3, 4, 5]
for i in y:
    print(i)

# while循环
z = 1
while z <= 10:
    print(z)
    z += 1
```    
### 函数
在Python中，可以使用def语句定义函数。函数可以接受参数并返回值。以下是一个函数的示例：

```python

def add(x, y):
    return x + y

result = add(5, 3)
print(result)  # 输出8
```
### 模块和包
Python中的模块是一个包含Python定义和语句的文件，可以通过import语句导入。Python中的包是一个包含多个模块的目录。

以下是一个模块和包的示例：

```python

# 模块
# mymodule.py
def greeting(name):
    print("Hello, " + name)

# 使用模块
import mymodule
mymodule.greeting("Alice")  # 输出Hello, Alice

# 包
# mypackage/mymodule.py
def add(x, y):
    return x + y

# 使用包
from mypackage import mymodule
result = mymodule.add(5, 3)
print(result)  # 输出8
```

## Python高级语法介绍
### 迭代器和生成器
Python中的迭代器是一个可以迭代访问的对象，例如列表、元组、字典等。Python中的生成器是一种更加高效的迭代器，使用yield语句生成值。

以下是一个迭代器和生成器的示例：

```python

# 迭代器
my_list = [1, 2, 3, 4, 5]
my_iterator = iter(my_list)

print(next(my_iterator))  # 输出1
print(next(my_iterator))  # 输出2
print(next(my_iterator))  # 输出3

# 生成器
def my_generator():
    yield 1
    yield 2
    yield 3

for i in my_generator():
    print(i)  # 输出1, 2, 3
```    
### 装饰器
Python中的装饰器是一种用于修改函数或类的行为的语法。装饰器本质上是一个函数，可以在不修改原函数或类的情况下修改其行为。

以下是一个装饰器的示例：

```python

# 装饰器
def my_decorator(func):
    def wrapper():
        print("Before the function is called.")
        func()
        print("After the function is called.")
    return wrapper

@my_decorator
def my_function():
    print("Hello, World!")

my_function()  # 输出Before the function is called. Hello, World! After the function is called.
```
### 异常处理
在Python中，可以使用try语句和except语句来处理异常。当程序出现异常时，Python会跳转到最近的except语句，并执行其中的代码。

以下是一个异常处理的示例：

```python

# 异常处理
try:
    x = 5 / 0
except ZeroDivisionError:
    print("Error: division by zero")
```    
### 多线程和多进程
在Python中，可以使用多线程和多进程来实现并发编程。多线程是指在一个程序中同时执行多个线程，多进程是指在一个程序中同时运行多个进程。

以下是一个多线程和多进程的示例：
```python
# 多线程
import threading

def print_numbers():
    for i in range(10):
        print(i)

def print_letters():
    for letter in "abcdefghij":
        print(letter)

thread1 = threading.Thread(target=print_numbers)
thread2 = threading.Thread(target=print_letters)

thread1.start()
thread2.start()

# 多进程
import multiprocessing

def square(number):
    return number ** 2

if __name__ == '__main__':
    pool = multiprocessing.Pool(processes=4)
    results = pool.map(square, [1, 2, 3, 4, 5])
    print(results)  # 输出[1, 4, 9, 16, 25]
```    
### 类和面向对象编程
Python是一种面向对象编程语言，支持类和对象的概念。可以使用class语句来定义类，使用对象来访问类中的属性和方法。

以下是一个类和面向对象编程的示例：

```python

# 类和面向对象编程
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self):
        print("Hello, my name is " + self.name + " and I am " + str(self.age) + " years old.")

person1 = Person("Alice", 25)
person1.greet()  # 输出Hello, my name is Alice and I am 25 years old.
```
### Lambda函数
Lambda函数是一种匿名函数，可以使用lambda关键字定义。Lambda函数通常用于简单的操作，例如对列表进行排序或筛选。

以下是一个Lambda函数的示例：

```python

# Lambda函数
numbers = [1, 2, 3, 4, 5]

squared_numbers = list(map(lambda x: x ** 2, numbers))
print(squared_numbers)  # 输出[1, 4, 9, 16, 25]

even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)  # 输出[2, 4]
```

### 生成式
Python中的生成式是一种用于生成列表、集合或字典的简便语法。生成式通常比使用循环来生成相同的结果更加简洁和可读。

以下是一个生成式的示例：

```python

# 生成式

numbers = [1, 2, 3, 4, 5]

squared_numbers = [x ** 2 for x in numbers]
print(squared_numbers)  # 输出[1, 4, 9, 16, 25]

even_numbers = [x for x in numbers if x % 2 == 0]
print(even_numbers)  # 输出[2, 4]
```


## 对象
Python对象分为几种：
1. 类对象（定义）
2. 实例对象


## deepcopy 

在Python中，深拷贝（deep copy）和浅拷贝（shallow copy）是用于复制对象的两种不同方式。

浅拷贝（shallow copy）是创建一个新的对象，该对象与原始对象共享内部对象的引用。换句话说，它只复制了对象的引用，而不是创建对象的副本。当对其中一个对象进行修改时，另一个对象也会受到影响。

深拷贝（deep copy）是创建一个全新的对象，并递归复制原始对象及其内部所有对象。它创建了原始对象及其所有内部对象的独立副本。因此，对其中一个对象进行修改不会影响另一个对象。

在Python中，可以使用以下方式实现深拷贝和浅拷贝：
```
import copy

# shallow copy
new_list = copy.copy(original_list)  # 浅拷贝列表
new_dict = copy.copy(original_dict)  # 浅拷贝字典
new_set = copy.copy(original_set)    # 浅拷贝集合

# deep copy
new_list = copy.deepcopy(original_list)  # 深拷贝列表
new_dict = copy.deepcopy(original_dict)  # 深拷贝字典
new_set = copy.deepcopy(original_set)    # 深拷贝集合
```

需要注意的是，深拷贝和浅拷贝的适用场景取决于您的需求。如果对象内部没有可变对象（如列表、字典等），并且您希望对其中一个对象的修改不会影响到另一个对象，那么浅拷贝就足够了。但如果对象内部包含了可变对象，并且您希望对其中一个对象的修改不影响到另一个对象，那么深拷贝是更安全和可靠的选择。