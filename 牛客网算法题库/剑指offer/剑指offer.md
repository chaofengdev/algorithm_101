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

