# Block



## block的分类

~~~
// block的分类(3类)
// * __NSGlobalBlock 全局block(程序的数据区域 .data区)
// * __NSMallocBlock__ 堆block
// * __NSStackBlock__ 栈block

int a = 100;
void (^block)(void) = ^ {         //= 重写(copy) 栈区copy到堆区
  NSLog(@"hello world  --%d", a);
};

NSLog(@"堆block %@", block);

NSLog(@"栈block %@", ^ {
  NSLog(@"hello world");
});
~~~



## Block的循环引用问题















##### 资料地址

[iOS开发之Block底层探索](https://www.bilibili.com/video/av45008532/)

[苹果系统：iOS开发：Block简单使用](https://www.iqiyi.com/w_19rwieo6qx.html)

[]()

[]()

[]()