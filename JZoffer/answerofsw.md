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
## 10.青蛙跳台阶  
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
## 11.旋转排序数组查找  
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
## 14.剪绳子  
>https://leetcode-cn.com/problems/jian-sheng-zi-lcof/  
***  
(1)数学计算：
```
class Solution {
public:
    int cuttingRope(int n) {
        int res=1;
        if(n==2) return 1;
        else if(n==3) return 2;
        while(n>0)
        {
            if(n<3 || n==4)
            {
                res *= n;
                break;
            }
            else 
            {
                res *= 3;
                n = n - 3;
            }
        }
        return res;
    }
};
```  
(2)动态规划：  
```
class Solution {
public:
    int cuttingRope(int n) {
        if(n<4) return n-1;
        vector<int> dp(n+1);
        dp[1] = 1;
        dp[2] = 2;
        dp[3] = 3;
        for(int i=4; i<=n; ++i)
        {
            int maxAns = 0;
            for(int j=1; j<=i/2; ++j)
            {
                maxAns = max(maxAns,dp[j]*dp[i-j]);
            }
            dp[i] = maxAns;
        }
        return dp[n];
    }
};
```  
(3) 2<=n<=1000  
>https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/  
***
```
class Solution {
public:
    int cuttingRope(int n) {
        if(n<4) return n-1;
        long res = 1;
        if(n%3==1)
        {
            res = 4;
            n = n-4;
        }
        if(n%3==2)
        {
            res = 2;
            n = n-2;
        }
        while(n>0)
        {
            res  = (res*3)%1000000007;
            n = n-3;
        }
        return res;
    }
};
```   
## 15.统计二进制中的1的个数
>https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/ 
***
```
class Solution {
public:
    int hammingWeight(uint32_t n) {
        //十进制转化二进制过程
        int count=0;
        while(n)
        {
            if(n%2) count++;
            n = n>>1;
        }
        return count;
    }
}; 
``` 
## 16.数值的整数次方  
>https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/  
***  
```
class Solution {
public:
    double myPow(double x, int n) {
        if(abs(x-1.0)<1e-7) return 1.0;
        double ans = 1.0;
        long num = n;
        if(n<0)
        {
            num = -num;
            x = 1/x;
        }
        while(num>0)
        {
            if(num&1) ans *= x;
            x *=x;
            num = num>>1;
        }
        return ans;
    }
};
```  
## 17.打印从1到最大的n位数  
>https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/ 
***
```
class Solution {
public:
    vector<int> printNumbers(int n) {
        int num=1;
        for(int i=0; i<n; ++i)
        {
            num *= 10;
        }
        vector<int> res(num-1);
        for(int i=0; i<num-1; ++i)
        {
            res[i] = i+1;
        }
        return res;
    }
};
```  
## 18.删除链表节点  
>https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/  
***
```
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        ListNode* preHead = new ListNode(0);
        preHead->next = head;
        ListNode* pre = preHead, *cur = head;
        //cur指向待删除的节点，pre指向前一个节点
        while(cur && cur->val != val)
        {
            cur = cur->next;
            pre = pre->next;
        }
        pre->next = cur->next;
        return preHead->next;
    }
};
```  
## 19.正则表达式匹配  
>https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/  
***
```
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(),n = p.size();
        vector<vector<bool>> dp(m+1,vector<bool>(n+1));
        dp[0][0] = true;
        for(int j=2; j<=n; ++j)
        {
            if(p[j-1]=='*') dp[0][j] = dp[0][j-2];
        }
        for(int i=1; i<=m; ++i)
        {
            for(int j=1; j<=n; ++j)
            {
                if(p[j-1] == '*' && j>=2)
                {
                    dp[i][j] = dp[i][j-2];
                    if(p[j-2]==s[i-1] || p[j-2]=='.') dp[i][j] = dp[i-1][j] || dp[i][j-1] || dp[i][j-2];
                }
                else
                {
                    dp[i][j] = dp[i-1][j-1] && (p[j-1]==s[i-1] || p[j-1]=='.');
                }
            }
        }
        return dp[m][n];
    }
};
```  
## 20.表示数值的字符串  
>https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/  
***
```
class Solution {
public:
    enum State {
        STATE_INITIAL,
        STATE_INT_SIGN,
        STATE_INTEGER,
        STATE_POINT,
        STATE_POINT_WITHOUT_INT,
        STATE_FRACTION,
        STATE_EXP,
        STATE_EXP_SIGN,
        STATE_EXP_NUMBER,
        STATE_END,
    };

    enum CharType {
        CHAR_NUMBER,
        CHAR_EXP,
        CHAR_POINT,
        CHAR_SIGN,
        CHAR_SPACE,
        CHAR_ILLEGAL,
    };

    CharType toCharType(char ch) {
        if (ch >= '0' && ch <= '9') {
            return CHAR_NUMBER;
        } else if (ch == 'e' || ch == 'E') {
            return CHAR_EXP;
        } else if (ch == '.') {
            return CHAR_POINT;
        } else if (ch == '+' || ch == '-') {
            return CHAR_SIGN;
        } else if (ch == ' ') {
            return CHAR_SPACE;
        } else {
            return CHAR_ILLEGAL;
        }
    }

    bool isNumber(string s) {
        unordered_map<State, unordered_map<CharType, State>> transfer{
            {
                STATE_INITIAL, {
                    {CHAR_SPACE, STATE_INITIAL},
                    {CHAR_NUMBER, STATE_INTEGER},
                    {CHAR_POINT, STATE_POINT_WITHOUT_INT},
                    {CHAR_SIGN, STATE_INT_SIGN},
                }
            }, {
                STATE_INT_SIGN, {
                    {CHAR_NUMBER, STATE_INTEGER},
                    {CHAR_POINT, STATE_POINT_WITHOUT_INT},
                }
            }, {
                STATE_INTEGER, {
                    {CHAR_NUMBER, STATE_INTEGER},
                    {CHAR_EXP, STATE_EXP},
                    {CHAR_POINT, STATE_POINT},
                    {CHAR_SPACE, STATE_END},
                }
            }, {
                STATE_POINT, {
                    {CHAR_NUMBER, STATE_FRACTION},
                    {CHAR_EXP, STATE_EXP},
                    {CHAR_SPACE, STATE_END},
                }
            }, {
                STATE_POINT_WITHOUT_INT, {
                    {CHAR_NUMBER, STATE_FRACTION},
                }
            }, {
                STATE_FRACTION,
                {
                    {CHAR_NUMBER, STATE_FRACTION},
                    {CHAR_EXP, STATE_EXP},
                    {CHAR_SPACE, STATE_END},
                }
            }, {
                STATE_EXP,
                {
                    {CHAR_NUMBER, STATE_EXP_NUMBER},
                    {CHAR_SIGN, STATE_EXP_SIGN},
                }
            }, {
                STATE_EXP_SIGN, {
                    {CHAR_NUMBER, STATE_EXP_NUMBER},
                }
            }, {
                STATE_EXP_NUMBER, {
                    {CHAR_NUMBER, STATE_EXP_NUMBER},
                    {CHAR_SPACE, STATE_END},
                }
            }, {
                STATE_END, {
                    {CHAR_SPACE, STATE_END},
                }
            }
        };

        int len = s.length();
        State st = STATE_INITIAL;

        for (int i = 0; i < len; i++) {
            CharType typ = toCharType(s[i]);
            if (transfer[st].find(typ) == transfer[st].end()) {
                return false;
            } else {
                st = transfer[st][typ];
            }
        }
        return st == STATE_INTEGER || st == STATE_POINT || st == STATE_FRACTION || st == STATE_EXP_NUMBER || st == STATE_END;
    }
};
```  
## 21.调整数组顺序使得奇数位于前半部分，偶数位于后半部分  
>https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/  
***   
(1)不使用额外空间：
```
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        if(nums.size()==0) return nums;
        int left = 0, right = nums.size()-1;
        while(left < right)
        {
            if(nums[left]%2==0) 
            {
                while(nums[right]%2==0 && left < right)
                {
                    right--;
                }
                if(left < right)
                {
                    swap(nums[left],nums[right]);
                    right--;
                    left++;
                }
            }
            else 
            {
                left++;
            }
        }
        return nums;
    }
};
```   

(2)使用额外空间：   
```
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int len = nums.size();
        if(len<2) return nums;
        vector<int> res(len);
        int left = 0, right = len - 1;
        for(int i=0; i<len; ++i)
        {
            if(nums[i]%2==0)
            {
                res[right--]=nums[i];
            }
            else
            {
                res[left++]=nums[i];
            }
        }
        return res;
    }
};
```  
## 22.链表中倒数第k个结点  
>https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/   
***  
```
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode* pre = head, *cur = head;
        for(int i=0; i<k && cur; ++i)
        {
            cur = cur->next;
        }
        while(cur)
        {
            cur = cur->next;
            pre = pre->next;
        }
        return pre;
    }
};
```  
## 24.反转链表  
>https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/  
***  
```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* cur = head;
        ListNode* pre = nullptr;
        while(cur)
        {
            ListNode* tmp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = tmp;
        }
        return pre;
    }
};
```  
## 25.合并两个排序链表  
>https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/  
***  
```
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1==nullptr) return l2;
        if(l2==nullptr) return l1;
        ListNode* pre = new ListNode(0);
        ListNode* cur = pre;
        while(l1 && l2)
        {
            if(l1->val < l2->val)
            {
                cur->next = l1;
                cur = cur->next;
                l1 = l1->next;
            }
            else
            {
                cur->next = l2;
                cur = cur->next;
                l2 = l2->next;
            }
        }
        if(l1) cur->next = l1;
        if(l2) cur->next = l2;
        ListNode* res = pre->next;
        delete pre;
        return res;
    }
};
```  
## 26.树的子结构  
>https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/  
***  
```
class Solution {
    bool isSub(TreeNode* A, TreeNode* B)
    {
        if(B==nullptr) return true;
        if(A==nullptr || A->val != B->val) return false;
        return isSub(A->left,B->left) && isSub(A->right,B->right);
    }
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if(!A || !B) return false;
        bool res = false;
        if(A->val == B->val) res = isSub(A,B);
        if(!res) res = isSubStructure(A->left,B);
        if(!res) res = isSubStructure(A->right,B);
        return res; 
    }
};
```  
## 27.二叉树的镜像  
>https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/    
思路：交换每一个结点的左右结点
***  
(1)DFS:  
```
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        if(root==nullptr) return root;
        swap(root->left,root->right);
        mirrorTree(root->left);
        mirrorTree(root->right);
        return root;
    }
};
```  
(2)栈模拟：  
```
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        if(root==nullptr) return root;
        stack<TreeNode*> stk;
        stk.push(root);
        while(!stk.empty())
        {
            TreeNode* node = stk.top();
            stk.pop();
            swap(node->left,node->right);
            if(node->right) stk.push(node->right);
            if(node->left) stk.push(node->left);
        }
        return root;
    }
};
```  
(3)BFS：  
```
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        if(root==nullptr) return root;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty())
        {
            int sz = que.size();
            for(int i=0; i<sz; ++i)
            {
                TreeNode* node = que.front();
                que.pop();
                swap(node->left,node->right);
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
        }
        return root;
    }
};
```  
## 28.对称的二叉树  
>https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/  
***  
DFS:  

```
class Solution {
    bool compare(TreeNode* left, TreeNode* right)
    {
        if(left && !right) return false;
        else if(!left && right) return false;
        else if(!left && !right) return true;
        else if(left->val != right->val) return false;
        bool outSide = compare(left->left, right->right);
        bool inside = compare(left->right, right->left);
        return outSide && inside;
    }
public:
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr) return true;
        bool res = compare(root->left, root->right);
        return res;
    }
};
```  
队列：  
```
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr) return true;
        queue<TreeNode*> que;
        que.push(root->left);
        que.push(root->right);
        while(!que.empty())
        {
            TreeNode* left = que.front();que.pop();
            TreeNode* right = que.front();que.pop();
            if(!left && !right) continue;
            if(!left || !right || left->val != right->val) return false;
            que.push(left->left);
            que.push(right->right);
            que.push(left->right);
            que.push(right->left);
        }
        return true;
    }
};
```  
## 29.顺时针打印矩阵  
>https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/   
***  
```
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> res;
        if(matrix.size()==0) return res;
        int left = 0, right = matrix[0].size()-1, top = 0, bottom = matrix.size()-1;
        while(left<=right && top<=bottom)
        {
            for(int j=left; j<=right; ++j)
            {
                res.push_back(matrix[top][j]);
            }
            for(int i=top+1; i<=bottom; ++i)
            {
                res.push_back(matrix[i][right]);
            }
            if(left<right && top<bottom)
            {
                for(int j=right-1; j>left; --j)
                {
                    res.push_back(matrix[bottom][j]);
                }
                for(int i=bottom; i>top; --i)
                {
                    res.push_back(matrix[i][left]);
                }
            }
            left++;
            top++;
            right--;
            bottom--;
        }
        return res;
    }
};
```  
## 30.包含min的栈  
>https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/  
***  
```
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> data,help;
    MinStack() {

    }
    void push(int x) {
        data.push(x);
        if(help.empty() || x<=help.top()) help.push(x);
        if(x>help.top())
        {
            help.push(help.top());
        }
    }
    void pop() {
        if(!data.empty() && !help.empty())
        {
            data.pop();
            help.pop();
        }
    }
    int top() {
        return data.top();
    }
    int min() {
        return help.top();
    }
};
```  
## 31.栈的压入和弹出序列  
>https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/  
***  
```
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> stk;
        int index = 0;
        for(int i=0; i<pushed.size(); ++i)
        {
            stk.push(pushed[i]);
            while(!stk.empty() && stk.top() == popped[index])
            {
                stk.pop();
                index++;
            }
        }
        return stk.empty();
    }
};
```  
## 32.从上到下打印二叉树  
>https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/  
***  
```
class Solution {
public:
    vector<int> levelOrder(TreeNode* root) {
        vector<int> res;
        if(root==nullptr) return res;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty())
        {
            int sz = que.size();
            for(int i=0; i<sz; ++i)
            {
                TreeNode* node = que.front();
                que.pop();
                res.push_back(node->val);
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
        }
        return res;
    }
};
```  
## 33.二叉搜索树的后序遍历序列  
>https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/  
***   
(1)分治法：
```
class Solution {
    bool dfs(vector<int>& postorder, int left, int right)
    {
        if(left>=right) return true;
        int pos = left;
        int root = postorder[right];
        while(postorder[pos]<root)
        {
            pos++;
        }
        int leftEnd = pos - 1;
        while(pos<right)
        {
            if(postorder[pos]<root) return false;
            pos++;
        }
        return dfs(postorder,left,leftEnd) && dfs(postorder,leftEnd+1,right-1);
    }
public:
    bool verifyPostorder(vector<int>& postorder) {
        int len = postorder.size();
        if(len==0) return true;
        return dfs(postorder,0,len-1);
    }
};
```   
(2)单调栈：  
```
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        if(postorder.size()==0) return true;
        stack<int> stk;
        int root = INT_MAX;
        for(int i=postorder.size()-1; i>=0; --i)
        {
            //如果左子树节点大于根结点，则出现错误
            if(postorder[i]>root) return false;
            //找到左子树对应的父结点
            while(!stk.empty() && stk.top()>postorder[i])
            {
                root = stk.top();
                stk.pop();
            }
            stk.push(postorder[i]);
        }
        return true;
    }
};
```   
## 34.二叉树中和为某一值的路径  
>https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/  
***   
```
class Solution {
    vector<vector<int>> res;
    vector<int> path;
    void dfs(TreeNode* root, int target, int sum)
    {
        if(root==nullptr) return;
        path.push_back(root->val);
        sum += root->val;
        if(root->left==nullptr && root->right==nullptr && sum == target)
        {
            res.push_back(path);
        }
        dfs(root->left,target,sum);
        dfs(root->right,target,sum);
        sum -= root->val;
        path.pop_back();
    }
public:
    vector<vector<int>> pathSum(TreeNode* root, int target) {
        res.clear();
        path.clear();
        if(root==nullptr) return res;
        dfs(root,target,0);
        return res;
    }
};
```   
## 35.复杂链表的复制  
>https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/  
***  
```
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(head==nullptr) return head;
        Node* cur = head;
        //创建新节点
        while(cur)
        {
            Node* tmp = cur->next;
            cur->next = new Node(cur->val);
            cur->next->next = tmp;
            cur = tmp;
        } 
        //修改random指针
        cur = head;
        while(cur)
        {
            if(cur->random)
            {
                cur->next->random = cur->random->next;
            }
            cur = cur->next->next;
        }
        //解绑定
        Node* pre = head;
        Node* res = head->next;
        cur = head->next;
        while(cur->next)
        {
            pre->next = pre->next->next;
            cur->next = cur->next->next;
            pre = pre->next;
            cur = cur->next;
        }
        pre->next = nullptr;
        return res;
    }
};
```  
## 36.二叉搜索树和双向链表  
>https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/  
***   
(1)递归：
```
class Solution {
    Node* head=nullptr,*tail=nullptr;
public:
    Node* treeToDoublyList(Node* root) 
    {
        if(!root) return root;
        inorder(root);
        head->left = tail;
        tail->right = head;
        return head;
    }
    void inorder(Node* root)
    {
        if(!root) return;
        inorder(root->left);
        if(!tail) head = root;
        else
        {
            tail->right = root;
            root->left = tail;
        }
        tail = root;
        inorder(root->right);
    }
};
```  
(2)迭代：  
```
class Solution {
    Node* head=nullptr, *tail=nullptr;
public:
    Node* treeToDoublyList(Node* root) {
        if(root==nullptr) return root;
        stack<Node*> stk;
        Node* cur = root;
        while(cur || !stk.empty())
        {
            while(cur)
            {
                stk.push(cur);
                cur = cur->left;
            }
            cur = stk.top();
            stk.pop();
            if(head==nullptr) head = cur;
            else
            {
                cur->left = tail;
                tail->right = cur;
            }
            tail = cur;
            cur = cur->right;
        }
        head->left = tail;
        tail->right = head;
        return head;
    }
};
```   
## 37.序列化二叉树  
>https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/  
***  
```
class Codec {
    string resStr;
    void dfs(TreeNode* root)
    {
        if(root==nullptr)
        {
            resStr += "null,";
            return;
        }
        resStr += (to_string(root->val)+',');
        dfs(root->left);
        dfs(root->right);
    }
    queue<string> que;
    TreeNode* dfs1()
    {
        string cur = que.front();
        que.pop();
        if(cur == "null") return nullptr;
        TreeNode* root = new TreeNode(stoi(cur));
        root->left = dfs1();
        root->right = dfs1();
        return root;
    }
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if(root==nullptr) return "null";
        dfs(root);
        return resStr;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        //分离数据
        int i=0,j=0;
        while(i<data.size())
        {
            j = i;
            while(data[i]!=',' && i<data.size())
            {
                i++;
            }
            que.push(data.substr(j,i-j));
            i++;
        }
        TreeNode* root = dfs1();
        return root;
    }
};
```  
## 38.字符串的排列  
>https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/  
***  
```
class Solution {
    vector<string> res;
    string path;
    void backtrack(string s, vector<bool>& used)
    {
        if(path.length()==s.length())
        {
            res.push_back(path);
            return;
        }
        for(int i=0; i<s.size(); ++i)
        {
            if(i>0 && s[i-1]==s[i] && used[i-1] == false) continue;//去重
            if(used[i]) continue;
            path += s[i];
            used[i] = true;
            backtrack(s,used);
            path.pop_back();
            used[i] = false;
        }
    }
public:
    vector<string> permutation(string s) {
        res.clear();
        path.clear();
        sort(s.begin(),s.end());
        vector<bool> used(s.size());
        backtrack(s,used);
        return res;
    }
};
```   
## 39.数组中出现次数超过一半的数字  
>https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/submissions/  
***  
```
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int,int> umap;
        int n = nums.size();
        for(int i=0; i<nums.size(); ++i)
        {
            if(umap[nums[i]]++>=n/2) return nums[i];
        }
        return -1;
    }
};
```   
## 40.最小的k个数  
>https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/  
***  
```
class Solution {
    int partition(vector<int>& nums, int left, int right)
    {
        int i = left, j = right;
        int x = nums[i];
        while(i<j)
        {
            while(nums[j]>=x && i<j) j--;
            if(i<j)
            {
                swap(nums[i++],nums[j]);
            }
            while(nums[i]<x && i<j) i++;
            if(i<j)
            {
                swap(nums[j--],nums[i]);
            }
        }
        nums[i] = x;
        return i;
    }
    void quickSort(vector<int>& arr, int left, int right)
    {
        if(left >= right) return;
        int i = partition(arr, left, right);
        quickSort(arr,left,i-1);
        quickSort(arr,i+1,right);
    }
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        vector<int> vec(k, 0);
        quickSort(arr,0,arr.size()-1);
        for (int i = 0; i < k; ++i) {
            vec[i] = arr[i];
        }
        return vec;
    }
};
```  
## 41.数据流中的中位数  
>https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/  
***   
(1)简单排序(超时)  
```
class MedianFinder {
public:
    /** initialize your data structure here. */
    vector<int> data;
    MedianFinder() {

    }
    
    void addNum(int num) {
        data.push_back(num);
    }
    
    double findMedian() {
        sort(data.begin(),data.end());
        int n = data.size();
        if(n%2) return double(data[n/2]);
        return double(data[n/2]+data[n/2-1])/2;
    }
};

```  
(2)双堆法：  
```
class MedianFinder {
    priority_queue<int, vector<int>, less<int> > maxheap;
    priority_queue<int, vector<int>, greater<int> > minheap;
public:
    /** initialize your data structure here. */
    vector<int> data;
    MedianFinder() {

    }
    
    void addNum(int num) {
        if(maxheap.size()==minheap.size())
        {
            maxheap.push(num);
            minheap.push(maxheap.top());
            maxheap.pop();
        }
        else
        {
            minheap.push(num);
            maxheap.push(minheap.top());
            minheap.pop();
        }
    }
    
    double findMedian() {
        int mid1 = maxheap.top(), mid2 = minheap.top();
        return maxheap.size()==minheap.size() ? (mid1+mid2)*0.5 : mid2;
    }
};
```  
## 42.连续子数组的最大和  
>https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/  
***  
```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        vector<int> dp(nums.size());
        dp[0] = nums[0];
        int res = nums[0];
        for(int i=1; i<nums.size(); ++i)
        {
            dp[i] = max(nums[i],nums[i]+dp[i-1]);
            if(dp[i]>res)
            {
                res = dp[i];
            }
        }
        return res;
    }
};
```  
## 43.1-n的整数中1出现的次数  
>https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/  
***  
```
class Solution {
public:
    int countDigitOne(int n) {
        long digit = 1;
        int high = n/10, cur = n%10, low = 0;
        int res=0;
        while(cur || high)
        {
            if(cur==0) 
            {
                res += digit * high;
            }
            else if(cur==1)
            {
                res += (high*digit + low + 1);
            }
            else
            {
                res += (high+1)*digit;
            }
            low += cur*digit;
            cur = high%10;
            high = high/10;
            digit *= 10;
        }
        return res;
    }
};
```  
## 44.数字序列中的某一位数字  
>https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/  
***  
```
class Solution {
public:
    int findNthDigit(int n) {
        if(n==0) return 0;
        int digit = 1,start=1;
        int index = start*9*digit;
        while(n>index)
        {
            n = n-index;
            digit++;
            start *= 10;
            index = start * 9 * digit;
        }
        int num = start + (n-1)/digit;
        int mod = (n-1)%digit;
        string str = to_string(num);
        int res = int(str[mod] - '0');
        return res;
    }
};
```   
## 45.把数组排成最小的数  
>https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/  
***  
```
class Solution {
    int parttion(vector<string>& strs, int left, int right)
    {
        int i=left, j =right;
        string x = strs[i];
        while(i<j)
        {
            while(i<j && x + strs[j] <= strs[j] + x) j--;
            if(i<j)
            {
                swap(strs[i++],strs[j]);
            }
            while(i<j && strs[i] + x <= x + strs[i]) i++;
            if(i<j)
            {
                swap(strs[j--],strs[i]);
            }
        }
        strs[i] = x;
        return i;
    }
    void quickSort(vector<string>& strs, int left, int right)
    {
        if(left>right) return;
        int i = parttion(strs,left,right);
        quickSort(strs,left,i-1);
        quickSort(strs,i+1,right); 
    }
public:
    string minNumber(vector<int>& nums) {
        vector<string> strs;
        for(int i=0; i<nums.size(); ++i)
        {
            strs.push_back(to_string(nums[i]));
        }
        quickSort(strs,0,nums.size()-1);
        string res;
        for(int i=0; i<strs.size();++i)
        {
            res += strs[i];
        }
        return res;
    }
};
```  
## 46.把数字翻译成字符串  
>https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/  
***   
递归：  
```
class Solution {
    int res=0;
    void translateNum(string &num,int start) {
        if(start==num.size()){//结束条件
            res++;
            return;
        }
        translateNum(num,start+1);//分支1
        string ss=num.substr(start,2);
        if(stoi(ss)<=25&&stoi(ss)>=10) {//剪枝操作
            translateNum(num,start+2);//分支2
        }
    }
public:
    int translateNum(int num) {
        string s=to_string(num);
        translateNum(s,0);
        return res;
    }
};
```  
DP:  
```
class Solution {
public:
    int translateNum(int num) {
        string str = "0" + to_string(num);
        vector<int> dp(str.size());
        dp[0]=1;
        dp[1]=1;
        for(int i=2; i<str.size(); ++i)
        {
            dp[i] = dp[i-1];
            int tmp = stoi(str.substr(i-1,2));
            if(tmp<=25 && tmp>=10)
            {
                dp[i]+=dp[i-2];
            }
        }
        return dp[str.size()-1];
    }
};
```   
## 47.礼物的最大价值  
>https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/  
***  
动态规划:  
```
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        int m = grid.size(),n=grid[0].size();
        for(int j=1; j<n; ++j)
        {
            grid[0][j] += grid[0][j-1];
        }
        for(int i=1; i<m; ++i)
        {
            grid[i][0] += grid[i-1][0];
        }
        for(int i=1;i<m;++i)
        {
            for(int j=1;j<n;++j)
            {
                grid[i][j] += max(grid[i-1][j],grid[i][j-1]);
            }
        }
        return grid[m-1][n-1];
    }
};
```  
回溯(超时)：   

```
class Solution {
    vector<vector<int>> result;
    vector<int> path;
    int res=0;
    void backtrack(vector<vector<int>>& grid, int i, int j, int sum)
    {
        if(i>=grid.size() || j>=grid[0].size()) return;
        sum += grid[i][j];
        if(i==grid.size()-1 && j==grid[0].size()-1 && res < sum)
        {
            res = sum;
        }
        backtrack(grid,i+1,j,sum);
        backtrack(grid,i,j+1,sum);
        sum -= grid[i][j];
    }
public:
    int maxValue(vector<vector<int>>& grid) {
        backtrack(grid,0,0,0);
        return res;
    }
};
```  
## 48.最长不含重复字符的子字符串  
>https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/  
***   
(1)暴力超时
```
class Solution {
    unordered_set<char> uset;
    bool check(string s)
    {
        for(int i=0; i<s.size(); ++i)
        {
            if(uset.find(s[i]) != uset.end()) return false;
            else
            {
                uset.insert(s[i]);
            }
        }
        return true;
    }
public:
    int lengthOfLongestSubstring(string s) {
        int res = 0;
        for(int i=0; i<s.size(); ++i)
        {
            for(int j=i; j<s.size(); ++j)
            {
                uset.clear();
                if(j-i+1>res && check(s.substr(i,j-i+1)))
                {
                    cout << s.substr(i,j-i+1) << endl;
                    res = j-i+1;
                }
            }
        }
        return res;
    }
};
```  
(2)双指针+哈希  
```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if(s.size()<=1) return s.size();
        int res=0;
        int left = 0, right = 0;
        unordered_set<char> uset;
        while(left<=right && right < s.size())
        {
            if(uset.find(s[right])==uset.end())
            {
                uset.insert(s[right++]);
            }
            else
            {
                uset.erase(s[left++]);
            }
            if(res<right-left) res = right-left;
        }
        return res;
    }
};
```   
## 49.丑数  
>https://leetcode-cn.com/problems/chou-shu-lcof/  
***  
```
class Solution {
public:
    int nthUglyNumber(int n) {
        int two = 0, three = 0, five = 0;
        vector<int> dp(n);
        dp[0] = 1; // dp 初始化

        for (int i = 1; i < n; ++i) {
            int t1 = dp[two] * 2, t2 = dp[three] * 3, t3 = dp[five] * 5;
            dp[i] = min(min(t1, t2), t3);

            if (dp[i] == t1) {++ two;}
            if (dp[i] == t2) {++ three;}
            if (dp[i] == t3) {++ five;}
        }
        return dp[n - 1];
    }
};
```   
## 50.第一个只出现一次的字符  
>https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/  
***  
```
class Solution {
public:
    char firstUniqChar(string s) {
        unordered_map<char,int> umap;
        for(int i=0; i<s.size(); ++i)
        {
            umap[s[i]]++;
        }
        for(int i=0; i<s.size(); ++i)
        {
            if(umap[s[i]]==1) return s[i];
        }
        return ' ';
    }
};
```  
## 51.数组中的逆序对  
>https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/  
***    
(1)暴力超时
```
class Solution {
public:
    int reversePairs(vector<int>& nums) {
        int n = nums.size();
        if(n<=1) return 0;
        int count = 0;
        for(int i=0; i<nums.size()-1; ++i)
        {
            for(int j=i+1; j<nums.size(); ++j)
            {
                if(nums[j]<nums[i]) count++;
            }
        }
        return count;
    }
};
```  
(2)归并  
```
class Solution {
    int res = 0;
    vector<int> help;
    void merge(vector<int>&nums, int left, int right)
    {
        for(int i=left; i<=right; ++i)
        {
            help[i] = nums[i];
        }
        int mid = left + ((right-left)>>1);
        int i=left, j = mid+1, k = left;
        while(i<=mid && j<=right)
        {
            if(help[i]<=help[j])
            {
                nums[k++] = help[i++];
            }
            else
            {
                nums[k++] = help[j++];
                res += mid - i + 1;
            }
        }
        while(i<=mid)
        {
            nums[k++] = help[i++];
        }
        while(j<=right)
        {
            nums[k++] = help[j++];
        }
    }
    void mergeSort(vector<int>& nums, int left, int right)
    {
        if(left>=right) return;
        int mid = left + ((right-left)>>1);
        mergeSort(nums,left,mid);
        mergeSort(nums,mid+1,right);
        merge(nums,left,right); 
    }
public:
    int reversePairs(vector<int>& nums) {
        int n = nums.size();
        if(n<=1) return 0;
        help.resize(n,0);
        mergeSort(nums,0,n-1);
        return res;
    }
};
```   
## 52.两个链表的第一个公共结点  
>https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof  
***  
```
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *node1 = headA;
        ListNode *node2 = headB;
        
        while (node1 != node2) {
            node1 = node1 != NULL ? node1->next : headB;
            node2 = node2 != NULL ? node2->next : headA;
        }
        return node1;
    }
};
```   
## 53.在排序数组中统计一个数字出现的次数  
>https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/  
***    
(1)暴力：
```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int count=0;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i] == target)
            count++;
        }
        return count;
    }
};
```   
(2)二分查找  
```
class Solution {
    int leftSearch(vector<int>& nums, int target)
    {
        int left=0,right=nums.size()-1;
        while(left<right)
        {
            int mid = left + ((right-left)>>1);
            if(nums[mid]>=target)
            {
                right = mid;
            }
            else
            {
                left = mid+1;
            }
        }
        if(nums[left]==target)
        return left;
        else 
        return -1;
    }
    int rightSearch(vector<int>& nums, int target)
    {
        int left=0,right=nums.size()-1;
        while(left<right)
        {
            int mid = left + ((right-left+1)>>1);
            if(nums[mid]<=target)
            {
                left = mid;
            }
            else
            {
                right = mid - 1;
            }
        }
        if(nums[right]==target) return right;
        else return -1;
    }
public:
    int search(vector<int>& nums, int target) {
        //折半查找左右边界
        int n = nums.size();
        if(n==0) return 0;
        int left_boundary = leftSearch(nums,target);
        int right_boundary = rightSearch(nums,target);
        if(left_boundary==-1) return 0;
        return right_boundary-left_boundary+1;
    }
};
```  
## 53.0-n-1中缺失的数字  
>https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/  
(1)暴力  
```
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int i;
        for(i=0; i<nums.size(); i++)
        {
            if(nums[i] != i) return i;
        }
        return i;
    }
};
```  
(2)二分  
```
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        int left=0, right=n-1;
        while(left<right)
        {
            int mid = left + ((right-left)>>1);
            if(nums[mid]>mid)
            {
                right = mid;
            }
            else
            {
                left = mid + 1;
            }
        }
        if(nums[left]>left) return left;
        else return left+1;
    }
};
```  
## 54.二叉搜索树的第k大结点  
>https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/  
***  
```
class Solution {
public:
    int kthLargest(TreeNode* root, int k) {
        stack<TreeNode*> stk;
        TreeNode* cur = root;
        while(!stk.empty() || cur)
        {
            while(cur)
            {
                stk.push(cur);
                cur = cur->right;
            }
            cur = stk.top();
            stk.pop();
            if(!(--k)) return cur->val;
            cur = cur->left;
        }
        return cur->val;
    }
};
```

