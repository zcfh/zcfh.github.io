---
title: hexo 博客搭建
categories:
 - 配置
 - doc
tag:
 - 配置
---
# 配置
具体配置参考 [Hexo 搭建个人博客](https://blog.csdn.net/yaorongke/article/details/119089190)
偷个懒，就不重新整理了--
## 常用功能
### 开启categories\tag
例如开启 categories
`hexo new page categories`
会生成文件 `source/categories/index.md`
```yml
---
title: categories
date: 2025-09-30 17:44:19
type: "categories"
---
```
**开启tag和categories相同，把上面的步骤中 categories 替换为tag即可。**

使用的使用，在文章前面加下面的配置
```yml
---
title: hexo 博客搭建
categories:
 - 配置
 - doc
tag:
 - 配置
---
```

**tag 和 categories 的差异在于**：categories，填多个后，是按照目录树一样往下切分，tag 有n个，就从n个入口进入。

# 本地启动

```sh
brew install hexo
cd ${doc_root}
npm install
hexo g
hexo server
```
