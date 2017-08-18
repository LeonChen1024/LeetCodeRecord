
[English ver]
# 8. String to Integer (atoi)
Implement atoi to convert a string to an integer.

Hint: Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

Notes: It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.

Update (2015-02-10):
The signature of the C++ function had been updated. If you still see your function signature accepts a const char * argument, please click the reload button  to reset your code definition.

---

Requirements for atoi:
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, INT_MAX (2147483647) or INT_MIN (-2147483648) is returned.
---


### Approach 1:
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

or


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


![Efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/8.%20String%20to%20Integer%20(atoi)/Images/OneResult.png?raw=true)

**Analysis**

The principle of these two method are similar ,except the judgement of the overflows ,first method use the long value compare to the Integer's limit value , the another one use Int value , notice that it can't be compare to the Integer's limit value , because the int value will be changed if it overflows , it id never be out of the Integer's range ,  so it use the int value of the last time you get to campare with the Max/10 ,and then use the value you want to connect to the value you get last time to campare with the Max%10 .

Time complexity ： O(n) 。n is the digit of the	x
Space complexity ： O(1) .

If you have any suggestions to make the logic and implementation more better , or you have some advice on my description. Please let me know!Thanks!
