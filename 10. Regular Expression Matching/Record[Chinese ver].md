[Chinese ver]
# 10. Regular Expression Matching

实现正则表达式匹配，支持 '.' 和 '\*' .

```
'.' 匹配任何单个字符.
'*' 匹配 0 个或多个前一个元素

匹配的字符串必须覆盖整个输入的字符串(而不是一部分)。

函数的形式应该为：
bool isMatch(const char *s, const char *p)

一些例子:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```

---

这里要注意，正则必须要完全匹配输入的字符串，并且 * 号如果选择0个，是将上一个元素直接算不存在，也属于完全匹配。


## 方法一 递归:

```java
class Solution {
    public boolean isMatch(String text, String pattern) {
        if (pattern.isEmpty()) return text.isEmpty();

        boolean first_match = (!text.isEmpty() &&
                               (pattern.charAt(0) == text.charAt(0) || pattern.charAt(0) == '.'));

        //if the pattern.charAt(1) == '*' it means your have two way to get it right , you can keep the '*' and it's preceding element or you can discard they and check the rest of them.
        if (pattern.length() >= 2 && pattern.charAt(1) == '*'){
            return (isMatch(text, pattern.substring(2)) ||
                    (first_match && isMatch(text.substring(1), pattern)));
        } else {
            return first_match && isMatch(text.substring(1), pattern.substring(1));
        }
    }
}
```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/10.%20Regular%20Expression%20Matching/Images/RecursionResult.png?raw=true)

**分析**
使用递归是一种比较易懂的方式，首先将他们的第一位进行比较，有这几种情况，如果pattern下一位是‘\*’，那么它可以舍弃或保留这两位的比较情况。更新text 和 pattern 后继续按位比较即可得出结果。

时间复杂度 ：
```
使用T，P 分别表示 text 和 pattern 的长度。在最差的情况下， isMatch(text[i:], pattern[2j:]) 将会被调用  i+j    次 ，
                                                                                          (  i  )
并且字符串的顺序将会使用 O(T - i) 和 O(P - 2*j) ，因此，复杂度为                    
 T    p/2   i+j                                                      [T+2P]
∑    ∑    (  i  )O(T+P-i-2j) , 经过一些转换我们可以证明他的界限是 O((T+P)2        ).
 i=0  j=0


```

空间复杂度 ：
```
                                                                                       [T+2P]
每一次调用 match，我们会创建上面介绍的字符串，可能会有重复的。如果内存没有被释放，这将会占用 O((T+P)2        ) 的空间，
                   2   2
即使P和T只要求有 O( T + P  ) 个不重复的尾缀。

```

//Todo complexity 


## 方法二 动态规划 :


自顶向下
``` java
enum Result {
    TRUE, FALSE
}

class Solution {
    //cache the intermediate result  
    Result[][] memo;

    public boolean isMatch(String text, String pattern) {
        memo = new Result[text.length() + 1][pattern.length() + 1];
        return dp(0, 0, text, pattern);
    }

    public boolean dp(int i, int j, String text, String pattern) {
        if (memo[i][j] != null) {
            return memo[i][j] == Result.TRUE;
        }
        boolean ans;
        if (j == pattern.length()){
            ans = i == text.length();
        } else{
            boolean first_match = (i < text.length() &&
                                   (pattern.charAt(j) == text.charAt(i) ||
                                    pattern.charAt(j) == '.'));

//if the pattern.charAt(1) == '*' it means your have two way to get it right , you can keep the '*' and it's preceding element or you can discard they and check the rest of them.
            if (j + 1 < pattern.length() && pattern.charAt(j+1) == '*'){
                ans = (dp(i, j+2, text, pattern) ||
                       first_match && dp(i+1, j, text, pattern));
            } else {
                ans = first_match && dp(i+1, j+1, text, pattern);
            }
        }
        memo[i][j] = ans ? Result.TRUE : Result.FALSE;
        return ans;
    }
}
```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/10.%20Regular%20Expression%20Matching/Images/TopDownResult.png?raw=true)



Bottom-Up Variation
自底向上
``` java
class Solution {
    public boolean isMatch(String text, String pattern) {
        boolean[][] dp = new boolean[text.length() + 1][pattern.length() + 1];
        dp[text.length()][pattern.length()] = true;

        for (int i = text.length(); i >= 0; i--){
            for (int j = pattern.length() - 1; j >= 0; j--){
                boolean first_match = (i < text.length() &&
                                       (pattern.charAt(j) == text.charAt(i) ||
                                        pattern.charAt(j) == '.'));
                if (j + 1 < pattern.length() && pattern.charAt(j+1) == '*'){
                    dp[i][j] = dp[i][j+2] || first_match && dp[i+1][j];
                } else {
                    dp[i][j] = first_match && dp[i+1][j+1];
                }
            }
        }
        return dp[0][0];
    }
}
```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/10.%20Regular%20Expression%20Matching/Images/BottomUpResult.png?raw=true)



**分析**
这个方法实际上还是用了方法一中的递归，不过它将大量重复计算的中间值进行了存储，节省了大量的开销。

时间复杂度 ： 使用T，P 分别表示 text 和 pattern 的长度。从 i=0,...,T 每个 i 只会调用一次 dp(i,j) ， j =0,...,P 也是如此。每次调用将会占用 O(1).因此，时间复杂度是 O(TP)
Let T, P be the lengths of the text and the pattern respectively. The work for every call to dp(i, j) for i=0, ... ,T; j=0, ... ,P is done once, and it is O(1) work. Hence, the time complexity is O(TP).
空间复杂度 ： 我们使用的空间只有缓存的 O(TP) 个 boolean 项。因此，空间复杂度是 O(TP).
 The only memory we use is the O(TP) boolean entries in our cache. Hence, the space complexity is O(TP).


如果你有更好的办法或者对我这里的描述有其他看法，请联系我。谢谢
