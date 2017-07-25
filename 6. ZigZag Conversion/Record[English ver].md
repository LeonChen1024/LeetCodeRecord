
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

First we need to know the meaning about the zigzag pattern, it means a pattern made up of small corners at variable angles, though constant within the zigzag, tracing a path between two parallel lines; it can be described as both jagged and fairly regular. In the begining i think it's one line in the middle and then some symmetry verticle line between the middle horizontal line,when the number of rows is 4(use number to represent the index of the String)it would look like that:

```
1   7    13
2   8    14
3 6 9 12 15
4   10   16
5   11   17
```
but the trueth is not , it should be looked like that:
```
1     7        13
2   6 8     12 14
3 5   9  11    15
4     10       16
```

### Approach 1 use String[]:
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

![Efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/6.%20ZigZag%20Conversion/Images/StringsResult.png?raw=true)

**Analysis**
This method put every line's String into the String[i] accordingly,in the last concate it together and then we get the result we want . When the position is in the first row , next time the position should move down,so step = 1.When the position is in the last Row , next time the position should move up , so the step = -1.

Time complexity ： O(n) 。n is the length of the string.
Space complexity ： O(n) .

### Approach 2 StringBuilder[]:

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

![Efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/6.%20ZigZag%20Conversion/Images/StringBuilderResult.png?raw=true)

**Analysis**
This method's principle is the same as the Approach 1 , it just replace the String into StringBuilder,it has a big optimization.

Time complexity ： O(n) 。n is the length of the string.
Space complexity ： O(n) .


### Approach 3 use interval

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

**Analysis**

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
This method used regular interval to calculate position .

Time complexity ： O(n) 。n is the length of the string.
Space complexity ： O(n) .


If you have any suggestions to make the logic and implementation more better , or you have some advice on my description. Please let me know!Thanks!
