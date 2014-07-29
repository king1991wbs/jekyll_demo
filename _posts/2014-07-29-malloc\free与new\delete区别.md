---
layout: post
---

本文整理自《王道--程序员求职宝典》
-----------------

相同点：都可用于申请动态内存和释放内存

不同点：

(1) 操作对象不同。

malloc与free是c/c++语言的标准库函数，new/delete是c++的运算符。

由于malloc/free是库函数而不是运算符，不在编译器控制权限之内，所以无法执行构造函数和析构函数。

new的执行过程是：首先，调用名为operator new 的标准库函数，分配足够大的原始的未类型初始化的内存，以保存指定类型的一个对象；接下来，运行该类型的
一个构造函数，用指定初始化式构造对像；最后，返回指向新分配并构造的对象的指针。

delete的执行过程：首先，对sp指指向的对象运行适当的析构函数；然后，通过调用名为operator delete的标准库函数释放该对象所用内存。

以上operator new 与 operator delete分别对应与malloc与free。

(2) 用法上也有所不同。

函数malloc的原型如下：
<pre>
<code>
void *malloc(size_t size);
</code>
</pre>
用malloc申请一块长度为length的整数类型内存，程序如下：
<pre>
<code>
int *p = (int *)malloc(sizeof(int)*length);
</code>
</pre>
1)malloc返回值的类型是<code>void*</code>,所以在调用malloc时要显示的进行类型转换，将<code>void*</code>转换成所需要的指针类型。

2）malloc函数本身并不识别要申请的内存是什么类型，它只关心内存的总字节数。

函数free的原型如下：
<pre>
<code>void free(void *memblock);</code>
</pre>

语句free\(p\)用来释放内存，如果p是NULL指针，那么free对p无论操作多少次都不会出问题。如果p不是NULL指针，那么free对p连续操作两次就会导致程序运行错误。

运算符new使用起来要比函数malloc简单的多，例如：
<pre>
<code>int *p2 = new int[length];</code>
</pre>
这是因为new内置了sizeof、类型转换和类型安全检查功能。对与非内部数据类型的对象而言，new在创建动态对象的同时完成了初始化工作（调用构造函数）。如果
对象有多个构造函数，那么new的语句也可以有多种形式。

如果用new创建对象数组，那么只能使用对象的无参构造函数。例如：
<pre><code>Obj *objects = new Obj[100];//创建100个动态对象</code></pre>
不能写成：
<pre><code>Obj *objects = new Obj[100](1);//创建100个动态对象的同时赋予初值1</code></pre>
在用delete释放对象数组时，留意不要丢了符号“\[ \]”。例如：
<pre>
<code>
delete [] objects;//正确用法
delete objects;//错误用法
</code>
</pre>
不同之处，总结如下：

(1) malloc与free是c/c++语言的标准库函数，new/delete是c++的运算符；

(2) new自动计算所需分配的空间，而malloc需要手工计算字节数；

(3) new是类型安全的，而malloc不是，比如：
<pre>
<code>
int *p = new float[2];//编译时出错
int *p = (int *)malloc(2*sizeof(double));//编译时无法指出错误
</code>
</pre>
(4)new调用operator new 分配足够的空间，并调用相关对象的构造函数，而malloc不能调用构造函数；delete将调用该实例的析构函数，然后调用类的operator 
delete,以释放该实例占用的空间，而free不能调用析构函数；

(5)malloc/free需要库文件支持，new/delete则不需要。
