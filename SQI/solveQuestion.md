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

