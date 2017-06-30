
[Chinese ver]1.两数求和 。给定一个整数的数组，返回两个数字的索引使得这两个数字加起来成为一个指定的目标值。
你可以假设每个输入都至少有一个解决方案，并且你不能使用相同的元素两次。

**Example:**
```
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

首先是一次错误的尝试
``` java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] testNums = new int[nums.length];
        int j = 0;
        for (int i=0;i<nums.length;i++){
            if(nums[i]<target){
                testNums[j] = nums[i];
                j=j+1;
            }
        }
        for(int l=0;l<j;l++){
            for(int k=l+1;k<j;k++){
                if(testNums[l]+testNums[k]==target){
                    return new int[]{l,k};
                }
            }
        }
      return null; 
    }
}

```

![出错啦！！](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/1%20Two%20Sum/Images/WrongResult.png?raw=true)


没理解好题意，这种解法最后得到的是我们自定义数组的序号，而不是题目要求的序号，晕倒。。。后来发现一个更严重的问题。。他没有说过不能有负数！！！！也就不需要判断数组里的值是否大于和。。。

然后重新构思一下，先来个基本的解法
#### 方法一：暴力循环
``` java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i=0;i<nums.length;i++){
           for(int k=i+1;k<nums.length;k++){
                if(nums[i]+nums[k]==target){
                    return new int[]{i,k};
                }
            }
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```

至于结果嘛，最简单的自然也高效不到哪里。
![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/1%20Two%20Sum/Images/BruteForceResult.png?raw=true)

**分析**
这个方法的原理很简单，就是将每一个值与其他的值循环遍历，看是否有符合条件的情况发生。稍微要注意的是   for(int k=i+1;k<nums.length;k++) 这里k=i+1是为了避免前面循环过的情况再次循环一遍。
时间复杂度 ： O(n^2) 。n个元素都要循环遍历数组内其余的元素，所以是n^2。
空间复杂度 ： O(1) .

#### 方法二：两次循环的 hash table

``` java
public class Solution {
   public int[] twoSum(int[] numbers, int target) {
    Map<Integer, Integer> map = new HashMap<Integer, Integer>();
    for (int i = 0; i < numbers.length; i++) {
        map.put(numbers[i],i);
    }
   for (int i = 0; i < numbers.length; i++) {
       int requestNum = target - numbers[i];
        if (map.containsKey(requestNum)&&map.get(requestNum)!=i) {
            return new int[]{i,map.get(requestNum)};
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
}
```
可以看到效率有了很大的提升。

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/1%20Two%20Sum/Images/Twopassresult.png?raw=true)

**分析**
这个方法的原理其实就是使用hash table 来将时间成本来替换空间成本，将一次复杂度为O(n)的循环变为接近O(1)的查找，为什么是接近呢，因为如果hash table发生大量的碰撞，就会导致复杂度向O(n)靠近。我们将数组里每一个元素的值作为key存入hash table，而将其序列号作为对应的value存入hash table，然后遍历数组查找是否有对应的值在hash table 的key中，有则取出该key对应的value。
时间复杂度 ： O(n)
空间复杂度 ： O(n)

#### 方法三：一次循环的 hash table

``` java
public class Solution {
   public int[] twoSum(int[] numbers, int target) {
    Map<Integer, Integer> map = new HashMap<Integer, Integer>();
    for (int i = 0; i < numbers.length; i++) {
       int requestNum = target - numbers[i];
        if (map.containsKey(requestNum)) {
            return new int[]{map.get(requestNum),i};
        }
        map.put(numbers[i],i);
    }
    throw new IllegalArgumentException("No two sum solution");
}
}
```
效率稍微提高了一些。。但是有些不能接受啊。怎么还只是在中间的位置。。。然后试了几次，大概在40%到50%徘徊，前面那些是用什么算的。。。为何那么快。理论上来说至少需要循环比对结果一次，也就是需要O(n)的复杂度。

![效率](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/1%20Two%20Sum/Images/One-pass_result.png?raw=true)

**分析**
这个方法的原理其实就是方法二的一个改善，因为我们不需要将全部的数组都放入hash table ，我们最终的目的是为了得到两个相加等于目标数的值的序号即可，所以我们在将数组里的值放入hash table 的时候就进行比对，一旦得到所需要的值立即结束循环。
时间复杂度 ： O(n)
空间复杂度 ： O(n)

如果你有更好的办法或者对我这里的描述有其他看法，请联系我。谢谢
