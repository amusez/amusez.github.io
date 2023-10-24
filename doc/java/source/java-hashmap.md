## java HashMap

#### 特点

1.  根据键的hashCode值存储数据
2.  **最多允许一条键为null的数据**,值不限制
3.  非线程安全(可使用ConcurrentHashMap或者Collections.synchronizedMap方法)

#### 数据结构

​	底层采用数组+链表+红黑树实现

#### 1.8中hash算法 / 寻址算法 优化

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

​	hash的计算为: 高16位与低16位进行异或

​	放到数组中的位置计算方式为: (n-1) & hash

​	取模运算的性能比较差,(n-1) & hash与对n取模的效果是一样的,但是性能会相对高很多

#### HashMap如何解决hash碰撞

​	出现hash冲突时,会在算出来的位置挂一个链表,让多个key-value对同时放在数组的一个位置里,效率为O(n)

​	在链表长度达到了一定的长度时,会转化为红黑树,效率为O(logn)

#### HashMap如何进行扩容

​	默认进行2倍扩容,在扩容时,会rehash
