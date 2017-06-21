
##2. Add Two Numbers[Chinese ver]

问题：你将获得两个非空 linked lists来表示两个非负整数。 数字以反向的顺序存储，并且它们的每个节点包含一位数字。 将两个数字相加并将其以 linked list的形式返回。

你可以假定这两个数字不包含任何前导零（即不存在首位出现0的情况），除了数字0本身。

输入 ：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出 ： 7 -> 0 -> 8

###方法一：

```
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
}
```

![效率](https://github.com/LeonChen/LeetCode-record/blob/master/2%20Add%20Two%20Num/Images/method1result.png?raw=true)

####分析

其实这个问题熟悉一下链表的使用和算术的逻辑就可以解决了。按位加过十进一，注意，加法的进位只会是0或者1，因为就算你用两个最大的数9+9最后也只是进一，进一之后再9+9+1也只会得到19，还是进一。所以主要的逻辑就是两个数的和sum/10得到的是进位，sum%10得到的是本位的数。dummyHead是一个假的数据头，用来做一个初始化，不使用也是可以的，但是在内部逻辑中就需要自己在处理第一位和后续的位数的逻辑区别。
注意有几种特殊情况。1.其中一个list的长度比另一个要长。2.其中一个list为空。3.加到最后一位时仍然还有进位

时间复杂度： O(max(m,n)).总共循环m和n中的最大次数。
空间复杂度 :   O(max(m,n)).我们得到的新的list 的长度是m,n中最大长度，加上一个假数据头。

###方法二


```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode ln1 = l1, ln2 = l2, head = null, node = null;
        int carry = 0, remainder = 0, sum = 0;
        head = node = new ListNode(0);

        while(ln1 != null || ln2 != null || carry != 0) {
            sum = (ln1 != null ? ln1.val : 0) + (ln2 != null ? ln2.val : 0) + carry;
            carry = sum / 10;
            remainder = sum % 10;
            node = node.next = new ListNode(remainder);
            ln1 = (ln1 != null ? ln1.next : null);
            ln2 = (ln2 != null ? ln2.next : null);
        }
        return head.next;
    }
}
```

![效率](https://github.com/LeonChen/LeetCode-record/blob/master/2%20Add%20Two%20Num/Images/method2result.png?raw=true)

这个是别人的做法，居然能够比方法一快了许多，说实话，其实这两个方法在整体思路上是差不多的，至于为什么会比前者快那么多我觉的是方法一在循环中不断新建了x,y等对象，由于计时时用了成千上百的例子验证所以导致了20ms左右的差距。以上纯属个人看法，如果你有更正确的想法，或者哪里有有关这种差距的论证，请告诉我。谢谢。
