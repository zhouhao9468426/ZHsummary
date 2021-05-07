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
## 137.只出现一次的数字(2021/4/30)  
>https://leetcode-cn.com/problems/single-number-ii/  
***
![](https://pic.leetcode-cn.com/28f2379be5beccb877c8f1586d8673a256594e0fc45422b03773b8d4c8418825-Picture1.png)  
***
```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for(int i=0; i<32; ++i)
        {
            int total = 0;
            for(auto num:nums)
            {
                total += ((num>>i)&1);
            }
            if(total%3)
            {
                ans = (ans | (1<<i));
            }
        }
        return ans;
    }
};
```  
## 690.员工的重要性(2021/5/1)  
>https://leetcode-cn.com/problems/employee-importance/   
***  
```
class Solution {
public:
    int getImportance(vector<Employee*> employees, int id) {
        unordered_map<int,Employee*> umap;
        for(auto employee:employees)
        {
            umap[employee->id] = employee;
        }
        int sum = 0;
        queue<int> que;
        que.push(id);
        while(!que.empty())
        {
            int curId = que.front();
            que.pop();
            sum += umap[curId]->importance;
            for(auto tmp:umap[curId]->subordinates)
            {
                que.push(tmp);
            }
        }
        return sum;
    }
};
```  
```
class Solution {
    unordered_map<int,Employee*> umap;
    int dfs(int id)
    {
        int ans = umap[id]->importance;
        for(auto ord : umap[id]->subordinates)
        {
            ans += dfs(ord);
        }
        return ans;
    }
public:
    int getImportance(vector<Employee*> employees, int id) {
        for(auto employee:employees)
        {
            umap[employee->id] = employee;
        }
        return dfs(id);
    }
};
```   
## 554.砖墙(2021/5/2)  
>https://leetcode-cn.com/problems/brick-wall/  
***
你的面前有一堵矩形的、由 n 行砖块组成的砖墙。这些砖块高度相同（也就是一个单位高）但是宽度不同。每一行砖块的宽度之和应该相等。

你现在要画一条 自顶向下 的、穿过 最少 砖块的垂线。如果你画的线只是从砖块的边缘经过，就不算穿过这块砖。你不能沿着墙的两个垂直边缘之一画线，这样显然是没有穿过一块砖的。

给你一个二维数组 wall ，该数组包含这堵墙的相关信息。其中，wall[i] 是一个代表从左至右每块砖的宽度的数组。你需要找出怎样画才能使这条线 穿过的砖块数量最少 ，并且返回 穿过的砖块数量 。

 
***
```
class Solution {
public:
    int leastBricks(vector<vector<int>>& wall) {
        unordered_map<int,int> umap;
        int maxCount = 0;
        for(auto &tmp : wall)
        {
            int sum=0;
            for(int i=0; i<tmp.size()-1; ++i)
            {
                sum += tmp[i];
                umap[sum]++;
            }
            for(auto &[u,count] : umap)
            {
                maxCount = max(maxCount,count);
            }
        }
        return wall.size() - maxCount;
    }
};
```  
## 7.整数反转(2021/5/3)  
>https://leetcode-cn.com/problems/reverse-integer/   
***  
```
class Solution {
public:
    int reverse(int x) {
        int res = 0;
        while(x)
        {
            int m = x%10;
            x = x/10;
            if(res>INT_MAX/10 || (res==INT_MAX/10) && (m>7)) return 0;
            if(res<INT_MIN/10 || (res==INT_MIN/10) && (m<-7)) return 0;
            res = res*10 + m; 
        }
        return res;
    }
};
```   
## 1473.粉刷房子(2021/5/4)  
>https://leetcode-cn.com/problems/paint-house-iii/  
***  
```
const int INF = 0x3f3f3f3f;  //INF + INF 不会爆int
int f[105][25][105];

class Solution {
public:
    int minCost(vector<int>& houses, vector<vector<int>>& cost, int m, int n, int target) {
        //无效的状态
        for(int i = 0; i <= m; i++){
            for(int j = 0; j <= n; j++){
                f[i][j][0] = INF;
            }
        }

        for(int i = 1; i <= m; i++){
            int color = houses[i - 1];
            for(int j = 1; j <= n;j++){
                for(int k = 1; k <= target; k++){
                    if(k > i) {
                        f[i][j][k] = INF;
                        continue;
                    }
                    //第i个房间已经上色
                    if(color != 0){
                        if(j == color){
                            int cur = INF;
                            //与上一个房间颜色不同
                            for(int p = 1; p <= n; p++){
                                if(p != j){ //颜色不同
                                    cur = min(cur,f[i - 1][p][k - 1]);
                                }
                            }
                            //与上一个房间颜色相同
                            f[i][j][k] = min(cur,f[i - 1][j][k]);
                        }
                        else f[i][j][k] = INF; //其他为无效状态
                    }    
                    else{ //第i个房间未上色
                        int u = cost[i - 1][j - 1];
                        //考虑第i个房间是否形成新的分区
                        //与上一个房间颜色不同，形成新分区
                        int cur = INF;
                        for(int p = 1; p <= n; p++){
                            if(p != j) cur = min(cur,f[i - 1][p][k - 1] + u);
                        }
                        //与上一个房间颜色相同，不形成新的分区
                        f[i][j][k] = min(cur,f[i - 1][j][k] + u);
                    }   
                }
            }
        }
        
        int ans = INF;
        for(int i = 1; i <= n; i++){
            ans = min(ans,f[m][i][target]);
        }
        return ans == INF ? -1 : ans;
    }
};
```   
## 740.删除并获得点数(2021/5/5)  
>https://leetcode-cn.com/problems/delete-and-earn/  
***  
```
class Solution {
    int rob(vector<int>& sum)
    {
        int size = sum.size();
        int first = sum[0],second = max(sum[0],sum[1]);
        for(int i=2; i<size; ++i)
        {
            int temp = second;
            second = max(first+sum[i],second);
            first = temp;
        }
        return second;
    }
public:
    int deleteAndEarn(vector<int>& nums) {
        int maxVal = 0;
        for(int num : nums)
        {
            maxVal = max(num,maxVal);
        }
        vector<int> sum(maxVal+1);
        for(int val : nums)
        {
            sum[val] += val;
        }
        return rob(sum);
    }
};
```  
## 1720.解码异或后的数组(2021/5/6)  
>https://leetcode-cn.com/problems/decode-xored-array/  
***  
```
class Solution {
public:
    vector<int> decode(vector<int>& encoded, int first) {
        int n = encoded.size()+1;
        vector<int> res(n);
        res[0] = first;
        for(int i=1; i<n; ++i)
        {
            res[i] = res[i-1] ^ encoded[i-1];
        }
        return res;
    }
};
```