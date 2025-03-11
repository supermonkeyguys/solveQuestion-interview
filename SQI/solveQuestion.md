## [合并区间-----3/9----25](https://leetcode.cn/problems/merge-intervals/description/?envType=study-plan-v2&envId=top-interview-150)

```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if(intervals.empty()) {
        return {};
    }
    sort(intervals.begin() , intervals.end());
    vector<vector<int>> ans;
    for(int i = 0 ; i < intervals.size() ; ++i ) {
        int L = intervals[i][0] , R = intervals[i][1];
        if(!ans.size() || ans.back()[1] < L ) {
            ans.push_back({L , R});
        }
        else {
            ans.back()[1] = max(ans.back()[1] , R );
        }
    } 
    return ans;
    }
};
```



## [三数之和 3/10---25](https://leetcode.cn/problems/3sum/?envType=study-plan-v2&envId=top-interview-150)

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> ans;

        sort(nums.begin(), nums.end());
        for (int i = 0; i < n; i ++ )
        {
            int k = n - 1;
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            for (int j = i + 1; j < n; j ++ )
            {
                if (j > i + 1 && nums[j] == nums[j - 1]) continue;
                int sum = nums[i] + nums[j];

                while (k > j && nums[k] + sum > 0) k --;
                if (k <= j) break;
                if (nums[k] + sum == 0) ans.push_back({nums[i], nums[j], nums[k]});
            }
        }

        return ans;
    }
};
```



```javascript
var threeSum = function(nums) {
    nums.sort((a,b) => a - b)
    const result = []
    for(var i = 0 ; i < nums.length - 2 ; i ++ ){
        if(i > 0 && nums[i] === nums[i - 1])continue;
        if(nums[i] > 0)break;

        var left = i + 1 
        var right = nums.length - 1;

        while(left < right){
            const sum = nums[i] + nums[left] + nums[right]
            if(sum > 0){
                right -- ;
            }
            else if(sum < 0){
                left ++ ;
            }
            else {
                result.push([nums[i],nums[left],nums[right]])
                while(left < right && nums[left] === nums[left + 1])left ++ ;
                while(left < right && nums[right] === right[right - 1])right -- ;
                left ++ ;
                right -- ;
            }
        }
    }
    return result;
};
```



## [删除有序数组中的重复项 3/10---25](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/?envType=study-plan-v2&envId=top-interview-150)

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        vector<int>noRep;
        for(int i = 0 ; i < nums.size() ; i ++ ){
            if(noRep.empty() || noRep.back() != nums[i])noRep.push_back(nums[i]);
        }
        nums = noRep;
        return noRep.size();
    }
};
```



```javascript
var removeDuplicates = function(nums) {
    var index = 1;
    for(var i = 1 ; i < nums.length ; i ++ ){
        if(nums[i] !== nums[i - 1]){
            nums[index ++ ] = nums[i];
        }
    }
    return index;
};
```



## [零钱兑换 3/11---25](https://leetcode.cn/problems/coin-change/description/?envType=study-plan-v2&envId=top-interview-150)

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        if(coins.size() == 0)return -1;
        vector<int>coinsCount(amount + 1,amount + 1);
        coinsCount[0] = 0;
        for(int i = 1 ; i <= amount ; i ++ ){
            for(auto coin : coins){
                if(coin <= i)
                coinsCount[i] = min(coinsCount[i],coinsCount[i - coin] + 1);
            }
        }
        if(coinsCount[amount] > amount)return -1;
        else return coinsCount[amount];
    }
};
```



```javascript
var coinChange = function(coins, amount) {
    var result = new Array(amount + 1).fill(amount + 1)
    result[0] = 0;
    coins.forEach(coin => {
        for(var i = coin ; i <= amount ; i ++ ){
            result[i] = Math.min(result[i],result[i - coin] + 1)
        }
    })
    return result[amount] == amount + 1 ? -1 : result[amount]
};
```



## [被围绕的区域 3/11---25](https://leetcode.cn/problems/surrounded-regions/submissions/608930518/?envType=study-plan-v2&envId=top-interview-150)

```c++
class Solution {
private:
    int dx[4] = {1,-1,0,0};
    int dy[4] = {0,0,1,-1};
public:
    void solve(vector<vector<char>>& board) {
        int n = board.size();
        int m = board[0].size();
        for(int i = 0 ; i < n ; i ++ ){
            if(board[i][0] == 'O')dfs(i,0,board);
            if(board[i][m - 1] == 'O')dfs(i,m - 1,board);
        }
        for(int i = 0 ; i < m ; i ++ ){
            if(board[0][i] == 'O')dfs(0,i,board);
            if(board[n - 1][i] == 'O')dfs(n - 1,i,board);
        }

        for(int i = 0 ; i < n ; i ++ ){
            for(int j = 0 ; j < m ; j ++ ){
                if(board[i][j] == 'O')board[i][j] = 'X';
                if(board[i][j] == 'Y')board[i][j] = 'O';
            }
        }
    }
    void dfs(int x,int y,vector<vector<char>>& board){
        int n = board.size();
        int m = board[0].size();
        if (x < 0 || x >= n || y < 0 || y >= m || board[x][y] != 'O') return;
        board[x][y] = 'Y';

        for(int i = 0 ; i < 4 ; i ++ ){
            int x1 = x + dx[i];
            int y1 = y + dy[i];
            if(x1 >= 0 && x1 < n && y1 >= 0 && y1 < m){
                dfs(x1,y1,board);
            }
        }

    }
};
```

