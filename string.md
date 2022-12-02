# String



```c++
isdigit(char c) / isalpha(char c)
islower(char c) / isupper(char c)
tolower(char c) / toupper(char c)
std::string to_string( int value); // 数字转字符串
int stoi( const std::string& str, std::size_t* pos = 0, int base = 10); // 字符串转数字，默认输入数字为十进制
string.substr(start, len); 
std::string::npos size_t // 最大值-1，find等不存在时的返回值
string::find(char c /*string str*/, size_t pos) //从pos位置查找字符c或字符串，返回第一个结果索引，不存在返回std::string::npos; 使用KMP算法O(n)
string::find_first_of //从pos位置开始查找字符c或字符串的任意字符，返回第一个结果索引位置  string::find_last_of 从尾部开始查找
string::find_first_not_of/string::find_last_not_of //找第一个不匹配的
```



## Split

```c++
// input string str and need to split str with char c;
for (int j = 0; j < str.size(); j++) {
    if (ISVALID(str[j])) {
        // adjust i if there requires without leading zeros
        i += (j > i && str[i] == '0');
    } else { // 1 2 3 _ _ 4 when j from _ to 4, i needs to be on 4.
        auto one = str.substr(i, j - i);
        i = j + 1;
    }
}
```



判断两个str字母个数和数量相等

```c++
RETURN_FALSE(str1.size() != str2.size());
for (auto c : str1) {
	map[c]++; // map可以为std::map 或 int map[256] = {0};
}

// str2各字母数量都不小于str1时，但由于str1和str2 长度相同，则表明各字母数量相同
for (auto c : str2) {
    RETURN_IF(map[c]-- <= 0, false);
}
return true;
```

## StringToInt

合法性检查

+ 只以“-”或数字开头，长度大于1时不允许"0开头"和"-0"开头
+ str[1..N-1] 均为数字

转换

+ 提取符号位预留

+ 计算过程中，要确保

  ```c++
  cur = '0' - str[i] // 若用int表示结果，则由于INT_MIN绝对值大于INT_MAX，只有负数才能存储最大的绝对值，因此计算过程用负数表示
  res = res * 10 + cur // 确保不会溢出, 要求负数res >= INT_MIN / 10 && cur >= INT_MIN % 10
  ```

+ 符号转换， 若`res == INT_MIN`且符号为负，则失败。 

#### 双指针

+ 获取最长无重复字符子串

  ```c++
  // str[i...j]字符串无重复字符
  // 若index = map[str[j + 1]]（字符最近的索引值）有效说明有重复str[j + 1], 则 i = index + 1， 同时需删除map中str[index + 1]左侧的字符
  // maxlen = max(maxlen, j - i)
  // 优化点为，无需删除map中str[index + 1]左侧的字符, 由于i一直向右移动，所以i = max(i, map[str[j]]) 就可以实现i的跳跃
  ```




## 旋转字符串

#### 定义

+  字符串`str[0...j]`, 两个子串`str[0...i]` 和 `str[i+1, j]` 互换位置得到str的旋转字符串
+ 如指定size=2时， “123456” 与 “345612”互为旋转字符串

#### 特点

+ 任意 `str= strA + strB`,  则任意`strB + strA` 均为str的旋转字符串
+ `str + str = strA + strB + strA + strB`, 因此str+str中与str等长的子串均为str的旋转字符串，所以判断另一字符串是否为str+str的子串即可。
+ `str1.size() == str2.size() && str.find(str1) != str.end()`

#### 实现1

+ 字符串反转方法reverse, 交换次数str.size()/2

+ 任意 `str= strA + strB`，指定了endA,  则如下求旋转字符串str'， 每个字符都都经过两次swap， 所以总swap次数为N = str.size() (少数奇数子串的中间字符交换1次或0次)

  ```c++
  // 先反转各子串，再反转整个字符串，记得到逆序排列的子串组合
  reverse(str, 0, endA);
  reverse(str, endA + 1, str.size() - 1);
  reverse(str, 0, str.size() - 1);
  ```

#### 实现2

+  `str= strA + strB`,指定endA, 如"1234567ABCD" 指定endA = 7

+ 交换左右子串长度较小的部分, 较短部分交换后就不再参与后续交换

+ 当endA=str.size()/2 时，该方法只有endA次的交换量，最多str.size()次，最少str.size()/2次

+ ```
  1234567ABCD -> (ABCD)5671234  -> (ABCD)2341(567) -> (ABCD)(1)342(567) -> (ABCD)(1)(2)43(567) -> (ABCD)(1)(2)(3)(4)(567)
  ```



## 回文

+ 判断完整字符串是否回文，可通过栈或者首尾双指针来实现。

+ 字符串str，`str[i...j]` 如果是回文字符串，那么其子串`str[i + 1... j - 1]` (要求 `j- 1 >= i + 1`)也一定是子串。由此可推到得到如下`O(n^2)`的**动态规划**方法，判断任意`str[i...j]` 是否是文字符串

````c++
// 若p[i][j]表示str[i...j] 是否是回文字符串, 则有
p[i][j] = str[i] == str[j] && (j - i < 2 || p[i + 1][j - 1])
    
// 根据以上公式， i需要递减，j需要递增，因此
for (int i = str.size() - 1; i >= 0; i--) {
    for (int j = i; j < str.size(); j++) {
        p[i][j] = str[i] == str[j] && (j - i < 2 || p[i + 1][j - 1]);
	}
}
````

+ 回文分割点

  如果dp[j] 表示`str[0...j]` 分割成各回文子串所需最少次数，则dp[j+1] 需判断`str[i...j+1]`是否是回文串，若是则`dp[j+1] = dp[i] + 1` ，这么推导发现dp[j]与dp[j+1] 没有关系。而同时利用上述回文判断结果需外围索引递减，因此优化使得dp[i]表示str[i...str.size()-1] 子串的分割为回文子串的最少次数。

  ```c++
  // 对于每个i, 遍历所有j, 当str[i...j] 是回文时，计算dp[j + 1] + 1， 即str[j+1, str.size()-1] 对应最小分割字数+1， 取所有对应最小值
  dp[i] = min{ p[i][j] && (dp[j + 1] + 1)} // j = [i, str.size())
  ```


#### 判断字符串是否由词典中单词组成(139. Word Break)

dp[j]表示str[0...j - 1]是否满足条件，则当str[i + 1...j]是词典中单词时，则有dp[j] = dp[i]

```c++
// 如果dp[i] 表示str[0...i], 那么dp[0]就表示str[0], dp[0] = dp[-1] && {Find(str[0])}, dp[-1]数组上不好整，所以整体移动一位dp[i] 表示str[0...i - 1]， 那么dp[1]才表示str[0], dp[0]赋值true
vector<bool> dp(s.length() + 1);
dp[0] = true;

for (int i = 1; i <= s.length(); i++) {
    for (int j = 0; j < i; j++) {
        if (dp[j] && word_set.find(s.substr(j, i - j)) != word_set.end()) {
            dp[i] = true;
            break;
        }
    }
}
return dp[s.length()];
```



## Trie字典树（发音为try）

#### 特性

+ 树中每个节点的各个路径上保存了不同值，利用字符串的公共前缀可以节省存储空间

```c++
struct TrieNode {
    int path; // 路过当前节点path数
    int end; // 以当前节点为结尾的数量
    map<char, TrieNode*> map; // 路径值为map的key值
    TrieNode() {
        path = 0;
        end = 0;
    }
}

class Trie {
public:
    void insert(string word); // 不存在的节点要新增，各节点path++， 尾节点end++
    void delete(string word); // path耗尽的节点即可删除并终止， 各个节点path--， 尾节点--
    bool search(string word); // 各节点均存在，且最后一个节点end!=0
    int prefixNum(string word); // 找word为前缀的单词个数，prefix各节点均存在后，返回最后一个节点的path
private:
    TrieNode root;
}
```

## KMP算法
