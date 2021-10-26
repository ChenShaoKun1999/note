## 数据结构

## ADT (abstract data type)

### 表(list)

 简单实现：数组
 缺点：保证够用就会浪费空间；插入、删除操作费时

 链表(linked list)实现：
 一些细节：表头(header)常留一个哑节点(dummy node)

 使用例： 桶式排序：范围从1到M的N个整数，建一个长M的表，按顺序记下每个对应值的count，再从左到右扫描数组打印出来。时间O(M+N)，空间O(M)
 基数排序(radix sort)：已知取值范围[0, M]的N个整数，取B(buckets)个桶，或称基底，每个桶的容量为N（即一个B*N二维数组）。从低位到高位分别用桶式排序
  派生结构
 双链表(double linked list) 两个方向都能走的表
 循环链表
 复杂链表    网状等复杂结构
         

### 栈(stack)

 所有的数据存取都发生在栈的顶(top)
 基本方法包括push（插入），pop（取出），top（看栈顶的数据），它们均花费常数时间
 链表实现：header -> top ->others
 缺点：malloc和free要占用较多时间；可以用第二个栈放本应该被free掉的节点、当需要节点时再从那边拿来避免

 数组实现：设置一个变量topOfStack记录栈顶位置（注：避免使用全局变量）
 缺点：要保证下标不越界（capacity）
 示例：
 struct Stack { int capacity; int topOfStack; Type *Array}
 用malloc申请stack和array

 用途示例：
 符号匹配 再文档中寻找每个"[", "("等有没有匹配的"]", ")"
 后缀表达式 计算带优先级的复杂表达式
 尾递归
         

### 队列(queue)

 从队尾(rear)一侧插入(enqueue)，队头(front)一侧删除(dequeue)

 链表实现：略
 循环数组实现：略
         

### 树(tree)

 当每个父节点的子节点数可能很多时，可以让每个节点包含内容、FirstChild、NextSibling

 先序遍历：先处理父节点，再处理子节点。如，
 int preorder_traversing( node ) {
      preorder_process(father);
      for each child in father
      travering(child);
 }

 后序遍历：反之。例：
 int postorder_traversing( node) {
      for each child in father
           postorder_traverse( child )
      process(father)
 }

​             

#### 二叉树(binary tree)

 每个节点至多有两个子节点

 应用1：表达式树(expression tree)，与普通的中缀表达式（如(a+b)*c）、用栈计算的后缀表达式可以互换
 应用2：二叉查找树，对每个节点X，其左子树中全部节点小于X，右子树大于X（假设没有相同元素）
 查找 略
 插入 先同查找，到没办法继续查（下一步该走的路已经没有节点），就插到该位置
 删除 记被删的节点为D
   若D为树叶（无子节点）则直接删除
   D有一个子节点，delete D and connect its child to its parent
   若有两个子节点，则找到右子树最小节点/左子树最大节点（记为M），用M代替D，并处理M的子树（注意到M至多有一个子节点）
   要删除的次数不多/需要频繁地删除&插入时，可以采用懒惰删除，即不进行删除，而是把D标记为已删除。这么做，通常会让深度增加一个小的常数

 如果要让同一元素重复出现，要给每个元素标记出现次数。此时删除记作出现次数-1

 完全随机的二叉查找树，平均深度=O(logN)，但如果经过大量删除&插入后会明显增加；如果输入非常糟糕，深度也可能很大

​             

#### AVL树(Adelson-Velskii and Landis)

 是比较简单的一种平衡查找树，它要求任何一个节点左右子树高度相差不超过1。实践中，平衡树往往比单纯的二叉树多消耗O(1)的时间，但能防止糟糕的输入

```
 插入
  先按照一般的插入，再通过一次旋转(rotation)恢复平衡
  设对于节点k1，其左子节点的左子树过高（右节点的右子树过高同理），进行一次单旋转
  示意图：
  |     k1               k2                     
  |    /  \             /  \                    
  |   k2   Z           X    k1                  
  |  /  \              X   /  \                 
  | X   Y                 Y    Z                
  | X  若左子节点的右子树过高，或右子节点的左子树过高，则如下图。（实际中B,C只有一个会太高，且哪个高不会影响结果）  |     k1               k3                     
  |    /  \             /  \                    
  |   k2   D          k2    k1                  
  |  /  \            / \    / \                 
  | A   k3          A   B  C   D                
  |    /  \             B  C                    
  |   B    C                                    
  |   B    C                                    
 删除
  实现比较复杂，略去。可以用懒惰删除代替
```


 其余同普通二叉树
         

#### 伸展树

 单次操作的时间界是O(N)，但是进行M次操作，其时间界是O(MlogN)，或称其摊还时间界为O(logN)
 伸展树很难分析，但是在实践中，它的编程实现要比平衡树更简单（因为考虑的情形少，而且不需要存储平衡信息），而且运行得更好
             

### 散列表(hash table)

 以O(1)时间进行插入，删除和查找，不支持排序

 实现：一个长table_size的数组 + 把元素尽可能均匀地映射到[0, table_size-1]的散列函数

 解决冲突(collision)的方法
 1、分离链接法(seperate chaining)
      将散列值相同的元素放到同一个支持查找的数据结构体中，如表、树。这种情况下，散列表的每个元素都是0或指向结构体的指针
      其优点是实现起来简单，不过需要额外的空间分配，这增加了消耗的时间和空间
 2、开放定址法(open addressing hashing)
      Hi(X) = H0(X) + F(i)
      如果H0已经被占用，就不断增加i直至有空位。注意删除操作的编写，以免删掉了中间一个，后面的元素查找不到
      定义装填因子(load factor)λ = 元素个数/散列表大小
           线性探测法：F(i) = i
                显然它能保证找到空单元，但容易形成聚集的单元。λ>0.5时，插入与查找需要的次数显著增加
           平方探测法：F(i) = i2
                当λ<0.5，且表的大小是一个素数时，总能装进去
           双散列
                F(i) = i*H2(X)，H2 ≠ 0
                不要忘记，table_size取素数
                理论上它能避免一次、二次聚集，预期探测次数与完全随机相当，但因为要使用第二个散列函数，效率不一定更高

 再散列(rehashing)
 当表已经快装满了的时候（如，平方探测法装满一半时，插入失败时，λ大于某个规定的数时），键一个两倍大的新表和一个新的散列函数，把原本的每个元素搬过去
 耗时O(N)

 可扩散列
 用硬盘实现散列表。略
         

### 堆(heap)

 可以实现功能：Insert, DeleteMin

 简单的实现：通过表实现，a)保持排序，插入消耗O(N)，删除最小值消耗O(!)，b)不排序，插入消耗O(1)，删除消耗O(N)； 通过树实现，插入/删除消耗O(logN)，但这太麻烦，而且平衡会很快地被破坏

​         

#### 二叉堆(binary heap)

 二叉堆是最普遍的优先队列实现方法，不加说明时堆往往指二叉堆。二叉堆能以平均O(1)，最坏O(logN)的时间插入、平均O(logN)，最坏O(logN)的时间删除

 逻辑结构：二叉堆是一棵完全填满的二叉树，除了底层之外都是填满的，其底层从左到右填满
 数据架构：二叉堆可以用数组实现，按照第一层-第二层从左到右-第三层从左到右-……的方式填入。 第i个节点的左子节点在2i，右子节点在2i+1，父节点在i/2
 堆序性质：使任一个节点比它的所有子节点都要小（或者都要大，取决于删除时删的时最小/最大值）

 Insert
 上滤(percolate up)策略：在末尾创建一个空穴，如果不能插入，就把空穴上浮（即把父节点移动到空穴，空穴自动上浮一层）
 一个细节：取一个很小的元素放在0位置处，可以避免反复检测有没有到头

 DeleteMin
 下滤(percolate down)策略：删除掉最小值之后留下来一个空穴，反复把比较小的子节点提上来（即把空穴下沉），当没办法继续下沉时候把末尾的元素放进来

 IncreaseKey
 增加某个位置的关键字的值。可以用下滤完成，相应的，IncreaseKey可以用上滤

 BuildHeap
 如果逐个插入，平均O(N)，最坏O(NlogN)
 任意顺序插入，对i/2~0的元素（除了最底下一层以外的元素）进行下滤排序（如果i太大，就把它沉下去）


 应用
 堆排序：先用BuildHeap排序，再按顺序删除。O(NlogN)
 队列模拟：略

 推广
 d-堆：每个节点有d个子节点。其优点是深度变浅了，Insert用时变短，但DeleteMin用时变长，而且d!=2^n时不能用数组简单地实现         

#### 左式堆(leftist heap)

 除了支持基本的堆操作之外，还支持合并(merge)操作
             