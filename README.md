# Neal's World

## Hexo使用
1. 安装hexo  
npm install -g hexo-cli  
2. 建站
```sh
hexo init <folder>
cd <folder>
npm install
```
3. 导入主题与配置主题
下载的主题item放在themes目录中,修改_config.yml中theme: item

4. 写作
```sh
hexo new "file_name"
```
5. 上传
配置_config.yml中的repo信息
```sh
deploy:
  - type: git
    repo: https://github.com/t8840/t8840.github.io.git
    branch: master
```
执行：hexo deploy

## 本博客设置
1. 背景图片
    - 搜索关键字: data-theme="dark"
2. banner图片
    - 动态图片: people.html:background
    - 轮播图:
3.     

4. admin密码设置
    - password_hash生成
    ```sh
    npm install bcrypt-nodejs
    $ node
    > const bcrypt = require('bcrypt-nodejs')
    > bcrypt.hashSync('your_password')
    ```

## 博客魔改教程
参考来源:
- https://butterfly.js.org/posts/21cfbf15/
- https://butterfly.zhheo.com/
- https://akilar.top/


## 本博客使用的工具列表
### 图片工具
1. favicon生成  
    - https://www.logosc.cn/logo/favicon
2. 背景图片
    - https://wallhaven.cc/
3. 图片压缩尺寸处理
    - https://www.topdodo.com/size    
4. 随机图片API
    - 动漫图片： 
        1. https://api.vvhan.com/api/acgimg
        2. https://www.dmoe.cc/random.php
    - 指定类型图片：
        1. https://source.unsplash.com/1600x900/?nature,water
        2. https://source.unsplash.com/random
        3. https://unsplash.it/1600/900?random
    - 风景
        1. https://api.vvhan.com/api/view

### 图床
1. https://7bu.top/

