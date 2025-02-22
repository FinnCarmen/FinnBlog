+++
date = '2025-01-31T20:39:47+08:00'
draft = false
title = '实现strStr'
+++

[题目链接](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

### KMP

#### 模板总结

```cpp
int next_arr[10000];
void get_next(const string &t)
{
    int j = -1;
    next_arr[0] = j;
    for (int i = 1; i < t.length(); i++)
    {
        while (j >= 0 && t[i] != t[j + 1])
        {
            j = next_arr[j];
        }
        if (t[i] == t[j + 1])
        {
            j++;
        }
        next_arr[i] = j;
    }
}

int KMP(const string &s, const string &t)
{
    if (t.empty())
        return 0;
    int j = 0;
    for (int i = 0; i < s.length(); i++)
    {
        while (j > 0 && s[i] != t[j])
        {
            j = next_arr[j - 1] + 1;
        }
        if (s[i] == t[j])
        {
            j++;
        }
        if (j == t.length())
        {
            return i - j + 1; // 找到匹配的位置
        }
    }
    return -1; // 未找到匹配
}
```

#### 题解

```cpp
class Solution {
public:
    vector<int> next_arr;
    void get_next(const string &t)
    {
        next_arr.resize(t.length(),-1);
        int j = -1;
        for(int i = 1;i<t.length();i++)
        {
            while(j>=0 && t[i] !=t[j+1])j=next_arr[j];
            if(t[i]==t[j+1])j++;
            next_arr[i] = j;
        }
    }

    int strStr(string haystack, string needle) {
        if(needle.empty())return 0;
        get_next(needle);
        int j = 0;
        for(int i=0;i<haystack.length();i++)
        {
            while(j>0 && haystack[i]!=needle[j])j=next_arr[j-1]+1;
            if(haystack[i]==needle[j])j++;
            if(j==needle.length())return i-needle.length()+1;
        }
        return -1;
    }
};
```
