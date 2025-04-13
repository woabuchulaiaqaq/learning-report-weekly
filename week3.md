

# Week3 #

这两周主要在推进吴恩达的机器学习课以及计网和操作系统，没有怎么刷算法题。在下一周看完最后一个section后开始学习java和深度学习。算法题只能在空闲时候抽空写几道了。

这里主要总结一下写的神经网络的lab：

# Neural Networks for Handwritten Digit Recognition, Binary

第一个lab是关于识别手写数字

这里使用神经网络来训练，第一个需要注意的点就是每一层的参数w和b的维数

![](/Users/zixuanchen/Desktop/截屏2025-03-22 19.52.21.png)

首先使用tensorflow的dense方法来构建神经网络

然后用fit方法实现反向传播等算法

接下来就是多维情况下的图像识别

这里我们使用relu作为激活函数，输出的时候使用softmax函数

softmax函数有一个特殊功能，就是让输出的数字变成一个概率数字，其公式与贝叶斯公式有一定的相似之处

![](/Users/zixuanchen/Desktop/截屏2025-03-22 19.57.38.png)

接下来学习了决策树的相关内容，决策树的基本构建步骤有如下几步：

首先计算各结点的熵

接下来根据数据化的描述将其分割，其中结果为1的在左子树，结果为0的在右子树

再计算各点的信息增益，![](/Users/zixuanchen/Desktop/截屏2025-03-22 20.04.15.png)

以此便利树，在树上进行划分，发现最好的划分结点。

## 算法 ##

主要写了几道算法题，难度都不大，主要是对哈希表有一个清晰的认识，以及要学会什么时候恰当使用哈希表

## 242.有效的字母异位词 ##



很经典的哈希表题目，但是重点是：我们要怎么想到用哈希表，我个人理解主要有以下几种情况：

1. 需要映射的时候，也就是由多个重复数据，需要对数据进行计数处理时
2. 给多个数组求和，求定值时



还有一个很重要的点，就是哈希表只有在很简单的情况下（单纯计数）会用数组，在一些复杂情况下（涉及数值以及重复数据的计算）时需要使用map和set作为容器。一般使用unorderedmap或set，时间复杂度小，同时可以帮助去除重复字符。

![](/Users/zixuanchen/Desktop/截屏2025-03-23 18.20.26.png)

![](/Users/zixuanchen/Desktop/截屏2025-03-23 18.21.38.png)

来说一说：使用数组和set来做哈希法的局限。

- 数组的大小是受限制的，而且如果元素很少，而哈希值太大会造成内存空间的浪费。
- set是一个集合，里面放的元素只能是一个key，而两数之和这道题目，不仅要判断y是否存在而且还要记录y的下标位置，因为要返回x 和 y的下标。所以set 也不能用。

map是一种`<key, value>`的结构，本题可以用key保存数值，用value在保存数值所在的下标。所以使用map最为合适。

**当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法**。

回到这道题

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

示例 1: 输入: s = "anagram", t = "nagaram" 输出: true

示例 2: 输入: s = "rat", t = "car" 输出: false

**说明:** 你可以假设字符串只包含小写字母。

就已经非常的easy，只需要把第一个数组中的字母在哈希表中加一，然后在再第二个数组中搜索，若搜索到则哈希表的值减一，若最后得到一个全为零的数组，则返回true。



[349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

给定两个数组 `nums1` 和 `nums2` ，返回 *它们的 交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

 

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例 2：**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的
```

这道题就需要用set了，很明显，因为这里存在重复的数字，如果用数组存第一个输出就应该是22，所以我们需要借助unordered_set。在第二个数组中搜索，只需要存在相同数字就把这个数字加入set中。



[454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)

给你四个整数数组 `nums1`、`nums2`、`nums3` 和 `nums4` ，数组长度都是 `n` ，请你计算有多少个元组 `(i, j, k, l)` 能满足：

- `0 <= i, j, k, l < n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

 

**示例 1：**

```
输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
输出：2
解释：
两个元组如下：
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

**示例 2：**

```
输入：nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
输出：1
```

这道题就是使用map的典型案例，看到题目中说到的四个数组，同时我们需要把四个数组分为两部分，为什么是两部分呢？

因为很明显我们需要进行数组内循环查找，如果分为1，3，那时间复杂度将会是n的三次方，而2，2则是n的平方！

所以我们只需要把前两个数组看成一组，并且计算a+b的值以及对应出现的次数，然后存入map中，这样就可以在第3，4个数组中查找a+b的相反数，若存在，则count+=a+b出现的次数（因为可能出现a+b值相等，但是有多种组合的情况）。

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
        unordered_map<int, int> umap; //key:a+b的数值，value:a+b数值出现的次数
        // 遍历大A和大B数组，统计两个数组元素之和，和出现的次数，放到map中
        for (int a : A) {
            for (int b : B) {
                umap[a + b]++;
            }
        }
        int count = 0; // 统计a+b+c+d = 0 出现的次数
        // 再遍历大C和大D数组，找到如果 0-(c+d) 在map中出现过的话，就把map中key对应的value也就是出现次数统计出来。
        for (int c : C) {
            for (int d : D) {
                if (umap.find(0 - (c + d)) != umap.end()) {
                    count += umap[0 - (c + d)];
                }
            }
        }
        return count;
    }
};
```





[15. 三数之和](https://leetcode.cn/problems/3sum/)

这道题与上方的四数之和不一样

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。 

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

**提示：**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

这两道题有本质区别，自然解题方法也不同。首先上题是输出个数，而这道题是直接输出组成元组的数。其次，这道题规定答案不能重复，一个元组内数字可以重复，但是最后的答案数组不能重复。所以需要进行去重操作。

所以我们使用双指针

这道题最需要注意的就是去重操作，所以我们可以先对原数组进行排序，这样就可以把相同的数字放在一起，方便去重。

去重操作主要是：当发现与前一个数据值相同，则直接向后移一位，即跳过这个数字，防止重复。

```cpp
lass Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        // 找出a + b + c = 0
        // a = nums[i], b = nums[left], c = nums[right]
        for (int i = 0; i < nums.size(); i++) {
            // 排序之后如果第一个元素已经大于零，那么无论如何组合都不可能凑成三元组，直接返回结果就可以了
            if (nums[i] > 0) {
                return result;
            }
            // 错误去重a方法，将会漏掉-1,-1,2 这种情况
            /*
            if (nums[i] == nums[i + 1]) {
                continue;
            }
            */
            // 正确去重a方法
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (right > left) {
                // 去重复逻辑如果放在这里，0，0，0 的情况，可能直接导致 right<=left 了，从而漏掉了 0,0,0 这种三元组
                /*
                while (right > left && nums[right] == nums[right - 1]) right--;
                while (right > left && nums[left] == nums[left + 1]) left++;
                */
                if (nums[i] + nums[left] + nums[right] > 0) right--;
                else if (nums[i] + nums[left] + nums[right] < 0) left++;
                else {
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    // 去重逻辑应该放在找到一个三元组之后，对b 和 c去重
                    while (right > left && nums[right] == nums[right - 1]) right--;
                    while (right > left && nums[left] == nums[left + 1]) left++;

                    // 找到答案时，双指针同时收缩
                    right--;
                    left++;
                }
            }

        }
        return result;
    }
};
```
