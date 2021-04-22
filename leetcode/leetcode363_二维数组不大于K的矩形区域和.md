## leetcode363.矩形区域不超过K的最大数值和  
> https://leetcode-cn.com/problems/max-sum-of-rectangle-no-larger-than-k/  
***
```
思路一：暴力解法（超时）  
    class Solution {
    int recSum(vector<vector<int>>& matrix, int m0, int n0, int m, int n)
    {
        int sum=0;//一定要初始化
        for(int i=m0; i<=m; ++i)
        {
            for(int j=n0; j<=n; ++j)
            {
                sum += matrix[i][j];
            }
        }
        return sum;
    }
	public:
    int maxSumSubmatrix(vector<vector<int>>& matrix, int k) {
        //暴力解法：遍历所有可能矩形块
        int m = matrix.size();
        int n = matrix[0].size();
        int ans = -1000001;
        for(int i=0; i<m; ++i)
        {
            for(int j=0; j<n; ++j)
            {
                for(int i1=i; i1<m; ++i1)
                {
                    for(int j1=j; j1<n; ++j1)
                    {
                        int tmp = recSum(matrix,i,j,i1,j1);
                        cout << tmp << endl;
                        if(tmp == k) return k;
                        else if(tmp < k && tmp > ans)
                        {
                            ans = tmp;
                        }
                    }
                }
            }
        }
        return ans;
    }
	};  
***
思路二：利用二维数组前缀和优化  
    class Solution {
	public:
    int maxSumSubmatrix(vector<vector<int>>& matrix, int k) {
        //暴力解法：遍历所有可能矩形块
        int m = matrix.size();
        int n = matrix[0].size();
        vector<vector<int>> sum(m+1,vector<int>(n+1));
        for(int i=1; i<=m; ++i)
        {
            for(int j=1; j<=n; ++j)
            {
                sum[i][j] = sum[i-1][j]+sum[i][j-1]-sum[i-1][j-1]+matrix[i-1][j-1];
            }
        }
        int ans = -1000001;
        for(int i=1; i<=m; ++i)
        {
            for(int j=1; j<=n; ++j)
            {
                for(int i1=i; i1<=m; ++i1)
                {
                    for(int j1=j; j1<=n; ++j1)
                    {
                        int tmp = sum[i1][j1]-sum[i1][j-1]-sum[i-1][j1]+sum[i-1][j-1];
                        cout << tmp << endl;
                        if(tmp == k) return k;
                        else if(tmp <= k && tmp > ans)
                        {
                            ans = tmp;
                        }
                    }
                }
            }
        }
        return ans;
    }
	};  
```