# [English ver]1 Two Sum



[TOC]

## Question

Two SumGiven an array of integers, return **indices** of the two numbers such that they add up to a specific target.
You may assume that each input would have ***exactly*** one solution, and you may not use the *same* element twice.

**Example:**
```
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

First time, i got a wrong try.
```
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

![wrong！！](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/1%20Two%20Sum/Images/WrongResult.png?raw=true)

I did not understand the meaning of problem , the last we get by this solution are the index of the array which we created , rather than the requirements of the index of he gave , fainted. . . Later  i had found a more serious problem. . The problem did not say there can not be negative number ! . . . It is not necessary to determine whether the value of the array is bigger than the target number. . .

And then i re-think about it, the first solution is a basic way.

## Approach 1：Brute Force

```
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

for the results,uh Well, the simplest solution often means the less efficient way.

![efficiency ](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/1%20Two%20Sum/Images/BruteForceResult.png?raw=true)

**Analysis**
The principle of this method is very simple, Loop through each element x and find if there is another value that plus x and then equals to target . you have to attention to "K = i + 1" of "for(int k=i+1;k<nums.length;k++)" is in order to  avoid the previous looped of the situation looped again.

Time complexity: O (n ^ 2). For each element, we try to find its complement by looping through the rest of array which takes O(n). so it is n ^ 2.
Space complexity: O (1).

## Approach 2 Two-pass Hash Table

```
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
You can see the efficiency has been greatly improved.

![efficiency ](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/1%20Two%20Sum/Images/Twopassresult.png?raw=true)

**Analysis**
The principle of this method is to use the hash table to replace the time cost of space costs, a complexity of O(n) change into near O(1) , why should i use "near", because if the hash table has a lot Of collision occurredthe, it will lead to complexity to near O(n) . We use the value of each element in the array as a key into the hash table, and its index number as the key's value into the hash table, and then traverse the array to find whether there is a corresponding value in the hash table key, if we find that, get the key's value .
Time complexity: O (n)
Space complexity: O (n)

## Approach 3 One-pass Hash Table

```
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

The efficiency is slightly improved.  But i can't accept that how is it just in the middle efficiency of the whole approach. . . And then i tried several times, it is always in the about 40% to 50% position, these methods in front of this solution's position how can they work?. . . Why so fast? Theoretically, it is necessary to loop the results at least once, that is, the complexity of O (n) is required.

![efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/1%20Two%20Sum/Images/One-pass_result.png?raw=true)

**Analysis**
The principle of this method actually is an improvement of the method two, because we do not need to put all the arrays into the hash table, what we need is to get the sum of two number which equals to the number of the target number , so we check if current element's complement already exists in the table when we put the array's element into the hash table , once we find the required value we stop the loop.
Time complexity: O (n)
Space complexity: O (n)

If you have a better method or have other opinions on the description of me here, please contact me. Thank you.
