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

首先我们来分析下什么是zigzag pattern,zigzag是锯齿或之字形的意思。是以一定的角度相交的两条线依次以其平行线进行重复相交的线路。一开始我以为是中间都是一行然后旁边是锯齿，当为4行的情况如下(以数字作为字符的index):

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
        //if the numRows is one ,or the s's length is less than numRows ,return s .
        if (numRows ==1 || s.length()<numRows){
            return s;
        }
        //loop the every char in the String, when the position is 0,the step is one ,is the position is numRowsIndex,the step is negative one
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

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/6.%20ZigZag%20Conversion/Images/StringsResult.png?raw=true)

**分析**
这个方法将每一行的字符串存入对应的String[i]中，最后在将String[]按顺序拼接起来就得到了我们要的结果。当目前的位置在第一行的时候，下一个位置应该向下移动，所以step =1 . 当位置在最后一行的时候，下一个位置应该向上移动，所以step =-1.
时间复杂度 ： O(n) 。n是字符串的长度
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
          //if the numRows is one ,or the s's length is less than numRows ,return s .
        if (numRows ==1 || s.length()<numRows){
            return s;
        }
          //loop the every char in the String, when the position is 0,the step is one ,is the position is numRowsIndex,the step is negative one
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

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/6.%20ZigZag%20Conversion/Images/StringBuilderResult.png?raw=true)

**分析**
这个方法原理和方法一一样，只是把String 换成 StringBuilder来处理，效率有了很大的提高

时间复杂度 ： O(n) 。n是字符串的长度
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
```
/*n=numRows
Δ=2n-2    1                           2n-1                         4n-3
Δ=        2                     2n-2  2n                    4n-4   4n-2
Δ=        3               2n-3        2n+1              4n-5       .
Δ=        .           .               .               .            .
Δ=        .       n+2                 .           3n               .
Δ=        n-1 n+1                     3n-3    3n-1                 5n-5
Δ=2n-2    n                           3n-2                         5n-4
*/
```
这个方法主要是通过间隔的规律来进行计算和排位。

时间复杂度 ： O(n) 。n是字符串长度
空间复杂度 ： O(n) .

如果你有更好的办法或者对我这里的描述有其他看法，请联系我。谢谢
