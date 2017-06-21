
[English ver]Given a string, find the length of the longest substring without repeating characters.

**Example:**
```
Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

First time i get a very tedious algorithm, because of i did not thinking about the problem carefully in the beginning, it causes that i hava to fix bug again and again because of the situation i haven't think about . this makes code become very redundant .

####The solution for the first time

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

![shame on this code ](https://github.com/LeonChen/LeetCode-record/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/most_tedious_result.png?raw=true)

i have thinking about did i have problems in the way i tried before, the twists before let me realize to think carefully and then write the code,i am not going to analyze this code, because i almost can't understand what i writen, I even  embarrassed to write the annotate, just leave A lesson to me. The next step i will optimize and rethink this approach.

####The solution for the second time

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

After the optimize ,the code is very clear. I haven't thought that there is a Math.max () method can be used util i re-conceived the code, still need more sensitivity of the api.

![效率](https://github.com/LeonChen/LeetCode-record/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/promote_self.png?raw=true)

**Analysis**
The principle of this method is to traverse each character, in order to avoid nest loop, we use the hashmap to replace the time cost of space costs,determine whether there is a character in front by hashmap, and store his index number information. How do we get the longest substring? In fact, it can be divided into the following situations:

We use (p) to represent the previous repeat character, (n) to represent the latter repeat character.
1. The character a is the first repeat character, and the length should be from the beginning to the location a(n)-1.
2. the character a is not the first repeat character, the last repeat character is b, if a(p) is behind b(p) then the length is between two a, if a(p) is in front of b(p), then the length is between a(n) and b(p) (because a(n) to a(p) contains two b, which is irregular)
3. This character does not repeat, the length is the character to location of the last repeat character a(p)+1.

Sort out the current situation util now .in fact, the conditions above can be sorted into one way to calculate, because the sub-string must be continuous, so it is to calculate the length between current character and the repeat character previous one ,and then get the largest length.

Time complexity: O(n).
Space complexity: O(n).

and then we look some type solution of this kind problem.

####Approach 1：Brute Force

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

![heartbreak](https://github.com/LeonChen/LeetCode-record/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/brute_result.png?raw=true)

**Analysis**
The principle of this method is very simple, the outermost loop L1 traverse of every character a, nest a loop L2 traverse of b (all the characters which is after a), use another loop with the hashset to traverse all the string between a and b which is beginning with a, and determine that if it is a string with no repeat character. Nested a three-tier loop! it lead to the results of Time Limit Exceeded, that means your method is too time-consuming, is not a good method.

Time complexity: O(n^3) . The first is the innermost [i, j) times cycle, the time complexity is O(j-i). And then is every cycle of j which takes [i + 1, n),
```
n
∑ O(j−i)
i+1
```
Finally count the outermost loop [0, n). The time complexity is
```
  n-1   n             n-1   (1+n-i)(n-i)
O( ∑  ( ∑ (j-i))) = O(∑    —————————————— ) = O(n^3)
  i=0  j=i+1          i=0         2
```

This transformation process is actually the use of the sum of the first n terms of an arithmetic progression, is too difficult to print the formula on my computer, i am not going to print the detail of the steps .
![the sum of the first n terms of an arithmetic progression](https://github.com/LeonChen/LeetCode-record/blob/master/Mathematical_Formula/arithmetic%20progression%20_nsum.png?raw=true)

Space complexity: O(min(n,m)) . n is the length of the string,m is the set of the size of charset.Because there are m didn't repeat value at most,so the hashset's size will be m at most.

This method can make a little optimization logically , the main idea is that if from a to b is already containing repeated characters, then a to the characters after b must also contain repeated characters. You can omit those comparisons.


####Approach 2：Sliding Window（K-Nearest Neighbor algorithm）

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

![efficiency](https://github.com/LeonChen/LeetCode-record/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/Sliding_Window_result.png.png?raw=true)

**Analysis**

Method 1 is repeatly to check each sub-string has duplicate character,but in fact, it is unnecessary, the second method is avoid repeated checks, when i ~ j has not repeat, we only need to check s[j + 1] is already in i ~ j characters . Until the characters of j + 1 is already in i~j, we get the longest non-repetitive substring which start with i character. we can stop the calculation When j is not less than n , because at this time [i, j) must be longer than [i + 1, j).

Time complexity: O(2n) = O(n)
Space complexity: O(min(m,n)) n is the length of the string,m is the size of the charset.Because there are m didn't repeat value at most,so the hashset's size will be m at most.

####Approach 3:Sliding Window Optimized

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

![efficiency](https://github.com/LeonChen/LeetCode-record/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/Sliding_Window_Optimized_hashmap_result.png?raw=true)

**Analysis**

This method is the improvement of the method two, we use the HashMap to save the characters and his position, when there are duplicate characters appeared, we can directly locate the location of the repeated characters (Note that the location here and the program's index number is different , it's first is 1 rather than our program is 0.), compared to the previous i and take a larger value assigned to the new i. Why should we take a large value can see the above solution for the second time.

Time complexity： O(n)
Space complexity： O(min(m,n)) the same to the method 2

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

![efficiency](https://github.com/LeonChen/LeetCode-record/blob/master/3.%20Longest%20Substring%20Without%20Repeating%20Characters/Images/Sliding_Window_Optimized_ascii_result.png?raw=true)

The most efficient one, the reason of this method can run more fast than the previous hashmap while they have the same complexity, simply said, in fact of most collection is composed of arrays, hashmap is actually composed by the array and the list , So the array's search speed will be better than the hashmap search speed.

**分析**

This method's principle is almost the same as the previous method which is using hashmap, but this method uses int[] to store key-value pairs, using characters to represent subscripts (yes, characters can be used as subscripts, exactly is Integer character constants can be used as subscript, it will be parsed to ASCII value), the location use as value.
Time complexity： O(n)
Space complexity： O(m) m is the charset's size.

If you have a better method or have other opinions on the description of me here, please contact me. Thank you.
