---
title: 一个关于冒号的bug
date: 2023-03-19 21:31:43
tags: bug
---

## 遇到问题

`npx hexo server`遇到报错：

```
ERROR {
  err: YAMLException: can not read a block mapping entry; a multiline key may not be an implicit key at line 4, column 1:

      ^
      at generateError (F:\test\hexoLearn\node_modules\js-yaml\lib\js-yaml\loader.js:167:10)
      at readBlockMapping (F:\test\hexoLearn\node_modules\js-yaml\lib\js-yaml\loader.js:1073:9)
      at composeNode (F:\test\hexoLearn\node_modules\js-yaml\lib\js-yaml\loader.js:1359:12)
      at readDocument (F:\test\hexoLearn\node_modules\js-yaml\lib\js-yaml\loader.js:1525:3)
      at loadDocuments (F:\test\hexoLearn\node_modules\js-yaml\lib\js-yaml\loader.js:1588:5)
      at Object.load (F:\test\hexoLearn\node_modules\js-yaml\lib\js-yaml\loader.js:1614:19)
      at parseYAML (F:\test\hexoLearn\node_modules\hexo-front-matter\lib\front_matter.js:69:23)
      at parse (F:\test\hexoLearn\node_modules\hexo-front-matter\lib\front_matter.js:50:12)
      at F:\test\hexoLearn\node_modules\hexo\lib\plugins\processor\post.js:51:20
      at tryCatcher (F:\test\hexoLearn\node_modules\bluebird\js\release\util.js:16:23)
      at Promise._settlePromiseFromHandler (F:\test\hexoLearn\node_modules\bluebird\js\release\promise.js:544:35)
      at Promise._settlePromise (F:\test\hexoLearn\node_modules\bluebird\js\release\promise.js:604:18)
      at Promise._settlePromise0 (F:\test\hexoLearn\node_modules\bluebird\js\release\promise.js:649:10)
      at Promise._settlePromises (F:\test\hexoLearn\node_modules\bluebird\js\release\promise.js:729:18)
      at Promise._fulfill (F:\test\hexoLearn\node_modules\bluebird\js\release\promise.js:673:18) {
    reason: 'can not read a block mapping entry; a multiline key may not be an implicit key',
    mark: Mark {
      name: null,
      buffer: 'title: 第一篇博客\ndate: 2023-03-19 19:33:41\ntags:博客相关\n\x00',
      position: 49,
      line: 3,
      column: 0
    }
  }
} Process failed: %s _posts/第一篇博客.md
```

使用`hexo clean`也无法解决

## 解决方案

> title: 第一篇博客
> date: 2023-03-19 19:33:41
> tags: 博客相关

这是对应博客的开头，tags冒号后面缺少一个空格，加上就好了，如图：

![bug截图](一个关于冒号的bug/bug_of_colon.png)



## 如何在博客中插入图片

发现在md文档同目录下新建img文件夹再引用没有用，查到了这篇

[ 解决Hexo无法显示图片的几种方案-CSDN博客](https://blog.csdn.net/weixin_42030522/article/details/123075824)

将`post_asset_folder`赋值为true，并将图片放入md文件同名文件夹
