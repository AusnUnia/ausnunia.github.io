> ## 静态代理和动态代理的区别
> <img width="800" height="600" alt="image" src="https://github.com/user-attachments/assets/177a01b4-ab43-4345-9645-54304d0e3211" />


> ## HashMap
> #### 插入操作流程
> 1.根据key计算hash值定位插入数组的位置</br>
> 2.如果定位到的数组位置没有元素 就直接插入。</br>
> 3.如果定位到的数组位置有元素就和要插入的 key 比较，如果 key 相同就直接覆盖</br>
> 4.如果 key 不相同，就判断 p 是否是一个树节点，如果是就调用e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value)将元素添加进入。</br>
> 5.如果不是树节点就遍历链表插入(插入的是链表尾部), jdk1.7及之前是插入头部。</br>

> ## ConcurrentHashMap
> #### 插入操作流程
> 1.根据key计算hash值定位插入数组的位置</br>
> 2.如果为空表示当前位置可以写入数据，利用 CAS 尝试写入，失败则自旋保证成功。</br>
> 3.如果当前位置的 hashcode == MOVED == -1,则需要进行扩容.</br>
> 4.如果都不满足，则利用 synchronized 锁写入数据。</br>
> 5.根如果数量大于 TREEIFY_THRESHOLD 则要执行树化方法，在 treeifyBin 中会首先判断当前数组长度 ≥64 时才会将链表转换为红黑树。
