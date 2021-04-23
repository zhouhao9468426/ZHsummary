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