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

