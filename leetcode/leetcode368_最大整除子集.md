#### leetcode368.最大整除子集  
>https://leetcode-cn.com/problems/largest-divisible-subset/  
***
代码：  
```  
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(),nums.end());
        vector<int> dp(n);
        vector<int> g(n);
        //类似于最长递增子序列
        for(int i=0; i<n; ++i)
        {
            int len=1 , pre = i;
            for(int j=0; j<i; ++j)
            {
                if(nums[i]%nums[j]==0)
                {
                    if(dp[j]+1>len)
                    {
                    len = dp[j]+1;
                    pre = j;
                    }
                }
            }
            dp[i] = len;
            g[i] = pre;
        }
        int idx = max_element(dp.begin(),dp.end())-dp.begin();
        int max = dp[idx];
        //利用g数组回溯
        vector<int> ans;
        while(ans.size() != max)
        {
            ans.push_back(nums[idx]);
            idx = g[idx];
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```   
类似题目：   
#### leetcode.300最长递增子序列
>https://leetcode-cn.com/problems/longest-increasing-subsequence/
***
代码：  
```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) 
    {
        int n = nums.size();
        if(n==0) return 0;
        vector<int> dp(n);
        for(int i=0; i<n; i++)
        {
            dp[i] = 1;
            for(int j=0; j<i; j++)
            {
                if(nums[j]<nums[i])
                dp[i] = max(dp[i],dp[j]+1);
            }
        }
        return *max_element(dp.begin(),dp.end());
    }
};
```
