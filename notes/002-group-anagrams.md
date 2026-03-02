# 今天是学习算法题的第二天，按照分类，做hot100中关于哈希表的第二题
## [2.字母异位词分组](https://leetcode.cn/problems/group-anagrams/description/?envType=study-plan-v2&envId=top-100-liked)
## 学习了vector容器的相关用法
### 方法
    用哈希表存错使用sort函数排序后的结果，用这个作为key，来存贮原来的字符串，这样排序后每个key里的value就是异位词的分组
##  代码实现
```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>>map;
        for (auto it :strs)
        {
            string s =it;
            sort(s.begin(),s.end());
            map[s].push_back(it);
        }
        vector<vector<string>>result;
        for (auto & p:map)
        {
            result.push_back(p.second);
        }
        return result;

    }
};
```
