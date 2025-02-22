+++
date = '2025-02-18T20:45:20+08:00'
draft = false
title = '复原IP地址'
+++

[题目链接](https://leetcode.cn/problems/restore-ip-addresses/)

```cpp
class Solution {
public:
    vector<string>res;
    bool isvalid(const string &s,int start,int end)
    {
        if(start>end)return false;
        if(s[start]=='0' && start !=end)
            return false;
        int num=0;
        for(int i=start;i<=end;i++)
        {
            if(s[i]>'9' || s[i]<'0')return false;
            num=num*10+(s[i]-'0');
            if(num>255)return false;
        }
        return true;
    }
    void backtracking(string s,int start,int sum)
    {
        if(sum==3)
        {
            if(isvalid(s,start,s.size()-1))
                res.push_back(s);
            return;
        }
        for(int i=start;i<s.size();i++)
        {
            if(isvalid(s,start,i))
            {
                s.insert(s.begin()+i+1,'.');
                sum+=1;
                backtracking(s,i+2,sum);
                s.erase(s.begin()+i+1);
                sum-=1;
            }
            else break;
        }
    }
    vector<string> restoreIpAddresses(string s) {
        res.clear();
        if(s.size()<4 || s.size()>12)return res;
        backtracking(s,0,0);
        return res;
    }
};
```
