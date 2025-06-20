---
title: hello world
date: 2025-06-20 16:40:33
tags:
  - test
excerpt: 这是一个打招呼笔记。快速打招呼。 
---
基于hexo博客框架，使用NexT Gemini主题。打造独特书写体验。

放一些整理得比较系统的笔记上来。在此之前，让我测试一下渲染功能吧。

# 文本格式支持
Words in regular format.
*斜体字*。
**还有加粗的标记。**
***很少用到的加粗斜体。***
**还有加粗的标记。**
*斜体字*。
Words in regular format.

# 代码格式支持

## java格式

在下面定义一个`java`类：

```java
public class HelloWorld {
    /**
     * 这是程序的入口点，main 方法。
     * 当你运行这个程序时，JVM (Java 虚拟机) 会从这里开始执行。
     * @param args 命令行参数
     */
    public static void main(String[] args) {
        // 使用 System.out.println() 方法将文本打印到控制台
        // println 会在打印后自动换行
        System.out.println("Hello, World!");
    }
}
```

## 其它格式

* python
```python
# 使用内置的 print() 函数来向控制台输出文本
print("Hello, World!")
```

* cpp
```cpp
// 引入输入输出流库，cout 就在这里定义
#include <iostream>

// main 函数是 C++ 程序的入口点
int main() {
    // std::cout 用于向标准输出（通常是控制台）发送数据
    // << 是流插入运算符
    // std::endl 用于输出一个换行符并刷新输出缓冲区
    std::cout << "Hello, World!" << std::endl;
    
    // 返回 0 表示程序成功执行完毕
    return 0;
}
```

* javascript
```js
// 使用 console.log() 方法将信息打印到控制台
console.log("Hello, World!");
```


# 图片和链接支持

现在来测试图片。确保使用的是`post_asset_folder`，这样会节约从obsidian迁移文档到`_post`文件夹中的时间。可以参考这个[链接](https://chrismroberts.com/2020/01/06/using-markdown-in-hexo-to-add-images/)。

下面是一张图片，推上找的。因为今天下雨了。

![](hello-world/1.png)

# 结束 

嗯，好了。可以慢慢往上面搬东西。评论和目录功能也良好。i18n和l10n以后再做吧。







