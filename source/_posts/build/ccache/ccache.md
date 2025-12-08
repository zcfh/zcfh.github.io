---
title: ccache 使用总结
categories:
 - build
 - ccache
tag:
 - build
---

# ccache 使用总结

# 模式

- preprocessor mode： 使用预处理文件作为缓存key
- direct mode：直接使用源码和include进行匹配 (默认true)
- depend mode: 基于direct mode，include文件通过-MMD生成，而不是预处理 (默认false)

## direct mode

- 直接模式下，除了通用的hash（编译器、
录等）外，以编译的输入文件和编译选项作为hash。  
- 当hash匹配后（编译参数和cpp文件相同），再根据include的文件hash判断是否命中缓存。include 文件的路径，来自上一次编译，并保存在manifest中的结果。  
- 若没有命中缓存，回退为 preprocessor mode 或  depend mode. 若为 preprocessor mode，头文件路径从预处理文件中读取。

### 注意事项
有一个坑是，对于下面的场景，会有缓存误命中：

```
// a.cc
#include "test.h"

g++ -c -I./path1 -I./path2 a.cc
```
1. 如果t1 时间，只存在./path2/test.h, 此时编译，使用的是./path2/test.h
2. 如果t2 时间，创建了 ./path1/test.h，此时编译，期望是使用 ./path2/test.h。 
3. 但是由于 **编译选项**不变，**a.cc**不变。所以hash没变，在直接模式下，又会发现先前include的文件路径 **path2/test.h** 的内容也没变，会认为命中了缓存。

ccache，额外把-I路径是否存在，也给加入了判断，一定程度可以绕过。


##  depend mode

依赖模式，基于直接模式，但是它不会做预处理，include的文件通过-MMD这类选项直接输出。

好处是：cache miss时，少一次预处理开销
坏处是：缓存命中率降低，比如一些选项变化 -Dxxx，但实际上没影响，也会导致cache miss

# 参考

[ccache](https://ccache.dev/manual/4.12.1.html)


