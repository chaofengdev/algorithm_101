# 数据结构

## 1.链表

#### 从尾到头打印链表

方法1：

> 顺序遍历链表，将节点值加入到结果集合中，
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

> 1.从表头开始递归进入每一个节点；
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

> 1.使用cur指针指向当前遍历到的节点，pre指向当前节点的前一个节点；
>
> 2.每次post指向cur.next，将cur指向pre，同时pre和cur各后移一位；
>
> 3.直到cur指向null为止，此时pre指向最后一个节点，该节点即为反转后链表的首元节点。

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
            ListNode post = cur.next;//先保存下个节点
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
        //逆转本级节点
        cur.next.next = cur;
        //尾部指针为空
        cur.next = null;
        return newHead;
    }
}
```

