## [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

### 难度：**Medium** 

### 题目：

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,0,1,2,2,5,6]` might become `[2,5,6,0,0,1,2]`).

You are given a target value to search. If found in the array return `true`, otherwise return `false`.

**Example 1:**

```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

**Example 2:**

```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```

**Follow up:**

- This is a follow up problem to [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/), where `nums` may contain duplicates.
- Would this affect the run-time complexity? How and why?

### 分析

承袭 [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)，允许数组中有重复元素出现，分析可保留大部分只需修改部分判断条件即可。

题目要求时间复杂度为O(log n)，并且不复制数组，又是在vector容器中查找指定数据，那么便使用二分查找

- ***`mid`等于`target`时退出***
- 当 `first`小于`mid`时，**可以确定`first ~ mid`是递增的**

  - 若`target`落在`first ~ mid`区间内，则将`mid`赋值给`last`缩小查找范围
  - 其它情况则将`mid+1`赋值给`first`缩小查找范围
- 当`first`大于`mid`时，**可以确定`mid ~ last`是递增的**
  - 若`target`落在`mid ~ last`区间内，则将`mid+1`赋值给`first`缩小查找范围
  - 其它情况则将`mid`赋值给`last`缩小查找范围
- 当`first`等于`mid`时，**first++即可**


### 代码
```C++
class Solution {
public:
    bool search(vector<int>& nums, int target) {
              int first = 0, last = nums.size();
        while(first != last){
            const int mid = first + (last - first) / 2;
            if(nums[mid] == target)
                return true;
            if(nums[first] < nums[mid]){
                if(nums[first] <= target && target < nums[mid])
                    last = mid;
                else
                    first = mid +1;
            }
            else if(nums[first] > nums[mid]){
                if(nums[mid] < target && target <= nums[last-1])
                    first = mid + 1;
                else
                    last = mid;
            }
            else
                first++;
        }
        return false;  
    }
};
```
### 时空间复杂度

时间复杂度为：O(log n)

空间复杂度为：O(1)
