# 数组



# 双指针

## 283. 移动0

![image-20240324201252847](C:\Users\hao\AppData\Roaming\Typora\typora-user-images\image-20240324201252847.png)

**解析：**

​		将不是0的元素移至数组前面，一个快指针查找不是0的元素，一个慢指针从第一个元素开始填(==只有发生了交换才移动慢指针==)。

```c++
void moveZeroes(vector<int>& nums) {
    auto fast = nums.begin(), slow = nums.begin();
    while(fast != nums.end()) {
        if(*fast != 0) {
            swap(*fast, *slow);
            slow++;
        }   
        fast++;
    }
}
```

## 11. 盛最多水的容器

![image-20240324210855540](C:\Users\hao\AppData\Roaming\Typora\typora-user-images\image-20240324210855540.png)

**解析：**

​		从两端开始向内收缩，因为每次移动指针宽度是不断缩小的，所以只有增加高度才可能比现在装更多的水。而如果移动较高的一边即使下一个高度比现在还高，由于最短边已经确定，所以装的水只会更少，所以只能移动高度较低的一端。

```c++
int maxArea(vector<int>& height) {
    int left = 0, right = height.size() - 1;
    int width = right;
    int max = 0;
    int temp = 0;
    while(left != right) {
        if(height[left] < height[right]) {
            temp = height[left] * width;
            left++;
        } else {
            temp = height[right] * width;
            right--; 
        }
        if(max < temp) {
            max = temp;
        }
        width--;
    }
    return max;

}
```

## 15. 三数之和

![image-20240325085403549](C:\Users\hao\AppData\Roaming\Typora\typora-user-images\image-20240325085403549.png)

**关键词：**==不重复==

​			不重复的问题一般可以通过排序来解决

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int length = nums.size() - 1;
        vector<vector<int>> ans;
        sort(nums.begin(), nums.end());
        for(int i = 0; i < length - 1; i++) {
            if (i > 0 && nums[i] == nums[i - 1])
                continue;
            auto result = twoSum(nums, i, -nums[i]);
            ans.insert(ans.end(), result.begin(), result.end());
        }
        return ans;
    }
    vector<vector<int>> twoSum(vector<int> & nums, int start, int target) {
        int left = start + 1;
        int right = nums.size() - 1;
        vector<vector<int>> ans;
        while(left < right) {
            if (nums[left] + nums[right] == target){
                ans.push_back({nums[start], nums[left], nums[right]});
                left++;
                while (left < right && nums[left] == nums[left - 1])
                    left++;
            } else if (nums[left] + nums[right] < target) {
                left++;
            } else {
                right--;
            }
        }
        return ans;
    }

};
```



## 42. 接雨水

![image-20240325084131711](C:\Users\hao\AppData\Roaming\Typora\typora-user-images\image-20240325084131711.png)

**双指针法**

解析：

​		当前柱子的接水量由，当前柱子的左边最高的柱子和右边的最高柱子的较小值确定；

​		从左右两边遍历数组，更新当前柱子左右两边柱子的最大值，直到两指针相遇。

```c++
int trap(vector<int>& height) {
    int left = 0, right = height.size() - 1;
    int lmax = 0, rmax = 0;
    int area = 0;
    // 双指针
    while(left < right) {
        if(height[left] < height[right]) {
            if(height[left] < lmax) {
                area += (lmax - height[left]);
            } else {
                lmax = height[left];
            }
            left++;
        } else {
            if(height[right] < rmax) {
                area += (rmax - height[right]);
            } else {
                rmax = height[right];
            }
            right--;
        }
    }
    return area;
}
```





# 滑动窗口

## 算法思路

![image-20240325151223687](C:\Users\hao\AppData\Roaming\Typora\typora-user-images\image-20240325151223687.png)

+ 首先，初始化 `window` 和 `need` 两个哈希表，记录窗口中的字符和需要凑齐的字符：

```c++
unordered_map<char, int> need, window;
for (char c : t) need[c]++;
```

+ 然后，使用 `left` 和 `right` 变量初始化窗口的两端，不要忘了，区间 `[left, right)` 是左闭右开的，所以初始情况下窗口没有包含任何元素：

```c++
int left = 0, right = 0;
int valid = 0; 
while (right < s.size()) {
    // 开始滑动
}
```

其中 valid 变量表示窗口中满足 need 条件的字符个数，如果 valid 和 need.size 的大小相同，则说明窗口已满足条件，已经完全覆盖了串 T。

现在开始套模板，只需要思考以下四个问题：

1、当移动 right 扩大窗口，即加入字符时，应该更新哪些数据？

2、什么条件下，窗口应该暂停扩大，开始移动 left 缩小窗口？

3、当移动 left 缩小窗口，即移出字符时，应该更新哪些数据？

4、我们要的结果应该在扩大窗口时还是缩小窗口时进行更新？

如果一个字符进入窗口，应该增加 window 计数器；如果一个字符将移出窗口的时候，应该减少 window 计数器；当 valid 满足 need 时应该收缩窗口；应该在收缩窗口的时候更新最终结果。

```c++
string minWindow(string s, string t) {
    unordered_map<char, int> need, window;
    for (char c : t) need[c]++;

    int left = 0, right = 0;
    int valid = 0;
    // 记录最小覆盖子串的起始索引及长度
    int start = 0, len = INT_MAX;
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 右移窗口
        right++;
        // 进行窗口内数据的一系列更新
        if (need.count(c)) {
            window[c]++;
            if (window[c] == need[c])
                valid++;
        }

        // 判断左侧窗口是否要收缩
        while (valid == need.size()) {
            // 在这里更新最小覆盖子串
            if (right - left < len) {
                start = left;
                len = right - left;
            }
            // d 是将移出窗口的字符
            char d = s[left];
            // 左移窗口
            left++;
            // 进行窗口内数据的一系列更新
            if (need.count(d)) {
                if (window[d] == need[d])
                    valid--;
                window[d]--;
            }                    
        }
    }
    // 返回最小覆盖子串
    return len == INT_MAX ?
        "" : s.substr(start, len);
}
```

需要注意的是，当我们发现某个字符在 window 的数量满足了 need 的需要，就要更新 valid，表示有一个字符已经满足要求。而且，你能发现，两次对窗口内数据的更新操作是完全对称的。

当 valid == need.size() 时，说明 T 中所有字符已经被覆盖，已经得到一个可行的覆盖子串，现在应该开始收缩窗口了，以便得到「最小覆盖子串」。

移动 left 收缩窗口时，窗口内的字符都是可行解，所以应该在收缩窗口的阶段进行最小覆盖子串的更新，以便从可行解中找到长度最短的最终结果。

https://leetcode.cn/problems/find-all-anagrams-in-a-string/solutions/9749/hua-dong-chuang-kou-tong-yong-si-xiang-jie-jue-zi-

## 3. 无重复字符的最长子串

![image-20240325164441231](C:\Users\hao\AppData\Roaming\Typora\typora-user-images\image-20240325164441231.png)

**出现次数：**==散列表==

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        //哈希表记录元素最后出现的位置
        unordered_map<char, int> hash;
        int ans = 0, left = 0;
        int i;
        for(i = 0; i < s.length(); ++i){
            char cur = s[i];
            if(hash.count(cur)){
                ans = max(ans, i - left);
                left = max(left, hash[cur] + 1);
            }
            hash[cur] = i;
        }
        return max(ans, i - left);
    }
};
```

## 438. 找到字符串中所有字母异位词

![image-20240325170400735](C:\Users\hao\AppData\Roaming\Typora\typora-user-images\image-20240325170400735.png)

```c++
vector<int> findAnagrams(string s, string p) {
    unordered_map<char, int> need, window;
    for(char c : p)
        need[c]++;

    int left = 0, right = 0;
    int valid = 0;
    vector<int> ans;
    while(right < s.size()) {
        // 向右滑动窗口
        char c = s[right];
        right++;
        if(need.count(c)) {
            window[c]++;
            if(window[c] == need[c])
                valid++;
        }
        while(right - left >= p.size()) {
            // 窗口大小满足，判断值是否满足要求
            if(valid == need.size())
                ans.push_back(left);
            // 更新窗口中的值
            char d = s[left];
            left++;
            if(need.count(d)) {
                if(window[d] == need[d])
                    valid--;
                window[d]--;
            }
        }
    }
    return ans;
}
```

