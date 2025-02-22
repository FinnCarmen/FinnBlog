+++
date = '2025-02-12T20:37:16+08:00'
draft = false
title = '前K个高频元素'
+++

[题目链接](https://leetcode.cn/problems/top-k-frequent-elements/)

```cpp
class Solution {
public:
    class cp {
    public:
        bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {
            return lhs.second > rhs.second;  // 小顶堆，按频率升序排列
        }
    };

    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> map;

        // 统计每个元素的频率
        for (int num : nums) {
            map[num]++;
        }

        // 定义小顶堆，堆中存储的是 {数字, 频率}
        priority_queue<pair<int, int>, vector<pair<int, int>>, cp> que;

        // 将所有元素插入堆中，堆的大小超过 k 时弹出堆顶
        for (const auto& entry : map) {
            que.push(entry);
            if (que.size() > k) {
                que.pop();
            }
        }

        // 将堆中的元素提取到结果数组中
        vector<int> res(k);
        for (int i = k - 1; i >= 0; i--) {
            res[i] = que.top().first;
            que.pop();
        }

        return res;
    }
};
```
