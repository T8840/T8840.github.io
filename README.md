### 本博客创建步骤：
#### 1. 本地搭建
快速开始同 VuePress 官网：
1、创建并进入一个新目录  
// 文件名自定义  
mkdir vuepress-starter && cd vuepress-starter  
2、使用你喜欢的包管理器进行初始化  
yarn init # npm init  
将 VuePress 安装为本地依赖  
3、yarn add -D vuepress # npm install -D vuepress  
4、创建你的第一篇文档，VuePress 会以 docs 为文档根目录，所以这个 README.md 相当于主页：  
mkdir docs && echo '# Hello VuePress' > docs/README.md  
5、在 package.json 中添加一些 scripts  
```javascript
{
  "scripts": {
    "docs:dev": "vuepress dev docs",
    "docs:build": "vuepress build docs"
  }
}
```  
6、在本地启动服务器  
yarn docs:dev # npm run docs:dev  

#### 2. 基础配置  
在文档目录下创建一个 .vuepress 目录，所有 VuePress 相关的文件都会被放在这里。此时你的项目结构可能是这样：  
```s
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
#### 3. 添加导航栏  
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
#### 4. 添加侧边栏    
现在我们添加一些 md 文档，目前文档的目录如下：    
```javascript
.
├─ docs
│  ├─ README.md
│  └─ .vuepress
│     └─ config.js
|  └─ go
|  	  └─ BasicLearning.md
|	  └─ InstallMethod.md
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
              path: '/handbook/DesignPhilosophy',
              collapsable: false, // 不折叠
              children: [
                { title: "设计哲学", path: "/handbook/DesignPhilosophy" },
                { title: "安装方法", path: "/handbook/InstallMethod" }
              ],
            }
          ]
    }
}
```  
对应的效果如下：  
