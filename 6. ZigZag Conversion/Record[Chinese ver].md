[Chinese ver]
# 6. ZigZag Conversion
字符串"PAYPALISHIRING"是通过一个如下给定行数的锯齿模式书写的:(你可能想要使用一个固定的字体来更好的显示它)

```
P   A   H   N
A P L S I I G
Y   I   R
```
然后一行一行的读取这个字符串:"PAHNAPLSIIGYIR"
编写代码实现获取一个字符串然后根据给出的行数来实现这个锯齿转换:
```
string convert(string text, int nRows);
```

convert("PAYPALISHIRING", 3) 应该返回 "PAHNAPLSIIGYIR".

---

首先我们来分析下什么是zigzag pattern,zigzag是锯齿或之字形的意思。一开始我以为是中间都是一行然后旁边是锯齿，当为4行的情况如下(以数字作为字符的index):

```
1   7    13
2   8    14
3 6 9 12 15
4   10   16
5   11   17
```
但是事实并不是这样的，zigzag pattern 4行的情况应该是如下：
```
1     7        13
2   6 8     12 14
3 5   9  11    15
4     10       16
```
字符以这种锯齿形分布。

#### 方法一 使用String[]：

``` java
public class Solution {
    public String convert(String s, int numRows) {
        String result = "";
        String[] strings = new String[numRows];
        for (int i =0;i<numRows;i++){
            strings[i] = "";
        }
        int position = 0 ;
        int numRowsIndex = numRows-1;
        int step = 1;
        if (numRows ==1 || s.length()<numRows){
            return s;
        }
        for (int i =0;i<s.length();i++){
            strings[position] += s.charAt(i);
            if (position ==0){
               step = 1;
            }
            if (position == numRowsIndex ){
                step = -1;
            }
            position = position+step;
        }

        for (int i =0;i<numRows;i++){
            result += strings[i];
        }

        return result;
    }
}
```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/SuccessResult1.png?raw=true)

**分析**
这个方法分为奇偶两种情况进行计算。首先对每一个字符串里的字符进行两种情况的计算，extendPalindrome方法的原理就是当对应的位置的字符相同时，就将左侧字符向左一位，右侧字符向右一位，然后再重复进行这个比较过程。
时间复杂度 ： O(n^2) 。n是字符串的长度
空间复杂度 ： O(n) .

#### 方法二 StringBuilder[]：

``` java
public class Solution {
    public String convert(String s, int numRows) {
        StringBuilder result = new StringBuilder("");
        StringBuilder[] strings = new StringBuilder[numRows];
        for (int i =0;i<numRows;i++){
            strings[i] = new StringBuilder("");
        }
        int position = 0 ;
        int numRowsIndex = numRows-1;
        int step = 1;
        if (numRows ==1 || s.length()<numRows){
            return s;
        }
        for (int i =0;i<s.length();i++){
            strings[position].append(s.charAt(i));
            if (position ==0){
               step = 1;
            }
            if (position == numRowsIndex ){
                step = -1;
            }
            position = position+step;
        }

        for (int i =0;i<numRows;i++){
            result.append(strings[i]);
        }
        return result.toString();
    }
}

```

```java
public class Solution {
    public String convert(String s, int numRows) {
        char[] c = s.toCharArray();
        int len = c.length;
        StringBuffer[] sb = new StringBuffer[numRows];
        for (int i = 0; i < sb.length; i++) sb[i] = new StringBuffer();

            int i = 0;
            while (i < len) {
            for (int idx = 0; idx < numRows && i < len; idx++) // vertically down
                sb[idx].append(c[i++]);
            for (int idx = numRows-2; idx >= 1 && i < len; idx--) // obliquely up
                sb[idx].append(c[i++]);
            }
        for (int idx = 1; idx < sb.length; idx++)
            sb[0].append(sb[idx]);
        return sb[0].toString();
    }
}
```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/5.%20Longest%20Palindromic%20Substring/Images/SuccessResult2.png?raw=true)

**分析**
这个方法主要是每当指针向右移时，我们都以这个位置的字符为结尾看是否能有新的长度为length(current length +1 或者 current length +2)的回文字符串。

时间复杂度 ： O(n^2) 。n是字符串长度。
空间复杂度 ： O(n) .

#### 方法三 间隔算法

```java
public class Solution {
    public String convert(String s, int numRows) {

        String result="";
        if(numRows==1)
            return s;
        int step1,step2;
        int len=s.length();
        for(int i=0;i<numRows;++i){
            step1=(numRows-i-1)*2;
            step2=(i)*2;
            int pos=i;
            if(pos<len)
                result+=s.charAt(pos);
            while(true){
                pos+=step1;
                if(pos>=len)
                    break;
                if(step1>0)
                    result+=s.charAt(pos);
                pos+=step2;
                if(pos>=len)
                    break;
                if(step2>0)
                    result+=s.charAt(pos);
            }
        }
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
