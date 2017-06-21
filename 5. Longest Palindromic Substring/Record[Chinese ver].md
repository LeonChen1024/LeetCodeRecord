[Chinese ver]
# 5. Longest Palindromic Substring

给定一个字符串s,找出其中最长的回文格式的子字符串。你可以假设长度的最大值为1000.

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.
Example:
```
Input: "babad"

Output: "bab"
```

Note: "aba" is also a valid answer.

Example:
```
Input: "cbbd"

Output: "bb"
```

---

一开始以为palindrome是重复的意思，走了很大的弯路，后来才知道指的是回文格式，就是一个顺着读和反过来读都一样的字符串。
由此可知他有两种情况，一种是奇数的情况，中间的一个字符独立，其余的字符以中间为轴两两对应。另一种是偶数的情况，所有的字符都以中间为轴两两对应。

#### 方法一：
``` java
public class Solution {
       private int lo, maxLen;

       public String longestPalindrome(String s) {
           int len = s.length();
           if (len < 2)
               return s;

           for (int i = 0; i < len - 1; i++) {
               extendPalindrome(s, i, i);  //assume odd length, try to extend Palindrome as possible
               extendPalindrome(s, i, i + 1); //assume even length.
           }
           return s.substring(lo, lo + maxLen);
       }

       private void extendPalindrome(String s, int j, int k) {
           while (j >= 0 && k < s.length() && s.charAt(j) == s.charAt(k)) {
               j--;
               k++;
           }
           if (maxLen < k - j - 1) {
               lo = j + 1;
               maxLen = k - j - 1;
           }
       }
   }
```

![效率](https://github.com/LeonChen/LeetCode-record/blob/master/1%20Two%20Sum/Images/WrongResult.png?raw=true)

**分析**
这个方法分为奇偶两种情况进行计算。首先对每一个字符串里的字符进行两种情况的计算，extendPalindrome方法的原理就是当对应的位置的字符相同时，就将左侧字符向左一位，右侧字符向右一位，然后再重复进行这个比较过程。
时间复杂度 ： O(n^2) 。n是字符串的长度
空间复杂度 ： O(n) .

#### 方法二：

``` java
public class Solution {
    public String longestPalindrome(String s) {
        String res = "";
        int currLength = 0;
        for(int i=0;i<s.length();i++){
            if(isPalindrome(s,i-currLength-1,i)){
                res = s.substring(i-currLength-1,i+1);
                currLength = currLength+2;
            }
            else if(isPalindrome(s,i-currLength,i)){
                res = s.substring(i-currLength,i+1);
                currLength = currLength+1;
            }
        }
        return res;
    }

    public boolean isPalindrome(String s, int begin, int end){
        if(begin<0) return false;
        while(begin<end){
        	if(s.charAt(begin++)!=s.charAt(end--)) return false;
        }
        return true;
    }
}

```

![效率](https://github.com/LeonChen/LeetCode-record/blob/master/1%20Two%20Sum/Images/BruteForceResult.png?raw=true)

**分析**
这个方法主要是每当指针向右移时，我们都以这个位置的字符为结尾看是否能有新的长度为length(current length +1 或者 current length +2)的回文字符串。

时间复杂度 ： O(n^2) 。n是字符串长度。
空间复杂度 ： O(n) .

如果你有更好的办法或者对我这里的描述有其他看法，请联系我。谢谢
