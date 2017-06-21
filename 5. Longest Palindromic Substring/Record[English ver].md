
[English ver]
# 5. Longest Palindromic Substring
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

I have misunderstood the meaning of the word palindrome , i thought it was the meaning of repeat,finally i know that it was the meaning of a string which is the same when you look from left to right and from right to left.
According to that , we know it has two situation,one the length is odd , another is even .

#### Approach 1 :
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

![Efficiency](https://github.com/LeonChen/LeetCode-record/blob/master/1%20Two%20Sum/Images/WrongResult.png?raw=true)

**Analysis**
This method divided the problem into two situation.First we loop the every char of the String,each char call the extendPalindrome method .The extendPalindrome method is used to find the char of corresponding position is the same or not . if so we move the index of the left char to left one ,and the right char to right one,and then repeat this procedure.

Time complexity ： O(n^2) 。n is the length of the string.
Space complexity ： O(n) .

#### Approach 2 :

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

![Efficiency](https://github.com/LeonChen/LeetCode-record/blob/master/1%20Two%20Sum/Images/BruteForceResult.png?raw=true)

**Analysis**
This method is when then index move to right , we used the char of this index as the end of the substring which length is current length +1 or current length +2, and see if is the Palindrome string .

Time complexity ： O(n^2) 。n is the length of the string.
Space complexity ： O(n) .

If you have any suggestions to make the logic and implementation more better , or you have some advice on my description. Please let me know!Thanks!



Question

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example:

Input: "babad"

Output: "bab"

Note: "aba" is also a valid answer.
Example:

Input: "cbbd"

Output: "bb"
Quick Navigation
Summary
Solution
Approach #1 (Longest Common Substring) [Accepted]
Approach #2 (Brute Force) [Time Limit Exceeded]
Approach #3 (Dynamic Programming) [Accepted]
Approach #4 (Expand Around Center) [Accepted]
Approach #5 (Manacher's Algorithm) [Accepted]
Summary

This article is for intermediate readers. It introduces the following ideas: Palindrome, Dynamic Programming and String Manipulation. Make sure you understand what a palindrome means. A palindrome is a string which reads the same in both directions. For example,
''aba''
''aba'' is a palindome,
''abc''
''abc'' is not.

Solution

Approach #1 (Longest Common Substring) [Accepted]

Common mistake

Some people will be tempted to come up with a quick solution, which is unfortunately flawed (however can be corrected easily):

Reverse SS and become S'S
​′
​​ . Find the longest common substring between SS and S'S
​′
​​ , which must also be the longest palindromic substring.
This seemed to work, let’s see some examples below.

For example,
S
=
''caba"
S=''caba",
S
′
=
''abac''
S′=''abac''.

The longest common substring between SS and S'S
​′
​​  is
''aba''
''aba'', which is the answer.

Let’s try another example:
S
=
''abacdfgdcaba''
S=''abacdfgdcaba'',
S
′
=
''abacdgfdcaba''
S′=''abacdgfdcaba''.

The longest common substring between SS and S'S
​′
​​  is
''abacd''
''abacd''. Clearly, this is not a valid palindrome.

Algorithm

We could see that the longest common substring method fails when there exists a reversed copy of a non-palindromic substring in some other part of SS. To rectify this, each time we find a longest common substring candidate, we check if the substring’s indices are the same as the reversed substring’s original indices. If it is, then we attempt to update the longest palindrome found so far; if not, we skip this and find the next candidate.

This gives us an O(n^2)O(n
​2
​​ ) Dynamic Programming solution which uses O(n^2)O(n
​2
​​ ) space (could be improved to use O(n)O(n) space). Please read more about Longest Common Substring here.

Approach #2 (Brute Force) [Time Limit Exceeded]

The obvious brute force solution is to pick all possible starting and ending positions for a substring, and verify if it is a palindrome.

Complexity Analysis

Time complexity : O(n^3)O(n
​3
​​ ). Assume that nn is the length of the input string, there are a total of \binom{n}{2} = \frac{n(n-1)}{2}(
​2
​n
​​ )=
​2
​
​n(n−1)
​​  such substrings (excluding the trivial solution where a character itself is a palindrome). Since verifying each substring takes O(n)O(n) time, the run time complexity is O(n^3)O(n
​3
​​ ).

Space complexity : O(1)O(1).

Approach #3 (Dynamic Programming) [Accepted]

To improve over the brute force solution, we first observe how we can avoid unnecessary re-computation while validating palindromes. Consider the case
''ababa''
''ababa''. If we already knew that
''bab''
''bab'' is a palindrome, it is obvious that
''ababa''
''ababa'' must be a palindrome since the two left and right end letters are the same.

We define P(i,j)P(i,j) as following:

P
(
i
,
j
)
=
{
true,
if the substring
S
i
…
S
j
 is a palindrome
false,
otherwise.

P(i,j)={true,if the substring Si…Sj is a palindromefalse,otherwise.
Therefore,

P(i, j) = ( P(i+1, j-1) \text{ and } S_i == S_j ) P(i,j)=(P(i+1,j−1) and S
​i
​​ ==S
​j
​​ )

The base cases are:

P(i, i) = true P(i,i)=true

P(i, i+1) = ( S_i == S_{i+1} ) P(i,i+1)=(S
​i
​​ ==S
​i+1
​​ )

This yields a straight forward DP solution, which we first initialize the one and two letters palindromes, and work our way up finding all three letters palindromes, and so on...

Complexity Analysis

Time complexity : O(n^2)O(n
​2
​​ ). This gives us a runtime complexity of O(n^2)O(n
​2
​​ ).

Space complexity : O(n^2)O(n
​2
​​ ). It uses O(n^2)O(n
​2
​​ ) space to store the table.

Additional Exercise

Could you improve the above space complexity further and how?

Approach #4 (Expand Around Center) [Accepted]

In fact, we could solve it in O(n^2)O(n
​2
​​ ) time using only constant space.

We observe that a palindrome mirrors around its center. Therefore, a palindrome can be expanded from its center, and there are only 2n - 12n−1 such centers.

You might be asking why there are 2n - 12n−1 but not nn centers? The reason is the center of a palindrome can be in between two letters. Such palindromes have even number of letters (such as
''abba''
''abba'') and its center are between the two
'b'
'b's.

public String longestPalindrome(String s) {
    int start = 0, end = 0;
    for (int i = 0; i < s.length(); i++) {
        int len1 = expandAroundCenter(s, i, i);
        int len2 = expandAroundCenter(s, i, i + 1);
        int len = Math.max(len1, len2);
        if (len > end - start) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expandAroundCenter(String s, int left, int right) {
    int L = left, R = right;
    while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {
        L--;
        R++;
    }
    return R - L - 1;
}
Complexity Analysis

Time complexity : O(n^2)O(n
​2
​​ ). Since expanding a palindrome around its center could take O(n)O(n) time, the overall complexity is O(n^2)O(n
​2
​​ ).

Space complexity : O(1)O(1).

Approach #5 (Manacher's Algorithm) [Accepted]

There is even an O(n)O(n) algorithm called Manacher's algorithm, explained here in detail. However, it is a non-trivial algorithm, and no one expects you to come up with this algorithm in a 45 minutes coding session. But, please go ahead and understand it, I promise it will be a lot of fun.
