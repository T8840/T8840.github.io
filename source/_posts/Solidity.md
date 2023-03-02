---
title: Solidity
cover: ''
tags: Solidity
categories: 编程语言
abbrlink: b9a928cc
date: 2023-02-26 22:05:05
updated: 2023-02-26 22:05:05
---
Solidity是一种基于以太坊区块链的智能合约编程语言，它具有类似于JavaScript和C++的语法和特性。

## Solidity入门语法
### 注释
Solidity支持两种注释方式，单行注释和多行注释。单行注释使用//，多行注释使用/* */。

```solidity

// 这是单行注释

/*
这是多行注释
*/
```
### 变量和数据类型
Solidity支持多种数据类型，包括布尔型、整型、地址、字符串和数组等。

- 布尔型
布尔型表示逻辑值，只有两个可能的值，即true和false。

```solidity

bool a = true;
bool b = false;
```
- 整型
整型表示整数值，可以分为有符号整型和无符号整型。

```solidity

int a = 123;  // 有符号整型
uint b = 456;  // 无符号整型
```
- 地址
地址类型表示以太坊账户的地址，它是一个20字节的值。

```solidity

address a = 0x1234567890123456789012345678901234567890;
```
-  字符串
字符串类型表示文本值。

```solidity

string a = "Hello World!";
```
-  数组
数组表示具有相同类型的值的有序集合。

```solidity

uint[] a = [1, 2, 3];
string[] b = ["Hello", "World"];
```
### 函数
Solidity中的函数可以接受参数和返回值。函数使用function关键字声明，可以指定函数的可见性和修饰符。

```solidity

function add(uint a, uint b) public pure returns (uint) {
    return a + b;
}
```
上面的代码定义了一个名为add的函数，它接受两个无符号整型参数a和b，返回它们的和。public表示该函数可以被外部调用，pure表示该函数不会读取或修改合约状态。

### 控制流
Solidity支持条件语句和循环语句。

- 条件语句
条件语句使用if和else关键字。

```solidity

uint a = 10;

if (a > 5) {
    // 如果a大于5，执行这里的代码
} else {
    // 否则执行这里的代码
}
```
- 循环语句
循环语句使用for和while关键字。

```solidity

for (uint i = 0; i < 10; i++) {
    // 执行10次循环
}

uint i = 10;

while (i > 0) {
    // 执行10次循环
    i--;
}

```
### 结构体
结构体表示一种自定义数据类型，可以包含多个字段。

```solidity

struct Person {
    string name;
    uint age;
}

Person p = Person("Alice", 20);
```
上面的代码定义了一个名为Person的结构体，它包含两个字段：name和age。然后我们创建了一个名为p的Person类型的实例。  
```solidity

pragma solidity ^0.8.0;

contract Example {
    struct Person {
        string name;
        uint age;
        bool isAdult;
    }

    Person public alice;

    constructor() {
        alice.name = "Alice";
        alice.age = 30;
        alice.isAdult = true;
    }

    function setAge(uint age) public {
        alice.age = age;
        if (age >= 18) {
            alice.isAdult = true;
        } else {
            alice.isAdult = false;
        }
    }
}
```
上面的代码定义了一个名为Person的结构体，包含三个字段：name（字符串类型）、age（无符号整数类型）和isAdult（布尔类型）。合约还包含一个名为alice的公共状态变量，其类型为Person，可以用来存储一个人的姓名、年龄和成年状态。合约还定义了一个名为setAge的函数，用于设置alice的年龄，并根据年龄计算其成年状态。

### 映射
映射表示一种将键映射到值的数据结构。

```solidity

mapping(address => uint) balances;

balances[0x1234567890123456789012345678901234567890] = 100;
```
上面的代码定义了一个名为balances的映射，它将地址映射到无符号整型。然后我们将地址0x1234567890123456789012345678901234567890的值设置为100。

```solidity
pragma solidity ^0.8.0;

contract Example {
    mapping(address => uint) public balances;

    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint amount) public {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        payable(msg.sender).transfer(amount);
        balances[msg.sender] -= amount;
    }
}
```
上面的代码定义了一个名为balances的映射，将每个地址映射到一个无符号整数值上，表示该地址的余额。合约还定义了两个函数：
- deposit：允许发送以太币到合约地址，并将其存入发送方的余额中。 
- withdraw：允许发送方从其余额中取出指定数量的以太币。 
在deposit和withdraw函数中，合约使用了msg.sender来引用当前交易的发送方地址，将其余额增加或减少，并使用require函数来确保余额不会变为负数。如果检查失败，函数会抛出一个错误消息，并停止执行。

映射是Solidity中的一项强大功能，可用于创建各种数据结构，如哈希表、关联数组等。在实际开发中，映射通常会更加复杂，并使用更高级的技术来确保其安全性和正确性

### 事件
Solidity中的事件是一种机制，用于在合约执行期间将通知发送给区块链上的其他应用程序。事件通常用于跟踪合约执行的状态变化，如交易成功或失败，合约状态更改等。
```solidity

event Transfer(address indexed from, address indexed to, uint value);

function transfer(address to, uint value) public returns (bool) {
    emit Transfer(msg.sender, to, value);
    return true;
}
```
上面的代码定义了一个名为Transfer的事件，它包含三个参数：from、to和value。然后我们在transfer函数中调用Transfer事件，记录转账操作。

另一个简单的事件示例：

```solidity
pragma solidity ^0.8.0;

contract Example {
    event Transfer(address indexed from, address indexed to, uint amount);

    function transfer(address to, uint amount) public {
        require(amount > 0, "Amount must be greater than zero");
        require(msg.sender.balance >= amount, "Insufficient balance");
        payable(to).transfer(amount);
        emit Transfer(msg.sender, to, amount);
    }
}
```
上面的代码定义了一个名为Transfer的事件，它有三个参数：from（地址类型，表示发送方地址）、to（地址类型，表示接收方地址）和amount（无符号整数类型，表示转账金额）。合约还定义了一个名为transfer的函数，用于从当前发送方地址向指定地址转移指定数量的以太币，并触发Transfer事件以通知其他应用程序。

在调用transfer函数时，合约会检查发送方余额是否足够，如果足够，则转移以太币并触发事件。其他应用程序可以监听Transfer事件，并根据其参数值更新其状态或执行其他操作。

事件是Solidity中的一项强大功能，可用于在合约执行期间与其他应用程序进行通信，如向前端Web应用程序发送通知、向后端服务器发送数据等。在实际开发中，事件通常会更加复杂，并使用更高级的技术来确保其安全性和正确性。


## Solidity高级语法
## 模块化
Solidity支持模块化，即将合约拆分为多个文件，每个文件可以包含一个或多个合约。

导入文件
在Solidity中，可以使用import关键字导入其他文件中定义的合约。

```solidity

import "./MyContract.sol";

contract AnotherContract {
    MyContract c = MyContract(0x1234567890123456789012345678901234567890);
}
```
上面的代码从文件MyContract.sol中导入了名为MyContract的合约，然后我们在AnotherContract合约中创建了一个MyContract类型的实例。

### 继承
Solidity支持合约的继承，即一个合约可以从另一个合约继承其状态变量和函数。

```solidity

contract ParentContract {
    uint public a = 123;

    function foo() public pure returns (uint) {
        return 456;
    }
}

contract ChildContract is ParentContract {
    function bar() public view returns (uint) {
        return a + foo();
    }
}
```
上面的代码定义了两个合约，ParentContract和ChildContract。ChildContract继承了ParentContract的状态变量a和函数foo。

### 抽象合约
在Solidity中，抽象合约是一种不能直接实例化的合约，它只提供了一组接口（函数签名），而没有实现任何功能。抽象合约可以被认为是一个纯虚基类，只定义了一些需要被实现的方法。

抽象合约通常用于定义通用的接口，以便其他合约可以继承它并实现接口中定义的方法。这使得合约更易于模块化，并使代码更加可读和易于维护。

以下是一个简单的抽象合约示例：

```solidity

pragma solidity ^0.8.0;

abstract contract PaymentGateway {
    function deposit(uint amount) public virtual;
    function withdraw(uint amount) public virtual;
}
```
上面的代码定义了一个名为PaymentGateway的抽象合约，它有两个函数接口：deposit和withdraw。这个合约不能被直接实例化，但可以被其他合约继承并实现这些接口。

以下是一个继承了PaymentGateway抽象合约并实现了其接口的示例合约：

```solidity

pragma solidity ^0.8.0;

contract Bank is PaymentGateway {
    mapping(address => uint) private balances;

    function deposit(uint amount) public override {
        require(amount > 0, "Amount must be greater than zero");
        balances[msg.sender] += amount;
    }

    function withdraw(uint amount) public override {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        payable(msg.sender).transfer(amount);
    }

    function getBalance() public view returns (uint) {
        return balances[msg.sender];
    }
}
```
上面的代码定义了一个名为Bank的合约，它继承了PaymentGateway抽象合约，并实现了其中定义的接口。Bank合约还定义了一个名为getBalance的函数，用于返回调用方的余额。

抽象合约是Solidity中的一个重要概念，它使得代码更加模块化和可重用。在实际开发中，可以使用抽象合约来定义通用的接口，并让其他合约继承它并实现这些接口，从而大大简化代码的开发和维护。

### 接口
Solidity支持接口，即仅定义函数签名而不提供实现的抽象合约。接口可以被其他合约实现。

```solidity

interface MyInterface {
    function foo() external returns (uint);
}

contract MyContract is MyInterface {
    function foo() public override returns (uint) {
        return 123;
    }
}
```
上面的代码定义了一个名为MyInterface的接口，它包含一个名为foo的函数。然后我们在MyContract合约中实现了foo函数。

### 消息
Solidity中有一些特殊的变量，它们称为“消息变量”，包含有关当前交易的信息。

msg.sender：当前交易的发送方地址。
msg.value：当前交易中发送的以太币数量。
msg.data：当前交易中传递的数据。
msg.sig：当前交易中函数调用的签名。

### 内联汇编
Solidity支持内联汇编，即在Solidity代码中直接嵌入汇编代码。

```solidity

function getGasPrice() public view returns (uint) {
    uint gasPrice;
    assembly {
        gasPrice := tx.gasprice
    }
    return gasPrice;
}
```
上面的代码使用内联汇编获取当前交易的Gas价格。

### 安全性
Solidity是一门高度安全的编程语言，旨在防止智能合约中的漏洞。以下是Solidity中常用的安全性特性：

访问控制：Solidity提供了modifier关键字，用于在函数执行前检查某个条件是否满足。 
溢出和下溢：Solidity提供了安全的数学库SafeMath，用于执行加、减、乘、除等操作时检查是否存在溢出或下溢。    
资源管理：Solidity中的状态变量和存储变量有不同的访问权限，防止恶意合约访问和修改合约状态。  
防重入攻击：Solidity提供了nonReentrant修饰符，用于防止合约被重复调用，从而避免恶意合约攻击。  
示例合约
以下是一个简单的Solidity合约，它模拟了一个简单的银行账户系统，支持存款、取款和查询余额功能。

```solidity
pragma solidity ^0.8.0;

contract Bank {
    mapping(address => uint) balances;

    event Deposit(address indexed from, uint value);
    event Withdrawal(address indexed to, uint value);

    function deposit() public payable {
        require(msg.value > 0, "Deposit amount must be greater than 0");
        balances[msg.sender] += msg.value;
        emit Deposit(msg.sender, msg.value);
    }

    function withdraw(uint amount) public {
        require(amount > 0, "Withdrawal amount must be greater than 0");
        require(amount <= balances[msg.sender], "Insufficient balance");
        balances[msg.sender] -= amount;
        payable(msg.sender).transfer(amount);
        emit Withdrawal(msg.sender, amount);
    }

    function balanceOf(address account) public view returns (uint) {
        return balances[account];
    }
```

上面的代码定义了一个名为`Bank`的合约，它包含一个名为`balances`的映射，用于存储每个地址的余额。合约还定义了三个函数：

- `deposit`：允许发送以太币到合约地址，并将其存入发送方的余额中。
- `withdraw`：允许发送方从其余额中取出指定数量的以太币。
- `balanceOf`：允许查询指定地址的余额。

合约还使用了事件`Deposit`和`Withdrawal`，用于记录每个存款和取款操作的发起方和数量。

这只是一个简单的示例合约，但它涵盖了Solidity中许多基本概念和语法。在实际开发中，合约通常会更加复杂，需要更高级的语法和技术来确保其安全性和正确性。

