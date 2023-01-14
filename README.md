## 介绍  
本博客采用VuePress，[相关指引](https://vuepress.vuejs.org/zh/guide/)

## 本博客创建步骤：
### 1. 本地搭建
快速开始同 [VuePress官网](https://vuepress.vuejs.org/)  
1-1、创建并进入一个新目录  
// 文件名自定义  
mkdir vuepress-starter && cd vuepress-starter  
1-2、使用你喜欢的包管理器进行初始化  
yarn init # npm init  
将 VuePress 安装为本地依赖  
1-3、yarn add -D vuepress # npm install -D vuepress  
1-4、创建你的第一篇文档，VuePress 会以 docs 为文档根目录，所以这个 README.md 相当于主页：  
mkdir docs && echo '# Hello VuePress' > docs/README.md  
1-5、在 package.json 中添加一些 scripts  
```javascript
{
  "scripts": {
    "docs:dev": "vuepress dev docs",
    "docs:build": "vuepress build docs"
  }
}
```  
1-66、在本地启动服务器  
yarn docs:dev # npm run docs:dev  

### 2. 基础配置  
在文档目录下创建一个 .vuepress 目录，所有 VuePress 相关的文件都会被放在这里。此时你的项目结构可能是这样：  
```sh
.
├─ docs
│  ├─ README.md
│  └─ .vuepress
│     └─ config.js
└─ package.json
```  
在 .vuepress 文件夹下添加 config.js，配置网站的标题和描述，方便 SEO：  
```javascript  
module.exports = {
  title: 'T8840 Blog',
  description: 'T8840的博客'
}
```  
### 3. 添加导航栏  
我们现在在页首的右上角添加导航栏，修改 config.js:  
```javascript
module.exports = {
    title: '...',
    description: '...',
    themeConfig: {
        nav: [
            { text: '首页', link: '/' },
            { 
                text: 'T8840 博客', 
                items: [
                    { text: 'Github', link: 'https://github.com/t8840' },
            }
        ]
    }
}
```  
更多的配置参考 VuePress 导航栏  
### 4. 添加侧边栏    
现在我们添加一些 md 文档，目前文档的目录如下：    
```sh
.
├─ docs
│  ├─ README.md
│  └─ .vuepress
│     └─ config.js
|  └─ go
|      └─basic
|  	    └─ DesignPhilosophy.md
|	      └─ InstallMethod.md
└─ package.json
```  
我们在 config.js 配置如下：  
```javascript
module.exports = {
    themeConfig: {
        nav: [...],
        sidebar: [
            {
                title: '欢迎学习',
                path: '/',
                collapsable: false, // 不折叠
                children: [
                    { title: "学前必读", path: "/" }
                ]
            },
            {
              title: "基础学习",
              path: '/go/basic/DesignPhilosophy',
              collapsable: false, // 不折叠
              children: [
                { title: "设计哲学", path: "/go/basic/DesignPhilosophy" },
                { title: "安装方法", path: "/go/basic/InstallMethod" }
              ],
            }
          ]
    }
}
```  
![对应的效果如下：](/assets/images/step4.png "step4.png")  
### 5. 更换主题  

现在基本的目录和导航功能已经实现，但如果还想要加载 loading、切换动画、模式切换（暗黑模式）、返回顶部、评论等功能呢，为了简化开发成本，我们可以直接使用主题，这里使用的主题是 vuepress-theme-reco：
现在我们安装 vuepress-theme-reco：   
yarn add vuepress-theme-reco  # npm install vuepress-theme-reco --save-dev  
然后在 config.js 里引用该主题：
```javascript
module.exports = {
  // ...
  theme: 'reco'
  // ...
}  
```
刷新一下页面，我们会发现一些细节的改变，比如加载时的 loading 动画、以及支持切换暗黑模式：
![对应的效果如下：](/assets/images/step5.png "step5.png")  
### 6. 部署
#### 6-1. 新建仓库推送部署 
在 Github 上新建一个仓库，这里我取得仓库名为：blog，对应，我们需要在 config.js 添加一个 base 路径配置：
```javascript
module.exports = {
  	// 路径名为 "/<REPO>/"
    base: '/blog/',
  	//...
}
```  
然后我们在项目 vuepress-starter 目录下建立一个脚本文件：deploy.sh，注意修改一下对应的用户名和仓库名：
```sh
#!/usr/bin/env sh

# 确保脚本抛出遇到的错误
set -e

# 生成静态文件
npm run docs:build

# 进入生成的文件夹
cd docs/.vuepress/dist

git init
git add -A
git commit -m 'deploy'

# 如果发布到 https://<USERNAME>.github.io/<REPO>
git push -f git@github.com:t8840/blog.git master:gh-pages

cd -
```  
然后命令行切换到 vuepress-starter 目录下，执行 sh deploy.sh，就会开始构建，然后提交到远程仓库，注意这里提交到了 gh-pages 分支，我们在GitHub上查看下对应仓库分支的代码
我们可以在仓库的 Settings -> Pages 中看到最后的地址：

#### 6-2. 利用 Github pages 托管多个站点 
[参考](https://github.com/zhongxia245/blog/issues/101)
### 7. 优化页面显示  
#### 7-1. 添加文章信息  
但我们也会发现，像条件类型这一篇文章，条件类型（Conditional Types） 竟然出现了两遍，这是因为这个主题自动提取了第一个大标题作为本文的标题，我们可以在每篇文章的 md 文件中添加一些信息修改： 
```javascript 
---
title: Go语言的设计哲学
author: T8840
date: '2023-01-01'
---
``` 

##### 7-2. 设置语言
注意，上图的文章时间，我们写入的格式为 2023-01-01 ，但是显示的是 01/01/2023，这是因为 VuePress 默认的 lang 为 en-US，我们修改一下 config.js：
```javascript
module.exports = {
  // ...
  locales: {
    '/': {
      lang: 'zh-CN'
    }
  },
  // ...
}  
```
可以发现时间换了一种展示方式：
![对应的效果如下：](/assets/images/step7-2.png "step7-2.png")  

##### 7-3. 开启目录结构
在原本的主题里，我们发现每篇文章的目录结构出现在左侧，而 vuepress-theme-reco 将原有的侧边栏的中的多级标题移出，生成子侧边栏，放在了页面的右侧，如果你要全局开启，可在页面 config.js 里设置开启：
```javascript 
module.exports = {
  //...
  themeConfig: {
    subSidebar: 'auto'
  }
  //...
}
```

#### 7-4. 修改主题颜色

VuePress 基于 Vue，所以主题色用的是 Vue 的绿色，然而 TypeScript 的官方色则是蓝色，那如何修改 VuePress 的主题色呢？  
你可以创建一个 .vuepress/styles/palette.styl 文件，文件代码如下：  
$accentColor = #3178c6  
此时可以发现主题颜色变了.
更多的颜色修改参考 VuePress 的 [palette.styl](https://vuepress.vuejs.org/zh/config/#palette-styl)。 

#### 7-5. 自定义修改样式
如果你想自定义修改一些 DOM 元素的样式呢？就比如在暗黑模式下,我们发现用作强调的文字颜色比较暗淡，在暗黑模式下看不清楚，我想改下这个文字的颜色和背景色呢？  
而 VuePress 提供了一种添加额外样式的简便方法。你可以创建一个 .vuepress/styles/index.styl 文件。这是一个 Stylus 文件，但你也可以使用正常的 CSS 语法。

我们在 .vupress 文件夹下创建这个目录，然后创建 index.styl 文件，代码如下：
``` javascript
// 通过检查，查看元素样式声明
.dark .content__default code {
    background-color: rgba(58,58,92,0.7);
    color: #fff;
}
```
此时文字就清晰了很多  
那之前我们提到的隐藏每篇文章的标题、作者、时间呢，其实也是类似的方式：
```javascript 
.page .page-title {
   display: none;
}
```
更多的颜色修改参考 VuePress 的 [index.styl](https://vuepress.vuejs.org/zh/config/#index-styl)。 
