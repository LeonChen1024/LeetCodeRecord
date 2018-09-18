
# 5. Longest Palindromic Substring

给定一个字符串s,找出其中最长的回文格式的子字符串。你可以假设长度的最大值为1000.

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

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/SuccessResult1.png?raw=true)

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

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/SuccessResult2.png?raw=true)

**分析**
这个方法主要是每当指针向右移时，我们都以这个位置的字符为结尾看是否能有新的长度为length(current length +1 或者 current length +2)的回文字符串。

时间复杂度 ： O(n^2) 。n是字符串长度。
空间复杂度 ： O(n) .

#### 方法三 Manacher算法

```java
public class Solution {
    public String longestPalindrome(String s) {
      char[] str = changeString(s);
		  String result = manacher(str,s);

		  return result;
}

/**
	 * 返回例如 #a#c#b#c#a#a#c#b#c#d#形式的字符串数组
	 *
	 * @param s
	 * @return
	 */
	public static char[] changeString(String s)
	{
	    char[] str = new char[s.length() * 2 + 1];

	    int i = 0;
	    for (; i < s.length(); i++)
	    {
	        str[2 * i] = '#';
	        str[2 * i + 1] = s.charAt(i);
	    }
	    str[2 * i] = '#';

	    return str;
	}

	/**
	 * manacher 算法实现找到回文字符串最长的一个
	 */
	public static String manacher(char[] s,String olds)
	{
		  String result = "";
	    int rad[] = new int[s.length];
	    int start = 0;
	    int end = 0;
	    //i index,j 回文半径，k
	    int i = 1, j = 0, k;

	    // 记录最长的回文串的长度
	    int maxLen = 0;
	    while (i < s.length)
	    {
	        // 扫描得出rad值
	        while (i - j - 1 > -1 && i + j + 1 < s.length
	                && s[i - j - 1] == s[i + j + 1]) {
	        	  j++;
	        }

	        if (maxLen < j ) {
				        maxLen =j;
				        start = i - j  ;
				        end =i + j ;

			       }
	        rad[i] = j;
	        maxLen = maxLen > j ? maxLen : j;

	        k = 1;
          //当回文中包含子回文，看子回文是否超出父回文边界。 分三种情况。
	        while (k <= rad[i] && rad[i - k] != rad[i] - k)
	        {
	            rad[i + k] = Math.min(rad[i - k], rad[i] - k);
	            k++;
	        }
	        i = i + k;
	        j = Math.max(j - k, 0);
	    }

	    result = olds.substring(start/2,end/2);
	    return result;

	}

}

```

**分析**
在这个问题中，回文的情况一共有两种，一种是奇数回文，一种是偶数回文，为了将他们合并成一种情况，我们可以在首尾和每两个字符中间加上一个特殊字符，如‘#’，形如"#a#b#b#c#a#".这样我们就将所有的回文情况合并成奇数回文的情况。

在字符串s中，我们用rad[i]表示第i个字符的回文半径，可以得到s[i-rad[i],i-1] = s[i+1,i+rad[i]]，只要求出了所有的rad，就求出了所有奇数长度的回文子串。

当我们得到了rad[1..i-1]的值，并通过比较对称字符得到当前字符i的rad值至少为j，求出了rad[i]。现在我们设一个指针k，从1循环到rad[i]，以此来求出[i+1,i+rad[i]]的rad值。

根据定义，黑色的部分是一个回文子串，两段红色的区间全等。
因为之前已经求出了rad[i-k]，所以直接用它.有3种情况：
（1）rad[i]-k < rad[i-k]
![Situation1.jpg](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/Situation1.jpg?raw=true)

如图,rad[i-k]的范围为墨绿色。因为黑色的部分是回文的，且墨绿色的部分超过了黑色的部分，所以rad[i+k]至少为rad[i]-k，即橙色的部分。有没有可能rad[i+k]的值大于rad[i]-k呢？这是不可能发生的，根据回文的特性我们知道，如果橙色部分以外也是回文，那么黑色的回文部分就可以向外拓展。与假设冲突。因此rad[i+k] = rad[i]-k。

（2）rad[i]-k > rad[i-k]
![Situation2.jpg](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/Situation2.jpg?raw=true)

如图,rad[i-k]的范围为墨绿色。因为黑色的部分是回文的，且墨绿色的部分在黑色的部分里面，根据回文特性，很容易得出：rad[i+k] = rad[i-k]。

根据上面两种情况，可以得出结论：当rad[i]-k != rad[i-k]的时候，rad[i+k] = min(rad[i]-k,rad[i-k])。

（3）rad[i]-k = rad[i-k]
![Situation3.jpg](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/Situation3.jpg?raw=true)
如图，通过和第一种情况对比之后会发现，因为墨绿色的部分没有超出黑色的部分，所以即使橙色的部分全等，也无法像第一种情况一样引出矛盾，因此橙色的部分是有可能全等的。但是，根据已知的信息，我们不知道橙色的部分是多长，因此就把i指针移到i+k的位置，j=rad[i-k]\(因为它的rad值至少为rad[i-k])，是否可以向外拓展等下个循环再做。

时间复杂度 ： O(n) 。看起来程序里用到了循环嵌套，但是实际上他只对没有计量过的i进行计量。
空间复杂度 ： O(n) .

如果你有更好的办法或者对我这里的描述有其他看法，请联系我。谢谢

## About Me

我的博客 [leonchen1024.com](http://leonchen1024.com/)

我的 GitHub [https://github.com/LeonChen1024](https://github.com/LeonChen1024)

微信公众号 

![wechat](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/Images/CooderQRcodem.jpg?raw=false)
