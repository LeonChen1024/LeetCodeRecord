
[Chinese ver]
## 3.最长的没有重复字符的子字符串

给你一个字符串，得出最长的一个没有重复字符的子字符串的长度。

**例子：**

给定“abcabcbb”，答案是“abc”，长度为3。

给定“bbbbb”，答案是“b”，长度为1。

给定“pwwkew”，答案是“wke”，长度为3.

注意答案必须是一个子字符串，“pwke”是一个子序列，而不是一个子字符串。

先来一个极其繁琐的算法，一开始没有经过太多的思考，导致不断有没考虑到情况发生，不断的修修改改，成了一个极其冗余的代码。。
### 初解

``` java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashMap<String,Integer> map = new HashMap<>();
        int maxSubLength = 0;
        int tempLength = 0;
        boolean isFirstRepeat = true;
        String beginSub = "";
        int bigStartNum = 0;
        for (int i=0;i<s.length();i++){
            beginSub = String.valueOf(s.charAt(i));
            if(map.containsKey(beginSub)){
                if(map.get(beginSub)>bigStartNum){
                    tempLength = i - map.get(beginSub);
                    bigStartNum = map.get(beginSub);
                }else{
                    tempLength = i - bigStartNum;
                }
                if(tempLength>maxSubLength){
                    maxSubLength = tempLength;
                }
                if(isFirstRepeat){
                    tempLength = i;
                    isFirstRepeat =false;
                    if(tempLength>maxSubLength){
                    maxSubLength = tempLength;
                }
                }
            }else{
                 tempLength = i - bigStartNum;
                   if(tempLength>maxSubLength){
                    maxSubLength = tempLength;
                }
            }
            map.put(beginSub,i);
        }
        if((s.length()-bigStartNum)>maxSubLength){
            maxSubLength = (s.length()-bigStartNum-1);
        }
        if(isFirstRepeat){
            return s.length();
        }

        return maxSubLength;
    }
}
```

![这代码。。没脸见人](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/most_tedious_result.png?raw=true)

痛定思过，惨痛的教训告诉我们要深思熟虑后再写代码，这个代码就不分析了，写出来的东西自己都要看不懂了，我连注释都不好意思写了，只是给自己留个教训。。接下来会对这个方法做一个优化和重新构思。

### 二次解

``` java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashMap<String,Integer> map = new HashMap<>();
		//max substring's length
        int maxSubLength = 0;
		//the sub character now
        String nowSub = "";
		//the bigger index which the string can start
        int bigStartNum = -1;
        for (int i=0;i<s.length();i++){
            nowSub = String.valueOf(s.charAt(i));
            if(map.containsKey(nowSub)){
				//if the character repeat,you can start the string from the larger one of the nowSub occur before this time and bigStartNum.
                bigStartNum = Math.max(map.get(nowSub), bigStartNum);
            }
			//the maxSubLength is the maximum number of the length of bigStartNum to i and old maxSubLength
            maxSubLength = Math.max(i - bigStartNum, maxSubLength);
            map.put(nowSub,i);
        }
        return maxSubLength;
    }
}
```

优化之后的代码就很清晰了。重新构思的时候才想到还有一个Math.max()的方法可以用，对api的敏感度还是不够。


![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/promote_self.png?raw=true)

**分析**
这个方法的原理就是将每一个字符进行遍历，为了避免多重循环嵌套，我们使用了hashmap来将时间成本换成空间成本，通过hashmap来判断前面是否有这个字符，并存储他的序号信息。那怎么得到最长的子字符串呢？其实可以分成以下几种情况：
我们在前一个重复的字符加上(p)来表示,后一个重复的字符加上(n)来表示
1.这个字符是第一个重复的字符a，长度应该是从开始到a(n)的前一位。
2.这个字符不是第一个重复的字符a,上一个重复的字符为b，如果a(p)在b(p)的后面那么长度是两个a之间的长度，如果a(p)在b(p)的前面，那么长度是a(n)到b(p)之间(因为a(n)到a(p)之间包含了两个b，这样的字符串是不合规矩的)
3.这个字符没有出现重复的情况，长度就是这个字符到上一个重复字符a(p)出现的位置加一。

整理一下现在的情况，其实以上的条件都可以整理成一种方式来计算，因为子字符串必须是连续的，所以就是计算当前字符到上一个任意重复字符点的长度，然后取出最大的一个。

时间复杂度 ： O(n) .
空间复杂度 ： O(n) .

接下来我们看一下这类题目的几类解法。

### 方法一：暴力循环

``` java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int ans = 0;
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j <= n; j++)
                if (allUnique(s, i, j)) ans = Math.max(ans, j - i);
        return ans;
    }

	//judge that all the characters in s is unique.
    public boolean allUnique(String s, int start, int end) {
        Set<Character> set = new HashSet<>();
        for (int i = start; i < end; i++) {
            Character ch = s.charAt(i);
            if (set.contains(ch)) return false;
            set.add(ch);
        }
        return true;
    }
}
```

![扎心了老铁](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/brute_result.png?raw=true)

**分析**
这个方法的原理很简单，就是最外层的循环L1遍历每一个字符a，嵌套一个遍历a之后所有字符b的循环L2，在L2中又用了一个循环L3用hashset的方式来遍历a到b间以a为开头的所有字符串，并判断是否是没有重复值的字符串。嵌套了三层的循环！！！直接导致了Time Limit Exceeded 的结果，表示你这个方法太耗时了，不予通过。
时间复杂度 ： O(n^3) . 首先是最内层的[i,j)次的循环，时间复杂度为O(j-i)。然后是每一个j的循环[i+1,n),
```
n
∑ O(j−i)
i+1
```
最后算上最外层的a的循环[0,n).时间复杂度为
```
  n-1   n             n-1   (1+n-i)(n-i)
O( ∑  ( ∑ (j-i))) = O(∑    —————————————— ) = O(n^3)
  i=0  j=i+1          i=0         2
```
这个转化过程呢其实就是利用的等差数列的n项和公式，太难打公式了，中间步骤就不详细写了。
![等差数列前n项和](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/Mathematical_Formula/arithmetic%20progression%20_nsum.png?raw=true)

空间复杂度 ： O(min(n,m)) . n就是字符串的长度，m是字母表的字符集的值。因为我们最多就可能有m个不重复的值，所以hashset的size最大也只会是m.

这个方法在逻辑上可以稍微做一个优化，主要的思路就是其实如果a到b已经是包含重复的字符了，那么a到b后面的字符也一定是包含重复字符的。可以省略了那些比较。

### 方法二：滑窗算法（也称K近邻算法）

``` java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Set<Character> set = new HashSet<>();
        int ans = 0, i = 0, j = 0;
        while (i < n && j < n) {
            // try to extend the range [i, j]
            if (!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                ans = Math.max(ans, j - i);
            }
            else {
                set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}
```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/Sliding_Window_result.png?raw=true)

**分析**
方法一重复检查每一个子字符串是否重复，其实没有必要这样，方法二就是避免了多次重复检查，当i~j没有重复时，我们只需要检测s[j + 1]是否和i~j中的字符重复。直到j+1的字符重复了，我们就得到了i字符的最长不重复字符。当j不小于n时就可以停止计算了，因为此时的[i,j)肯定要长于[i+1,j)。

时间复杂度 ： O(2n) = O(n)
空间复杂度 ： O(min(m,n)) n就是字符串的长度，m是字母表的字符集的值。因为我们最多就可能有m个不重复的值，所以hashset的size最大也只会是m.

### 方法三：改良后的滑窗算法

HashMap
``` java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        return ans;
    }
}
```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/Sliding_Window_Optimized_hashmap_result.png?raw=true)

**分析**
这个方法是对方法二的改进，我们使用HashMap来保存出现的字符和他的位置，当出现重复字符时，我们可以直接定位到重复字符的位置(注意这里的位置和程序上的序号是不同的，他的第一位就是1而不是我们程序上的0.)，对比以前的i取较大的值赋值为新的i。为什么取大值可以看上面的二次解

时间复杂度 ： O(n)
空间复杂度 ： O(min(m,n)) 和方法二相同

ASCII 128
``` java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        int[] index = new int[128]; // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            i = Math.max(index[s.charAt(j)], i);
            ans = Math.max(ans, j - i + 1);
            index[s.charAt(j)] = j + 1;
        }
        return ans;
    }
}
```

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/Sliding_Window_Optimized_ascii_result.png?raw=true)

效率最快的一种，在同样的复杂度上为什么会比上一种hashmap的方法快那么多呢，简单点说，其实集合大部分都是由数组构成的，hashmap其实就是由数组加链表封装而成的，所以在速度上数组查找会优于hashmap查找。

**分析**
这个方法原理和上一个使用hashmap的方法大致相同，但是它使用的是int[]来存储键值对，使用字符来做下标(是的，字符可以做下标，准确的来说是整型字符常量可以做下标，会被解析为他的ASCII码值)，位置来做值。

时间复杂度 ： O(n)
空间复杂度 ： O(m) m是字符表的大小

如果你有更好的办法或者对我这里的描述有其他看法，请联系我。谢谢
