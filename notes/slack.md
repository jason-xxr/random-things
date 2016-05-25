test

1893 Pepsi [7:54 PM] 
Count of Range Sum

1893 Pepsi [7:54 PM] 
added a Java snippet: Count of Range Sum Merge Sort 
```java
public int countRangeSum(int[] nums, int lower, int upper) {
    int n = nums.length;
    long[] sums = new long[n + 1];
    for (int i = 0; i < n; ++i)
        sums[i + 1] = sums[i] + nums[i];
    return countWhileMergeSort(sums, 0, n + 1, lower, upper);
}
​
private int countWhileMergeSort(long[] sums, int start, int end, int lower, int upper) {
    if (end - start <= 1) return 0;
    int mid = (start + end) / 2;
    int count = countWhileMergeSort(sums, start, mid, lower, upper) 
              + countWhileMergeSort(sums, mid, end, lower, upper);
    int j = mid, k = mid, t = mid;
    long[] cache = new long[end - start];
    for (int i = start, r = 0; i < mid; ++i, ++r) {
        while (k < end && sums[k] - sums[i] < lower) k++;
        while (j < end && sums[j] - sums[i] <= upper) j++;
        while (t < end && sums[t] < sums[i]) cache[r++] = sums[t++];
        cache[r] = sums[i];
        count += j - k;
    }
    System.arraycopy(cache, 0, sums, start, t - start);
    return count;
}
```

GGG 校招 [7:56 PM] 
等等

[7:56] 
容我看看题目先:smile:

1893 Pepsi [7:56 PM] 
这题题目意思是说，给你一个数组，找到数组里subarray sum在某个区间[a,b]的个数

[7:57] 
那么很显然我们可以首先做一个naive的解法


1893 Pepsi [7:57 PM] 
added a Java snippet: Naive 
```java
public int countRangeSum(int[] nums, int lower, int upper) {
    int n = nums.length;
    long[] sums = new long[n + 1];
    for (int i = 0; i < n; ++i)
        sums[i + 1] = sums[i] + nums[i];
    int ans = 0;
    for (int i = 0; i < n; ++i)
        for (int j = i + 1; j <= n; ++j)
            if (sums[j] - sums[i] >= lower && sums[j] - sums[i] <= upper)
                ans++;
    return ans;
}
```

1893 Pepsi [7:57 PM] 
Naive的解法就是先求一下prefix sum，这步是预处理

Francis Yang [7:58 PM] 
这个只是sum对吧。。不需要update？

1893 Pepsi [7:58 PM] 
然后O(n^2)过一下就ok了

[7:58] 
不需要update

Francis Yang [7:58 PM] 
预处理On2 sum O1

1893 Pepsi [7:58 PM] 
不是sum，是有多少个这样的sum

[7:59] 
[-2, 5, -1]

Francis Yang [7:59 PM] 
误会了。。原来是这个题

1893 Pepsi [8:00 PM] 
https://leetcode.com/problems/count-of-range-sum/
Count of Range Sum | LeetCode OJ
Given an integer array nums, return the number of range sums that lie in [lower, upper] inclusive. Range sum S(i, j) is defined as the sum of the elements in nums between indices i and j (i ≤ j), inclusive. Note: A naive algorithm of O(n2) is trivial. You MUST do better than that. Example: Given nums = [-2, 5, -1], lower = -2, upper = 2, Return 3. The three ranges are : [0, 0], [2, 2], [0, 2] and their respective sums are: -2, -1, 2. Credits:Special thanks to @dietpepsi for adding this problem and creating all test cases.

[8:00] 
这个题

[8:00] 
用segtree啊bit啊，这些的解法我就不说了

[8:00] 
单说一下mergesort的解法

Shawn Zhou [8:01 PM] 
大神写的mergesoet方法很通俗易懂 能回答下bit的解答吗

Francis Yang [8:01 PM] 
好 现在有了naive 开始优化

1893 Pepsi [8:01 PM] 
bit的解答请问别人啦~~

[8:01] 
比如某个2d bit写得很溜的gg妹纸

Francis Yang [8:02 PM] 
日常黑gg妹纸

1893 Pepsi [8:02 PM] 
这题首先求完prefix sum之后

Hechen Liu [8:02 PM] 
naive 的解法，所以 sum[i] 存的是 从 0 到 i 的sum，然后从 i 到 j 就用 sum[j] - sum[i]，嗯，直观明了

1893 Pepsi [8:02 PM] 
题目就已经跟count of smaller after self是同一个题目了

Shawn Zhou [8:02 PM] 
pepsi mergesort的分析真是层层递进 我感觉面试用这个解法很赞 但是也想了解下bit的 各位大神懂得来讲讲

1893 Pepsi [8:03 PM] 
@stupidbird911: 我还没开始讲捏

Francis Yang [8:03 PM] 
啊哈哈哈哈

1893 Pepsi [8:04 PM] 
wechat里是谁要问这个题的

Jason Guo [8:04 PM] 
吓我一跳，刚过来以为已经讲完了

Shawn Zhou [8:04 PM] 
看到你的blog了啊

Eric Chen [8:04 PM] 
到底是merge sort还是quick sort啊？

1893 Pepsi [8:04 PM] 
merge sort

Eric Chen [8:05 PM] 
merge sort indices？好吧

Jason Guo [8:05 PM] 
让乐神继续讲嘛

Tianchun Yang [8:05 PM] 
搬小板凳来

1893 Pepsi [8:05 PM] 
不是merge sort indices，其实是merge sort sums

Eric Chen [8:05 PM] 
懂了

1893 Pepsi [8:06 PM] 
当你求完prefix sum之后，题目就变成了这个

[8:06] 
`count of a <= S[j] - S[i] <= b with j > i`

[8:06] 
然后你如果只看一边的话

[8:06] 
`count of S[j] - S[i] <= b with j > i`

[8:07] 
这个题目跟count of smaller after self是完全一样的，它就是这题的特殊情况b = 0 (edited)

[8:07] 
那么怎么做呢？

[8:09] 
我们希望知道i后面有多少个满足条件的j，因此我希望indices有序

Jason Guo [8:09 PM] 
这就想到逆序对了么？

1893 Pepsi [8:09 PM] 
同时需要值有序

[8:09] 
那么我如果排序的话，值有序了，但indice就无序了

Hechen Liu [8:10 PM] 
请大神讲一下，看到这个问题，如何把它转化为merge sort，这一步的思考是怎么来的？

1893 Pepsi [8:11 PM] 
所以呢，我需要j在i的后面仍然在i的后面（注意可以无序），且后面的这些值有序

[8:11] 
也就是说我需要在排序的过程中来完成这个count

[8:11] 
举个例子

[8:12] 
Sum数组是[5 1 3 6 2 4]， 我们的前半数组排序之后是[1 3 5]，后半数组排序之后是[2 4 6]

[8:13] 
很显然[2 4 6]的indices都在[1 3 5]的后面

[8:13] 
且我可以很容易知道[2 4 6]里有多少个比1小的（0个），比3小的，以及比5小的

[8:14] 
这个过程是O(n)的，两部分都有序，可以用two pointer

[8:15] 
大概就是这样子

[8:15] 
如果不明白可以先试着用这个思路来做一下count of smaller after self这个题

[8:16] 
https://leetcode.com/problems/count-of-smaller-numbers-after-self/
Count of Smaller Numbers After Self | LeetCode OJ
You are given an integer array nums and you have to return a new counts array. The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i]. Example: Given nums = [5, 2, 6, 1] To the right of 5 there are 2 smaller elements (2 and 1). To the right of 2 there is only 1 smaller element (1). To the right of 6 there is 1 smaller element (1). To the right of 1 there is 0 smaller element. Return the array [2, 1, 1, 0].

Eric Chen [8:16 PM] 
感谢大神，撒花

[8:16] 
:sunglasses:


1893 Pepsi [8:17 PM] 
added a Java snippet: Count of Range Sum 
```java
public int countRangeSum(int[] nums, int lower, int upper) {
    int n = nums.length;
    long[] sums = new long[n + 1];
    for (int i = 0; i < n; ++i)
        sums[i + 1] = sums[i] + nums[i];
    return countWhileMergeSort(sums, 0, n + 1, lower, upper);
}
​
private int countWhileMergeSort(long[] sums, int start, int end, int lower, int upper) {
    if (end - start <= 1) return 0;
    int mid = (start + end) / 2;
    int count = countWhileMergeSort(sums, start, mid, lower, upper) 
              + countWhileMergeSort(sums, mid, end, lower, upper);
    int j = mid, k = mid, t = mid;
    long[] cache = new long[end - start];
    for (int i = start, r = 0; i < mid; ++i, ++r) {
        while (k < end && sums[k] - sums[i] < lower) k++;
        while (j < end && sums[j] - sums[i] <= upper) j++;
        while (t < end && sums[t] < sums[i]) cache[r++] = sums[t++];
        cache[r] = sums[i];
        count += j - k;
    }
    System.arraycopy(cache, 0, sums, start, t - start);
    return count;
}
```
Add Comment Collapse

1893 Pepsi [8:17 PM] 
最后的代码再贴一次

Tianchun Yang [8:17 PM] 
撒花

1893 Pepsi [8:17 PM] 
:joy:

Jason Guo [8:19 PM] 
撒花

Francis Yang [8:19 PM] 
Strong HIre

1893 Pepsi [8:20 PM] 
:expressionless:

[8:20] 
轻tx

Jason Guo [8:20 PM] 
@tianchun_yang: 再讲讲bit的方法？

Francis Yang [8:20 PM] 
经HC讨论后，决定发给乐神offer。

Tianchun Yang [8:23 PM] 
我也用的merge sort

Coding Buffet [8:38 PM] 
@dietpepsi: 但是这题在写的时候的边界条件到底是包括还是不包括，中点位置比较tricky，有啥经验吗？

1893 Pepsi [8:39 PM] 
一般来说我的区间都定义为左闭右开

[8:39] 
[left, mid) [mid, right)这样子

Coding Buffet [8:40 PM] 
拿你刚才的例子，array有6个元素，预处理后sums array应该是7 个元素？

1893 Pepsi [8:41 PM] 
不是的，我那个例子已经是sum array了

[8:41] 
我没有写原array

Coding Buffet [8:41 PM] 
ok，那就是sum array代表到位置i的和，对吧？

1893 Pepsi [8:42 PM] 
嗯，具体index你得自己推敲一下

[8:42] 
不同人定义不一样

Coding Buffet [8:42 PM] 
恩

1893 Pepsi [8:42 PM] 
我的代码里S[0] = 0

[8:42] 
S[1] = nums[0]这样子 (edited)

Coding Buffet [8:43 PM] 
对，我看你是length+1

1893 Pepsi [8:43 PM] 
是的

[8:44] 
加一个dummy 0比较好写后面

Coding Buffet [8:46 PM] 
我第一次写没加dummy 0，申请的sums array[i]就是代表到当前位置i的和，怎么都不过，后来就放弃了 ：（
