---
title: PYTHON-Cpython
cover: ''
date: 2023-09-18 04:21:18
updated: 2023-09-18 04:21:18
tags:
categories:
---

## PyObject

底层任何类型创建都会使用到PyObject
PyObject是由2部分组成：引用计数（ob_refcnt)和一个指针(ob_type)


## Python/C API

Python###_From@@@
C的类型创建Python类型：如PythonFloat_FromDouble


## 对象创建
分配内存的过程。
PyObject的创建对象过程很有意思：
__new__与__init创建的机制原理可以在 tp_new和tp_init中找到答案：https://www.cnblogs.com/traditional/p/13410961.html
```
static PyObject *
type_call(PyTypeObject *type, PyObject *args, PyObject *kwds)
{	
    //当我们创建一个类的实例对象的时候，会去调用元类的__call__方法，所以是这里的tp_call
    //比如Girl("古明地觉", 16) 等价于 type.__call__(Girl, "古明地觉", 16)
    //所以走到了这一步
    
    //调用__new__方法， 拿到其返回值
    obj = type->tp_new(type, args, kwds);

    if (type->tp_init != NULL) {
        //调用__init__，将返回值obj传递给__init__中的self，并在里面设置属性
        int res = type->tp_init(obj, args, kwds);
    return obj;
}
```

### 对象的行为



### 引用计数

print(sys.getrefcount(int)) 是一行 Python 代码，用于获取整数类型对象的引用计数


### 垃圾回收