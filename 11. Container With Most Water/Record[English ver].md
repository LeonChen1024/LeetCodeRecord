[English ver]
# 11. Container With Most Water

IGiven n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.



Time complexity : O(n^2)O(n
​2
​​ ). Calculating area for all \frac{n(n-1)}{2}
​2
​
​n(n−1)
​​  height pairs.
Space complexity : O(1)O(1). Constant extra space is used.



### Approach 1 Brute Force :


```java
public class Solution {
    public int maxArea(int[] height) {
        int maxarea = 0;
        for (int i = 0; i < height.length; i++)
            for (int j = i + 1; j < height.length; j++)
                maxarea = Math.max(maxarea, Math.min(height[i], height[j]) * (j - i));
        return maxarea;
    }
}
```

![Efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/10.%20Regular%20Expression%20Matching/Images/RecursionResult.png?raw=true)

**Analysis**
it is simple . just trase all possible case.

Time complexity ：$O(n^2)$ . Calculating area for all $\frac{n(n-1)}{2}$

Space complexity ：O(1)

## Approach 2 Two Pointer :

``` java
public class Solution {
    public int maxArea(int[] height) {
        //l represent the left index , r represent right index
        int maxarea = 0, l = 0, r = height.length - 1;
        while (l < r) {
          //area is limit by shorter line
            maxarea = Math.max(maxarea, Math.min(height[l], height[r]) * (r - l));
            if (height[l] < height[r])
                l++;
            else
                r--;
        }
        return maxarea;
    }
}
```

![Efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/10.%20Regular%20Expression%20Matching/Images/BottomUpResult.png?raw=true)

**Analysis**

The area formed by two lines will always limit by the shorter one . when shorter one didn't changed , the area will be limit by the distance of two lines ,  the area will be smaller if the distance is shorter than another one . This approach use the beginning of the array and the end of the array which store the length of the lines , and then get the area of these two lines , move the point of the shorter line to the another side , get the area and keep the larger one . Repeat this process finally get the largest area .


Time complexity ： O(n)

Space complexity ：O(1)

If you have any suggestions to make the logic and implementation more better , or you have some advice on my description. Please let me know!Thanks!
