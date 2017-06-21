
Question ：You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

###Approach 1：
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

![Efficiency](https://github.com/LeonChen/LeetCode-record/blob/master/2%20Add%20Two%20Num/Images/method1result.png?raw=true)

####Analysis

Actually, the problem is easy to resolved if you are familiar with the use of the list and the logic of the arithmetic . Plus every bit base on decimal, note that the addition of the carry will only be 0 or 1, because even if you use the two largest number 9 +9 in finally you only get carry 1, then use the largest number 9 +9 + 1 will only Get 19, the carry is 1 too. So the main logic is (the sum of two numbers )sum / 10 is the carry, sum% 10 is the number of the this bit . DummyHead is a fake data header, used to do an initialization, it is possible if you don't want to use it, but in the internal logic you need to deal with the difference logic between the first bit of the result and the number of subsequent .
Note that there are several special cases. 1. The length of one list is longer than the other. 2. One of the lists is empty. 3. There is still have a carry in the last bit

Time complexity: O (max (m, n)). The total number of cycles is the largest number between m and n.
Space complexity: O (max (m, n)). The length of the new list we create is the maximum length of m, n, plus a dummy header.


###Approach 2

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

![Efficience](https://github.com/LeonChen/LeetCode-record/blob/master/2%20Add%20Two%20Num/Images/method2result.png?raw=true)

This is someone else's approach, actually these two methods in the overall thinking is almost the same ,but this approach is more faster than approach 1 . why ? I think the approach 1 is always create a int x,y and other objects in the cycle , because the timing was based on thousands of examples , result a gap about 20ms. This is purely personal think , if you have more correct ideas, or somewhere is an argument about the reason this gap, please tell me. Thank you.
