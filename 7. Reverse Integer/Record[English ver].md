
[English ver]Easy
# 7. Reverse Integer
Reverse digits of an integer.

Example1: x = 123, return 321
Example2: x = -123, return -321

Note:
The input is assumed to be a 32-bit signed integer. Your function should return 0 when the reversed integer overflows.

---
spoilers

Have you thought about this?
Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!

If the integer's last digit is 0, what should the output be? ie, cases such as 10, 100.

Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?

For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

---


### Approach 1 use StringBuilder:
``` java
public class Solution {

	    public int reverse(int x) {
          //judge x is negative or not
	        boolean isNegative = false;
	        int result =0;
	        if (x<0){
	            isNegative = true;
	            x = Math.abs(x);
	        }

	        StringBuilder oldSB = new StringBuilder(String.valueOf(x));
	        StringBuilder newSB = new StringBuilder();
          //reverse the x use StringBuilder
	        for (int i = 0;i < oldSB.length() ; i++){
	            newSB.append(oldSB.charAt(oldSB.length()-i-1));
	        }

	        try{
	            result = Integer.valueOf(newSB.toString());
	        }
	        catch(Exception e){
              //if overflows return the result 0
	            return result;
	        }
	        if (isNegative){
	            result = -result;
	        }

	        return result;
	    }
	}
```

![Efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/6.%20ZigZag%20Conversion/Images/StringsResult.png?raw=true)

**Analysis**
This approach turn Integer to a positive Integer and reverse it with StringBuilder. use try catch to deal with the overflows . if the input is negative , we can change the result to negative in last.

Time complexity ： O(n) 。n is the digit of the	x
Space complexity ： O(1) .

### Approach 2 use int:

``` java
public class Solution1 {
   public int reverse(int x)
	{
	    int result = 0;

	    while (x != 0)
	    {
          //get the last digit of the x.
	        int tail = x % 10;
          //add a digit in the end of the newResult ,and update the newResult's last digit
	        int newResult = result * 10 + tail;
          //judge if newResult is overflows
	        if ((newResult - tail) / 10 != result)
	        { return 0; }
	        result = newResult;
          //remove the last digit of x
	        x = x / 10;
	    }

	    return result;
	}
}
```

or

```java
public class Solution2 {
  public int reverse(int x) {
        long rev= 0;
        while( x != 0){
            //add a digit in the end of the newResult ,and update the newResult's last digit
            rev= rev*10 + x % 10;
            //remove the last digit of x
            x= x/10;
            //judge if newResult is overflows
            if( rev > Integer.MAX_VALUE || rev < Integer.MIN_VALUE)
                return 0;
        }
        return (int) rev;
    }
}
```

![Efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/6.%20ZigZag%20Conversion/Images/StringBuilderResult.png?raw=true)

**Analysis**
These two Solution has the same principle . They use the input x%10 to get the remainder which is the last digit of the x, and then use result*10 to add a digit to result and update the last digit to the x%10 , after that update x to x/10.
The only difference of the two solution is the way to judge the result is overflows or not. Solution1 is use the new result remove last digit campare to the old result,if they are equals to each other there is not overflows,else there is overflows. In other side , Solution2 is use the new result compare to the Integer's limit threshold ,if it didn't out of the threshold ,there is not overflows,else there is a overflows, notice that , long  rev = 0; in the code,your can;t use Integer to replace it , because once Integer overflows,java will change the value to make it between the limit threshold of the Integer. So  rev > Integer.MAX_VALUE || rev < Integer.MIN_VALUE will never be true.

Time complexity ： O(n) 。n is the digit of the	x.
Space complexity ： O(1) .

If you have any suggestions to make the logic and implementation more better , or you have some advice on my description. Please let me know!Thanks!
