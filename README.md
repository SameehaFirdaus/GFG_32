# GFG_32
---
Difficulty: Medium  
Source: 160 Days of Problem Solving  
Tags:
  - Arrays
  - Divide and Conquer
  - Searching
---

# 🚀 _Day 32. K-th element of two Arrays_ 🧠


The problem can be found at the following link: [Question Link](https://www.geeksforgeeks.org/batch/gfg-160-problems/track/searching-gfg-160/problem/k-th-element-of-two-sorted-array1317)

## 💡 **Problem Description:**

Given two sorted arrays `a[]` and `b[]`, along with an integer `k`, the task is to find the element that would appear at the k-th position in the combined sorted array of `a[]` and `b[]`.

## 🔍 **Example Walkthrough:**

Input:  
```
a[] = [2, 3, 6, 7, 9], b[] = [1, 4, 8, 10], k = 5
```
Output:  
```
6
```
Explanation: The combined sorted array would be `[1, 2, 3, 4, 6, 7, 8, 9, 10]`. The 5th element is `6`.

Input:  
```
a[] = [100, 112, 256, 349, 770], b[] = [72, 86, 113, 119, 265, 445, 892], k = 7
```
Output:  
```
256
```
Explanation: The combined sorted array is `[72, 86, 100, 112, 113, 119, 256, 265, 349, 445, 770, 892]`. The 7th element is `256`.

**Constraints:**
- $\( 1 \leq \text{size of } a, b \leq 10^6 \)$
- $\( 1 \leq k \leq \text{size of } a + \text{size of } b \)$
- $\( 0 \leq a[i], b[i] < 10^8 \)$



## 🎯 **My Approach:**

1. **Binary Search on Cuts:**
   - The problem can be viewed as finding the correct "cut point" in the arrays `a[]` and `b[]` such that the k-th element lies at the boundary between the left and right halves of the merged arrays.
   - Use binary search to adjust the cut point in `a[]`, and calculate the corresponding cut point in `b[]`.

2. **Key Observations:**
   - The left part of the combined array must consist of the largest `k` elements from `a[]` and `b[]` combined.
   - For the cut to be valid:
     - The largest element on the left part of `a[]` (cut1) should be ≤ the smallest element on the right part of `b[]` (cut2).
     - Similarly, the largest element on the left part of `b[]` (cut2) should be ≤ the smallest element on the right part of `a[]` (cut1).

3. **Binary Search Implementation:**
   - Perform binary search on `a[]`'s possible cut points while ensuring the constraints for valid cuts are satisfied.
   - Return the maximum of the largest elements from both left parts once a valid cut is found.

## 🕒 **Time and Auxiliary Space Complexity** 

- **Expected Time Complexity:** O(log(min(a, b))), as the binary search is performed on the smaller array.
- **Expected Auxiliary Space Complexity:** O(1), as we use only a constant amount of additional space.

## 📝 **Solution Code**
## Code (Python)

```python
class Solution:

    def kthElement(self, a, b, k):
        if len(a) > len(b):
            return self.kthElement(b, a, k)

        n, m = len(a), len(b)
        low, high = max(0, k - m), min(k, n)

        while low <= high:
            cut1 = (low + high) // 2
            cut2 = k - cut1

            l1 = a[cut1 - 1] if cut1 > 0 else float('-inf')
            l2 = b[cut2 - 1] if cut2 > 0 else float('-inf')
            r1 = a[cut1] if cut1 < n else float('inf')
            r2 = b[cut2] if cut2 < m else float('inf')

            if l1 <= r2 and l2 <= r1:
                return max(l1, l2)
            elif l1 > r2:
                high = cut1 - 1
            else:
                low = cut1 + 1

        return -1
```
