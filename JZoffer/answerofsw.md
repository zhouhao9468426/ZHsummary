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