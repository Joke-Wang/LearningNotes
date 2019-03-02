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

循环应用：对象互相引用形成闭环，不能释放的问题。（A持有B，B持有A）

解决方案：打破引用闭环。

具体解决方案：

方案一：

~~~objective-c
//block外使用__weak弱引用，block内部使用__strong强引用
__weak typeof(<#self#>) weak<#Self#> = <#self#>;
__strong typeof(<#self#>) strong<#Self#> = weak<#Self#>;
~~~

方案二：

~~~objective-c
//在block外部，使用__block修饰block内部使用的参数，使用完成后释放对象。
//__block 所起到的作用就是只要观察到该变量被 block 所持有，就将“外部变量”在栈中的内存地址放到了堆中。进而在block内部也可以修改外部变量的值。
//__block的本质是将对象copy到堆区
~~~















##### 视屏资料资料地址

[iOS开发之Block底层探索](https://www.bilibili.com/video/av45008532/)

[苹果系统：iOS开发：Block简单使用](https://www.iqiyi.com/w_19rwieo6qx.html)

[]()



##### 相关参考资料

[计算机中栈区，堆区，静态存储区，文字常量存储区，代码区的详细解释](https://blog.csdn.net/dotneterbj/article/details/8021200)

[]()

[]()