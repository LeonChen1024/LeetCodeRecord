[Chinese ver]
# 11. Container With Most Water

给定 n 个非负整数 $a_1,a_2,...,a_n$, 他们每一个代表一个坐标点 (i , $a_i$ ). 绘制 n 条竖直的线两端的坐标分别为 (i,$a_i$) 和 (i,0) . 找到两条线，他们和 x轴组成一个容器，使得这个容器能装最多的水。

注意：容器上端的线不能是斜的并且n至少为2.


## 方法一 暴力循环:

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

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/10.%20Regular%20Expression%20Matching/Images/RecursionResult.png?raw=true)

**分析**
不多说了，就是全部的可能性都遍历一遍。

时间复杂度 ：$O(n^2)$ . n 条直线可能性遍历一次为 $\frac{n(n-1)}{2}$

空间复杂度 ：O(1)

## 方法二 两点计算 :

``` java
public class Solution {
    public int maxArea(int[] height) {
        //l 代表左边的坐标， r 代表右边的坐标
        int maxarea = 0, l = 0, r = height.length - 1;
        while (l < r) {
          //组成的面积取决于短的一条线
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

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/10.%20Regular%20Expression%20Matching/Images/TopDownResult.png?raw=true)

**分析**
两条线组合成的面积受短的一条线的长度制约，当短的线不变的时候，面积是由两条线的距离决定的，所以距离近的面积一定小与距离远的。这个方法先使用开始的一点和最后的一点组成的面积，然后将较短的一条线的点向另一边移动，并得出面积比较后留下较大的。重复这个过程得到最大的面积。

时间复杂度 ：O(n) .

空间复杂度 ：O(1)



如果你有更好的办法或者对我这里的描述有其他看法，请联系我。谢谢
