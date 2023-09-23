---
title: TEST-Pytest
cover: 'https://images.unsplash.com/photo-1692602602346-28f89f471ff1?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxfDB8MXxyYW5kb218MHx8fHx8fHx8MTY5NTA4OTYxOQ&ixlib=rb-4.0.3&q=80&w=1080'
date: 2023-09-17 00:28:13
updated: 2023-09-17 00:28:13
tags: Test
categories: 测试开发
---


pytest 是一个开源的第三方单元测试框架，第一个版本发布于 2009 年。同 unittest 比起来，pytest 功能更多，设计更复杂，上手难度也更高。但 pytest 的最大优势在于，它把 Python 的一些惯用写法与单元测试很好地融合了起来。因此，当你掌握了 pytest 以后，用它写出的测试代码远比用 unittest 写的简洁。

为了更好地展示 pytest 的能力，下面我试着用它来写单元测试。
假设 Python 里的字符串没有提供 upper() 方法，我得自己编写一个函数，来实现将字符串转换为大写的功能。

string_utils.py
```
def string_upper(s: str) -> str:
    """将某个字符串里的所有英文字母由小写转换为大写"""
    chars = []
    for ch in s:
        # 32 是小写字母与大写字母在 ASCII 码表中的距离
        chars.append(chr(ord(ch) - 32))
    return ''.join(chars)
　　为了测试函数的功能，我用 pytest 写了一份单元测试：

　　文件：test_string_utils.py

from string_utils import string_upper

def test_string_upper():
    assert string_upper('foo') == 'FOO'
```    
　　相信你已经发现了，用 pytest 编写的单元测试代码与 unittest 有很大不同。

　　首先，TestCase 类消失了。使用 pytest 时，你不必用一个 TestCase 类来定义测试用例，用一个以 test 开头的普通函数也行。

　　其次，当你要进行断言判断时，不需要调用任何特殊的 assert{X}() 方法，只要写一条原生的断言语句 assert {expression} 就好。

　　正因为这些简化，用 pytest 来编写测试用例变得非常容易。

　　用 pytest 执行上面的测试文件，会输出以下结果：

$ pytest test_string_utils.py
===================== test session starts =====================
platform darwin -- Python 3.8.1, pytest-6.2.2
rootdir: /python_craftman/
collected 1 item

test_string_utils.py .                                   [100%]

====================== 1 passed in 0.01s ======================
　　看上去一切顺利，string_upper() 函数可以通过测试。

　　但话说回来，就测试用例的覆盖率来说，我写的测试代码根本就不合格。因为我的用例只有输入字符全为小写的情况，并没有考虑到其他场景。比如，当输入字符串为空、输入字符串混合了大小写时，我们其实并不知道函数是否能返回正确结果。

　　为了让单元测试覆盖更多场景，最直接的办法是在 test_string_utils.py 里增加测试函数。

　　比如：
```
from string_utils import string_upper

def test_string_upper():
    assert string_upper('foo') == 'FOO'

def test_string_empty(): ➊
    assert string_upper('') == ''

def test_string_mixed_cases():
    assert string_upper('foo BAR') == 'FOO BAR'
```    
❶ 新增两个测试函数

　　虽然像上面这样增加函数很简单，但 pytest 其实为我们提供了更好的工具。

用 parametrize 编写参数化测试

在单元测试领域，有一种常用的编写测试代码的技术：表驱动测试（table-driven testing）。

当你要测试某个函数在接收到不同输入参数的行为时，最直接的做法是像上面那样，直接编写许多不同的测试用例。但这种做法其实并不好，因为它很容易催生出重复的测试代码。

表驱动测试是一种简化单元测试代码的技术。它鼓励你将不同测试用例间的差异点抽象出来，提炼成一张包含多份输入参数、期望结果的数据表，以此驱动测试执行。如果你要增加测试用例，直接往表里增加一份数据就行，不用写任何重复的测试代码。

在 pytest 中实践表驱动测试非常容易。pytest 为我们提供了一个强大的参数测试工具：pytest.mark.parametrize。利用该装饰器，你可以方便地定义表驱动测试用例。

以测试文件 test_string_utils.py 为例，使用 parametrize 后的测试代码
```
import pytest
from string_utils import string_upper

@pytest.mark.parametrize(
    's,expected', ➊
    [
        ('foo', 'FOO'), ➋
        ('', ''),
        ('foo BAR', 'FOO BAR'),
    ],
)
def test_string_upper(s, expected): ➌
    assert string_upper(s) == expected ➍
```    
❶ 用逗号分隔的参数名列表，也可以理解为数据表每一列字段的名称

❷ 数据表的每行数据通过元组定义，元组成员与参数名一一对应

❸ 在测试函数的参数部分，按 parametrize 定义的字段名，增加对应参数

❹ 在测试函数内部，用参数替换静态测试数据

利用 parametrize 改造测试用例后，代码会变精简许多。接着，我们试着运行测试代码：
```
$ pytest test_string_utils.py
================= test session starts =================
platform darwin -- Python 3.8.1, pytest-6.2.2
rootdir: /python_craftman/
collected 1 item

test_string_utils.py ..F                          [100%]

======================= FAILURES =======================
__________ test_string_upper[foo BAR-FOO BAR] __________

s = 'foo BAR', expected = 'FOO BAR'

    @pytest.mark.parametrize(
        's,expected',
        [
            ('foo', 'FOO'),
            ('', ''),
            ('foo BAR', 'FOO BAR'),
        ],
    )
    def test_string_upper(s, expected):
>       assert string_upper(s) == expected
E       assert 'FOO\x00"!2' == 'FOO BAR'
E         - FOO BAR
E         + FOO"!2

test_string_utils.py:25: AssertionError
=============== short test summary info ================
FAILED test_string_utils.py::test_string_upper[foo BAR-FOO BAR]
============= 1 failed, 2 passed in 0.13s ==============
```
哐当！测试出错了。

可以看到，在处理字符串 'foo BAR' 时，string_upper() 并不能给出预期的结果，导致测试失败。

接下来我们尝试修复这个问题。在 string_upper() 函数的循环内部，我可以增加一条过滤逻辑：只有当字符是小写字母时，才将它转换成大写。代码如下所示：
```
def string_upper(s: str) -> str:
    """将某个字符串里的所有英文字母由小写转换为大写"""
    chars = []
    for ch in s:
        if ch >= 'a' and ch <= 'z': ①
            # 32 是小写字母与大写字母在 ASCII 码表中的距离
            chars.append(chr(ord(ch) - 32))
        else:
            chars.append(ch)
    return ''.join(chars)
```    
❶ 新增过滤逻辑，仅处理小写字母

再次执行单元测试：
```
================== test session starts ===================
platform darwin -- Python 3.8.1, pytest-6.2.2
rootdir: /python_craftman/

collected 3 items

test_string_utils.py ...                            [100%]

=================== 3 passed in 0.01s ====================
```
这次，修改后的 string_upper() 函数完美通过了所有的测试用例。

在本节中，我演示了如何使用 @pytest.mark.parametrize 定义参数化测试，避免编写重复的测试代码。下面，我会介绍 pytest 的另一个重要功能：fixture（测试固定件）。

 

使用 @pytest.fixture 创建 fixture 对象

在编写单元测试时，我们常常需要重复用到一些东西。比如，当你测试一个图片操作模块时，可能需要在每个测试用例开始时，重复创建一张临时图片用于测试。

这类被许多单元测试依赖、需要重复使用的对象，常被称为 fixture。在 pytest 框架下，你可以非常方便地用 @pytest.fixture 装饰器创建 fixture 对象。

举个例子，在为某模块编写测试代码时，我需要不断用到一个长度为 32 的随机 token 字符串。为了简化测试代码，我可以创造一个名为 random_token 的 fixture的 conftest.py
```
import pytest
import string
import random

@pytest.fixture
def random_token() -> str:
    """生成随机 token"""
    token_l = []
    char_pool = string.ascii_lowercase + string.digits
    for _ in range(32):
        token_l.append(random.choice(char_pool))
    return ''.join(token_l)
```    
定义完 fixture 后，假如任何一个测试用例需要用到随机 token，不用执行 import，也不用手动调用 random_token() 函数，只要简单调整测试函数的参数列表，增加 random_token 参数即可：

def test_foo(random_token):
    print(random_token)
之后每次执行 test_foo() 时，pytest 都会自动找到名为 random_token 的 fixutre 对象，然后将 fixture 函数的执行结果注入测试方法中。

假如你在 fixture 函数中使用 yield 关键字，把它变成一个生成器函数，那么就能为 fixture 增加额外的清理逻辑。比如，下面的 db_connection 会在作为 fixture 使用时返回一个数据库连接，并在测试结束需要销毁 fixture 前，关闭这个连接：

@pytest.fixture
def db_connection():
    """创建并返回一个数据库连接"""
    conn = create_db_conn() ➊
    yield conn
    conn.close() ➋
❶ yield 前的代码在创建 fixture 前被调用

❷ yield 后的代码在销毁 fixture 前被调用

除了作为函数参数，被主动注入测试方法中以外，pytest 的 fixture 还有另一种触发方式：自动执行。

通过在调用 @pytest.fixture 时传入 autouse=True 参数，你可以创建一个会自动执行的 fixture。举个例子，下面的 prepare_data 就是一个会自动执行的 fixture：

@pytest.fixture(autouse=True)
def prepare_data():
    # 在测试开始前，创建两个用户
    User.objects.create(...)
    User.objects.create(...)
    yield
    # 在测试结束时，销毁所有用户
    User.objects.all().delete()
无论测试函数的参数列表里是否添加了 prepare_data，prepare_data fixture 里的数据准备与销毁逻辑，都会在每个测试方法的开始与结束阶段自动执行。这类自动执行的 fixture，非常适合用来做一些测试准备与事后清理工作。

除了 autouse 以外，fixture 还有一个非常重要的概念：作用域（scope）。

在 pyetst 执行测试时，每当测试用例第一次引用某个 fixture，pytest 就会执行 fixture 函数，将结果提供给测试用例使用，同时将其缓存起来。之后，根据 scope 的不同，这个被缓存的 fixture 结果会在不同的时机被销毁。而再次引用 fixture 会重新执行 fixture 函数获得新的结果，如此周而复始。

pytest 里的 fixture 可以使用五种作用域，它们的区别如下。

(1) function（函数）：默认作用域，结果会在每个测试函数结束后销毁。

(2) class（类）：结果会在执行完类里的所有测试方法后销毁。

(3) module（模块）：结果会在执行完整个模块的所有测试后销毁。

(4) package（包）：结果会在执行完整个包的所有测试后销毁。

(5) session（测试会话）：结果会在测试会话（也就是一次完整的 pytest 执行过程）结束后销毁。

举个例子，假如你把上面 random_token fixture 的 scope 改为 session：

@pytest.fixture(scope='session')
def random_token() -> str:
    ...
那么，无论你在测试代码里引用了多少次 random_token，在一次完整的 pytest 会话里，所有地方拿到的随机 token 都是同一个值。

因为 random_token 的作用域是 session，所以当 random_token 第一次被测试代码引用，创建出第一个随机值以后，这个值会被后续的所有测试用例复用。只有等到整个测试会话结束，random_token 的结果才会被销毁。

总结一下，fixture 是 pytest 最为核心的功能之一。通过定义 fixture，你可以快速创建出一些可复用的测试固定件，并在每个测试的开始和结束阶段自动执行特定的代码逻辑。



### Pytest与Allure的结合
在安装allure之前，先确认电脑已经安装了jdk1.8+

1. 下载allure
allure的官网下载地址：
https://github.com/allure-framework/allure2/releases

如果上边的地址不可以，就用下边的地址：
https://repo.maven.apache.org/maven2/io/qameta/allure/allure-commandline/
选择一个版本（windows下载.zip包就可以）：

下载完直接解压就好了（记住路径）

打开包，打到bin目录，找到allure.bat双击运行

2. 配置allure系统环境变量
【计算机--属性--高级系统设置--环境变量--系统变量--path--编辑】

环境变量添加刚才解压时allure的地址 放bin文件的路径

3.cmd窗口验证环境变量配置是否成功
检验环境变量配置成功：打开终端命令行，输入：allure

4.安装allure-pytest:
pip install allure-pytest

5.运行用例时使用allure生成报告
运行  pytest.main(['--alluredir', 'report/result', 'testdemo.py']) 之后，生成的是json文件

6.查看测试报告
需要在终端运行命令：allure generate ./report/result -o ./report/html --clean 生成html文件
1， 配置文件中，
addopts = -s --alluredir=./report/result--reruns 0

2，进入report上级目录，即点击一下你的项目名，在Terminal中执行命令  

