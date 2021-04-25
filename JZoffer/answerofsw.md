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
