
[English ver]
# 4. Median of Two Sorted Arrays
There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

```
Example 1:
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

```
Example 2:
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```


## Approach 1:

``` java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        //how many number we need
        int needNum = 0;
        //the index of the number we need
        int needIndex = 0;
        //the median value
        double median = 0;
        int big = 0;
        int stand = 0;
        //the total array
        int[] insertnum ;

        int m = nums1.length;
        int n = nums2.length;

        if((m+n)%2==0){
            needNum = 2;
            needIndex = (m+n)/2-1;
        }else{
            needNum=1;
            needIndex = (m+n)/2;
        }
        insertnum = new int[m+n];
        //nums1 index
        int j =0;
        //nums2 index
        int k = 0;
        for(int i =0;i<m+n;i++){
            if(k==n){
                insertnum[i] = nums1[j];
                j++;
            }
            else if(j==m){
                insertnum[i] = nums2[k];
                k++;
            }else if(nums1[j]>nums2[k]){
                insertnum[i] = nums2[k];
                k++;
            }else{
                insertnum[i] = nums1[j];
                j++;
            }
        }
        if(needNum == 1){
            median = Double.valueOf(insertnum[needIndex]);
        }else{
            median = Double.valueOf(insertnum[needIndex]+insertnum[needIndex+1]) /2.0;
        }

        return median;

    }
}
```

by the way ,this method often failed , maybe is the complexity is too big.

![efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/4.%20Median%20of%20Two%20Sorted%20Arrays/Images/1SuccessResult.png?raw=true)

**Analysis**
This method is easy,first we figure out how to get the median value and how many number we need for calculate median value when the sum of the two arrays size is odd-lengthed or even-lengthed. And then we put the two arrays together sorted from small to large , and then use the method we got to calculate the median value.

Time complexity: O(m+n)
Space complexity:O(m+n)

we can have a optmization of this approach,like that

``` java

public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int needNum = 0;
        int needIndex = 0;
        double median = 0;
        int big = 0;
        int stand = 0;
        int[] insertnum ;

        int m = nums1.length;
        int n = nums2.length;

        if((m+n)%2==0){
            needNum = 2;
            needIndex = (m+n)/2-1;
        }else{
            needNum=1;
            needIndex = (m+n)/2;
        }
        insertnum = new int[(m+n)/2+1];
        int j =0;
        int k = 0;
        for(int i =0;i<m+n;i++){

            if(k==n){
                insertnum[i] = nums1[j];
                j++;
            }
            else if(j==m){
                insertnum[i] = nums2[k];
                k++;
            }else if(nums1[j]>nums2[k]){
                insertnum[i] = nums2[k];
                k++;
            }else{
                insertnum[i] = nums1[j];
                j++;
            }

             if (i >= needIndex ){
                if (needNum == 1){
                    median = Double.valueOf(insertnum[needIndex]);
                    return median;
                }else{
                     if (i == needIndex+1){
                         median = Double.valueOf(insertnum[needIndex]+insertnum[needIndex+1]) /2.0;
                         return median;
                     }
                }
            }

        }
    return median;
    }
}

```

**Analysis**

The optmization is we get the index of the median value ,  when we sorted the array to the index , we can calculate the median value and we don't need to sorted all the number.

![efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/4.%20Median%20of%20Two%20Sorted%20Arrays/Images/1OptimizeSuccessResult.png?raw=true)

## Approach 2

``` java
public class Solution {
       public double findMedianSortedArrays(int[] nums1, int[] nums2) {
           int N1 = nums1.length;
           int N2 = nums2.length;
           if (N1 < N2)
               return findMedianSortedArrays(nums2, nums1);    // Make sure A2 is the shorter one.

           if (N2 == 0)
               return ((double) nums1[(N1 - 1) / 2] + (double) nums1[N1 / 2]) / 2;  // If A2 is empty

           int lo = 0, hi = N2 * 2;
           while (lo <= hi) {
               int mid2 = (lo + hi) / 2;   // Try Cut 2
               int mid1 = N1 + N2 - mid2;  // Calculate Cut 1 accordingly

               double L1 = (mid1 == 0) ? Integer.MIN_VALUE : nums1[(mid1 - 1) / 2];    // Get L1, R1, L2, R2 respectively
               double L2 = (mid2 == 0) ? Integer.MIN_VALUE : nums2[(mid2 - 1) / 2];
               double R1 = (mid1 == N1 * 2) ? Integer.MAX_VALUE : nums1[(mid1) / 2];
               double R2 = (mid2 == N2 * 2) ? Integer.MAX_VALUE : nums2[(mid2) / 2];

               if (L1 > R2)
                   lo = mid2 + 1;        // A1's lower half is too big; need to move C1 left (C2 right)
               else if (L2 > R1)
                   hi = mid2 - 1;    // A2's lower half too big; need to move C2 left.
               else
                   return ((L1 > L2 ? L1 : L2) + (R1 > R2 ? R2 : R1)) / 2;    // Otherwise, that's the right cut.
           }
           return -1;
       }
   }
```

![efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/4.%20Median%20of%20Two%20Sorted%20Arrays/Images/2SeccessResult.png?raw=true)

**Analysis**

this method is a little hard to understand,many implementations consider odd-lengthed and even-lengthed arrays as two different cases and treat them separately. In fact , this is a little twist.
First, we can define 'MEDIAN' i:

"if we cut the sorted array to two halves of equal length, then median is the average of the max number of the small half and min of larger half , is the number near to the cut .

For example, for [ 1 2 ], we make the cut between 1 and 2:
[1 | 2]
the median = (1+2)/2.0 = 1.5 .
The same to the two arrays , we only need to make sure numbers in the right side of the cut of the two arrays bigger than the left side of the cut ,and the size of the left is equal to the size of the right .
I'll use '|' to represent a cut, and (number | number) to represent a cut made through a number in this article.

for example , [1 2 3],the cut is like [1 (2|2) 3].the median = (2+2)/2.0 = 2.

now we use 'L' to represent the number in the left of the cut ,'R' to represent the number in the right.

we can found that

N        Index of L / R
1               0 / 0
2               0 / 1
3               1 / 1
4               1 / 2
5               2 / 2
6               2 / 3
7               3 / 3
8               3 / 4

It is not hard to conclude that index of L = (N-1)/2, and R = N/2. the median = (L + R)/2 = (A[(N-1)/2] + A[N/2])/2

To get ready for the two array situation,we add a few empty number (represented as '#') in between numbers. and use 'positions' to represented the position of that .

[1 2] -> [# 1 # 2 #] (N=2)
position  0 1 2 3 4  (N_Position = 5)

[3 4] -> [# 3 # 4 #] (N=2)
position  0 1 2 3 4  (N_Position = 5)

there are always 2*N+1 'positions' of which array length is N. Therefore, the middle cut should be made on the Nth position (0-based). index(L) = (CutPosition-1)/2, index(R) = (CutPosition)/2.

notice that, when we cut the two arrays there are 2N1+2N2+2 positions,so,each side of the cut should be N1+N2 positions, the two position lefted is the cut . so when A2's cut is at C2 = K , than A1's cut should be C1 = N1 + N2 -K,for example ,C2 = 2, 那么 C1 = 2 + 2 - C2 = 2.

[# 1 | 2 #]
[# 3 | 4 #]

now we have two L and two R.

L1 = A1[(C1-1)/2]; R1 = A1[C1/2];
L2 = A2[(C2-1)/2]; R2 = A2[C2/2];

L1 = A1[(2-1)/2] = A1[0] = 1; R1 = A1[2/2] = A1[1] = 2;
L2 = A2[(2-1)/2] = A2[0] = 3; R2 = A1[2/2] = A1[1] = 4;

How can we figure out this cut is the cut we want ?
the numbers in left side must be smaller than the numbers in right side .
Because A1 and A2 is sorted , L1 <= R1  && L2 <= R2.so we only need to confirm that L1 <= R2 and L2 <= R1.

Now we can use binary search to find out the result.



If L1 > R1, it means there are too many large numbers on the left half of A1, then we must move C1 to the left at the same time move C2 to the right;
If L2 > R1, then there are too many large numbers on the left half of A2, and we must move C2 to the left , and move C1 to the right .
Otherwise, make it opposite.

After we find the cut, the medium can be computed as (max(L1, L2) + min(R1, R2)) / 2;

1. since C1 and C2 can be mutually determined from each other, we might as well select the shorter array (say A2) and only move C2 around, and calculate C1 accordingly. That way we can achieve a run-time complexity of O(log(min(N1, N2)))
2. The only edge case is when a cut falls on the 0th(first) or the 2Nth(last) position. For instance, if C2 = 2N2, then R2 = A2[2*N2/2] = A2[N2], which exceeds the boundary of the array. To solve this problem, we can imagine that both A1 and A2 actually have two extra elements, INT_MAX at A[-1] and INT_MAX at A[N].


Time complexity ： O(log(min(m,n))) 。m,n is the length of the two arrays.
space complexity ： O(1) .

## Approach 3：

``` java
public class Solution {
  public double findMedianSortedArrays(int A[], int B[]) {
       int n = A.length;
       int m = B.length;
       // the following call is to make sure len(A) <= len(B).
       // yes, it calls itself, but at most once, shouldn't be
       // consider a recursive solution
       if (n > m)
           return findMedianSortedArrays(B, A);

       // now, do binary search
       int k = (n + m - 1) / 2;
       int l = 0, r = Math.min(k, n); // r is n, NOT n-1, this is important!!
       while (l < r) {
           int midA = (l + r) / 2;
           int midB = k - midA;
           if (A[midA] < B[midB])
               l = midA + 1;
           else
               r = midA;
       }

       // after binary search, we almost get the median because it must be between
       // these 4 numbers: A[l-1], A[l], B[k-l], and B[k-l+1]

       // if (n+m) is odd, the median is the larger one between A[l-1] and B[k-l].
       // and there are some corner cases we need to take care of.
       int a = Math.max(l > 0 ? A[l - 1] : Integer.MIN_VALUE, k - l >= 0 ? B[k - l] : Integer.MIN_VALUE);
       if (((n + m) & 1) == 1)
           return (double) a;

       // if (n+m) is even, the median can be calculated by
       //      median = (max(A[l-1], B[k-l]) + min(A[l], B[k-l+1]) / 2.0
       // also, there are some corner cases to take care of.
       int b = Math.min(l < n ? A[l] : Integer.MAX_VALUE, k - l + 1 < m ? B[k - l + 1] : Integer.MAX_VALUE);
       return (a + b) / 2.0;
   }
}
```

![efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/4.%20Median%20of%20Two%20Sorted%20Arrays/Images/3SuccessResult.png?raw=true)

**Analysis**
this method's principle is same as the Approach 2.

Time complexity ： O(log(min(m,n))) 。m,n is the length of the two arrays.
space complexity ： O(1) .

## Approach 4:

``` java
public class Solution {
  public double findMedianSortedArrays(int[] A, int[] B) {
              int m = A.length, n = B.length;
              int l = (m + n + 1) / 2;//position of l element (not the index)
              int r = (m + n + 2) / 2;//position of r element (not the index)
              return (getkth(A, 0, B, 0, l) + getkth(A, 0, B, 0, r)) / 2.0;
          }

  public double getkth(int[] A, int aStart, int[] B, int bStart, int k) {
          //when the start position is bigger than A.length -1 ,means the median doesn't in the A.
          if (aStart > A.length - 1) return B[bStart + k - 1];
          if (bStart > B.length - 1) return A[aStart + k - 1];
          if (k == 1) return Math.min(A[aStart], B[bStart]);

          int aMid = Integer.MAX_VALUE, bMid = Integer.MAX_VALUE;
          if (aStart + k/2 - 1 < A.length) aMid = A[aStart + k/2 - 1];
          if (bStart + k/2 - 1 < B.length) bMid = B[bStart + k/2 - 1];

          if (aMid < bMid)
              return getkth(A, aStart + k/2, B, bStart, k - k/2);// Check: aRight + bLeft
          else
              return getkth(A, aStart, B, bStart + k/2, k - k/2);// Check: bRight + aLeft
  }

}
```

![efficiency](https://github.com/LeonChen1024/LeetCodeRecord/blob/master/4.%20Median%20of%20Two%20Sorted%20Arrays/Images/4SuccessResult.png?raw=true)

**Analysis**
this method is like that, compare the two arrays's median recursively, and every time ignore a half of the two arrays. if A's median is smaller than B's median , then we keep A's right and B's left.Otherwise opposite. last ,we will get L and R , use (L+R)/2.0 to get the median.notice that L and R is not index, isn't start from 0 . when the numbers of the sum of two array's size is odd-lengthed L is the same as R,Otherwise is different.

Time complexity ： O(log(m + n))
Space complexity ： O(1)

If you have any suggestions to make the logic and implementation more better , or you have some advice on my description. Please let me know!
