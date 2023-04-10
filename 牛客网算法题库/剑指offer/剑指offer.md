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



#### 链表中环的入口结点

![fig1](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/jianzhi_II_022_fig1.png)

![alt](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/8B355F0FCD615249270C1B1DEBC84C52)

> 这题标准中档题，表面考察链表，实际考察数学。
>
> slow走过`fs=a+b`，fast走过`ff=a+n(b+c)+b`，又因为`ff=2fs`，则`a+n(b+c)+b=2(a+b)`，则
>
> `a=(n-1)(b+c)+c`，即从相遇点开始，额外设置指针ptr指向链表头部，随后与slow每次向后移动一个位置，最终在入环点相遇。

> - step 1：使用[BM6.判断链表中是否有环](https://www.nowcoder.com/practice/650474f313294468a4ded3ce0f7898b9?tpId=295&sfm=html&channel=nowcoder)中的方法判断链表是否有环，并找到相遇的节点。
> - step 2：慢指针继续在相遇节点，快指针回到链表头，两个指针同步逐个元素逐个元素开始遍历链表。
> - step 3：再次相遇的地方就是环的入口。

```java
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {

    public ListNode EntryNodeOfLoop(ListNode pHead) {
        ListNode slow = hasCycle(pHead);
        if(slow == null) {//无环，返回null
            return null;
        }
        ListNode ptr = pHead;//快指针回到表头，这里使用ptr代替fast
        while(ptr != slow) {
            ptr = ptr.next;
            slow = slow.next;
        }
        return slow;//返回公共点，即入口结点
    }
    //判断链表是否有环，有环返回公共点，无环返回null
    public ListNode hasCycle(ListNode head) {
        //快慢指针
        ListNode slow = head;
        ListNode fast = head;
        //遍历到公共结点
        while(fast != null && fast.next != null) {//无环退出并且返回null
            //快指针移动两步，慢指针移动一步
            slow = slow.next;
            fast = fast.next.next;
            //相遇则有环，返回公共结点
            if(fast == slow) {
                return slow;
            }
        }
        return null;
    }
}
```



#### 链表中倒数最后k个结点

> 这题唤醒了我的一个陈年问题，就是for循环的判断。
>
> for循环语句是支持迭代的一种通用结构，是最有效、最灵活的循环结构。for循环在第一次反复之前要进行初始化，即执行初始表达式;随后，对布尔表达式进行判定，若判定结果为true，则执行循环体，否则，终止循环;最后在每一次反复的时候，进行某种形式的“步进”，即执行迭代因子。
>
> 即java中，for可以一次不执行，有些语言中for循环必须执行一次，注意区分。

方法1：双指针

方法就是下图，但具体写法，很难第一次就像官方那么优雅。

![alt](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/15B43339B02E276F1B14C9F3B03A53CF)

```java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 *   public ListNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param pHead ListNode类 
     * @param k int整型 
     * @return ListNode类
     */
    public ListNode FindKthToTail (ListNode pHead, int k) {
        // write code here
        //判空
        if(pHead == null) return null;
        //特例
        if(k == 0) return null;
        //特例
        int len = 0;
        ListNode cur = pHead;
        while(cur != null) {
            cur = cur.next;
            len++;
        }
        if(k > len) return null;//返回长度为0的链表
        //倒数第n个结点其实就是链表逆序走n-1步
        ListNode fast = pHead;
        ListNode slow = pHead;
        //fast先走k-1步
        while(k - 1 > 0) {//注意这里的写法
            fast = fast.next;
            k--;
        }
        //slow、fast同时走，直到fast走到最后一个结点
        while(fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        return slow;//slow指向的即为倒数第k个结点
    }
}
```

```java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 *   public ListNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param pHead ListNode类 
     * @param k int整型 
     * @return ListNode类
     */
    public ListNode FindKthToTail (ListNode pHead, int k) {
        // write code here
        //双指针
        ListNode slow = pHead;
        ListNode fast = pHead;
        //快指针先走k步
        for(int i = 0; i < k; i++) {
            if(fast != null) {
                fast = fast.next;
            }else {//达不到k步，说明链表过短（这个判断很精彩）
                return null;
            }
        }
        //快慢指针同步
        while(fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

方法2：暴力求解

> 上图中，长度为7的链表，倒数第2个，就是正数第6个，也就是走5步（7-2）。

```java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 *   public ListNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param pHead ListNode类 
     * @param k int整型 
     * @return ListNode类
     */
    public ListNode FindKthToTail (ListNode pHead, int k) {
        // write code here
        //遍历链表统计链表长度
        int len = 0;
        ListNode cur = pHead;
        while(cur != null) {
            cur = cur.next;
            len++;
        }
        //特例
        if(len < k) return null;
        //找到len - k + 1个结点，即走len - k步的结点
        cur = pHead;
        for(int i = 0; i < len - k; i++) {
            cur = cur.next;
        }
        return cur;
    }
}
```

#### 复杂链表的复制 *

方法1：利用哈希表

> java中的HashMap是重要的知识点，需要掌握。详细可见菜鸟教程https://www.runoob.com/java/java-hashmap.html
>
> 还有就是掌握常见遍历HashMap的方式，详细可见https://cloud.tencent.com/developer/article/1751974

> - step 1：建立哈希表，key为原始链表的节点，value为拷贝链表的节点。
> - step 2：遍历原始链表，依次拷贝每个节点，并连接指向后一个的指针，同时将原始链表节点与拷贝链表节点之间的映射关系加入哈希表。
> - step 3：遍历哈希表，对于每个映射，拷贝节点的ramdom指针就指向哈希表中原始链表的random指针。

```java
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*/
import java.util.*;
public class Solution {
    public RandomListNode Clone(RandomListNode pHead) {
        //哈希表
        HashMap<RandomListNode, RandomListNode> map = new HashMap<>();//哈希表，保存原链表结点与复制后的结点对
        //头结点
        RandomListNode dummyNode = new RandomListNode(-1);
        dummyNode.next = pHead;
        //工作指针
        RandomListNode cur = pHead;//指向原结点
        RandomListNode pre = dummyNode;//指向复制后结点（这里要理解清楚）
        //遍历链表：复制结点，新建单链表
        while(cur != null) {//这里只考虑单链表的情况
            RandomListNode clone = new RandomListNode(cur.label);//新建结点
            map.put(cur, clone);//放入哈希表
            pre.next = clone;
            pre = pre.next;//指针后移
            cur = cur.next;//指针后移
        }
        //遍历哈希表：本题难点所在，务必思考清楚
        for(HashMap.Entry<RandomListNode, RandomListNode> entry : map.entrySet()) {//遍历哈希表的方式之一
            if(entry.getKey().random == null) {//该结点random为null
                entry.getValue().random = null;
            }else {//该节点random不为null
                entry.getValue().random = map.get(entry.getKey().random);//这里主要是考虑清楚哈希表的映射关系
            }
        }
        return dummyNode.next;
    }
}

```

> 上面采用遍历哈希表的方式，实际操作完全不需要那么麻烦，可以将整个过程拆分为，
>
> 遍历原链表，保存结点映射对；
>
> 遍历原链表，构建新链表。
>
> 我觉得k神讲的比较通透：https://leetcode.cn/problems/fu-za-lian-biao-de-fu-zhi-lcof/solution/jian-zhi-offer-35-fu-za-lian-biao-de-fu-zhi-ha-xi-/

```java
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*/
import java.util.*;
public class Solution {
    public RandomListNode Clone(RandomListNode pHead) {
        HashMap<RandomListNode, RandomListNode> map = new HashMap<>();
        RandomListNode cur = pHead;
        //遍历原链表，保存结点映射对
        while(cur != null) {
            RandomListNode node = new RandomListNode(cur.label);
            map.put(cur, node);
            cur = cur.next;
        }
        //再次遍历原链表，构建新链表
        cur = pHead;
        while(cur != null) {
            map.get(cur).next = map.get(cur.next);
            map.get(cur).random = map.get(cur.random);
            cur = cur.next;
        }
        return map.get(pHead);
    }
}
```

方法2：双指针

这题双指针，没做过的基本不可能想到。确实比较精彩。

> - step 1：遍历链表，对每个节点新建一个拷贝节点，并插入到该节点之后。
> - step 2：使用双指针再次遍历链表，两个指针每次都移动两步，一个指针遍历原始节点，一个指针遍历拷贝节点，拷贝节点的随机指针跟随原始节点，指向原始节点随机指针的下一位。--这里完全可以使用一个指针即可。
> - step 3：再次使用双指针遍历链表，每次越过后一位相连，即拆分成两个链表。--这里使用两个指针，同时需要保存新链表的首元结点。

![img](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/1604747742-aMDdkM-Picture14.png)

```java
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*/
public class Solution {
    public RandomListNode Clone(RandomListNode pHead) {
        if(pHead == null) return null;
        //遍历链表，复制结点
        RandomListNode cur = pHead;
        while(cur != null) {
            RandomListNode temp = new RandomListNode(cur.label);//新建结点
            temp.next = cur.next;
            cur.next = temp;
            cur = temp.next;
        }
        //构建新链表结点的random指向
        cur = pHead;
        while(cur != null) {
            if(cur.random == null) {
                cur.next.random = null;
            }else {
                cur.next.random = cur.random.next;//设计该结构的核心原因，通过该方法少使用一个哈希表
            }
            cur = cur.next.next;//下一个原结点
        }
        //拆分两个链表
        RandomListNode res = pHead.next;//保存结果
        RandomListNode cur1 = pHead;//原链表
        RandomListNode cur2 = pHead.next;//复制后链表
        while(cur2.next != null) {//这个条件不太好想到，当然也有其他写法
            cur1.next = cur1.next.next;
            cur1 = cur1.next;
            cur2.next = cur2.next.next;
            cur2 = cur2.next;
        }
        cur1.next = null;//同样，这里需要单独处理
        return res;
    }
}

```

显然，看透了拆分两个链表的逻辑后，可以更优雅的改成下面的代码：

```java
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*/
public class Solution {
    public RandomListNode Clone(RandomListNode pHead) {
        if(pHead == null) return null;
        //遍历链表，复制结点
        RandomListNode cur = pHead;
        while(cur != null) {
            RandomListNode temp = new RandomListNode(cur.label);//新建结点
            temp.next = cur.next;
            cur.next = temp;
            cur = temp.next;
        }
        //构建新链表结点的random指向
        cur = pHead;
        while(cur != null) {
            if(cur.random == null) {
                cur.next.random = null;
            }else {
                cur.next.random = cur.random.next;//设计该结构的核心原因，通过该方法少使用一个哈希表
            }
            cur = cur.next.next;//下一个原结点
        }
        //拆分两个链表
        RandomListNode res = pHead.next;//保存结果
        RandomListNode cur1 = pHead;//原链表
        RandomListNode cur2 = pHead.next;//复制后链表
        while(cur1 != null) {//cur1.next不会等于null
            cur1.next = cur1.next.next;
            cur1 = cur1.next;
            if(cur2.next != null) {//排除了cur2.next == null的情况
                cur2.next = cur2.next.next;
            }
            cur2 = cur2.next;//就不需要对cur1或者cur2单独处理了，比较优雅的写法
        }
        return res;
    }
}

```



#### 删除链表中的重复结点 *

方法1：直接比较删除

> 这是一个升序链表，重复的节点都连在一起，我们就可以很轻易地比较到重复的节点，然后将所有的连续相同的节点都跳过，连接不相同的第一个节点。
>
> 1.增加头结点，方便删除首元节点；定义cur为已经访问过的链表结点，表示这个结点不会和后面的结点重复。
>
> 2.每次比较cur.next与cur.next.next元素值，如果不相等，则cur前进1；
>
> 3.如果相等，则保存相等的结点元素值，开while循环，依次删除重复结点，直到遇到后续第一个不重复的结点；
>
> 4.返回首元结点。

![alt](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/0A01E83A481A4919FAE203E7BB77FDD3)

```java
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public ListNode deleteDuplication(ListNode pHead) {
        //特例
        if(pHead == null || pHead.next == null) {
            return pHead;
        }
        //头结点：因为涉及到删除单链表
        ListNode dummyNode = new ListNode(-1);
        dummyNode.next = pHead;
        ListNode cur = dummyNode;
        //每次比较后两个结点值是否相同
        while(cur.next != null && cur.next.next != null) {
            if(cur.next.val != cur.next.next.val) {//值不相同
                cur = cur.next;
            }else {//值相同，保存cur.next的值，跳过所有相同值的结点
                int temp = cur.next.val;//本题的temp设计的比较巧妙
                while(cur.next != null && cur.next.val == temp) {//本题关键
                    cur.next = cur.next.next;
                }
            }
        }
        return dummyNode.next;//返回首元节点
    }
}

```

方法2：暴力求解、哈希表

> 1.遍历链表，用哈希表记录每个结点出现的次数；
>
> 2.增加头结点，遍历该链表，对于每个结点检查哈希表中的计数，只留下计数为1的，其他情况都删除；
>
> 3.返回首元结点。
>
> 这个方法的好处是原链表不需要有序，坏处是借助了o(n)的空间。

```java
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
*/
import java.util.*;
public class Solution {
    public ListNode deleteDuplication(ListNode pHead) {
        //特例
        if(pHead == null || pHead.next == null) {
            return pHead;
        }
        //哈希表
        HashMap<Integer, Integer> map = new HashMap<>();
        //工作指针
        ListNode cur = pHead;
        //遍历链表，统计结点出现的次数
        while(cur != null) {
            if(map.containsKey(cur.val)) {
                map.put(cur.val, map.get(cur.val) + 1);
            }else {
                map.put(cur.val, 1);
            }
            cur = cur.next;
        }
        //再次遍历链表
        //头结点：因为涉及到删除单链表
        ListNode dummyNode = new ListNode(-1);
        dummyNode.next = pHead;
        cur = dummyNode;
        while(cur.next != null) {
            if(map.get(cur.next.val) != 1) {
                cur.next = cur.next.next;
            }else {
                cur = cur.next;
            }
        }
        return dummyNode.next;//返回首元节点
    }
}

```

#### 删除链表的结点

![alt](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/074232413F62F32D1B2134AF0B9ED494)

```java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 *   public ListNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param head ListNode类 
     * @param val int整型 
     * @return ListNode类
     */
    public ListNode deleteNode (ListNode head, int val) {
        // write code here
        //特例
        if(head == null) {
            return null;
        }
        //头结点
        ListNode dummyNode = new ListNode(-1);
        dummyNode.next = head;
        ListNode pre = dummyNode;
        ListNode cur = head;
        //遍历链表
        while(cur != null) {
            if(cur.val == val) {
                pre.next = cur.next;
                pre = cur;
                cur = cur.next;
            }else {
                pre = cur;
                cur = cur.next;
            }
        }
        return dummyNode.next;
    }
}
```

虽然上面的编码规范良好，但完全可以只使用一个指针pre。

```java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 *   public ListNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param head ListNode类 
     * @param val int整型 
     * @return ListNode类
     */
    public ListNode deleteNode (ListNode head, int val) {
        // write code here
        //头结点
        ListNode dummyNode = new ListNode(-1);
        dummyNode.next = head;
        ListNode pre = dummyNode;
        while(pre.next != null) {
            if(pre.next.val == val) {
                pre.next = pre.next.next;
            }else {
                pre = pre.next;
            }
        }
        return dummyNode.next;
    }
}
```



#### NC2 重排链表

方法1：截断链表+反转链表+合并链表

> 1.将链表分为前后两段；
>
> 2.将后半段链表反转；
>
> 3.从前半段链表和后半段链表的首元结点开始，逐个合并形成新链表。

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public void reorderList(ListNode head) {
        if(head == null) return;
        //截断链表
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next;
            if(fast.next != null) {//这种写法也比较巧妙
                fast = fast.next;
            }
        }
        ListNode temp = slow.next;//保存后段链表的首元结点
        slow.next = null;//截断链表
        //反转链表
        ListNode head2 = reversion(temp);
        //连接两个链表成为新链表
        //ListNode dummyNode = new ListNode(-1);
        //dummyNode.next = head;
        ListNode cur1 = head;
        ListNode cur2 = head2;
        while(cur1 != null && cur2 != null) {//两个工作指针均不为空
            ListNode node1 = cur1.next;
            ListNode node2 = cur2.next;
            cur1.next = cur2;
            cur1 = node1;
            cur2.next = node1;
            cur2 = node2;
        }
    }
    //反转链表
    public ListNode reversion(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null) {
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;//返回反转后链表的首元结点
    }
}
```

####  NC96 回文链表

![image-20230309170855489](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/image-20230309170855489.png)

> - step 1：慢指针每次走一个节点，快指针每次走两个节点，快指针到达链表尾的时候，慢指针刚好到了链表中点。
> - step 2：从中点的位置，开始往后将后半段链表反转。
> - step 3：按照方法三的思路，左右双指针，左指针从链表头往后遍历，右指针从链表尾往反转后的前遍历，依次比较遇到的值。

这题的难点在于，如何恰当分割链表，实际上链表不需要真的断开，比如下面的解答就通过不断开链表，实现简化代码书写。

```java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 the head
     * @return bool布尔型
     */
    public boolean isPail (ListNode head) {
        // write code here
        //截断链表
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        //反转链表
        slow = reversion(slow);
        fast = head;
        //比较两个链表是否相同
        return isSame(slow, fast);
    }
    //比较两个链表是否相同
    public boolean isSame(ListNode head1, ListNode head2) {
        while(head1 != null && head2 != null) {
            if(head1.val != head2.val) {
                return false;
            }
            head1 = head1.next;
            head2 = head2.next;
        }
        return true;//不需要考虑长度问题
    }
    //反转链表
    public ListNode reversion(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null) {
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
}
```

更严谨的写法如下：

```java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 the head
     * @return bool布尔型
     */
    public boolean isPail (ListNode head) {
        // write code here
        if(head == null || head.next == null) return true;
        //截断链表
        ListNode fast = head.next;
        ListNode slow = head;
        while(fast.next != null && fast.next.next != null) {//这样slow指向奇数链表***第一个或者偶数链表****的第二个。
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode head2 = slow.next;
        if(fast.next != null) {//表示奇数链表
            head2 = slow.next.next;
        }
        slow.next = null;//断开连接
        //反转链表
        head2 = reversion(head2);
        //比较两个链表是否相同
        return isSame(head, head2);
    }
    //比较两个链表是否相同
    public boolean isSame(ListNode head1, ListNode head2) {
        while(head1 != null && head2 != null) {
            if(head1.val != head2.val) {
                return false;
            }
            head1 = head1.next;
            head2 = head2.next;
        }
        return true;//不需要考虑长度问题
    }
    //反转链表
    public ListNode reversion(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null) {
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
}
```

上面两种写法的差异是，slow指针遍历到哪里停止，如果while循环的条件是`fast != null && fast.next != null`则slow会停留在奇数的中点，偶数的中点偏右。

![image-20230309172533323](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/image-20230309172533323.png)

如果按照第二种写法，则slow指针会停留在左边。体会一下两者的差异，后面可以直接断开链表，前面则不行。当然前面完全可以多用一个pre指针，达到同样的效果。

![image-20230309172818332](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/image-20230309172818332.png)



## 二叉树

#### 二叉树的深度

方法1：递归

![alt](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/912E54FDA2FC2312AE3F1E8387E0C1B6)

> 二叉树的深度就等于根节点这个1层加上左子树和右子树深度的最大值。
>
> - **终止条件：** 当进入叶子节点后，再进入子节点，即为空，没有深度可言，返回0.
> - **返回值：** 每一级按照上述公式，返回两边子树深度的最大值加上本级的深度，即加1.
> - **本级任务：** 每一级的任务就是进入左右子树，求左右子树的深度。

```
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    public int TreeDepth(TreeNode root) {
        //终止条件
        if(root == null) {
            return 0;
        }
        //本级任务：树的深度等于左子树或者右子树深度的最大值加一
        int depth1 = TreeDepth(root.left);
        int depth2 = TreeDepth(root.right);
        int depth = Math.max(depth1, depth2) + 1;
        //返回值
        return depth;
    }
}
```

稍微整理一下：

```java
public class Solution {
    public int TreeDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }
        return Math.max(TreeDepth(root.left), TreeDepth(root.right)) + 1;
    }
}
```

方法2：层次遍历bfs

> 经典算法，没什么好说的。需要注意，队列的使用。
>
> `Queue<TreeNode> queue = new LinkedList<>();`
>
> queue.isEmpty() 判空
>
> queue.offer(node) 结点入队
>
> queue.poll() 结点出队
>
> queue.size() 队列内元素大小

![alt](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/3B5A6883A44EDAD8E2563E74BB4C2846)

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
import java.util.*;
public class Solution {
    public int TreeDepth(TreeNode root) {
        //特例：空节点没有深度
        if(root == null) return 0;
        //队列：维护层次结点
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 0;
        while(!queue.isEmpty()) {
            int len = queue.size();//每层的结点数
            for(int i = 0; i < len; i++) {//一次遍历一层的结点
                TreeNode node = queue.poll();//遍历的是该层结点
                if(node.left != null) queue.offer(node.left); 
                if(node.right != null) queue.offer(node.right);
            }
            depth++;//遍历一层，深度加一
        }
        return depth;
    }
}

```

#### 按之字形顺序打印二叉树

> 题目本身很简单，需要注意的是：
>
> 1.Collections.reverse(list) 没有返回值；
>
> 2.注意temp集合的新建时机，是每层新建一个；
>
> 3.也可以使用boolean flag来判断是否要逆转该层。

![alt](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/387D7FA985B64AFE794DE2D1DD4E00D4)

```java
import java.util.*;

/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    public ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        TreeNode root = pRoot;
        //特例
        if(root == null) return null;
        //结果集合
        ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
        //队列
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        //层次计数器
        int depth = 0;
        while(!queue.isEmpty()) {
            int n = queue.size();
            ArrayList<Integer> temp = new ArrayList<>();//这个每层都新建，所以不需要重置
            for(int i = 0; i < n; i++) {
                TreeNode node = queue.poll();
                temp.add(node.val);
                if(node.left != null) queue.offer(node.left);
                if(node.right != null) queue.offer(node.right);
            }
            depth++;
            // if(depth % 2 != 0) {//奇数层正序，偶数层逆序
            //     res.add(temp);
            // }else {
            //     Collections.reverse(temp);//注意该工具方法无返回值
            //     res.add(temp);
            // }
            if(depth % 2 == 0) {//偶数层逆序，奇数层不变即可
                Collections.reverse(temp);
            }
            res.add(temp);      
        }
        return res;
    }

}
```

官方给出了一个非常精彩的双栈解答。这里直接贴答案。

![alt](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/4001D0A4E11E0FA0428C08B35F9ABAB8)

```java
import java.util.*;
public class Solution {
    public ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        TreeNode head = pRoot;
        ArrayList<ArrayList<Integer> > res = new ArrayList<ArrayList<Integer>>();
        if(head == null)
            //如果是空，则直接返回空list
            return res; 
        Stack<TreeNode> s1 = new Stack<TreeNode>();
        Stack<TreeNode> s2 = new Stack<TreeNode>();
        //放入第一次
        s1.push(head); 
        while(!s1.isEmpty() || !s2.isEmpty()){ 
            ArrayList<Integer> temp = new ArrayList<Integer>();
            //遍历奇数层
            while(!s1.isEmpty()){ 
                TreeNode node = s1.pop();
                //记录奇数层
                temp.add(node.val); 
                //奇数层的子节点加入偶数层
                if(node.left != null)  
                    s2.push(node.left);
                if(node.right != null) 
                    s2.push(node.right);
            }
            //数组不为空才添加
            if(temp.size() != 0)  
                res.add(new ArrayList<Integer>(temp));
            //清空本层数据
            temp.clear(); 
            //遍历偶数层
            while(!s2.isEmpty()){ 
                TreeNode node = s2.pop();
                //记录偶数层
                temp.add(node.val);  
                //偶数层的子节点加入奇数层
                if(node.right != null)  
                    s1.push(node.right);
                if(node.left != null) 
                    s1.push(node.left);
            }
            //数组不为空才添加
            if(temp.size() != 0) 
                res.add(new ArrayList<Integer>(temp));
            //清空本层数据
            temp.clear(); 
        }
        return res;
    }

}

```



#### 二叉搜索树的第k个结点

方法1：中序遍历（递归）

> - step 1：设置全局变量count记录遍历了多少个节点，res记录第k个节点。
> - step 2：另写一函数进行递归中序遍历，当节点为空或者超过k时，结束递归，返回。
> - step 3：优先访问左子树，再访问根节点，访问时统计数字，等于k则找到。
> - step 4：最后访问右子树。

![图片说明](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/AD840828372A5B112A2F2C26B88D4243)

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param proot TreeNode类 
     * @param k int整型 
     * @return int整型
     */
    private int count;//记录遍历到第几个结点
    private TreeNode res;//记录第k个结点
    public int KthNode (TreeNode proot, int k) {
        // write code here
        dfs(proot, k);
        if(res != null) {
            return res.val;
        }
        return -1;
    }
    //中序遍历
    public void dfs(TreeNode root, int k) {
        //递归出口
        if(root == null) return;//if(root == null || count > k) return; 可以提高一点效率
        //左子树
        dfs(root.left, k);
        count++;
        if(count == k) res = root;//记录第k个结点
        //右子树
        dfs(root.right,k);
    }
}
```

方法2：中序遍历（非递归）

> - step 1：用栈记录当前节点，不断往左深入，直到左边子树为空。
> - step 2：再弹出栈顶（即为当前子树的父节点），访问该节点，同时计数。
> - step 3：然后再访问其右子树，其中每棵子树都遵循左中右的次序。
> - step 4：直到第k个节点返回，如果遍历结束也没找到，则返回-1.

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param proot TreeNode类 
     * @param k int整型 
     * @return int整型
     */
    public int KthNode (TreeNode proot, int k) {
        // write code here
        TreeNode root = proot;
        //特例
        if(root == null) return -1;//空树返回-1
        //计数器
        int count = 0;
        //栈
        Stack<TreeNode> stack = new Stack<>();
        while(!stack.isEmpty() || root != null) {//循环条件特殊
            while(root != null) {//每颗子树从最左开始
                stack.push(root);
                root = root.left;
            }
            TreeNode node = stack.pop();
            count++;
            if(count == k) return node.val;//找到第k个结点，返回node.val
            root = node.right;
        }
        return -1;//没有找到，返回-1
    }
}
```



#### 重建二叉树

方法1：递归

> - step 1：先根据前序遍历第一个点建立根节点。
> - step 2：然后遍历中序遍历找到根节点在数组中的位置。
> - step 3：再按照子树的节点数将两个遍历的序列分割成子数组，将子数组送入函数建立子树。
> - step 4：直到子树的序列长度为0，结束递归。

![图片说明](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/2939E21521C22C46A95A8B8DFA62CE0D)

```java
import java.util.*;
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre,int [] vin) {
        //递归出口
        if(pre.length == 0 || vin.length == 0) {
            return null;
        }
        //新建根节点
        TreeNode root = new TreeNode(pre[0]);
        //遍历中序序列，找到第一个根节点，左右分别递归处理
        for(int i = 0; i < vin.length; i++) {
            if(vin[i] == pre[0]) {
                //构建左子树
                root.left = 
                reConstructBinaryTree(Arrays.copyOfRange(pre, 1, i + 1), Arrays.copyOfRange(vin, 0, i));
                //构建右子树
                root.right = 
                reConstructBinaryTree(Arrays.copyOfRange(pre, i + 1, pre.length), Arrays.copyOfRange(vin, i + 1, vin.length));
                break;
            }
        }
        //返回根结点
        return root;
    }
}
```

方法2：非递归

> 没看懂，放个链接后面再看吧。
>
> https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6?tpId=13&tqId=23282&ru=%2Fpractice%2F57aa0bab91884a10b5136ca2c087f8ff&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking&sourceUrl=%2Fexam%2Foj%2Fta%3Fpage%3D1%26tpId%3D13%26type%3D13



#### 树的子结构

方法1：递归 两层前序遍历

本题递归并不是很好想，有点饶。仔细体会思考。

参考链接：https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/solution/mian-shi-ti-26-shu-de-zi-jie-gou-xian-xu-bian-li-p/

牛客官方解答应该有误。

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        if(root1 == null || root2 == null) return false;//空树不是任意树的子结构
        boolean flag1 = recursion(root1, root2);//同一根结点
        boolean flag2 = HasSubtree(root1.left, root2);//左子树
        boolean flag3 = HasSubtree(root1.right, root2);//右子树
        return flag1 || flag2 || flag3;//本质上是先序遍历
    }
    //规定B与A同一个根节点前提下，B是否是A的子结构
    public boolean recursion(TreeNode root1, TreeNode root2) {
        if(root2 == null) return true;//注意：这里返回true，是退出条件
        if(root1 == null || root1.val != root2.val) return false;
        return recursion(root1.left, root2.left) && recursion(root1.right, root2.right);//左右子树结点值要全部相等
    }
}
```

换一种写法，本质相同。

本质上这题有个基础题，即确定root1.val == root2.val的前提下，如何判断B是A的子结构，就是compare方法的功能。

那么回到本题，如何在A中找到B的根节点吗，递归的先序遍历即可，如果找到了，就返回true，如果没有找到，就去左子树或者右子树中找。

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        if(root2 == null) return false;//特例
        return find(root1, root2);
    }
    //B是否为A的子结构，可以分为两步，
    //从A中找到B的根节点
    //在上面的前提下，B的结点是否都在A中
    public boolean find(TreeNode root1, TreeNode root2) {
        if(root1 == null) return false;//A中没有B的根节点
        if(root2 == null) return false;//B为空肯定不存在A中
        if(root1.val == root2.val && compare(root1, root2)) {//节点值相同的前提下，并且B是A的子结构
            return true;
        }
        return find(root1.left, root2) || find(root1.right, root2);//左子树和右子树中寻找B的根节点，左右子树中找到一个即可
    }
    //严格定义：判断B的结点是否都在A上
    public boolean compare(TreeNode root1, TreeNode root2) {
        if(root2 == null) return true;//B没有结点，则返回true
        if(root1 == null || root1.val != root2.val) return false;//A没有结点，则返回false
        return compare(root1.left, root2.left) && compare(root1.right, root2.right);
    }
}
```

方法2：层序遍历 两层层序遍历

> 根据方法一，所以这道题的思路，无非就是在A树中遍历每个节点尝试找到那个子树，然后每次以该节点出发能不能将子树与B树完全匹配。能用前序遍历解决，我们也可以用层次遍历来解决。

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
import java.util.*;
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        //特例
        if(root2 == null || root1 == null) return false;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root1);
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(node.val == root2.val && helper(node, root2)) {//关键
                return true;
            }
            if(node.left != null) queue.offer(node.left);
            if(node.right != null) queue.offer(node.right);
        }
        return false;
    }
    public boolean helper(TreeNode root1, TreeNode root2) {
        Queue<TreeNode> queue1 = new LinkedList<>();
        Queue<TreeNode> queue2 = new LinkedList<>();
        queue1.offer(root1);
        queue2.offer(root2);
        while(!queue2.isEmpty()) {
            TreeNode node1 = queue1.poll();
            TreeNode node2 = queue2.poll();
            if(node1 == null || node1.val != node2.val) return false;//关键
            //A可以添加空结点，B不添加空结点
            //每次判断A是否为空结点，如果是空结点，则直接返回false
            //这里不是很好理解，可以细细揣摩
            if(node2.left != null) {
                queue2.offer(node2.left);
                queue1.offer(node1.left);
            }
            if(node2.right != null) {
                queue2.offer(node2.right);
                queue1.offer(node1.right);
            }
        }
        return true;
    }
}

```

#### 二叉树的镜像

方法1：层序遍历

层序遍历的同时交换左右子结点。

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param pRoot TreeNode类 
     * @return TreeNode类
     */
    public TreeNode Mirror (TreeNode pRoot) {
        // write code here
        TreeNode root = pRoot;
        if(root == null) return null;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            //交换
            TreeNode temp = node.left;
            node.left = node.right;
            node.right = temp;
            if(node.left != null) {
                queue.offer(node.left);
            }
            if(node.right != null) {
                queue.offer(node.right);
            }
        }
        return root;
    }
}
```

方法2：后序遍历

> 因为我们需要将二叉树镜像，意味着每个左右子树都会交换位置，如果我们从上到下对遍历到的节点交换位置，但是它们后面的节点无法跟着他们一起被交换，因此我们可以考虑自底向上对每两个相对位置的节点交换位置，这样往上各个子树也会被交换位置。
>
> 自底向上的遍历方式，我们可以采用后序递归的方法。
>
> - step 1：先深度最左端的节点，遇到空树返回，处理最左端的两个子节点交换位置。
> - step 2：然后进入右子树，继续按照先左后右再回中的方式访问。
> - step 3：再返回到父问题，交换父问题两个子节点的值。
>
> 有一说一，代码异常简洁，非常简单的一道题。

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param pRoot TreeNode类 
     * @return TreeNode类
     */
    public TreeNode Mirror (TreeNode pRoot) {
        // write code here
        TreeNode root = pRoot;
        if(root == null) {
            return null;
        }
        TreeNode left = Mirror(root.left);
        TreeNode right = Mirror(root.right);
        root.left = right;
        root.right = left;
        return root;
    }
}
```

#### 从上往下打印二叉树

方法1：层序遍历

```java
import java.util.ArrayList;
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
import java.util.*;
public class Solution {
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        ArrayList<Integer> res = new ArrayList();
        if(root == null)
            //如果是空，则直接返回空数组
            return res; 
        //队列存储，进行层次遍历
        Queue<TreeNode> q = new ArrayDeque<TreeNode>(); 
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode cur = q.poll();
            res.add(cur.val);
            //若是左右孩子存在，则存入左右孩子作为下一个层次
            if(cur.left != null)
                q.add(cur.left);
            if(cur.right != null)
                q.add(cur.right);
        }
        return res;
    }
}


```

方法2：递归形式的层序遍历 

> 有点诡异，纯当了解了。
>
> - step 1：首先判断二叉树是否为空，空树没有遍历结果。
> - step 2：使用递归进行层次遍历输出，每次递归记录当前二叉树的深度，每当遍历到一个节点，如果为空直接返回。
> - step 3：如果遍历的节点不为空，输出二维数组中一维数组的个数（即代表了输出的行数）小于深度，说明这个节点应该是新的一层，我们在二维数组中增加一个一维数组，然后再加入二叉树元素。
> - step 4：如果不是step 3的情况说明这个深度我们已经有了数组，直接根据深度作为下标取出数组，将元素加在最后就可以了。
> - step 5：处理完这个节点，再依次递归进入左右节点，同时深度增加。因为我们进入递归的时候是先左后右，那么遍历的时候也是先左后右，正好是层次遍历的顺序。
> - step 6：最后将二维数组中的结果依次送入一维数组。

```java
import java.util.*;
public class Solution {
    void traverse(TreeNode root, ArrayList<ArrayList<Integer> > res, int depth) {
        if(root != null){
            //新的一层
            if(res.size() < depth){  
                ArrayList<Integer> row = new ArrayList(); 
                res.add(row);
                row.add(root.val);
            //读取该层的一维数组，将元素加入末尾
            }else{ 
                ArrayList<Integer> row = res.get(depth - 1);
                row.add(root.val);
            }
        }
        else
            return;
        //递归左右时深度记得加1
        traverse(root.left, res, depth + 1); 
        traverse(root.right, res, depth + 1);
    }
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        ArrayList<Integer> res = new ArrayList();
        ArrayList<ArrayList<Integer> > temp = new ArrayList<ArrayList<Integer> >();
        if(root == null)
            //如果是空，则直接返回
            return res; 
        //递归层次遍历 
        traverse(root, temp, 1); 
        //送入一维数组
        for(int i = 0; i < temp.size(); i++)
            for(int j = 0; j < temp.get(i).size(); j++)
                res.add(temp.get(i).get(j));
        return res;
    }
}

```



#### 二叉搜索树的后序遍历序列

这题还是有一定难度的。

方法1：递归

![img](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/e8f9f419e91b180c7c299897358d884a274a1500ba07489785c6f8382413c49f-Picture9.png)

```java
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        int len = sequence.length;
        if(len == 0) return false;
        return dfs(sequence, 0, len - 1);//[0,len-1]
    }
    //判断数组arr在区间[left,right]上是否是二叉搜索树的后序遍历序列
    public boolean dfs(int[] arr, int left, int right) {
        //递归出口：只剩一个结点
        if(left >= right) return true;
        //int root = arr[right]; 
        //遍历数组，找到第一个大于arr[right]的值及下标
        int index = left;
        while(arr[index] < arr[right]) {//这里使用for循环，需要多考虑多种临界情况
            index++;
        }
        int mid = index;//右子树的第一个结点或者根结点
        //判断右子树是否合法
        while(index < right) {
            if(arr[index] < arr[right]) {
                return false;
            }
            index++;
        }
        //两个子区间内部的情况需要继续递归判断
        return dfs(arr, left, mid - 1) && dfs(arr, mid, right - 1);
    }
}
```

思考这种写法为什么不对，应该如何改进：

```java
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        int len = sequence.length;
        if(len == 0) return false;
        return dfs(sequence, 0, len - 1);//[0,len-1]
    }
    //判断数组arr在区间[left,right]上是否是二叉搜索树的后序遍历序列
    public boolean dfs(int[] arr, int left, int right) {
        //递归出口
        if(left >= right) return true;
        int root = arr[right]; 
        //遍历数组，找到第一个大于arr[right]的值及下标
        int mid = left;
        for(int i = left; i < right; i++) {
            if(arr[i] > arr[right]) {
                mid = i;//右子树的第一个结点下标
                break;
            }
        }
        //判断右子树是否合法
        for(int i = mid; i < right; i++) {
            if(arr[i] < arr[right]) {
                return false;
            }
        }
        return dfs(arr, left, mid - 1) && dfs(arr, mid, right - 1);
    }
}
```

改进：

```java
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        int len = sequence.length;
        if(len == 0) return false;
        return dfs(sequence, 0, len - 1);//[0,len-1]
    }
    //判断数组arr在区间[left,right]上是否是二叉搜索树的后序遍历序列
    public boolean dfs(int[] arr, int left, int right) {
        //递归出口
        if(left >= right) return true;//[4,6,7,5]
        int root = arr[right]; 
        //遍历数组，找到第一个大于arr[right]的值及下标
        int mid = left;
        for(int i = left; i <= right; i++) {
            if(arr[i] >= arr[right]) {//这里必须要大于等于，[6,7]时，mid必须指向7
                mid = i;//右子树的第一个结点下标或者根节点下标
                break;
            }
        }
        //判断右子树是否合法
        for(int i = mid; i < right; i++) {
            if(arr[i] < arr[right]) {
                return false;
            }
        }
        return dfs(arr, left, mid - 1) && dfs(arr, mid, right - 1);
    }
}
```

方法2：单调栈（推荐方法）

```java
...
```

#### 二叉树中和为某一值的路径（一）

方法1：递归

> - step 1：每次检查遍历到的节点是否为空节点，空节点就没有路径。
> - step 2：再检查遍历到是否为叶子节点，且当前sum值等于节点值，说明可以刚好找到。
> - step 3：检查左右子节点是否可以有完成路径的，如果任意一条路径可以都返回true，因此这里选用两个子节点递归的或。

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param root TreeNode类 
     * @param sum int整型 
     * @return bool布尔型
     */
    public boolean hasPathSum (TreeNode root, int sum) {
        // write code here
        if(root == null) return false;
        //if(sum == 0) return false;
        return dfs(root, sum);
    }
    //递归判断是否有结点累加和为sum
    public boolean dfs(TreeNode root, int sum) {
        //递归出口
        if(root == null) return false;//勿忘，空结点找不到路径
        if(root.left == null && root.right == null) {//叶子结点且余数为0
            if(sum - root.val == 0) return true;
        }
        //本级任务
        boolean flag1 = dfs(root.left, sum - root.val);
        boolean flag2 = dfs(root.right, sum - root.val);
        //返回值
        return flag1 || flag2;
    }
}
```

代码改进

```java
public class Solution {
    /**
     * 
     * @param root TreeNode类 
     * @param sum int整型 
     * @return bool布尔型
     */
    public boolean hasPathSum (TreeNode root, int sum) {
        // write code here
        if(root == null) return false; //过了叶子结点，没找到符合条件的路径
        if(root.left == null && root.right == null) {
            if(sum == root.val) return true;//找到该路径
        }
        return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
    }
}
```

方法2：先序遍历

这题是少数可以用迭代非通法来解决的，如果换成通法，则因为先遍历到最左边子节点，无法有效回溯。--这点存疑。

关于二叉树遍历的前序遍历的通法与非通法，详见本仓库数据结构与算法的二叉树遍历篇。

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param root TreeNode类 
     * @param sum int整型 
     * @return bool布尔型
     */
    public boolean hasPathSum (TreeNode root, int sum) {
        // write code here
        if(root == null) return false;
        Stack<TreeNode> stack1 = new Stack<>();//保存每个结点
        Stack<Integer> stack2 = new Stack<>();//保存当前的路径和
        stack1.push(root);
        stack2.push(root.val);
        while (!stack1.isEmpty()) {
            TreeNode node = stack1.pop();
            int cur_sum = stack2.pop();//当前为止的路径和
            if(node.left == null && node.right == null && cur_sum == sum) {//叶子结点且路径和为sum则找到答案
                return true;
            }
            if(node.right != null) {
                stack1.push(node.right);
                stack2.push(node.right.val + cur_sum);
            }
            if(node.left != null) {
                stack1.push(node.left);
                stack2.push(node.left.val + cur_sum);
            }
        }
        return false;//没有找到合适的结点
    }
}
```

这里只是让我们判断是否存在，如果让我们求出具体的路径呢？

显然这里的一大难点是如何恰当回溯，以及如何恰当返回`List<List<Integer>>`。

这里给出leetcode上的题目：二叉树中和为某一值的路径。

```java
class Solution {
    List<Integer> list = new ArrayList<>();
    List<List<Integer>> res = new ArrayList<>();
    //int cur_sum = 0;
    public List<List<Integer>> pathSum(TreeNode root, int target) {
        recursion(root, target);
        return res;
    }
    public void recursion(TreeNode root, int target) {
        if(root == null) return;
        target = target - root.val;
        list.add(root.val);//记录该结点值
        if(root.left == null && root.right == null && target == 0) {
            res.add(new ArrayList<>(list));//复制一个list加入到res集合中
        }
        recursion(root.left, target);
        recursion(root.right, target);
        list.remove(list.size() - 1);//删除该结点值，即回溯
        return;
    }
}
```

#### 二叉树中和为某一值的路径（二）

方法1：递归

同上，但多加了两个集合，用来正确返回题设集合。

```java
public class Solution {
    ArrayList<Integer> list = new ArrayList<>();
    ArrayList<ArrayList<Integer>> res = new ArrayList<>();
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int expectNumber) {
        recursion(root, expectNumber);
        return res;
    }
    public void recursion(TreeNode root, int target) {
        if(root == null) return;
        target = target - root.val;
        list.add(root.val);//记录该结点值
        if(root.left == null && root.right == null && target == 0) {
            res.add(new ArrayList<>(list));//复制一个list加入到res集合中
        }
        recursion(root.left, target);
        recursion(root.right, target);
        list.remove(list.size() - 1);//删除该结点值，即回溯
        return;
    }
}
```

方法2：BFS

这个方法就厉害了，使用队列，同步保存根节点到本结点的全部路径。

核心判断逻辑是，如果遇到叶子结点，则计算队列中路径长度，是否与target相等，相等则将该路径加入结果集合。

简单纯粹且暴力，这才是算法的美妙之处，想到就能写出来。暴力求解才是浪漫。

```java
import java.util.ArrayList;
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
import java.util.*;
public class Solution {
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int expectNumber) {
        //辅助队列
        Queue<TreeNode> nodeQ = new LinkedList<>();
        Queue<ArrayList<Integer>> pathQ = new LinkedList<>();//保存从根结点到本结点的所有结点元素
        //结果集合
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        if(root == null) return res;
        //添加根结点
        nodeQ.offer(root);
        pathQ.offer(new ArrayList<Integer>(Arrays.asList(root.val)));//详见Arrays.asList()用法
        //BFS
        while(!nodeQ.isEmpty()) {
            TreeNode node = nodeQ.poll();//结点出队
            ArrayList<Integer> list = pathQ.poll();//路径出队
            //list.add(node.val);//将该结点值加入路径--这里是错的，list已经包含node结点值。务必思考清楚
            if(node.left == null && node.right == null) {//叶子结点
                int cur_sum = 0;
                for(int i = 0; i < list.size(); i++) {
                    cur_sum += list.get(i);
                }
                if(cur_sum == expectNumber) {//当前路径和等于目标值
                    //因为这里要添加路径，所以才设计了pathQ这个看起来奇怪的队列
                    res.add(list);//注意可以直接保存list本身，因为每次都生成新的list
                }
            }
            if(node.left != null) {
                ArrayList<Integer> left = new ArrayList<>(list);//这里需要新的集合
                left.add(node.left.val);
                nodeQ.offer(node.left);
                pathQ.offer(left);
            }
            if(node.right != null) {
                ArrayList<Integer> right = new ArrayList<>(list);//这里需要新的集合
                right.add(node.right.val);
                nodeQ.offer(node.right);
                pathQ.offer(right);

            }
        }
        return res;
    }
}
```

#### 二叉树中和为某一值的路径（三）

方法1：两次递归遍历

> 辅助函数每次寻找根节点开始的符合条件的路径；
>
> 主函数先序遍历二叉树的所有结点，每个结点都是一个起点，即子树的根结点。
>
> - step 1：每次将原树中遇到的节点作为子树的根节点送入dfs函数中查找有无路径，如果该节点为空则返回。--核心解题思路
> - step 2：然后递归遍历这棵树每个节点，每个节点都需要这样操作。
> - step 3：在dfs函数中，也是往下递归，遇到一个节点就将sum减去节点值再往下。
> - step 4：剩余的sum等于当前节点值则找到一种情况。

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @param sum int整型 
     * @return int整型
     */
    int res = 0;//符合条件的路径总数
    public int FindPath (TreeNode root, int sum) {
        // write code here
        //递归出口
        if(root == null) return res;
        //本级任务
        dfs(root, sum);
        //进入子节点继续找
        FindPath(root.left, sum);
        FindPath(root.right, sum);
        return res;
    }
    //查询以某结点为根的路径数
    public void dfs(TreeNode root, int sum) {
        //递归出口
        if(root == null) return ;
        //本级任务：判断sum与当前结点值是否相同
        if(root.val == sum) res++;
        //进入子节点继续找
        dfs(root.left, sum - root.val);
        dfs(root.right, sum - root.val);
    }

}
```

下面的方法精彩但小众。先贴在这里，以后再看吧。

```java
import java.util.*;
public class Solution {
    //记录路径和及条数
    private HashMap<Integer, Integer> mp = new HashMap<Integer, Integer>(); 
    //last为到上一层为止的累加和
    private int dfs(TreeNode root, int sum, int last){ 
        //空结点直接返回
        if(root == null) 
            return 0;
        int res = 0;
        //到目前结点为止的累加和
        int temp = root.val + last; 
        //如果该累加和减去sum在哈希表中出现过，相当于减去前面的分支
        if(mp.containsKey(temp - sum))  
            //加上有的路径数
            res += mp.get(temp - sum); 
        //增加该次路径和
        mp.put(temp, mp.getOrDefault(temp, 0) + 1);
        //进入子结点
        res += dfs(root.left, sum, temp); 
        res += dfs(root.right, sum, temp); 
        //回退该路径和，因为别的树枝不需要这边存的路径和
        mp.put(temp, mp.get(temp) - 1);
        return res;
    }

    public int FindPath (TreeNode root, int sum) {
        //路径和为0的有1条
        mp.put(0, 1); 
        return dfs(root, sum, 0);
    }
}

```



#### 二叉搜索树与双向链表

方法1：递归

> 显然，如果双向链表结点元素递增，则必须使用中序遍历来遍历二叉搜索树。
>
> **知识点1：二叉树递归**
>
> 递归是一个过程或函数在其定义或说明中有直接或间接调用自身的一种方法，它通常把一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解。因此递归过程，最重要的就是查看能不能讲原本的问题分解为更小的子问题，这是使用递归的关键。
>
> 而二叉树的递归，则是将某个节点的左子树、右子树看成一颗完整的树，那么对于子树的访问或者操作就是对于原树的访问或者操作的子问题，因此可以自我调用函数不断进入子树。
>
> **知识点2：二叉搜索树**
>
> 二叉搜索树是一种特殊的二叉树，它的每个节点值大于它的左子节点，且大于全部左子树的节点值，小于它右子节点，且小于全部右子树的节点值。因此二叉搜索树一定程度上算是一种排序结构。
>
> **具体做法：**
>
> - step 1：创建两个指针，一个指向题目中要求的链表头（head），一个指向当前遍历的前一节点（pre)。
> - step 2：首先递归到最左，初始化head与pre。
> - step 3：然后处理中间根节点，依次连接pre与当前节点，连接后更新pre为当前节点。
> - step 4：最后递归进入右子树，继续处理。
> - step 5：递归出口即是节点为空则返回。

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    TreeNode pre = null;//指向当前结点的前一个结点
    TreeNode head = null;//返回链表首元结点
    public TreeNode Convert(TreeNode pRootOfTree) {
        TreeNode root = pRootOfTree;//根节点的工作指针
        //递归出口
        if(root == null) return null;
        //本级任务
        //完成左子树的转换，或者理解为，递归到最左最小值
        Convert(root.left);
        //找到最小值，初始化head与pre
        if(pre == null) {
            head = root;//用来记录返回值，即链表首元结点
            pre = root;
        //当前结点与上一结点建立连接，将pre设置为当前结点值
        }else {
            pre.right = root;
            root.left = pre;
            pre = root;
            //root = root.right;
        }
        Convert(root.right);
        return head;
    }
}

```

方法2：非递归中序遍历

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
import java.util.*;
public class Solution {
    public TreeNode Convert(TreeNode pRootOfTree) {
        TreeNode cur = pRootOfTree;//工作指针
        TreeNode head = null;
        TreeNode pre = null;
        Stack<TreeNode> stack = new Stack<>();
        boolean flag = true;//标记位，表示该结点是否为首元结点
        while(cur != null || !stack.isEmpty()) {
            while(cur != null) {
                stack.push(cur);
                cur = cur.left;
            }
            TreeNode node = stack.pop();
            if(flag) {
                head = node;
                pre = node;
                flag = false;//更新flag
            }else {
                pre.right = node;
                node.left = pre;
                pre = node;//更新pre
            }
            cur = node.right;
        }
        return head;
    }
}

```

#### 判断是否为平衡二叉树

方法1：先序遍历+判断深度

> 这个方法容易想到，但会产生大量的重复运算，时间复杂度很高。
>
> 思路是构造一个获取当前子树深度的函数`depth()`，通过比较某子树的左右子树的深度差是否成立，来判断子树是否为二叉搜索树。若所有子树平衡，则此树平衡。

![图片说明](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/B894208C51B667B8FA70DD13C43DF19E)

```java
import java.util.*;
import java.lang.*;
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
        if(root == null) return true;
        boolean left = IsBalanced_Solution(root.left);
        boolean right = IsBalanced_Solution(root.right);
        if(left && right && Math.abs(depth(root.left) - depth(root.right)) < 2) {
            return true;
        }
        return false;
    }
    //求二叉树高度
    public int depth(TreeNode root) {
        if(root == null) return 0;
        int left_depth = depth(root.left);
        int right_depth = depth(root.right);
        return Math.max(left_depth, right_depth) + 1;
    }
}
```

方法2：后序遍历+剪枝

> 思路是对二叉树做后序遍历，从底至顶返回子树的深度，如果判定某子树不是平衡树则“剪枝”，直接向上返回。
>
> 设计一个递归函数，
>
> 当结点左右子树深度差小于等于1，则返回当前子树的深度；
>
> 当结点左右子树深度差大于2，则返回-1，代表此子树不是平衡树。

![图片说明](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/0D44B6EFD155E85E52000C8E9D63935F)

```java
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
        if(root == null) return false;
        if(depth(root) == -1) {
            return false;
        }else {
            return true;
        }
    }
    //求解树的高度的同时判断是否为二叉平衡树
    //返回-1表示不是二叉平衡树，否则表示树的高度
    public int depth(TreeNode root) {
        if(root == null) return 0;//空树，是二叉平衡树，返回树的高度0
        int left = depth(root.left);//求左子树的高度
        if(left < 0) return -1;
        int right = depth(root.right);
        if(right < 0) return -1;
        if(Math.abs(right - left) > 1) {
            return -1;//不是二叉平衡树，返回-1
        }else {
            return Math.max(left, right) + 1;//是二叉平衡树，返回树的高度
        }
        //return 0;
    }
}
```

#### 二叉树的下一个结点

方法1：暴力求解

> 输入的是某个结点并不是根节点，所以通过next指针找到根结点；
>
> 根据根节点进行中序遍历，将所有结点保存在集合中；
>
> 遍历集合，找到根结点，下一个就是我们要找的“下一个结点”。--这里的特殊情况是，最后一个结点没有下一个结点，所以直接遍历到倒数第二个结点即可，否则返回null。

```java
/*
public class TreeLinkNode {
    int val;
    TreeLinkNode left = null;
    TreeLinkNode right = null;
    TreeLinkNode next = null;

    TreeLinkNode(int val) {
        this.val = val;
    }
}
*/
import java.util.*;
public class Solution {
    List<TreeLinkNode> list = new ArrayList<>();
    public TreeLinkNode GetNext(TreeLinkNode pNode) {
        //难点：获取根结点
        TreeLinkNode root = pNode;
        while(root.next != null) {
            root = root.next;
        }
        inOrder(root);
        for(int i = 0; i < list.size() - 1; i++) {//难点：最后一个结点不需要遍历
            if(list.get(i) == pNode) {
                return list.get(i + 1);
            }
        }
        return null;
    }
    //中序遍历
    public void inOrder(TreeLinkNode root) {
        if(root == null) return;
        inOrder(root.left);
        list.add(root);
        inOrder(root.right);
    }
}

```

方法2：分类直接寻找

> 仔细观察，可以把中序下一结点归为几种类型：
>
> 1. 有右子树，下一结点是右子树中的最左结点，例如 B，下一结点是 H
> 2. 无右子树，且结点是该结点父结点的左子树，则下一结点是该结点的父结点，例如 H，下一结点是 E
> 3. 无右子树，且结点是该结点父结点的右子树，则我们一直沿着父结点追朔，直到找到某个结点是其父结点的左子树，如果存在这样的结点，那么这个结点的父结点就是我们要找的下一结点。例如 I，下一结点是 A；例如 G，并没有符合情况的结点，所以 G 没有下一结点

![img](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/1078265_1565349624574_CDC679BEBBE282E170AB6FE0DCA8445E)

```java
/*
public class TreeLinkNode {
    int val;
    TreeLinkNode left = null;
    TreeLinkNode right = null;
    TreeLinkNode next = null;

    TreeLinkNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public TreeLinkNode GetNext(TreeLinkNode pNode) {
        //直接分三种情况来解决
        //1.该结点有右子树，则下一个结点是右子树的最左边结点
        if(pNode.right != null) {
            TreeLinkNode cur = pNode.right;
            while(cur.left != null) cur = cur.left;
            return cur;
        }
        //2.该结点无右子树，该节点是父结点的左子结点，下一个结点就是父结点
        while(pNode.next != null && pNode == pNode.next.left) {
            return pNode.next;
        }
        //3.该结点无右子树，该节点是父结点的右子结点，下一个结点是
        //沿着父结点追溯，直到找到“某个结点是其父节点的左子树”--这里不是很好理解
        if(pNode.next != null) {
            TreeLinkNode parentNode = pNode.next;//该结点的父结点
            while(parentNode.next != null && parentNode == parentNode.next.right) {//难点：结合图像来看比较直观
                parentNode = parentNode.next;//向上追溯
            }
            return parentNode.next;//返回这个结点的父结点，即为满足条件的“下一个结点”
        }
        return null;//除了以上三种情况，返回null表示没有“下一个结点”
    }
}

```

#### 对称的二叉树

方法1：层次遍历

> 如果直接层次遍历，需要判断每一层是否是回文串，比较麻烦。
>
> 我们使用两个队列，分别对根节点的左右子树进行层序遍历，这样每次比较弹出来的两个结点值是否相等，来判断是否对称。
>
> 所有结点都判断完毕后，返回true，说明子树是对称的。详见下面这张图。

![alt](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/85DF4B5D7201281FAB466F5273C21994)

```java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
import java.util.*;
public class Solution {
    boolean isSymmetrical(TreeNode pRoot) {
        //利用两个队列，分别遍历左右子树并互相比较
        //这样就不用判断同层是否是回文串了
        TreeNode root = pRoot;
        if(root == null) return true;
        Deque<TreeNode> queue1 = new LinkedList<>();
        Deque<TreeNode> queue2 = new LinkedList<>();
        queue1.offer(root.left);
        queue2.offer(root.right);
        while(!queue1.isEmpty() && !queue2.isEmpty()) {
            TreeNode node1 = queue1.poll();
            TreeNode node2 = queue2.poll();
            if(node1 == null && node2 == null) {//空结点略过
                continue;
            }
            if(node1 == null || node2 == null || node1.val != node2.val) {
                return false;
            }
            //左子树、右子树
            queue1.offer(node1.left);
            queue1.offer(node1.right);
            //右子树、左子树
            queue2.offer(node2.right);
            queue2.offer(node2.left);
        }
        return true;//没有检查到不匹配，说明是对称的。
    }
}

```

方法2：递归

这题的递归还真不好想，看看k神是怎么分析的。

![image-20230320181916890](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/image-20230320181916890.png)

```java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    boolean isSymmetrical(TreeNode pRoot) {
        TreeNode root = pRoot;
        if(root == null) return true;
        return recursion(root.left,root.right);
    }
    //判断两棵树是否镜像对称
    public boolean recursion(TreeNode L, TreeNode R) {
        if(L == null && R == null) return true;
        if(L == null || R == null || L.val != R.val) {
            return false;
        }
        return recursion(L.left, R.right) && recursion(L.right, R.left);//难点在这里，仔细思考一下
    }
}

```

#### 把二叉树打印成多行

经典题型。

```java
import java.util.*;
public class Solution {
    ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        Deque<TreeNode> queue = new LinkedList<>();
        TreeNode root = pRoot;
        if(root == null) return res;//判空
        queue.offer(root);
        while (!queue.isEmpty()) {
            int n = queue.size();
            ArrayList<Integer> list = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                TreeNode node = queue.poll();
                list.add(node.val);
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            res.add(list);
        }
        return res;
    }

}
```

#### 序列化二叉树

> 这题真的细节很多，也是稍微有点复杂度，有实际应用的题目。
>
> 所以需要考虑的东西就比较多，不是那么简单层序遍历。

```java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }
}
*/
import java.util.*;

public class Solution {
    int INF = -1;
    TreeNode emptyNode = new TreeNode(INF);//空结点定义
    String Serialize(TreeNode root) {
        if (root == null) return "";
        Deque<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root);
        StringBuilder sb = new StringBuilder();
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            //int value = node.val;
            sb.append(node.val + ",");//从队列中取出元素进行拼接
            //判断左右子结点是否为空，为空加入空结点
            //注意，这里要首先判断该结点是否为emptyNode结点，如果是则不作任何处理
            if (node.val != -1) {//!node.equals(emptyNode)
                if (node.left == null) {
                    queue.offer(emptyNode);
                } else {
                    queue.offer(node.left);
                }
                if (node.right == null) {
                    queue.offer(emptyNode);
                } else {
                    queue.offer(node.right);
                }
            }
        }
        System.out.println(sb.toString());//1,2,3,-1,-1,6,7,-1,-1,-1,-1,
        return sb.toString();
    }
    TreeNode Deserialize(String str) {
        if(str == "") return null;//空串代表空树
        String[] strArr = str.split(",");
        int n = strArr.length;
        //怎么序列化就怎么反序列化
        TreeNode root = new TreeNode(Integer.parseInt(strArr[0]));//构建根结点
        Deque<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root);
        //遍历字符数组，反序列化二叉树
        for(int i = 1; i < n - 1; i += 2) {//注意这里的下标，难点在于这里的细节，细节有很多
            TreeNode node = queue.poll();//父结点
            int a = Integer.parseInt(strArr[i]);//node的左子结点值
            int b = Integer.parseInt(strArr[i + 1]);//node的右子结点值
            //ab等于-1时，表示空结点，直接不处理，默认指向null
            if(a != -1) {
                node.left = new TreeNode(a);
                queue.offer(node.left);
            }
            if(b != -1) {
                node.right = new TreeNode(b);
                queue.offer(node.right);
            }
        }
        return root;//返回树的根结点
    }
}
```

来自leetcode上的k神的写法，可以学到很多东西。

> 这题算基本功题，在数量掌握二叉树BFS的基础上，
>
> 合理使用api，包括String类转Integer，字符串的截取和拆分成数组，StringBuilder对象删除指定位置的字符等等。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        //层序遍历BFS实现序列化二叉树
        //空树，返回字符串"[]"
        if(root == null) return "[]";
        //保存序列化后的二叉树
        StringBuilder sb = new StringBuilder();
        //遍历二叉树的辅助队列
        //Deque<TreeNode> queue = new ArrayDeque<>();ArrayDeque不能添加null
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        sb.append("[");
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(node != null) {
                sb.append(node.val).append(",");//提高性能
                queue.offer(node.left);
                queue.offer(node.right);
            }else {
                sb.append("null").append(",");
            }
        }
        sb.deleteCharAt(sb.length() - 1);//新api删除指定位置的字符
        sb.append("]");
        //System.out.println(sb.toString());//相比于标准输出，会多输出叶子结点的左右空指针
        return sb.toString();//[1,2,3,null,null,4,5,null,null,null,null]
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        //层序遍历BFS实现反序列化二叉树
        if(data.equals("[]")) return null;
        String[] strArr = data.substring(1,data.length() - 1).split(",");//合理使用api
        Queue<TreeNode> queue = new LinkedList<>();
        TreeNode root = new TreeNode(Integer.parseInt(strArr[0]));//新建根结点
        queue.offer(root);
        int index = 1;
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(!strArr[index].equals("null")) {
                node.left = new TreeNode(Integer.parseInt(strArr[index]));//合理使用api
                queue.offer(node.left);
            }
            index++;
            if(!strArr[index].equals("null")) {
                node.right = new TreeNode(Integer.parseInt(strArr[index]));//合理使用api
                queue.offer(node.right);
            }
            index++;
        }
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

#### 两个结点的公共祖先 *

方法1：路径比较法--暴力求解

> 这题足够简单暴力，但是有个问题不太好解决，如何写findPath求解根结点到目标结点的路径？
>
> 要求将路径保存到ArrayList<Integer> list中，仔细思考下写法。
>
> 体会一下回溯和剪枝终止回溯的方法。

```java
 import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 * }
 */

public class Solution {
    /**
     *
     * @param root TreeNode类
     * @param o1 int整型
     * @param o2 int整型
     * @return int整型
     */
    boolean flag = false; //记录是否找到到o的路径--难点
    public int lowestCommonAncestor (TreeNode root, int o1, int o2) {
        // write code here
        //保存两条路径
        ArrayList<Integer> path1 = new ArrayList<>();
        ArrayList<Integer> path2 = new ArrayList<>();
        //递归
        findPath(root, path1, o1);
        flag = false;//重置flag
        findPath(root, path2, o2);
        //求两条路径最后一个相同的元素并返回
        int res = 0;//临时保存相同的元素
        for (int i = 0; i < path1.size() && i < path2.size(); i++) {
            int x = path1.get(i);
            int y = path2.get(i);
            if (x == y) {
                res = x;
            } else {
                break;
            }
        }
        return res;
    }
    //查找从根结点到某个结点(值)的路径，并返回结果集合
    public void findPath(TreeNode root, ArrayList<Integer> path, int value) {//注意：path为引用传递
        if(root == null || flag) {//找到直接返回（这里可以剪枝，减少时间复杂度）
            return ;
        }
        path.add(root.val);
        if(root.val == value) {
            flag = true;//表示已经找到
            return ;
        }
        findPath(root.left, path, value);
        findPath(root.right, path, value);
        if(flag) return ;//找到，则直接返回，如果不返回而是回溯则会丢失目标结点元素--本题关键
        path.remove(path.size() - 1);//没找到，则回溯
    }
}
```

针对查找从根结点到目标结点（值）的路径，还可以这样写。--来自chatgpt的解答。

> 主要解决了一个问题，就是找到目标结点如何正确返回的问题。前者是通过flag标志，如果flag为true，则表示找到了目标结点，不用回溯当前结点；反之，需要回溯当前结点。
>
> 后者是通过自带的boolean返回值，如果左右子树都是false，则没有找到目标结点，需要回溯；左子树为true，表示左子树中找到目标值，直接返回true，右子树同理，如果都没有找到，则回溯并返回false。

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 * }
 */

public class Solution {
    /**
     *
     * @param root TreeNode类
     * @param o1 int整型
     * @param o2 int整型
     * @return int整型
     */
    boolean flag = false; //记录是否找到到o的路径--难点
    public int lowestCommonAncestor (TreeNode root, int o1, int o2) {
        // write code here
        //保存两条路径
        ArrayList<Integer> path1 = new ArrayList<>();
        ArrayList<Integer> path2 = new ArrayList<>();
        //递归
        findPath(root, path1, o1);
        //flag = false;//重置flag
        findPath(root, path2, o2);
        //求两条路径最后一个相同的元素并返回
        int res = 0;//临时保存相同的元素
        for (int i = 0; i < path1.size() && i < path2.size(); i++) {
            int x = path1.get(i);
            int y = path2.get(i);
            if (x == y) {
                res = x;
            } else {
                break;
            }
        }
        return res;
    }
    //查找从根结点到某个结点(值)的路径，并返回结果集合
    public boolean findPath(TreeNode root, ArrayList<Integer> path,
                         int value) {//注意：path为引用传递
        if (root == null) {
        return false;
    }
    // 将当前节点的值添加到路径中
    path.add(root.val);
    // 如果当前节点的值等于目标值，打印路径并返回true
    if (root.val == value) {
        System.out.println(path);
        return true;
    }
    // 在左子树中查找目标值
    boolean found = findPath(root.left, path, value);
    // 如果在左子树中找到了目标值，返回true
    if (found) {
        return true;
    }
    // 在右子树中查找目标值
    found = findPath(root.right, path, value);
    // 如果在右子树中找到了目标值，返回true
    if (found) {
        return true;
    }
    // 如果左子树和右子树中都没有找到目标值，删除当前节点的值，并返回false
    path.remove(path.size() - 1);
    return false;

    }

}
```

方法2：递归--很简单的写法但是不好理解。

> 这种方法需要拓展原递归函数的含义，具体见注释。
>
> 总之没有提前背过，基本不太可能写出这种方法。

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param root TreeNode类 
     * @param o1 int整型 
     * @param o2 int整型 
     * @return int整型
     */
    public int lowestCommonAncestor (TreeNode root, int o1, int o2) {
        // write code here
        if(root == null) return -1;//表示没有找到最近公共祖先
        if(root.val == o1 || root.val == o2) return root.val;//pq其中一个为root，则直接返回root值
        //o1o2均存在，左子树中找o1o2的最近公共祖先；其中一个不存在，则找存在的一个；都不存在，返回-1
        int left = lowestCommonAncestor(root.left, o1, o2);
        //o1o2均存在，右子树中找o1o2的最近公共祖先；其中一个不存在，则找存在的一个；都不存在，返回-1
        int right = lowestCommonAncestor(root.right, o1, o2);
        if(left == -1 && right == -1) {
            return -1;
        }
        if(left == -1) {
            return right;
        }
        if(right == -1) {
            return left;
        }
        return root.val;//if(left != -1 && right != -1)
    }
}
```

#### 二叉搜索树的最近公共祖先

方法1：直接将二叉搜索树当做二叉树，利用递归

> 方法同上。

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @param p int整型 
     * @param q int整型 
     * @return int整型
     */
    public int lowestCommonAncestor (TreeNode root, int p, int q) {
        // write code here
        if(root == null) return -1;
        if(root.val == p || root.val == q) {
            return root.val;
        }
        int left = lowestCommonAncestor(root.left, p, q);
        int right = lowestCommonAncestor(root.right, p, q);
        if(left != -1 && right != -1) return root.val;
        if(left == -1) return right;
        if(right == -1) return left;
        return -1;//if(left == -1 && right == -1)
    }
}
```

方法2：利用二叉搜索树的性质

> 一个不太好的解答。
>
> 问题在于最后必须要返回，但是返回-1不太恰当因为穷举了所有的可能性，这个return不会执行。那么如何更改呢？
>
> 虽然正确性提高，但是可读性降低了。仁者见仁智者见智吧。

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     *
     * @param root TreeNode类
     * @param p int整型
     * @param q int整型
     * @return int整型
     */
    public int lowestCommonAncestor (TreeNode root, int p, int q) {
        if (root == null) return -1;
        // write code here
        if (root.val == p || root.val == q) {
            return root.val;
        }
        if ((root.val > p && root.val < q) || (root.val < p && root.val > q)) {
            return root.val;
        }
        if (root.val < p && root.val < q) {
            return lowestCommonAncestor(root.right, p, q);
        }
        if (root.val > p && root.val > q) {
            return lowestCommonAncestor(root.left, p, q);
        }
        return -1;
    }

}
```

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     *
     * @param root TreeNode类
     * @param p int整型
     * @param q int整型
     * @return int整型
     */
    public int lowestCommonAncestor (TreeNode root, int p, int q) {
        //空树找不到公共祖先
        if (root == null) return -1;
        // write code here
        if (root.val == p || root.val == q) {
            return root.val;
        }
        //pq在该节点两边说明这就是最近公共祖先
        if ((root.val > p && root.val < q) || (root.val < p && root.val > q)) {
            return root.val;
        }else if (root.val < p && root.val < q) {//pq都在该节点的左边
            return lowestCommonAncestor(root.right, p, q);
        }else{//pq都在该节点的右边
            return lowestCommonAncestor(root.left, p, q);
        }
    }

}
```

方法3：路径比较法

> getPath找到路径并保存到集合中，然后比较两个集合，找到最后一个不同的元素返回即可。
>
> 暴力求解更考验代码基本功。因为是二叉搜索树，所以不需要像普通二叉树那样利用递归求路径，直接循环判断即可。

```java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @param p int整型 
     * @param q int整型 
     * @return int整型
     */
    public int lowestCommonAncestor (TreeNode root, int p, int q) {
        // write code here
        //特例
        if(root == null) return -1;
        //求两条路径
        ArrayList<Integer> path1 = getPath(root, p);
        ArrayList<Integer> path2 = getPath(root, q);
        System.out.println(path1);
        System.out.println(path2);
        //比较两条路径
        int res = 0;
        int index = 0;
        while(index < path1.size() && index < path2.size()) {
            if(path1.get(index) == path2.get(index)) {
                res = path1.get(index);
            }else {
                break;//提前结束
            }
            index++;
        }
        return res;
    }
    public ArrayList<Integer> getPath(TreeNode root, int target) {
        ArrayList<Integer> res = new ArrayList<>();
        TreeNode cur = root;
        if(root == null) return res;
        while(cur.val != target) {
            res.add(cur.val);
            if(target < cur.val) {
                cur = cur.left;
            }
            if(target > cur.val) {
                cur = cur.right;
            }
        }
        res.add(cur.val);//补充target结点值
        return res;
    }
}
```

## 队列和栈

#### 用两个栈实现队列

方法1：

> 这个思路稍微复杂一点，纯个人想法。
>
> 判断栈2是否为空，不为空就依次弹出；
>
> 如果为空，将栈1的元素弹出到栈2，然后弹出一个栈顶元素。
>
> 这样做是为了避免，123进去栈1，然后321进栈2，弹出1，然后又进了4，此时要弹出的是2，所以不能直接把4进栈1然后入栈2，而是等123都从栈2弹出后，元素4才能进栈2.
>
> 稍微有点拗口，我用chatgpt来解释一下。
>
> > 这是一个实现队列的Java代码。在这个队列中，push方法用于向队列中添加元素，pop方法用于从队列中弹出元素。
> >
> > 该队列的内部实现使用了两个栈stack1和stack2。元素首先被添加到stack1中，当需要弹出元素时，如果stack2不为空，则直接从stack2中弹出元素；否则，将stack1中的元素逐个弹出，并依次放入stack2中，然后再从stack2中弹出元素。这样做的目的是为了保证队列的先进先出的原则。
> >
> > 具体来说，push方法将元素压入stack1中，而pop方法则首先判断stack2是否为空，如果不为空，则直接从stack2中弹出元素；否则，将stack1中的所有元素逐个弹出，并放入stack2中，然后再从stack2中弹出元素。这样做的效果是将原来在stack1中的元素顺序反转，从而保证弹出的元素顺序与添加的顺序一致。最后，pop方法返回弹出的元素。

```java
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();

    public void push(int node) {
        //压多少进stack1，弹出放进stack2
        stack1.push(node);
    }

    public int pop() {
        //这里容易出错，如果stack2中还有元素，优先弹出stack2中的元素，不能直接将stack1中的元素弹出放进stack2
        if (!stack2.isEmpty()) {
            return stack2.pop();
        } else {//stack2为空，则弹出stack1的元素进stack2
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
            return stack2.pop();
        }
        //return stack2.pop();
    }
}

```

方法2：

> 方法2思路更简单，但是复杂度更高。
>
> 每次正常将node压栈，取出的时候，每次都将栈1的元素都放入到栈2，栈2的栈顶元素弹出并记录，然后将栈2的元素都放回栈1，同时返回刚刚记录的元素值。

```java
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();

    public void push(int node) {
        //压多少进stack1，弹出放进stack2
        stack1.push(node);
    }

    public int pop() {
        while(!stack1.isEmpty()) {
            stack2.push(stack1.pop());
        }
        int res = stack2.pop();
        while(!stack2.isEmpty()) {
            stack1.push(stack2.pop());
        }
        return res;
    }
}
```

#### 包含min函数的栈

方法1：双栈法、单调栈

```java
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<>();//栈
    Stack<Integer> stack2 = new Stack<>();//辅助栈，保存截止到当前元素的最小值
    
    public void push(int node) {
        stack1.push(node);
        if(stack2.isEmpty()) {//stack2可能为空
            stack2.push(node);
        }else {//stack2不为空的情况
            if(stack2.peek() > node) {
                stack2.push(node);
            }else {
                stack2.push(stack2.peek());
            }
        }
    }
    
    public void pop() {
        stack1.pop();
        stack2.pop();
    }
    
    public int top() {
        return stack1.peek();
    }
    
    public int min() {
        return stack2.peek();//stack2的栈顶元素始终是stack1目前的最小元素值
    }
}

```

核心部分可以更改一下。

```java
public void push(int node) {
        stack1.push(node);
        //空或者新元素较小，则入栈
        if(stack2.isEmpty() || stack2.peek() > node) {
            stack2.push(node);
        }else {//重复加入栈顶
            stack2.push(stack2.peek());
        }
    }
```

#### 栈的压入、弹出序列

> 来自chatgpt的解释。
>
> 这个算法是用来判断一个给定的弹出序列 `popA` 是否合法，也就是是否可以由一个给定的压入序列 `pushA` 得到。算法使用了辅助栈 `stack` 来模拟压入和弹出操作，遍历 `pushA` 数组，依次将元素压入 `stack` 中。在每次压入操作后，需要检查 `stack` 栈顶元素是否与 `popA` 数组当前需要弹出的元素相同，如果相同，则将栈顶元素弹出，并将 `popA` 数组的指针向后移动一位。重复这个过程，直到 `pushA` 数组遍历结束。最后，如果 `stack` 栈为空，则说明 `popA` 序列是一个合法的弹出序列，返回 `true`，否则返回 `false`。

```java
import java.util.*;

public class Solution {
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        Stack<Integer> stack = new Stack<>();//辅助栈
        int index = 0;
        for(int i = 0; i < pushA.length; i++) {//遍历pushA
            stack.push(pushA[i]);
            while(!stack.isEmpty() && stack.peek() == popA[index]) {
                stack.pop();
                index++;
            }
        }
        return stack.isEmpty();//判断栈是否为空，为空则返回true，表示弹出序列正确。
    }
}
```

#### 翻转单词序列

方法1：栈

> 利用str.split(" ")将字符串分割为字符串数组，
>
> 取出字符串数组中的各字符串，依次进栈出栈，
>
> 用res来保存结果并返回。
>
> 注意要弹出栈中多余的一个空格。

```java
import java.util.*;
public class Solution {
    public String ReverseSentence(String str) {
        Stack<String> stack = new Stack<>();
        String[] temp = str.split(" ");
        for(int i = 0; i < temp.length; i++) {
            stack.push(temp[i]);
            stack.push(" ");
        }
        StringBuilder res = new StringBuilder();
        if(!stack.isEmpty()) {//弹出多的空格
            stack.pop();
        }
        while(!stack.isEmpty()) {
            res.append(stack.pop());
        }
        return res.toString();        
    }
}

```

当然，因为已经有字符串数组存在，所以我们直接逆序遍历数组，除了最后一个字符串后不需要加上空格，其他的都需要加一个空格，我们借助StringBuilder提高效率。

```java
import java.util.*;

public class Solution {
    public String ReverseSentence(String str) {
        str = str.trim();
        String[] strArr = str.split(" ");
        StringBuilder res = new StringBuilder();
        int i = 0;
        for(i = strArr.length - 1; i >= 0; i--) {
            res.append(strArr[i]);
            if(i != 0) {
                res.append(" ");
            }
        }
        return res.toString();
    }
}
```

方法2：两次翻转

![alt](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/7269932FDD7F8BA760B50D8A119A60C0)

> 这种方法就不需要借助过多的api，考察基本功。
>
> 以前写过，但是又忘了。写法很多，可以自己尝试多种方法。

```java
public class Solution {
    public String ReverseSentence(String str) {
        char[] chArr = str.toCharArray();//字符串转字符数组
        reverse(chArr, 0, chArr.length - 1);//整体翻转
        for(int i = 0; i < str.length(); i++) {
            int j = i;
            while(j <= chArr.length - 1 && chArr[j] != ' ') {//以空格为界找到第一个单词，注意j可能越界
                j++;
            }
            reverse(chArr, i, j - 1);
            i = j;
        }
        return new String(chArr);//字符数组与字符串的转换
    }
    //字符串翻转函数
    public void reverse(char[] chArr, int left, int right) {
        while(left < right) {//考虑字符数组偶数或者奇数的情况
            swap(chArr, left++, right--);
        }
    }
    //字符交换函数
    public void swap(char[] chArr, int left, int right) {
        char temp = chArr[left];
        chArr[left] = chArr[right];
        chArr[right] = temp;
    }
}

```



#### 滑动窗口的最大值 *

方法1：暴力求解

> 暴力求解，两层循环，第一层为窗口的起点，第二层为窗口的长度，即遍历了所有窗口的每个位置。
>
> - step 1：第一次遍历数组每个位置作为窗口的起点。
> - step 2：从每个起点开始遍历窗口长度，查找其中的最大值。

```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> maxInWindows(int [] num, int size) {
        ArrayList<Integer> res = new ArrayList<>();
        if(size == 0 || size > num.length) return res;
        for(int i = 0; i < num.length - size + 1; i++) {//注意越界
            int max = Integer.MIN_VALUE;
            for(int j = i; j < i + size; j++) {
                max = Math.max(max, num[j]);
            }
            res.add(max);
        }
        return res;
    }
}
```

方法2：单调队列

参考：https://leetcode.cn/problems/sliding-window-maximum/solution/dong-hua-yan-shi-dan-diao-dui-lie-239hua-hc5u/

言简意赅，并且解答的代码也充满了艺术性。

```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> maxInWindows(int [] num, int size) {
        ArrayList<Integer> res = new ArrayList<Integer>();
        if(size == 0) return res;
        ArrayDeque<Integer> queue = new ArrayDeque<>();//双端队列，存数组下标
        for(int right = 0; right < num.length; right++) {
            while(!queue.isEmpty() && num[queue.peekLast()] <= num[right]) {
                queue.pollLast();
            }
            queue.addLast(right);
            int left = right - size + 1;//滑动窗口左边界
            if(queue.peekFirst() < left) {//队列首的元素不在滑动窗口内
                queue.pollFirst();
            }
            if(right + 1 >= size) {//窗口形成
                res.add(num[queue.peekFirst()]);//单调队列
            }
        }
        return res;
    }
}
```

然后这种代码很难第一次就想到，下面这个更容易想到，本质是一样的。

明天再补。这是leetcode上官方解答。

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        //模拟单调队列
        Deque<Integer> deque = new ArrayDeque<>();
        //结果数组
        int[] res = new int[nums.length - k + 1];
        //处理第一个窗口
        for(int i = 0; i < k; i++){
            while(!deque.isEmpty() && nums[i] > deque.peekLast()){
                deque.pollLast();
            }
            deque.offerLast(nums[i]);
        }
        //定义res指针，并更新res
        int index = 0;
        res[index] = deque.peekFirst();
        index++;
        //处理接下来的窗口(本题逻辑重点，主要是画图理解过程)
        for(int j = k; j < nums.length; j++){
            //窗口第一个元素等于队头元素，因为窗口滑动，此时要进行出队操作
            if(!deque.isEmpty() && deque.peekFirst() == nums[j - k]){
                deque.pollFirst();
            }
            //和处理第一个窗口类似，如果更大，就将队列中小的出队；如果更小，就直接入队（本题核心）
            while(!deque.isEmpty() && nums[j] > deque.peekLast()){
                deque.pollLast();
            }
            deque.offerLast(nums[j]);
            res[index] = deque.peekFirst();
            index++;
        }
        //返回结果
        return res;
    }
}
```

补充的leetcode解答牛客版本，其实理解了还是很简单的。

```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> maxInWindows(int [] num, int size) {
        ArrayDeque<Integer> queue = new ArrayDeque<Integer>();//保存元素值
        ArrayList<Integer> res = new ArrayList<Integer>();
        if(size == 0 || num.length < size) return res;
        for(int right = 0; right < size; right++) {//[0,size-1]
            while(!queue.isEmpty() && num[right] >= queue.peekLast()) {
                queue.pollLast();
            }
            queue.offerLast(num[right]);
        }
        res.add(queue.peekFirst());//这里不要忘记了。
        for(int right = size; right < num.length; right++) {//[size,num.length-1]
            if(!queue.isEmpty() && num[right - size] == queue.peekFirst()) {
                queue.pollFirst();
            }
            while(!queue.isEmpty() && num[right] >= queue.peekLast()) {
                queue.pollLast();
            }
            queue.offerLast(num[right]);
            res.add(queue.peekFirst());
        }
        return res;
    }
}
```

## 搜索算法

#### 数字在升序数组中出现的次数 *

方法1：暴力

```java
public class Solution {
    public int GetNumberOfK(int [] array , int k) {
        int res = 0;
        for(int i = 0; i < array.length; i++) {
            if(array[i] == k) {
                res++;
            }
        }
       return res;
    }
}
```

方法2：二分

> 二分是老大难了，看起来简单但是边界条件真的很难处理。
>
> 一个通用的方法是五点七边的模板：https://www.youtube.com/watch?v=JuDAqNyTG4g&t=754s
>
> 解答本题完全可以用到，但显然不是特别美观。

![image-20230403165738906](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/image-20230403165738906.png)

```java
public class Solution {
    public int GetNumberOfK(int [] array, int k) {
        if (array.length == 0) return 0;
        int left_index = binarySearch(array, k);
        //System.out.println(left_index);
        int right_index = binarySearch2(array, k);
        //System.out.println(right_index);
        return right_index - left_index + 1;
    }
    //二分查找：第一个等于target的元素下标
    public int binarySearch(int[] array, int target) {
        int left = -1;
        int right = array.length;
        while (left + 1 < right) { //left + 1 == right退出循环
            int mid = (right - left) / 2 + left;
            if (array[mid] < target) { //isBlue()
                left = mid;
            } else {
                right = mid;
            }
        }
        return right;
    }
    //二分查找：最后一个等于target的元素下标
    public int binarySearch2(int[] array, int target) {
        int left = -1;
        int right = array.length;
        while (left + 1 < right) { //left + 1 == right退出循环
            int mid = (right - left) / 2 + left;//isBlue()
            if (array[mid] <= target) {
                left = mid;
            } else {
                right = mid;
            }
        }
        return left;
    }
}
```

显然二分查找还有其他更加优雅的写法。

```java
public class Solution {
    public int GetNumberOfK(int [] array, int k) {
        if (array.length == 0) return 0;   
        return binarySearch(array, k + 0.5) - binarySearch(array, k - 0.5);
    }
    //二分查找：left指向小于target区间最右，right指向大于等于target区间最左
    public int binarySearch(int[] array, double target) {//double target
        int left = -1;
        int right = array.length;
        while (left + 1 < right) { //left + 1 == right退出循环
            int mid = (right - left) / 2 + left;
            if (array[mid] < target) { //isBlue()
                left = mid;
            } else {
                right = mid;
            }
        }
        return right;
    }
}
```



#### 二维数组中的查找

> 首先看四个角，左上与右下必定为最小值与最大值，而左下与右上就有规律了：**左下元素大于它上方的元素，小于它右方的元素，右上元素与之相反**。既然左下角元素有这么一种规律，相当于将要查找的部分分成了一个大区间和小区间，每次与左下角元素比较，我们就知道目标值应该在哪部分中，于是可以利用分治思维来做。

![图片说明](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/81B83FAE4B34DCEFE9C1EB670AE1CCB0)

```java
public class Solution {
    public boolean Find(int target, int [][] array) {
        //行列数
        int row = array.length;
        int col = array[0].length;
        //特例
        if (row == 0) return false;
        //从左下角开始遍历--本题关键
        for (int i = row - 1, j = 0; i >= 0 &&
                j <= col - 1; ) { //没有“迭代因子”
            if (target > array[i][j]) {
                j++;
            } else if (target < array[i][j]) {
                i--;
            } else {
                return true;
            }
        }
        return false;
    }
}
```

其实这种情况用for挺别扭的，应该是用while更好一些。

```java
public class Solution {
    public boolean Find(int target, int [][] array) {
        //行列数
        int row = array.length;
        int col = array[0].length;
        //特例
        if (row == 0) return false;
        //从左下角开始遍历--本题关键
        int row_index = row - 1;
        int col_index = 0;
        while(row_index >= 0 && col_index <= col - 1) {
            if (target > array[row_index][col_index]) {
                col_index++;
            } else if (target < array[row_index][col_index]) {
                row_index--;
            } else {
                return true;
            }
        }
        return false;
    }
}
```

#### 旋转数组的最小数字

方法1：二分搜索

```java
public class Solution {
    public int GetNumberOfK(int [] array, int k) {
        if (array.length == 0) return 0;   
        return binarySearch(array, k + 0.5) - binarySearch(array, k - 0.5);
    }
    //二分查找：left指向小于target区间最右，right指向大于等于target区间最左
    public int binarySearch(int[] array, double target) {//double target
        int left = -1;
        int right = array.length;
        while (left + 1 < right) { //left + 1 == right退出循环
            int mid = (right - left) / 2 + left;
            if (array[mid] < target) { //isBlue()
                left = mid;
            } else {
                right = mid;
            }
        }
        return right;
    }
}
```

方法2：遍历

```java
import java.util.*;
public class Solution {
    public int minNumberInRotateArray(int [] array) {
        //数组一定有元素
        int res = array[0]; 
        //遍历数组
        for(int i = 1; i < array.length; i++) 
            //每次维护最小值
            res = Math.min(res, array[i]); 
        return res;
    }
}
```

#### 字符串的排列 *

方法1：深度优先遍历+回溯

![image.png](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/1600386643-uhkGmW-image.png)

```java
import java.util.*;
public class Solution {
    //结果集合
    ArrayList<String> res = new ArrayList<>();
    public ArrayList<String> Permutation(String str) {
        //字符串转字符数组
        char[] chArr = str.toCharArray();
        //保存路径
        ArrayList<Character> path = new ArrayList<>();
        //标记元素是否访问过
        boolean[] used = new boolean[chArr.length];
        //排序，便于剪枝
        Arrays.sort(chArr);
        //深度优先遍历
        dfs(chArr, path, used);
        //返回结果集合
        return res;
    }
    //深度优先遍历
    public void dfs(char[] chArr, ArrayList<Character> path, boolean[] used) {
        //递归出口
        if (chArr.length == path.size()) {
            //将字符连接成字符串
            String str = "";
            for (char ch : path) {
                str += ch;
            }
            //添加到结果集合
            res.add(str);
            return;//可以不加
        }
        //遍历数组
        for (int i = 0; i < chArr.length; i++) {
            //元素已经被访问过
            if (used[i]) {
                continue;
            }
            //剪枝
            if(i >= 1 && chArr[i] == chArr[i - 1] && !used[i - 1]) {//剪枝条件
                continue;
            }
            //状态变量
            used[i] = true;
            path.add(chArr[i]);
            //递归
            dfs(chArr, path, used);
            //回溯
            used[i] = false;
            path.remove(path.size() - 1);
        }
    }
}
```

附上leetcode的全排列Ⅱ解答：

这里dfs传入3个参数，其中path可以提出来成为全局变量，效果一样；

这里比牛客的简单一些，因为不需要处理字符串，直接使用数组即可。

```java
class Solution {
    //结果集合
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> permuteUnique(int[] nums) {
        //记录每个结果
        List<Integer> path = new ArrayList<>();
        //标记已访问的元素
        boolean[] visited = new boolean[nums.length];//默认是false
        //数组排序，便于剪枝
        Arrays.sort(nums);
        //深度优先遍历
        dfs(nums, path, visited);
        //返回结果
        return res;
    }
    public void dfs(int[] nums, List<Integer> path, boolean[] visited) {
        //递归出口
        if(nums.length == path.size()) {
            res.add(new ArrayList(path));
            return;
        }
        //本级任务
        for(int i = 0; i < nums.length; i++) {
            //当前元素已经被访问过，直接检查下一个元素
            if(visited[i]) {
                continue;
            }
            //剪枝
            if(i >= 1 && nums[i] == nums[i - 1] && !visited[i - 1]) {//剪枝条件：当前元素与上一个元素相同，且上一个元素刚刚被撤销选择。例子[1,1,2]。
                continue;
            }
            //将当前元素加入路径
            path.add(nums[i]);
            //标记当前元素为已访问
            visited[i] = true;
            //递归遍历剩余元素
            dfs(nums, path, visited);
            //回溯，撤销刚刚的选择
            path.remove(path.size() - 1);
            visited[i] = false;
        }
        
    }
}
```

甚至可以适当简化一下dfs参数。

```java
class Solution {
    //结果集合
    List<List<Integer>> res = new ArrayList<>();
    //记录每个结果
    List<Integer> path = new ArrayList<>();
    //标记已访问的元素
    boolean[] visited = new boolean[8];//默认是false
    public List<List<Integer>> permuteUnique(int[] nums) {
        //数组排序，便于剪枝
        Arrays.sort(nums);
        //深度优先遍历
        dfs(nums);
        //返回结果
        return res;
    }
    public void dfs(int[] nums) {
        //递归出口
        if(nums.length == path.size()) {
            res.add(new ArrayList(path));
            return;
        }
        //本级任务
        for(int i = 0; i < nums.length; i++) {
            //当前元素已经被访问过，直接检查下一个元素
            if(visited[i]) {
                continue;
            }
            //剪枝
            if(i >= 1 && nums[i] == nums[i - 1] && !visited[i - 1]) {//剪枝条件：当前元素与上一个元素相同，且上一个元素刚刚被撤销选择。例子[1,1,2]。
                continue;
            }
            //将当前元素加入路径
            path.add(nums[i]);
            //标记当前元素为已访问
            visited[i] = true;
            //递归遍历剩余元素
            dfs(nums);
            //回溯，撤销刚刚的选择
            path.remove(path.size() - 1);
            visited[i] = false;
        }
        
    }
}
```

#### 数字序列中某一位的数字

方法1：添0补齐

> - step 1：使用循环求n是属于几位数，i从1位数开始，每次给它增加10^i个0，即n位数往后推10^i。
> - step 2：找到目标数字后，通过数字对位数求除法找到该数字，转化为字符串类型。
> - step 3：再通过对位数取模判断是哪一位数。

```java
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     *
     * @param n int整型
     * @return int整型
     */
    public int findNthDigit (int n) {
        // write code here
        //记录n是几位数
        int i = 1;
        while (i * Math.pow(10, i) <
                n) { //每次比较i*10^i与n的大小，n最终落在i位数上（用于确定 n 所在的数字是几位数）
            //前面添0增加的位
            n += Math.pow(10, i); //不断添0，每次添加的都是10^i个0
            i++;
        }
        String num = "" + (n / i);//第num个数组
        return (int) (num.charAt(n % i)) - (int)('0');//结果需要从String转回int
    }
}
```

方法2：位数相减

![image-20230406172752069](https://typora-1256823886.cos.ap-nanjing.myqcloud.com/2022/image-20230406172752069.png)

```java
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     *
     * @param n int整型
     * @return int整型
     */
    public int findNthDigit (int n) {
        // write code here
        //记录是几位数字
        int digit = 1;
        //记录当前区间的起始数字：1，10，100，1000...
        long start = 1;
        //记录当前区间之前一共有多少位数字
        long sum = 9;//初始化为9的原因
        //定位n在哪个区间上
        while (n > sum) { //n <= sum 退出循环
            n = (int)(n - sum);//注意类型转换
            start = start * 10;
            digit++;
            //该区间的总位数
            sum = 9 * digit * start;
        }
        //定位n在哪个数字上
        String num_str = "" + (start + (n - 1) / digit);
        //定位n在数字的哪一位上
        int index = (n - 1) % digit;
        //返回数字
        return (int)(num_str.charAt(index)) - (int)('0');
    }
}
```

## 动态规划

#### 连续子数组的最大和 *

方法1：暴力求解--均超时

时间复杂度为O(n^3)的解法：超时

```java
public class Solution {
    public int FindGreatestSumOfSubArray(int[] nums) {
        int max = Integer.MIN_VALUE;//求最大值的一般初值都是这个
        //int sum = 0;
        for(int i = 0; i < nums.length; i++) {
            for(int j = i; j < nums.length; j++) {
                int sum = 0;//计算sum(i,j)
                for(int k = i; k <= j; k++) {
                    sum += nums[k];
                }
                max = Math.max(max, sum);
            }
        }
        return max;
    }
}
```

时间复杂度为O(n^2)的解法：超时

```java
public class Solution {
    public int FindGreatestSumOfSubArray(int[] nums) {
        int max = Integer.MIN_VALUE;//求最大值的一般初值都是这个
        //int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;//sum(i,j)=sum(i,j-1)+nums[j];
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                max = Math.max(max, sum);
            }
        }
        return max;
    }
}
```

方法2：动态规划

```java
class Solution {
    public int maxSubArray(int[] nums) {
        //定义最大和
        int max = nums[0];
        //dp表示以nums[i]为结尾的连续子数组的最大和
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        for(int i = 1; i < nums.length; i++) {
            if(dp[i - 1] < 0) {
                dp[i] = nums[i];
            }else {
                dp[i] = dp[i - 1] + nums[i];
            }
            max = Math.max(max,dp[i]);
        }
        return max;
    }
}
```

简化一下写法：

```java
class Solution {
    public int maxSubArray(int[] nums) {
        //定义最大和
        int max = nums[0];
        //dp表示以nums[i]为结尾的连续子数组的最大和
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        for(int i = 1; i < nums.length; i++) {
            dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]);
            max = Math.max(max,dp[i]);
        }
        return max;
    }
}
```

方法3：动态规划优化

```java
public int FindGreatestSumOfSubArray(int[] array) {
        int sum = 0;
        int max = array[0];
        for(int i=0;i<array.length;i++){
            // 优化动态规划，确定sum的最大值
            sum = Math.max(sum + array[i], array[i]);
            // 每次比较，保存出现的最大值
            max = Math.max(max,sum);
        }
        return max;
}
```

