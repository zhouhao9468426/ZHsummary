### 377.组合总和IV(2021/4/24)  
>https://leetcode-cn.com/problems/combination-sum-iv/  
***
(1)回溯(超时)-可打印出所有结果
```
class Solution {
    vector<vector<int>> res;
    vector<int> path;
    void backtrack(vector<int>& nums, int target, int sum)
    {
        if(sum == target)
        {
            res.push_back(path);
        }
        for(int i=0; i<nums.size() && sum + nums[i] <= target; ++i)
        {
            path.push_back(nums[i]);
            sum += nums[i];
            backtrack(nums,target,sum);
            path.pop_back();
            sum -= nums[i];
        }
    }
public:
    int combinationSum4(vector<int>& nums, int target) {
        res.clear();
        path.clear();
        sort(nums.begin(),nums.end());
        backtrack(nums,target,0);
        for(int i=0; i<res.size(); ++i)
        {
            for(int j=0; j<res[i].size(); ++j)
            {
                cout << res[i][j] << ' ';
            }
            cout << endl;
        }
        return res.size();
    }
};
```   
(2) 动态规划  
```
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target+1);
        dp[0]=1;
        for(int i=0; i<=target; ++i)
        {
            for(int j=0; j<nums.size(); ++j)
            {
                if(i-nums[j]>=0 && dp[i-nums[j]]+dp[i]<INT_MAX)
                {
                    dp[i] += dp[i-nums[j]];
                }
            }
        }
        return dp[target];
    }
};
```