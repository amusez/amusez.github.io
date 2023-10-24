## java中4种引用类型

1.  强引用
    1.  把一个对象赋给一个引用变量,这个引用变量就是一个强引用.
    2.  当一个对象被强引用变量引用时,它处于可达状态,不可能被垃圾回收机制回收,即使该对象以后不会被用到也不会被回收,因此强引用是造成java内存泄漏的主要原因之一
2.  软引用
    1.  软引用需要用SoftReference类实现
    2.  当系统内存足够时不会被回收,当系统内存空间不足时会被回收,软引用通常用在对内存敏感的程序中
3.  弱引用
    1.  弱引用使用WeakReference类实现
    2.  比软引用的生存期更短,只要垃圾回收机制一运行,不管JVM的内存空间是否足够都会回收该对象占用的内存
4.  虚引用
    1.  虚引用使用PhantomReference类实现
    2.  不能单独使用,必须和引用队列联合使用
    3.  虚引用的主要作用是跟踪对象被垃圾回收的状态