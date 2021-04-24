## 03.数组中的重复数字  
>https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/  
***  
```
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        vector<int> count(nums.size());
        for(int i=0; i<nums.size(); ++i)
        {
            count[nums[i]]++;
            if(count[nums[i]]>1) return nums[i];
        }
        return -1;
    }
};
```  
## 04.二维数组中查找  
>https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/  
***  
```
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if(matrix.size()==0) return false;
        //以右上角或左下角做标准，这里选择右上角作为比较起点
        int i = 0, j = matrix[0].size()-1;
        while(i<matrix.size() && j>=0)
        {
            if(matrix[i][j]==target) return true;
            else if(matrix[i][j]<target) i++;
            else
            {
                j--;
            }
        }
        return false;
    }
};
```  
## 05.替换空格
>https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/  
***  
```
class Solution {
public:
    string replaceSpace(string s) {
        string res;
        for(int i=0; i<s.size(); ++i)
        {
            if(s[i] == ' ')
            {
                res += "%20";
            }
            else
            {
                res += s[i];
            }
        }
        return res;
    }
};
```  
## 06.逆序打印链表  
>https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/  
***
```
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        vector<int> res;
        while(head)
        {
            res.push_back(head->val);
            head = head->next;
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```  
## 07.重构二叉树  
>https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/  
***
```
class Solution {
    unordered_map<int,int> umap;
    TreeNode* dfs(vector<int>& preorder, vector<int>& inorder, int preL, int preR, int inL, int inR)
    {
        if(preL > preR) return nullptr; //结束条件
        int rootVal = preorder[preL]; //根结点值为前序遍历第一个数
        int index = umap[rootVal]; //根结点在中序遍历中的位置索引
        int leftLen = index - inL; //左子树的长度
        TreeNode* root = new TreeNode(rootVal);
        root->left = dfs(preorder,inorder,preL+1,preL+leftLen,inL,index-1);
        root->right = dfs(preorder,inorder,preL+leftLen+1,preR,index+1,inR);
        return root;
    }
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        if(n==0) return nullptr;
        for(int i=0; i<n; ++i)
        {
            umap[inorder[i]] = i;
        }
        return dfs(preorder,inorder,0,n-1,0,n-1);
    }
};
```  
## 09.用两个栈来实现队列  
>https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/ 
***
```  
class CQueue {
    stack<int> stkIn,stkOut;
public:
    CQueue() {
        while(!stkIn.empty())
        {
            stkIn.pop();
        }
        while(!stkOut.empty())
        {
            stkOut.pop();
        }
    }
    
    void appendTail(int value) {
        stkIn.push(value);
    }
    
    int deleteHead() {
        if(stkOut.empty())
        {
            while(!stkIn.empty())
            {
                stkOut.push(stkIn.top());
                stkIn.pop();
            }
        }
        if(stkOut.empty())
        {
            return -1;
        }
        else
        {
            int tmp = stkOut.top();
            stkOut.pop();
            return tmp;
        }
    }
};
```  
## 10.斐波拉契数列   
>https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/  
***
```
class Solution {
public:
    int fib(int n) {
        if(n==0) return 0;
        vector<int> dp(n+1);
        dp[1] = 1;
        for(int i=2; i<=n; ++i)
        {
            dp[i] = (dp[i-1] + dp[i-2])%1000000007;
        }
        return dp[n];
    }
};
```  
## 11.青蛙跳台阶  
>https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/   
***
```
class Solution {
public:
    int numWays(int n) {
        if(n<=1) return 1;
        int pre=1,cur=1;
        int sum = 0;
        for(int i=2; i<=n; ++i)
        {
            sum = (pre + cur)%1000000007;
            pre = cur;
            cur = sum;
        }
        return cur;
    }
};
```  
## 12.旋转排序数组查找  
>https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/ 
***
考虑以下三种情况：  
1 0 1 1 1   
1 1 0 1 1  
1 1 1 0 1  
当nums[mid] > nums[right]时，最小元素位于[mid+1,right]区间内;  
当nums[mid] <= nums[right]时，考虑上面的三种情况，可以确定最小元素位于[left,right-1]内。  
```
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int left = 0, right = numbers.size()-1;
        while(left < right)
        {
            if(numbers[left]<numbers[right]) return numbers[left];
            int mid = left + ((right-left)>>1);
            if(numbers[mid]>numbers[right])
            {
                left = mid + 1;
            }
            else
            {
                right--;
            }
        }
        return numbers[left];
    }
};
```  
## 12.矩阵中的路径  
>https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/ 
***
```
class Solution {
    bool backtrack(vector<vector<char>>& board, string word, int index, int i, int j, vector<vector<bool>>& visited)
    {
        if(i<0 || j<0 || i>=board.size() || j>=board[0].size() || word[index]!=board[i][j]) return false;
        if(index == word.size()-1) return true;
        //上下左右四个方向
        bool res = false;
        if(word[index] == board[i][j])
        {
            visited[i][j] = true;
            res = (backtrack(board,word,index+1,i,j+1,visited) || backtrack(board,word,index+1,i,j-1,visited)
            || backtrack(board,word,index+1,i-1,j,visited) || backtrack(board,word,index+1,i+1,j,visited));
            visited[i][j] = false;
        }
        return res;
    }
public:
    bool exist(vector<vector<char>>& board, string word) {
        int m=board.size();
        int n=board[0].size();
        vector<vector<bool>> visited(m,vector<bool>(n));
        for(int i=0; i<m; ++i)
        {
            for(int j=0; j<n; ++j)
            {
                if(backtrack(board,word,0,i,j,visited))
                {
                    return true;
                }
            }
        }
        return false;
    }
};
```  
## 13.机器人运动范围  
>https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/  
***  
迭代：  
```
class Solution {
    int getSum(int num)
    {
        int sum = 0;
        while(num)
        {
            sum += num%10;
            num = num/10;
        }
        return sum;
    }
public:
    int movingCount(int m, int n, int k) {
        vector<vector<int>> visit(m,vector<int>(n));
        visit[0][0] = 1;
        int res = 1;
        for(int i=0; i<m; ++i)
        {
            if(getSum(i)>k) break;
            for(int j=0; j<n; ++j)
            {
                if(getSum(i)+getSum(j)>k || i==0 && j==0) continue;
                if(i>0) visit[i][j] |= visit[i-1][j];
                if(j>0) visit[i][j] |= visit[i][j-1];
                res += visit[i][j];
            }
        }
        return res;
    }
};
```  
递归：  
```
class Solution {
    int getSum(int num)
    {
        int sum = 0;
        while(num)
        {
            sum += num%10;
            num = num/10;
        }
        return sum;
    }
    int dfs(int m, int n, int k, int i, int j, vector<vector<bool>>& visit)
    {
        if(i<0 || j<0 || i>=m || j>=n || getSum(i)+getSum(j)>k || visit[i][j]) return 0;
        visit[i][j] = true;
        return 1+dfs(m,n,k,i+1,j,visit)+dfs(m,n,k,i,j+1,visit)+dfs(m,n,k,i-1,j,visit)+dfs(m,n,k,i,j-1,visit);
    }
public:
    int movingCount(int m, int n, int k) {
        vector<vector<bool>> visit(m,vector<bool>(n));
        int res = dfs(m,n,k,0,0,visit);
        return res;
    }
};
```
