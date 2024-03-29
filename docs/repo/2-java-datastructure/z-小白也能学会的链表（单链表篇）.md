学习目标：用Java实现的(带头结点的)单链表——构造+CRUD；<br/>文章目录：<br/>[一、头结点/虚拟结点](#fCbCw)<br/>[二、Java实现代码](#qccRF)<br/>[(一) Node的实现特点](#DNUve)<br/>[1. 内部类](#jLWKS)<br/>[2. 为什么Node类被定义为static](#ywrdF)<br/>[3. Node类的实现写法](#n332P)<br/>[(二) static的更多妙用](#kMyxW)<br/>[1. 操作函数都定义为static的原因](#kORbB)<br/>[2. 为什么dummyHead和构造函数不用static修饰？](#ooeD8)<br/>[(三) 函数中先判断空表的原因](#v0CCJ)<br/>[(四) insertNode、deleteNode函数流程（带dummyHead从而统一了操作）](#wBjXH)<br/>[1. 插入结点](#RNj5M)<br/>[2. 删除结点](#gGcj9)
<a name="fCbCw"></a>
## 一、头结点/虚拟结点
这里“虚拟结点 dummy node”的概念其实是大学教材《算法与数据结构》中描述的“附加头结点”，主要目的是为了简化链表操作，使对链表的所有结点的操作更为统一。以及这个结点的data值不会被使用，初始化为0或-1等等都是可以的。不同语境下术语有所差异，主要是理解作用和意思。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/34752664/1698316465548-59f1e1c0-8804-4caa-bda9-207ff819bc94.png#averageHue=%23f3f3f3&clientId=udadf8411-af48-4&from=paste&height=212&id=u45c1ad50&originHeight=424&originWidth=1088&originalType=binary&ratio=2&rotation=0&showTitle=false&size=95735&status=done&style=none&taskId=u84f7e6a1-ded3-4cd4-a930-2f86717a88c&title=&width=544)
<a name="qccRF"></a>
## 二、Java实现代码
```java
/**
 * 表示带有虚拟头节点的基础单链表。
 * 提供插入、删除和获取链表长度的方法。
 */
public class SinglyLinkList {

    /**
     * 链表中的节点表示。
     * 每个节点包含数据和对下一个节点的引用。
     */
    static class Node {
        /** 节点中存储的数据 */
        final int data;
        
        /** 链表中的下一个节点 */
        Node next;

        /**
         * 使用给定的数据构造新节点。
         *
         * @param data 存储在节点中的数据
         */
        public Node(int data) {
            this.data = data;
            this.next = null;
        }
    }

    /** 链表的虚拟头节点 */
    public Node dummyHead;

    /**
     * 使用指定的虚拟头节点构造一个空的链表。
     *
     * @param dummyHead 虚拟头节点
     */
    public SinglyLinkList(Node dummyHead) {
        this.dummyHead = dummyHead;
        this.dummyHead.next = null;
    }

    /**
     * 返回链表的长度。
     *
     * @param dummyHead 链表的虚拟头节点
     * @return 链表的长度
     */
    public static int getLength(Node dummyHead) {
        Node flag = dummyHead;
        int count = 0;
        while (flag.next != null) {
            flag = flag.next;
            count++;
        }
        return count;
    }

    /**
     * 在链表的指定位置插入节点。
     *
     * @param dummyHead  链表的虚拟头节点
     * @param nodeInsert 要插入的节点
     * @param position   节点应插入的位置
     * @return 更新后链表的虚拟头节点
     */
    public static Node insertNode(Node dummyHead, Node nodeInsert, int position) {
        int size = getLength(dummyHead);
        if (position < 1 || position > size+1) {
            System.out.println("位置参数越界");
            return dummyHead;
        }
        Node flag = dummyHead;
        int count = 0;
        while(count < position - 1) {
            flag = flag.next;
            count++;
        }
        nodeInsert.next = flag.next;
        flag.next = nodeInsert;
        return dummyHead;
    }

    /**
     * 从链表的指定位置删除节点。
     *
     * @param dummyHead 链表的虚拟头节点
     * @param position  节点应删除的位置
     * @return 更新后链表的虚拟头节点
     */
    public static Node deleteNode(Node dummyHead, int position) {
        if (dummyHead == null) {
            System.out.println("空表");
            return null;
        }
        int size = getLength(dummyHead);
        if (position < 1 || position > size) {
            System.out.println("位置参数越界");
            return dummyHead;
        }
        Node flag = dummyHead;
        int count = 0;
        while(count < position - 1) {
            flag = flag.next;
            count++;
        }
        flag.next = flag.next.next;
        return dummyHead;
    }
}

```
<a name="DNUve"></a>
### （一）Node的实现特点
<a name="jLWKS"></a>
#### 1. 内部类
将节点定义为内部类是一种组织代码的方式，这样可以将与某个类紧密相关的辅助类隐藏在主类中，使外部代码更加简洁。对于当前链表的情况，Node仅在链表类内部使用，因此将其作为内部类是有意义的。
<a name="ywrdF"></a>
#### 2. 为什么`Node`类被定义为`static`

- **独立性**：当前内部类不依赖于其外部类的实例。换句话说，`static`内部类不持有对其外部类的任何实例的	引用。这对于节点来说是很有意义的，因为每个节点并不需要知道它所属的链表实例。 
- **内存效率**：由于非`static`内部类会持有对其外部类实例的引用，因此它通常会占用更多的内存。对于链表这样的数据结构，可能会包含大量的节点，所以使用`static`内部类有助于减少内存消耗。 
- **可访问性**：如果您希望外部类能够直接访问内部类，即使没有外部类的实例，那么内部类必须是`static`的。在链表的上下文中，您可能希望能够创建一个新的节点，而不必首先创建链表的实例。 
<a name="n332P"></a>
#### 3. Node类的实现写法

- 这里写的是解算法题时适合的实现方法，并不符合OOAD编码规范，但简单清晰，可读性更高；
<a name="kMyxW"></a>
### （二）static的更多妙用
<a name="kORbB"></a>
#### 1. 操作函数都定义为static的原因
static修饰的操作是属于类本身的，不依赖于任何类的SinglyLinkList实例，而只依赖于它的输入参数。这样，就可以不需要创建该类的实例即可调用这个方法。
<a name="ooeD8"></a>
#### 2. 为什么dummyHead和构造函数不用static修饰？

   - 头结点，不能static，是因为不可能所有类共用一个dummyHead；
   - 而Java中，构造函数（或称为构造器）是用来初始化新创建的对象的特殊方法。不能被标记为**static**，因为它们的目的就是为新创建的对象实例设置初始状态；
<a name="v0CCJ"></a>
### （三）函数中先判断空表的原因
在任何其他操作之前检查list是否为空是一个很好的做法，原因如下：

- **效率**：它可以防止不必要的计算，可以立即返回。
- **错误预防**：通过在开始时检查可能导致错误的条件，我们可以防止方法后期可能出现的潜在问题。
<a name="wBjXH"></a>
### （四）insertNode、deleteNode函数流程（带dummyHead从而统一了操作）
<a name="RNj5M"></a>

#### 1. 插入结点
> public static Node insertNode(Node dummyHead, Node nodeInsert, int position);

- 验证插入位置的合法性: 1 ~ (size+1)；
- 合法插入位置的情况下，继续进行一下操作：
  1. 因为要把参数结点插入到第positon个位置，flag后移到(position-1)的位置上（提前在位置有效性做了限制，flag是一定可以后移的）；
  2. 执行参数结点的插入操作；

- 考虑头结点统一操作的作用：极限操作下（1/(size+1)位)，因为flag.next = null，从而统一了操作；

  <a name="gGcj9"></a>

#### 2. 删除结点
> public static Node deleteNode(Node dummyHead, int position);

   1. 空表判断；
   2. 验证删除位置的合法性: 1 ~ size
   3.  合法删除位置的情况下，继续进行一下操作：
      - 删除第position位置的结点，依然是flag后移到(position-1)的位置上；
      
      - 执行参数结点的插入操作
   4.  考虑头结点统一操作的作用：极限操作下（1/size位)，因为flag.next = null，从而统一了操作；
