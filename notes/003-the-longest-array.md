# 学完哈希表，努力解决这个问题失败，还是代码水平太菜了，想不到方法，直接记录题解了
## 思路，先用set去掉重复的值，为了减少计算时间，从其中先找到最小的值num，再计数，用count可以返回0或1，然后从num+1开始计数，统计currentStreak的个数，保存其中的最大值，最后返回答案。
### 代码实现
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int>num_set;
        for (const int & num :nums)
        {
            num_set.insert(num);
        }
        int longestStreak = 0;
        for (const int &num:num_set)
        {
            if (!num_set.count(num-1))
            {
                int currentNum=num;
                int currentStreak=1;
                while(num_set.count(currentNum+1))
                {
                    currentNum+=1;
                    currentStreak+=1;
                }
                longestStreak=max(longestStreak,currentStreak);
            }
        }
        return longestStreak;
    }
};
```
### 太牛了，我原本思路是每个数存在键值对中，用unordered_map<int,vector<int>>,以数为键，再把数存在对应的vector，每存一个，把vector移动到num-1的键中，最后返回每个vector的长度，取最大值返回，最后实在觉得太麻烦，思路太垃圾了，没法实现这个算法，官解值得学习，非常nb，希望以后我也能写出来这种代码。