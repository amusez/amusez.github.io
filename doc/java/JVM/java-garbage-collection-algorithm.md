## JVM垃圾回收算法

1.  分代收集理论
    1.  弱分代假说: 绝大多数对象都是朝生夕灭的
    2.  强分代解说: 熬过越多次垃圾收集过程的对象就越难以消亡
    3.  跨代引用假说: 跨代引用相对于同代引用来说仅占极少数
2.  标记-清除算法
    1.  标记所有需要回收的对象,在标记完成后,统一回收所有被标记的对象
    2.  标记存活的对象,统一回收所有未被标记的对象
    3.  缺点
        1.  **执行效率不稳定**,如果java堆中包含大量对象且大部分是需要回收的,必须进行大量标记和清除的工作,导致标记和清除两个过程的执行效率都随对象数量增长而降低
        2.  **内存空间碎片化问题**,标记-清除后会产生大量不连续的内存碎片,可能导致当需要分配较大对象时无法找到足够的连续内存而提前触发另一次垃圾回收动作
3.  标记-复制算法
    1.  将可用内存按容量划分为大小相等的两块,每次只使用其中的一块,当这一块的内存用完了,就将还存活的对象复制到另一块上,然后将已使用过的内存空间一次清理掉
        1.  优点
            1.  对于多数对象都是可回收的情况,算法需要复制的是占少数的存活对象,而且每次都是针对半区进行内存回收,分配内存时也就不用考虑有空间碎片的复杂情况,只要移动堆顶指针,按顺序分配即可,这样实现简单,运行高效
        2.  缺点
            1.  如果内存中多数对象都是存活的,这种算法将会产生大量的内存间复制的开销
            2.  将可用内存缩小为了原来的一半,空间浪费过多
    2.  ​	可分为三个阶段
        1.  标记阶段: 从GC Roots集合开始,标记活跃对象
        2.  转移阶段: 把活跃对象复制到新的内存地址上
        3.  重定位阶段: 因为转移导致对象的地址发送了变化,这重定位阶段,所有指向对象旧地址的指针都要调整到对象的新地址上
4.  标记-整理算法
    1.  标记过程与标记-清除算法一致,之后让所有存活的对象都向内存空间一端移动,然后直接清理掉边界以外的内存
5.  虚拟机可以平时多数时间采用标记-清除算法,直到内存空间的碎片化程度已经大到影响对象分配时,再采用标记-整理算法收集一次