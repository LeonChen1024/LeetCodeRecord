[Chinese ver]
# 8. String to Integer (atoi)

实现一个 atoi 方法来将一个 string 转换成一个 integer.

提示：仔细的思考所有可能的输入情况。如果你想要一个挑战，请不要看以下内容并问你自己什么是可能的输入情况。

注意：模糊的指定输入范围就是这个题目的目的(比如没有给输入规范)。你负责收集前面所有的输入要求。

---

atoi 的要求：
函数首先丢弃所有首个非空格字符之前的所有空格。然后从这个字符开始，得到一个初始化的加号或减号开始的尽可能多的位数，然后将他们转换成数字值。

这个字符串在数字形式的子字符串后可以有其他的字符，这些字符将会被忽略而且不会对这个方法有影响。

如果第一个非空格的字符序列不是一个合法的整数，或者如果并没有符合要求的序列比如字符串是空的或者它只包含了空格字符，不执行转化。

如果不能执行转换，返回0.如果正确值超出了可以描述的值范围，返回 INT_MAX (2147483647) 或者 INT_MIN (-2147483648).
---

### 方法一 :
``` java
public class Solution {
    public int myAtoi(String str) {
        int index = 0 , sign = 1 ;
        long result = 0 ;

				//if str is a empty string or null
        if (str.equals(null)||str.length()==0 ){
            return 0;
        }

				//remove the ' ' in the front
        while (str.charAt(index)==' '&&index < str.length()){
            index++;
        }

				//take the sign
        if(str.charAt(index)=='+'||str.charAt(index)=='-'){
            sign = str.charAt(index)=='+' ? 1 : -1;
            index ++;
        }

				//convert number and avoid overflows
        while (index < str.length()){

            if(str.charAt(index)<='9'&&str.charAt(index)>='0'){
                int current = str.charAt(index)-'0';
                result = result * 10 + current;
								// check if the str is overflows
                if(result>Integer.MAX_VALUE&&sign==1){
                    return Integer.MAX_VALUE;
                }else if(-result<Integer.MIN_VALUE&&sign==-1){
                    return Integer.MIN_VALUE;
                }
                index++;
            }else {
                break;
            }
        }

        return sign*((int)result);

    }
}

```

或者


``` java
public class Solution2 {
	public int myAtoi(String str) {
    int index = 0, sign = 1, total = 0;
    //1. Empty string
    if(str.length() == 0) return 0;

    //2. Remove Spaces
    while(str.charAt(index) == ' ' && index < str.length())
        index ++;

    //3. Handle signs
    if(str.charAt(index) == '+' || str.charAt(index) == '-'){
        sign = str.charAt(index) == '+' ? 1 : -1;
        index ++;
    }

    //4. Convert number and avoid overflow
    while(index < str.length()){
        int digit = str.charAt(index) - '0';
        if(digit < 0 || digit > 9) break;

        //check if total will be overflow after 10 times and add digit
        if(Integer.MAX_VALUE/10 < total || Integer.MAX_VALUE/10 == total && Integer.MAX_VALUE %10 < digit)
            return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;

        total = 10 * total + digit;
        index ++;
    }
    return total * sign;
}
}

```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/tree/master/8.%20String%20to%20Integer%20(atoi)/Images/OneResult.png?raw=true)

**分析**
这两段代码的原理是大致相同的，主要区别就是判断溢出的方式，第一个使用的 long 和 Integer 的区间进行比较来判断是否溢出；第二个使用的是 Int ，但是不能直接和 Integer 的区间进行比较，因为Int 的数据一旦溢出会被处理掉，不会出现区间外的值，所以是将前一次得到的 Int 值和 Max／10 比较，然后将这次取得的数值和 Max%10进行比较

时间复杂度 ： O(n) 。n是输入的位数
空间复杂度 ： O(1) .


如果你有更好的办法或者对我这里的描述有其他看法，请联系我。谢谢
