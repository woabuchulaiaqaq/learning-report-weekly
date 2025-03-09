# week2

这周主要学习了一些机器学习知识，西瓜树和吴恩达同步看到了神经网络，希望下周助学金下来可以开始适当的做点练习。算法这周主要是刷了几道链表的题，难度不高，主要是复习和巩固以前不会的题型。

## 关于链表

什么是链表，链表是一种通过指针串联在一起的线性结构，每一个节点由两部分组成，一个是数据域一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向null（空指针的意思）。

链表的入口节点称为链表的头结点也就是head。

一般算法题都会自己定义一个 dummyhead ,也就是一个虚拟头节点，来辅助做题。即dummyhead->next=head

需要牢记单链表的定义，有时候题目不会给出

```cpp
struct ListNode {
    int val;  // 节点上存储的元素
    ListNode *next;  // 指向下一个节点的指针
    ListNode(int x) : val(x), next(NULL) {}  // 节点的构造函数
};
```

同时还有一个很值得注意的地方，也就是判断条件的时候必须先写  `a!=nullptr&&a->next!=nullptr.  而不能反过来，因为若a是空指针，那a->next就是一个未被定义的指针。

### 几道经典的例题

Leetcode 209

![](/Users/zixuanchen/Desktop/截屏2025-03-08 22.00.44.png)

非常经典的一道题，总体思路就是通过循环，把每一个结点的next连到他的前一个结点上。这时就要采用双指针来协助储存这两个结点（一前一后）。

首先定义一个cur指针，指向头结点，再定义一个pre指针，初始化为null。

然后就要开始反转了，首先要把 cur->next 节点用tmp指针保存一下，也就是保存一下这个节点。

为什么要保存一下这个节点呢，因为接下来要改变 cur->next 的指向了，将cur->next 指向pre ，此时已经反转了第一个节点了。

接下来，就是循环走如下代码逻辑了，继续移动pre和cur指针。

最后，cur 指针已经指向了null，循环结束，链表也反转完毕了。 此时我们return pre指针就可以了，pre指针就指向了新的头结点。

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* temp; // 保存cur的下一个节点
        ListNode* cur = head;
        ListNode* pre = NULL;
        while(cur) {
            temp = cur->next;  // 保存一下 cur的下一个节点，因为接下来要改变cur->next
            cur->next = pre; // 翻转操作
            // 更新pre 和 cur指针
            pre = cur;
            cur = temp;
        }
        return pre;
    }
};
```

与之对应的题目，还有删除链表中的倒数第n个结点。

Leetcode 19

![](/Users/zixuanchen/Desktop/截屏2025-03-08 22.06.06.png)

这道题看上去简单，实际上用计数的方法会产生很多奇怪的问题，所以这里有一个解决倒数问题的简单方法。

假设给定要找倒数第n个结点，先用fast指针指向第n+1个数，然后此时fast和slow同步往后移动，这样当fast为空指针时，slow刚好在要删除结点的前面 *因为两者的差是固定的n+1* 类似于形成了一个窗口，这个窗口逐渐平移直至最后。

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* slow = dummyHead;
        ListNode* fast = dummyHead;
        while(n-- && fast != NULL) {
            fast = fast->next;
        }
        fast = fast->next; // fast再提前走一步，因为需要让slow指向删除节点的上一个节点
        while (fast != NULL) {
            fast = fast->next;
            slow = slow->next;
        }
        slow->next = slow->next->next; 
        
        // ListNode *tmp = slow->next;  C++释放内存的逻辑
        // slow->next = tmp->next;
        // delete tmp;
        
        return dummyHead->next;
    }
};
```





## 机器学习

这周主要看了决策树和正则化的一些知识。

决策树是一种常见的机器学习方法，我们希望从给定的训练数据集中学得一个合适的模型对新示例进行分类。其基本流程遵守简单而直观的分而治之。

决策树的生成是一个递归过程，从每一个结点中选取最优的划分属性，进行合适的划分。

这就引出了问题，我们要怎么进行选择的划分。信息熵是度量样本集合纯度的常见指标，信息熵越大，样本纯度越高。由此，我们可以得出以属性a划分的信息增益，而增益越高，则意味着纯度提升越大，以此来辅助我们的划分属性选择。

同时，信息增益可能对可取值个数多的属性有所偏好，为了减少这种不利影响，我们一般使用增益率来选择划分最优属性。

### 剪枝处理

是一种对付过拟合的手段，主要分为预剪枝与后剪枝为主。

剪枝的主要判断手段是对剪枝前后验证集中的正确率进行对比。

#### 预剪枝

先讨论非叶子结点，当该结点进行划分后验证的正确率比不进行划分高，则进行划分。若更低，则进行剪枝处理。

优势：减少了时间开销

劣势：可能导致欠拟合

#### 后剪枝

先讨论最后一个非叶子结点，自下而上。

优势：欠拟合风险小

劣势：训练时间开销大

