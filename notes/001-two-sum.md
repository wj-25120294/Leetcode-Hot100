#[这是我大一下学期开始正式刷代码题目的第一天2026/03/01，不知道未来是什么样，希望我坚持下去，先从leetcode的hot100开始]
# [1.两数之和](https://leetcode.cn/problems/two-sum/?envType=study-plan-v2&envId=top-100-liked)
## 先学习什么是哈希表，用ai练习了哈希表的创建，增删改查，下面是代码
```cpp
#include<bits/stdc++.h>
using namespace std;
class HashMap{
private:
    static const int BASE =5;
    vector<list<pair<int,string>>> buckets;
    int hashFunc(int key) const{
        return key%BASE;
    }
public:
    HashMap():buckets(BASE){}
    void put(int key,const string &value)
    {
        int idx =hashFunc(key);
        for (auto &p :buckets[idx])
        {
            if (p.first==key)
            {
                p.second = value;
                return;
            }
        }
        buckets[idx].push_back({key,value});
    }
    string get(int key) const 
    {
        int idx = hashFunc(key);
        for (const auto &p:buckets[idx])
        {
            if (p.first==key)
            {
                return p.second;
            }
        }
        return "";
    }
    void remove(int key)
    {
        int idx=hashFunc(key);
        for (auto it =buckets[idx].begin();it!=buckets[idx].end();++it)
        {
            if(it->first == key)
            {
                buckets[idx].erase(it);
                return;
            }
        }
    }
};
int main()
{
    HashMap map;
    map.put(12836, "小哈");
    map.put(15937, "小啰");
    map.put(16750, "小算");
    cout << map.get(15937) << endl;
    map.remove(15937);
    cout << map.get(15937) << endl;
    return 0;
}
```
### 方法一
使用双重循环遍历数组，找到对应的两个下标，简单粗暴，但是时间复杂度太高
## 代码实现
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for (int i=0;i<nums.size();i++)
        {
            for (int j=i+1;j<nums.size();j++)
            {
                if (nums[i]+nums[j]==target)
                {
                    return{i,j};
                }
            }
        }
        return {} ;
    }
};
```
### 方法二
学习过leetcode官方题解后，知道可以用哈希表存贮历遍过的元素，再用find函数，直接得出两个下标，由于哈希表的时间复杂度是O（1）所以时间非常快，我复制敲了一遍。
## 代码实现
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int>map;
        for (int i=0;i<nums.size();++i)
        {
            auto it =map.find(target-nums[i]);
            if (it!=map.end())
            {
                return {it->second,i};
            }
            map[nums[i]]=i;
        }
        return {};
    }
};
```