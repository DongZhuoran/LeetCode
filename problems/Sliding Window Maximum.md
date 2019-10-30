# <a href='https://leetcode.com/problems/sliding-window-maximum/'>239. Sliding Window Maximum</a>

## Problem
Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

<strong>Example:</strong>
```
Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 

Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

<strong>Note:</strong>
You may assume k is always valid, 1 ≤ k ≤ input array's size for non-empty array.

<strong>Follow up:</strong>
Could you solve it in linear time?

## Solution
- Approach #1: Sliding Window. ```Time O(N)```
```
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums.length == 0) return nums;
        
        int n = nums.length;
        int[] res = new int[n - k + 1];
        Deque<Integer> dq = new LinkedList<>();
        
        for (int i = 0; i < n; ++ i) {
            while (dq.size() > 0 && nums[i] >= nums[dq.getLast()])
                dq.removeLast();
            
            dq.addLast(i);
            if (i - k + 1 >= 0) res[i - k + 1] = nums[dq.getFirst()];
            if (i - k + 1 == dq.getFirst()) dq.removeFirst();
        }
        return res;
    }
```

- Approach for Mirror Problem: Sliding Window Minimum
```
    public int[] minSlidingWindow(int[] nums, int k) {
        if (nums.length == 0) return nums;
        
        int[] res = new int[nums.length - k + 1];
        Deque<Integer> dq = new LinkedList<>();
        for (int i = 0; i < nums.length; ++ i) {
            while (dq.size() > 0 && nums[dq.getLast()] >= nums[i])
                dq.removeLast();
            
            dq.addLast(i);
            if (i - k + 1 >= 0) res[i - k + 1] = nums[dq.getFirst()];
            if (i - k + 1 == dq.getFirst()) dq.removeFirst();
        }
        return res;
    }
```
