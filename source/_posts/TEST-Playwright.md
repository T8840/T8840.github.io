---
title: TEST-Playwright
cover: 'https://images.unsplash.com/photo-1692317023059-499bf304ef55?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxfDB8MXxyYW5kb218MHx8fHx8fHx8MTY5NTA4OTYzMg&ixlib=rb-4.0.3&q=80&w=1080'
date: 2023-09-17 10:49:10
updated: 2023-09-17 10:49:10
tags: Test
categories: 测试开发
---


## Install

- pip install method
```
pip install playwright
playwright install
```

## Usage

### Element Locate and Operation

1) Element Locate

- Find Single Element：querySelector(engine=body)
- Find Multi Elements：querySelectorAll(engine=body)
- Find Single Element and Auto Wait：waitForSelector(engine=body)

2) Compare Locate Selector
1. By的8种定位方式，实际为4种
id、name、tag name、class name（java和pythona将该4种都归为CSS）
xpath、link text、partial link text、css selector
2. W3C标准规定的webDriver协议为5种定位方式
CSS、Link text、Partial link text、Tag name、XPath
3. Playwright将选择器汇总为3种
CSS、XPATH（支持逻辑表达式和函数）、TEXT
The Playwright Selector Rule:
- CSS：ID选择器、类选择器、元素选择器、属性选择器、通配选择器、层次选择器。
- XPath：XML路径语言，通过“路径标识符”，导航XML文档的，在类XML种（HTML）也可以使用。
- Text： 结构化内容（html，xml，json）使用模糊匹配（忽略大小写，忽略前后空格，搜索子字符串）及精确匹配、非结构化内容使用正则匹配

3) Common Element Operation

下拉选择框：selectOpion、value、label、index
文件上传：setInputFiles、单个文件、多个文件、拖放上传
鼠标点击：click、dbclick
鼠标拖动：down、up
鼠标移动：move
触摸屏幕：tag
键盘按键：press
截屏、录屏：screenshot、recordVideo


### Mock API



## Doc

