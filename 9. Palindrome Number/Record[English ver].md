---
title: LeetCode 9. Palindrome Number
date: 2020-01-28
tags:
- English
- Algorithm
- LeetCode
category:
- [English,Algorithm,LeetCode]
toc: true
thumbnail: /2020/01/28/LeetCode-9-Palindrome-Number/Record-cn/titleThumb.jpg
banner: /2020/01/28/LeetCode-9-Palindrome-Number/Record-cn/title.jpg
---



[English ver]

# 9. Palindrome Number

Determine whether an integer is a palindrome. Do this without extra space.

---

click to show spoilers.

Some hints:
Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.

---


### Approach 1 Revert the number:


```java
public boolean isPalindrome(int x) {

    if (x < 0||(x % 10 == 0 && x != 0)) return false;

    //use to calculate the number we used to revert the number ,and the biggest digit
    int temp = x;
    //the number reverted
    int revertedNumber = 0;

    while (temp >= 10){
        revertedNumber *=10;
        revertedNumber += temp%10;
        temp /=10;
    }
    //compare the revertedNumber and x/10,and the temp and x%10.because the case of overflows
    return revertedNumber == x / 10 && temp == x % 10;
}
```

![Efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/9.%20Palindrome%20Number/Images/RevertNumber1Result.png?raw=true)

**Analysis**
Because of the case overflows ,so revert the whole x except the largest digit , and then compare reverted Number with x which except the smallest digit , at last campare the biggest digit and the smallest digit.

Time complexity ： O($log{_10}n$)
Space complexity ： O(1) .


### Approach 2 Revert half of the number:
``` java
class Solution {
    public boolean isPalindrome(int x) {
        // Special cases:
        // As discussed above, when x < 0, x is not a palindrome.
        // Also if the last digit of the number is 0, in order to be a palindrome,
        // the first digit of the number also needs to be 0.
        // Only 0 satisfy this property.
        if(x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int revertedNumber = 0;
        while(x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }

        // When the length is an odd number, we can get rid of the middle digit by revertedNumber/10
        // For example when the input is 12321, at the end of the while loop we get x = 12, revertedNumber = 123,
        // since the middle digit doesn't matter in palidrome(it will always equal to itself), we can simply get rid of it.
        return x == revertedNumber || x == revertedNumber/10;

    }
}

```


![Efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/9.%20Palindrome%20Number/Images/RevertNumberResult.png?raw=true)

**Analysis**

First of all we should take care of some edge cases. All negative numbers are not palindrome, and number x which x % 10 == 0 && x != 0 can not be palindrome ,because the number can't be both 0 in the first digit and last digit. Then we start to convert number , we can use x%10 to get the last digit of x , update x to x/10 ,it remove the last digit ,and then use x%10 to get the last number of x , use the number we have get before to times 10 plus this number. Continuing this process would give us the reverted number with more digits. when the original number is less than the reversed number, it means we've processed half of the number digits.

Because palindrome has odd and even ,so we need to deal with both of them.When the palindrome is odd the reverted number will have one more digit ,but this digit we don't need to consider about ,we only need to remove this digit and compare the rest of it ,when palindrome is even ,if we convert the half of number x ,if they don't equal to each other it is not a palindrome , even x is bigger than reverted number , which make it revert one more time , the reverted number will hava more two digit to x ,remove one digit will nerver make it palindrome.

Time complexity ：O($log{_10}n$)
Space complexity ： O(1) .

If you have any suggestions to make the logic and implementation more better , or you have some advice on my description. Please let me know!Thanks!



# About Me

我的博客 [leonchen1024.com](http://leonchen1024.com/)

我的 GitHub [https://github.com/LeonChen1024](https://github.com/LeonChen1024)

微信公众号 

![wechat](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/Images/CooderQRcodem.jpg?raw=true)