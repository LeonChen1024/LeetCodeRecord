[Chinese ver]
# 9. Palindrome Number

判断一个整数是不是一个回文。不要使用额外的空间。

---

提示:
负整数可以是回文吗？（比如-1）

如果你想要将整数转换成字符串，注意使用额外的空间的限制。

你还可以尝试反转整数。然而，如果你已经解决了"Reverse Integer" 这个问题，那你应该知道反转一个整数可能会造成溢出。你要如何处理这种情况？

这里还有一个更通用的方法来解决这个问题。

---

### String前后比较:
没有认真审题的做法 ，写完才想起来不能使用额外的空间，尽管如此还是留着吧

``` java
class Solution {
    public boolean isPalindrome(int x) {
        if (x<0){
            return false;
        }
        //turn int x to the String
        String xs = String.valueOf(x);

        //the index of the number left and right
        int indexL = 0;
        int indexR = xs.length()-1;

        //continue to campare the number if the index of left didn't meet the index of right
        while (indexL<indexR){
            if (xs.charAt(indexL)!=xs.charAt(indexR)){
                return false;
            }else{
                indexL ++;
                indexR --;
            }
        }
        return true;

    }
}

```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/8.%20String%20to%20Integer%20(atoi)/Images/OneResult.png?raw=true)

**分析**
首先将 int x 转换成 String ，再将这个 String 的首尾对应的位置的值进行比较。

时间复杂度 ： O(n) 。n是x的位数
空间复杂度 ： O(1) .

### 方法一 反转数字:

```java
public boolean isPalindrome(int x) {

    if (x < 0||(x % 10 == 0 && x != 0)) return false;

    //use to calculate the number we used to revert the number ,and the biggest digit
    int temp = x;
    //the number reverted
    int revertedNumber = 0;

    while (temp >= 10){
        revertedNumber *=10;
        revertedNumber += temp%10;
        temp /=10;
    }
    //compare the revertedNumber and x/10,and the temp and x%10.because the case of overflows
    return revertedNumber == x / 10 && temp == x % 10;
}
```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/8.%20String%20to%20Integer%20(atoi)/Images/OneResult.png?raw=true)

**分析**
因为有可能会发生溢出，所以反转除了最大位以外的x，然后将反转的数字和去除最小一位的x 进行比较，将第一位和最后一位单独再进行比较。

时间复杂度 ： O($log{_10}n$)
空间复杂度 ： O(1) .

### 方法二 反转一半的数字 Revert half of the number:

``` java
class Solution {
    public boolean isPalindrome(int x) {
        // Special cases:
        // As discussed above, when x < 0, x is not a palindrome.
        // Also if the last digit of the number is 0, in order to be a palindrome,
        // the first digit of the number also needs to be 0.
        // Only 0 satisfy this property.
        if(x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int revertedNumber = 0;
        while(x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }

        // When the length is an odd number, we can get rid of the middle digit by revertedNumber/10
        // For example when the input is 12321, at the end of the while loop we get x = 12, revertedNumber = 123,
        // since the middle digit doesn't matter in palidrome(it will always equal to itself), we can simply get rid of it.
        return x == revertedNumber || x == revertedNumber/10;

    }
}

```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/8.%20String%20to%20Integer%20(atoi)/Images/OneResult.png?raw=true)

**分析**
First of all we should take care of some edge cases. All negative numbers are not palindrome, Continuing this process would give us the reverted number with more digits.Since we divided the number by 10, and multiplied the reversed number by 10, when the original number is less than the reversed number, it means we've processed half of the number digits.
首先我们需要注意几个特殊情况：一个是负数是不可能是回文的，还有一个是整十的数也不可能是回文，因为首尾不可能都为0。 然后我们开始反转数字，我们可以用 x%10 获得最后一位数，然后更新 x 为 x/10 去掉已经反转了的最后一位数，然后再用 x%10 再得到现在的最后一位数，将已经得到数乘以10 再加上现在得到的这一位数得到反转的数字，重复这个过程直到我们反转了一半以上的数字。如何知道我们反转了一半以上的数字呢？当x不大于反转的数的时候我们就已经反转了一半以上的数字。
因为回文有奇数和偶数两种，所以我们都要处理，当回文是奇数的时候反转的数字将会比x 多一位，但是这一位是不需要考虑的，我们只要去掉这一位进行比较就可以了，当回文是偶数的时候，如果反转了一半的数字x 不等于反转的数字那么它不是回文，即使x比反转的数字大，使得他又反转了一次，反转的数字将会比x多两位，即使去掉一位也不会导致错误的判断。


时间复杂度 ： O($log{_10}n$) 
空间复杂度 ： O(1) .


如果你有更好的办法或者对我这里的描述有其他看法，请联系我。谢谢
