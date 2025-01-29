+++
date = '2025-01-28T08:41:21+08:00'
draft = false
title = '四数相加II'
+++

[题目链接](https://leetcode.cn/problems/4sum-ii/)

O(n)+O(n)=O(n)

很自然的方法

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int,int>map;
        for(int a:nums1)
        {
            for(int b:nums2)
            {
                map[a+b]++;
            }
        }
        int ans = 0;
        for(int c:nums3)
        {
            for(int d:nums4)
            {
                if(map.count(-c-d))ans+=map[-c-d];
            }
        }
        return ans;
    }
};
```
