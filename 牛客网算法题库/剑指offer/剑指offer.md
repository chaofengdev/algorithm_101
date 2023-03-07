# 数据结构

## 1.链表

#### 从尾到头打印链表

方法1：

> 顺序遍历链表，将结点值加入到结果集合中，
>
> 但在加入的时候灵活使用add方法。

```java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> list = new ArrayList<>();
        ListNode cur = listNode;
        while (cur != null) {
            list.add(0,cur.val);//头插法，性能损耗较大
            cur = cur.next;
        }
        return list;
    }
}

```

方法2：栈

> 1.顺序遍历链表，将链表的值push到栈中；
>
> 2.依次pop栈中的元素，加入到结果集合中。

```java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/
import java.util.ArrayList;
import java.util.Stack;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> res = new ArrayList<>();
        Stack<Integer> stack = new Stack<>();
        ListNode cur = listNode;
        while (cur != null) {
            stack.push(cur.val);
            cur = cur.next;
        }
        while (!stack.isEmpty()) {
            res.add(stack.pop());
        }
        return res;
    }
}

```

方法3：递归

> 1.从表头开始递归进入每一个结点；
>
> 2.当`cur == null`时，递归开始返回，每次返回添加一个值进入输出数组；
>
> 3.直到递归返回表头。

```java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/
import java.util.ArrayList;
import java.util.Stack;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> res = new ArrayList<>();
        ListNode cur = listNode;
        recursion(cur, res);//递归函数
        return res;
    }
    public void recursion(ListNode cur, ArrayList<Integer> res) {
        if(cur != null) {
            recursion(cur.next, res);
            res.add(cur.val);
        }
    }
}

```

#### 反转链表

方法1：三指针

> 1.使用cur指针指向当前遍历到的结点，pre指向当前结点的前一个结点；
>
> 2.每次post指向cur.next，将cur指向pre，同时pre和cur各后移一位；
>
> 3.直到cur指向null为止，此时pre指向最后一个结点，该结点即为反转后链表的首元结点。

注意下图的pre指的是上面的post，该图只是示意用。

![图片说明](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/2C7D195FD1E7D347D9205934864B3226)

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode cur = head;
        ListNode pre = null;
        while(cur != null) {
            ListNode post = cur.next;//先保存下个结点
            cur.next = pre;
            pre = cur;
            cur = post;
        }
        return pre;//返回新链表
    }
}
```

方法2：递归 *

这个递归看起来简单，但实际不太好想，二刷还是一头雾水。

但也有进步，起码看答案一瞬间就看懂了哈哈哈。

甚至看到答案，一刷的动态图都能联想到，可几分钟前为啥就想不起来呢？

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode ReverseList(ListNode head) {
        //递归出口
        if(head == null || head.next == null) {
            return head;
        }
        //定义工作指针
        ListNode cur = head;
        //定义递归函数返回头指针
        ListNode newHead = ReverseList(cur.next);
        //逆转本级结点
        cur.next.next = cur;
        //尾部指针为空
        cur.next = null;
        return newHead;
    }
}
```

#### 合并两个排序的链表

方法1：双指针

> - step 1：判断空链表的情况，只要有一个链表为空，那答案必定就是另一个链表了，就算另一个链表也为空。
> - step 2：新建一个空的表头后面连接两个链表排序后的结点，两个指针分别指向两链表头。
> - step 3：遍历两个链表都不为空的情况，取较小值添加在新的链表后面，每次只把被添加的链表的指针后移。
> - step 4：遍历到最后肯定有一个链表还有剩余的结点，它们的值将大于前面所有的，直接连在新的链表后面即可。

![alt](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/82953D04639BD2356F6032F90DAF845F)

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        //判空
        if(list1 == null) {
            return list2;
        }
        if(list2 == null) {
            return list1;
        }
        ListNode cur1 = list1;
        ListNode cur2 = list2;
        ListNode dummyNode = new ListNode(0);//虚拟结点 该结点的作用是表示一个新的链表
        ListNode cur = dummyNode;//新链表的工作指针，负责穿针引线
        while(cur1 != null && cur2 != null) {
            if(cur1.val < cur2.val) {
                cur.next = cur1;
                cur1 = cur1.next;
            }else {
                cur.next = cur2;
                cur2 = cur2.next;
            }
            cur = cur.next;//指针后移
        }
        if(cur1 != null) {
            cur.next = cur1;
        }else {
            cur.next = cur2;
        }
        return dummyNode.next;
    }
}

```

方法2：双指针递归

我只能说，太tm精彩了，我确实没想到。

> - step 1：每次比较两个链表当前结点的值，然后取较小值的链表指针往后，另一个不变，两段子链表作为新的链表送入递归中。
> - step 2：递归回来的结果我们要加在当前较小值的结点后面，相当于不断在较小值后面添加结点。
> - step 3：递归的终止是两个链表有一个为空。

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        //递归出口
        if(list1 == null) {
            return list2;
        }
        if(list2 == null) {
            return list1;
        }
        //本级任务
        if(list1.val <= list2.val) {
            list1.next = Merge(list1.next, list2);
            return list1;
        }else {
            list2.next = Merge(list1, list2.next);
            return list2;
        }
    }
}

```



#### 两个链表的第一个公共结点

方法1：双指针

> - step 1：判断链表情况，其中有一个为空，则不能有公共节点，返回null。
> - step 2：两个链表都从表头开始同步依次遍历。
> - step 3：不需要物理上将两个链表连在一起，仅需指针在一个链表的尾部时直接跳到另一个链表的头部。
> - step 4：根据上述说法，第一个相同的节点便是第一个公共节点。

这题想明白过程很容易，但写的时候却不是很容易。比如下图就有细节是错的。

while(cur1 != cur2)很容易想到，当cur1 == cur2时退出循环，此时cur1指的就是公共结点，但有考虑过不相交的两条链表吗？其实是一样的，不相交的两条链表，cur1与cur2都为null，仍然满足while循环退出条件。

这样循环里的逻辑就很明显了，只要cur1为null，则将cur指向另一条链表的首元结点，cur2同理。这就解释了为什么判断条件不是cur1.next == null 而是 cur1 == null的原因，即两条不相交的链表会在前者的情况下陷入死循环。所以很容易看到下图漏了一步cur1、cur2为null的情况。

当然还有特例判空，这里不赘述了。

![图片说明](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/5A2B72A1B8025376B0594D79C65288EE)

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        //特例
        if(pHead1 == null || pHead2 == null) {
            return null;
        }
        //工作指针
        ListNode cur1 = pHead1;
        ListNode cur2 = pHead2;
        //遍历链表
        while(cur1 != cur2) {//主要是想明白：不相交的链表，遍历完成也会正常退出这个循环
            cur1 = (cur1 == null) ? pHead1 : cur1.next;
            cur2 = (cur2 == null) ? pHead2 : cur2.next;
        }
        return cur1;
    }
}

```

方法2：暴力求解

上面的方法更巧妙，但是要说优雅，我觉得还是暴力求解比较优雅。

> - step 1：单独的遍历两个链表，得到各自的长度。
> - step 2：求得两链表的长度差n，其中较长的链表的指针从头先走n步。
> - step 3：两链表指针同步向后遍历，遇到第一个相同的节点就是第一个公共节点。

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        int len1 = ComputeLen(pHead1);
        int len2 = ComputeLen(pHead2);
        if(len1 >= len2) {
            int len = len1 - len2;//链表长度差
            //长链表先走len步
            for(int i = 0; i < len; i++) {
                pHead1 = pHead1.next;
            }
            //两个链表同时移动，遇到公共节点停止
            while(pHead1 != null && pHead2 != null && pHead1 != pHead2) {
                pHead1 = pHead1.next;
                pHead2 = pHead2.next;
            }
        }else {
            int len = len2 - len1;//链表长度差
            //长链表先走len步
            for(int i = 0; i < len; i++) {
                pHead2 = pHead2.next;
            }
            //两个链表同时移动，遇到公共节点停止
            while(pHead1 != null && pHead2 != null && pHead1 != pHead2) {
                pHead1 = pHead1.next;
                pHead2 = pHead2.next;
            }
        }
        return pHead1;
    }
    //计算链表长度
    public int ComputeLen(ListNode head) {
        ListNode cur = head;
        int len = 0; 
        while(cur != null) {//遍历完结点，计数加1
            cur = cur.next;
            len++;
        }
        return len;
    }
}
```

