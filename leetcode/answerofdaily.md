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
## 897.递增顺序搜索树(2021/4/25)   
>https://leetcode-cn.com/problems/increasing-order-search-tree/  
***  
递归一：
```
class Solution {
    TreeNode* pre;
    void dfs(TreeNode* root)
    {
        if(root==nullptr) return;
        dfs(root->left);
        pre->right = root;
        root->left = nullptr;
        pre = root;
        dfs(root->right);
    }
public:
    TreeNode* increasingBST(TreeNode* root) {
        TreeNode* newRoot = new TreeNode(0);
        pre = newRoot;
        dfs(root);
        return newRoot->right;
    }
};
```  
递归二：  
```
class Solution {
    void dfs(TreeNode* root, vector<int>& res)
    {
        if(root==nullptr) return;
        dfs(root->left,res);
        res.push_back(root->val);
        dfs(root->right,res);
    }
public:
    TreeNode* increasingBST(TreeNode* root) {
        if(root==nullptr) return nullptr;
        vector<int> res;
        dfs(root,res);
        TreeNode* cur = new TreeNode(res[0]);
        TreeNode* pre = cur;
        for(int i=1; i<res.size(); ++i)
        {
            TreeNode* tmp = new TreeNode(res[i]);
            cur->right = tmp;
            cur = tmp;
        }
        return pre;
    }
};
```  
迭代：  
```
class Solution {
public:
    TreeNode* increasingBST(TreeNode* root) {
        if(root==nullptr) return nullptr;
        TreeNode* pre = new TreeNode(0);
        TreeNode* res = pre;
        stack<TreeNode*> stk;
        TreeNode* cur = root;
        while(!stk.empty() || cur)
        {
            while(cur)
            {
                stk.push(cur);
                cur = cur->left;
            }
            cur = stk.top();
            stk.pop();
            res->right = cur;
            cur->left = nullptr;
            res = cur;
            cur = cur->right;
        }
        return pre->right;
    }
};
```  
## 1011.在D天内送达包裹的能力(2021/4/26)  
>https://leetcode-cn.com/problems/capacity-to-ship-packages-within-d-days/  
***  
```
class Solution {
public:
    int shipWithinDays(vector<int>& weights, int D) {
        int left=0,right=0;
        for(int i=0; i<weights.size(); ++i)
        {
            if(weights[i]>left)
            {
                left = weights[i];
            }
            right += weights[i];
        }
        while(left<right)
        {
            int mid = left + ((right-left)>>2);
            int day=1,cur=0;
            for(int i=0; i<weights.size(); ++i)
            {
                if(cur+weights[i]>mid)
                {
                    day++;
                    cur=0;
                }
                cur += weights[i];
            }
            if(day<=D)
            {
                right = mid;
            }
            else
            {
                left = mid + 1;
            }
        }
        return left;
    }
};
```  
## 938.二叉搜索树范围和(2021/4/27)
>https://leetcode-cn.com/problems/range-sum-of-bst/  
*** 
```
class Solution {
    int res=0;
    void dfs(TreeNode* root, int low, int high)
    {
        if(root == nullptr) return;
        if(root->val<=high && root->val>=low) res += root->val;
        dfs(root->left,low,high);
        dfs(root->right,low,high);
    }
public:
    int rangeSumBST(TreeNode* root, int low, int high) {
        res = 0;
        dfs(root,low,high);
        return res;
    }
};
```  
## 633.平方数之和(2021/4/28)  
>https://leetcode-cn.com/problems/sum-of-square-numbers/  
*** 
```
class Solution {
public:
    bool judgeSquareSum(int c) {
        long left=0, right=sqrt(c)+1;
        while(left <= right)
        {
            long sum = left*left + right*right;
            if(sum==c) return true;
            else if(sum<c) 
            {
                left++;
            }
            else
            {
                right--;
            }
        }
        return false;
    }
};
```  
## 679.24点游戏(2021/4/29)   
>https://leetcode-cn.com/problems/24-game/  
方法一：  
```
class Solution{
    void backtrack(vector<double> &arr, double eps, bool &res)
    {
        if(res) return;
        if(arr.size()==1)
        {
            if(abs(arr[0]-24)<eps)
            {
                res = true;
            }
            return;
        }
        for(int i=0; i<arr.size(); ++i)
        {
            for(int j=0; j<i; ++j)
            {
                double num1 = arr[i],num2=arr[j];
                vector<double> data = {num1+num2,num1*num2,num1-num2,num2-num1};
                if(num1>eps) data.push_back(num2/num1);
                if(num2>eps) data.push_back(num1/num2);
                arr.erase(arr.begin()+i);
                arr.erase(arr.begin()+j);
                for(auto tmp:data)
                {
                    arr.push_back(tmp);
                    backtrack(arr,eps,res);
                    arr.pop_back();
                }
                arr.insert(arr.begin()+j,num2);
                arr.insert(arr.begin()+i,num1);
            }
        }
    }
public:
    bool judgePoint24(vector<int>& nums) {
        bool res = false;
        double eps = 0.00001;
        vector<double> arr(nums.begin(),nums.end());
        backtrack(arr,eps,res);
        return res;
    }
};
```   
方法二：  
```
class Solution {
public:
    bool canCross(vector<int>& stones) {
        int n = stones.size();
        if(stones[1]>1) return false;
        vector<vector<bool>> dp(n,vector<bool>(n));
        dp[0][0] = true;
        for(int i=1; i<n; ++i)
        {
            if(stones[i]-stones[i-1]>i)
            {
                return false;
            }
        }
        for(int i=1; i<n; ++i)
        {
            for(int j=0; j<i; ++j)
            {
                int k = stones[i]-stones[j];
                if(k>j+1) continue;
                if(k>0)
                {
                    dp[i][k] = dp[j][k-1] || dp[j][k] || dp[j][k+1];
                }
                if(dp[i][k] && i==n-1) return true;
            }
        }
        return false;
    }
};
```