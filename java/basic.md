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
> 2.如果桶为空表示当前位置可以写入数据，利用 CAS 尝试写入，失败则自旋保证成功。</br>
> 3.如果当前桶的 hashcode == MOVED == -1,说明此时整个 Map 正在进行扩容迁移，当前线程会调用 helpTransfer() 方法协助扩容，然后再继续执行 put</br>
> 4.如果桶既不为空,也没有在扩容都，则用 synchronized 加锁,之后判断是链表或者树插入数据(和HashMap一样)。</br>
> 5.插入完成后，如果桶内链表长度达到 8，且数组总长度达到 64，则将链表转化为红黑树，以提高查询效率。</br>
> 6.最后调用 addCount() 方法更新元素个数。如果达到扩容阈值，则触发扩容流程。</br>
