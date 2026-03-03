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
> ##### 为什么ConcurrentHashMap中使用 synchronized 而不是 ReentrantLock？
> 1.减少内存开销：ReentrantLock 需要继承 AbstractQueuedSynchronizer (AQS)，每个对象都需要额外的内存空间。而 synchronized 是原生指令，且锁信息存在对象头中。
> 2.低竞争下的性能：在大多数情况下，不同线程访问同一个桶的概率极低。synchronized 在这种“无竞争”或“低竞争”的状态下，经过 JVM 的偏向锁、轻量级锁优化，性能已经不输甚至超过了 ReentrantLock。

> ## Java 编译与运行机制
>  1.编译期：从 .java(源代码) 到 .class(字节码)</br>
>  2.运行期：从 .class(字节码) 到机器码</br>
>>  A. 类加载 (Class Loading)</br>
>  JVM 的类加载器将 .class 文件加载到内存中，并进行安全校验，确保代码没有被恶意篡改。  </br>
>  B. 解释执行 (Interpreting)</br>
>  在程序启动初期，解释器（Interpreter） 会逐行读取字节码并将其翻译成当前操作系统的机器码。</br>
>  优点：启动快，不需要等待编译。</br>
>  缺点：运行效率低，因为同一段循环代码每次执行都要重新翻译。</br>
>  C. JIT 即时编译 (Just-In-Time Compilation)</br>
>  这是 Java 性能飙升的关键。当 JVM 发现某段代码（如一个方法或循环）运行得非常频繁时，会将其标记为 “热点代码”。</br>
>  JIT 编译器 会直接把这些热点代码编译成底层的 本地机器码，并存入代码缓存（Code Cache）。</br>
>  下次执行这段代码时，CPU 直接运行机器码，速度与 C++ 程序几乎无异。</br>

> ## ClassNotFoundException 和 NoClassDefFoundError 的区别
> ClassNotFoundException 是Exception，发生在使用反射等动态加载时找不到类，是可预期的，可以捕获处理。 </br>
> NoClassDefFoundError 是Error，是编译时存在的类，在运行时链接不到了（比如 jar 包缺失），是环境问题，导致 JVM 无法继续。</br>
