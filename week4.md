# week4

这周主要学习了字符串的一个重要算法：KMP算法

KMP算法本质上分为两个部分：next数组的构建和原数组匹配next数组

next数组本质上是一个前缀表，也就是给出当前子串时有几个前缀与后缀相同。

比如aaba，他的值应该是1

aabaa则应该是2

前缀表的本质，就是在你在主数组中寻找子串时可以节约时间，跳过中途肯定不能匹配的部分（若一个子串前面部分不重合，那他后移一位一定与主串不重合），直接寻找下一个可能匹配的地方，大大节省了时间。

[28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

**示例 1：**

```
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 。
```

**示例 2：**

```
输入：haystack = "leetcode", needle = "leeto"
输出：-1
解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
```



毫无疑问，KMP算法经常被应用于找子串，找重复的场合。

在这里，先构建前缀表也就是next数组，一共有四步：

1.初始化

2.前后缀不相同的情况

3.前后缀相同的情况

4.更新next值

```cpp
void getNext(int* next, const string& s){
    int j = 0;
    next[0] = j;
    for(int i = 1; i < s.size(); i++) { // 注意i从1开始
        while (j > 0 && s[i] != s[j ]) { // 前后缀不相同了
            j = next[j-1]; // 向前回退
        }
        if (s[i] == s[j ]) { // 找到相同的前后缀
            j++;
        }
        next[i] = j; // 将j（前缀的长度）赋给next[i]
    }
}
```

注意i一定是要在j前方，所以for循环时i应该初始为1.

而当前后缀不相同的情况，则应该让j会退，直到会退到第一个前后缀相同的地方或0处。

当前后缀相同，j向前一步，并把j赋给i处的next值。

j可以看成一个指针，也可以看成一个值，他的作用主要是把next[i]的值形象化为前后缀相等的情况（比如当j=1，next值也为1，相同的前后缀就有2个）

再来看主函数，大致的遍历思路与next一致，不一样的是这时的j就是子串s的一个指针，并将其与主串对比。

还是若不相等，j回退，若相等j前进。

最后当j到子串尾，则进行一个简单的数值计算返回索引。

```cpp
int strStr(string haystack, string needle) {
        if (needle.size() == 0) {
            return 0;
        }
        vector<int> next(needle.size());
        getNext(&next[0], needle);
        int j = 0;
        for (int i = 0; i < haystack.size(); i++) {
            while(j > 0 && haystack[i] != needle[j]) {
                j = next[j - 1];
            }
            if (haystack[i] == needle[j]) {
                j++;
            }
            if (j == needle.size() ) {
                return (i - needle.size() + 1);
            }
        }
        return -1;
    }
};
```

[459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

给定一个非空的字符串 `s` ，检查是否可以通过由它的一个子串重复多次构成。

 

**示例 1:**

```
输入: s = "abab"
输出: true
解释: 可由子串 "ab" 重复两次构成。
```

**示例 2:**

```
输入: s = "aba"
输出: false
```

**示例 3:**

```
输入: s = "abcabcabcabc"
输出: true
解释: 可由子串 "abc" 重复四次构成。 (或子串 "abcabc" 重复两次构成
```

这题看到重复子串就可以反应过来，也许可以用kmp算法，但是怎么用呢？

仔细看一个重复字符串，发现重复字符串的末尾的next值一定是重复子串长度的整数倍，所以我们可以通过len%(len-(next[len-1]))==0来判断。

```cpp
class Solution {
public:
    void getNext (int* next, const string& s){
        next[0] = -1;
        int j = -1;
        for(int i = 1;i < s.size(); i++){
            while(j >= 0 && s[i] != s[j + 1]) {
                j = next[j];
            }
            if(s[i] == s[j + 1]) {
                j++;
            }
            next[i] = j;
        }
    }
    bool repeatedSubstringPattern (string s) {
        if (s.size() == 0) {
            return false;
        }
        int next[s.size()];
        getNext(next, s);
        int len = s.size();
        if (next[len - 1] != -1 && len % (len - (next[len - 1] + 1)) == 0) {
            return true;
        }
        return false;
    }
};
```

这种题主要是要找到规律，想通过数学手段找到证明有点困难。

KMP的主要思想是**当出现字符串不匹配时，可以知道一部分之前已经匹配的文本内容，可以利用这些信息避免从头再去做匹配了。**