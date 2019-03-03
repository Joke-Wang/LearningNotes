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

为什么声明block是用copy？

~~~objective-c
	给一个block进行赋值操作时（=），实际上就是做了一个copy的动作,并创建一个堆空间来存储block（所以实际上block属性用strong也没问题，因为在ARC下他会自动把block丢到堆区）
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

//单独使用__weak解决循环引用，在某种特定环境下是错误的。当block中有延时操作，延时过程中block的持有者被释放时，block内部访问的变量被释放变为nil。
~~~

方案二：

~~~objective-c
//在block外部，使用__block修饰block内部使用的参数，使用完成后释放对象。
//__block 所起到的作用就是只要观察到该变量被 block 所持有，就将“外部变量”在栈中的内存地址放到了堆中。进而在block内部也可以修改外部变量的值。
//__block的本质是将对象copy到堆区
~~~

方案三：

~~~objective-c
//将block需要使用的参数作为block的参数传入block
~~~



## block底层原理

~~~objective-c
 	  clang  rewrite函数可以把OC  C 还原出底层代

      block 底层（语言C C++）实际是结构体struct
     
      block 可以自动捕获 外部成员变量
     
      没有__block修饰的外部变量A，是以值传递的方式传递到block，block内部定义一个变量去接受外部变量的值
     在block修改变量A，编译错误，因为 外部变量是存储在栈里  赋值之后的block存储在堆中，不能跨域操作
     
     有__block修饰外部变量B： __block的作用， 获取外部变量B地址，进行指针传递的方式传给block， block内部创建一个指针变量与外部变量B指向同一片内存空间，所以 有__block修饰外部变量B  可以在block内部修改
~~~



##### 视屏资料资料地址

[iOS开发之Block底层探索](https://www.bilibili.com/video/av45008532/)

[苹果系统：iOS开发：Block简单使用](https://www.iqiyi.com/w_19rwieo6qx.html)



##### 相关参考资料

[计算机中栈区，堆区，静态存储区，文字常量存储区，代码区的详细解释](https://blog.csdn.net/dotneterbj/article/details/8021200)

[iOS中__block 关键字的底层实现原理](https://www.cnblogs.com/fengmin/p/5524369.html)

