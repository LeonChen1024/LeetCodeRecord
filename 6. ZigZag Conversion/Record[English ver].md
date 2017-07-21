
[English ver]
# 6. ZigZag Conversion
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line: "PAHNAPLSIIGYIR"
Write the code that will take a string and make this conversion given a number of rows:
```
string convert(string text, int nRows);
```
convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".

---

I have misunderstood the meaning of the word palindrome , i thought it was the meaning of repeat,finally i know that it was the meaning of a string which is the same when you look from left to right and from right to left.
According to that , we know it has two situation,one the length is odd , another is even .

### Approach 1 :
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

![Efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/SuccessResult1.png?raw=true)

**Analysis**
This method divided the problem into two situation.First we loop the every char of the String,each char call the extendPalindrome method .The extendPalindrome method is used to find the char of corresponding position is the same or not . if so we move the index of the left char to left one ,and the right char to right one,and then repeat this procedure.

Time complexity ： O(n^2) 。n is the length of the string.
Space complexity ： O(n) .

### Approach 2 :

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

![Efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/SuccessResult2.png?raw=true)

**Analysis**
This method is when then index move to right , we used the char of this index as the end of the substring which length is current length +1 or current length +2, and see if is the Palindrome string .

Time complexity ： O(n^2) 。n is the length of the string.
Space complexity ： O(n) .


### Approach 3 (Manacher's Algorithm)

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

**Analysis**

In this question,the Palindrome has two situation, one is even Palindrome , another is odd Palindrome. In order to combine them into one situation, we can add a special char like '#' between every two char of the string which is neaby ,and the begining and the ending of the string . like  "#a#b#b#c#a#". Now we have combine the two situation into together.

In the String s ,we use rad[i] to represent the Palindrome radius of index i , we can get s[i-rad[i],i-1] = s[i+1,i+rad[i]] easily , once we get the all the rad of the String , we get the all the Palindrome which is even length .

when we get the value of rad[1..i-1],and get the rad value of the char i is at least j through compare char which is symmetry.Now we suppose that we have a index k , it start from 1 to rad[i],we can use it to get the rad valule of the [i+1,i+rad[i]].

According to the acept of the Palindrome , we know the part of the black is a Palindrome, two parts of the red have the same length.
Because we have calculate the value of rad[i-k],so we can use it . There are three situations:
(1)rad[i]-k < rad[i-k]
![Situation1.jpg](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/Situation1.jpg?raw=true)

As the picture , rad[i-k]'s range is blackish green line . Because of the black line is Palindrome,and a part of blackish green line is out of the black line,so rad[i+k] is at least rad[i]-k (the orange line).Is there have any possible that rad[i+k] is larger than rad[i]-k? it's impossible ,according to the acept of the Palindrome ,we know that if the part out of the orange line is the Palindrome , than the black line can expand outward. Is different from the suppose we have done . so rad[i+k] = rad[i]-k。

（2）rad[i]-k > rad[i-k]
![Situation2.jpg](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/Situation2.jpg?raw=true)

As the picture , rad[i-k]'s range is the blackish green line . Because the black line is Palindrome and the blackish green line is in the black line , according to the acept of the Palindrome ,we can get that rad[i+k] = rad[i-k] easily.

According to the situation we have talk , we can get that when rad[i]-k != rad[i-k] ,rad[i+k] = min(rad[i]-k,rad[i-k])。

（3）rad[i]-k = rad[i-k]
![Situation3.jpg](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/Situation3.jpg?raw=true)

As the picture , after compare to the first situation , we can find that because the blackish line is in the black line , so even the orange line is equals , it can't cause a contradiction like situation 1 ,so the orange line can be equals . But , according to the message we know , we have not idear how long is the orange line , so we put the i to the i+k position , j=rad[i-k]\(because it's rad is at least rad[i-k]), wait to next loop to calculate it can expand out or not .


时间复杂度 ： O(n) 。it looks like the program is use the nesting loop ,but actualy it only calculate the i which haven't calculated.
空间复杂度 ： O(n) .


If you have any suggestions to make the logic and implementation more better , or you have some advice on my description. Please let me know!Thanks!
