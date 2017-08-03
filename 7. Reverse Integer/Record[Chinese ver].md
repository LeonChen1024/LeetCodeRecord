[Chinese ver]
# 7. Reverse Integer
颠倒一个 integer 的前后位数。

例子: x = 123, return 321
例子: x = -123, return -321

注意：
输入是一个32位的有符号 integer . 当颠倒的 interger 溢出的时候你的函数应该返回0。

---
提示

你是否注意到了这些？
在你编程之前你应该先想想这些问题。如果你已经考虑到了这些那是很加分的！

如果 integer 的最后一位是0，应该输出什么？比如，10，100.

你是否注意到了颠倒的 integer 可能会溢出？假设输入是一个 32位的 integer ，那么1000000003的颠倒溢出了。你该怎么处理这种情况？

为了解决这个问题，假设你的函数当颠倒的 integer 溢出的话返回0.

---

### 方法一 使用StringBuilder：

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

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/6.%20ZigZag%20Conversion/Images/StringsResult.png?raw=true)

**分析**
这个方法使用StringBuilder 将 Integer 转换成正数进行颠倒的操作，使用 try catch 来处理溢出的情况。如果输入是负数，最后的结果在转为负数。
时间复杂度 ： O(n) 。n是输入的位数
空间复杂度 ： O(1) .

### 方法二 使用int：

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

或者


``` java
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

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/6.%20ZigZag%20Conversion/Images/StringBuilderResult.png?raw=true)

**分析**
这两个方法的原理都是一样的，通过将输入的x%10取余数得到最后一位数的值，然后使用 result*10 将result 增加一位并将最后一位更新为余数值，然后更新x为x/10。
这两个方法不同的地方是判断溢出的方法，Solution1 在更新 result 之后将新的 result 去掉最后一位和旧的 result 进行比较，看是否一致来判断是否溢出。Solution2 则是将新的 result 和 Integer 的临界值进行比较来判断是否溢出，要注意的是 Solution2 里的 long rev= 0; 一定不能使用 Integer ，因为一旦 Integer 溢出了 rev 就会被自动处理溢出，所以  rev > Integer.MAX_VALUE || rev < Integer.MIN_VALUE 永远不会为true。

时间复杂度 ： O(n) 。n是输入的位数
空间复杂度 ： O(1) .


如果你有更好的办法或者对我这里的描述有其他看法，请联系我。谢谢
