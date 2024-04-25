# Data Structure
## Vector
```cpp
std::vector<int> vec;
vec.push_back(1);
vec.pop_back();
vec.size();
vec.empty();
```
    
## Queue
```cpp
std::queue<int> q;
q.push(1);
q.push(2);
q.pop();
std::cout << "Front of queue: " << q.front() << std::endl;
std::cout << "Back of queue: " << q.back() << std::endl;
```

## Stack
```cpp
std::stack<int> s;
s.push(1);
s.push(2);
s.pop();
```

## Priority_Queue
```cpp
std::priority_queue<int> pq; // 最大堆
pq.push(3);
pq.push(1);
pq.pop();

priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> que; // 最小堆，堆顶是最小值

auto compare = [](const pair<int, int> &lhs, const pair<int, int> &rhs) {
   return (lhs.first > rhs.first) || (lhs.first == rhs.first && lhs.second > rhs.second);
};
priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(compare)> que(compare);

que.emplace(0, k);
auto [val, cur] = que.top();
        
```

## map/Unordered_map/ set/ unordered_set
```cpp
std::map<std::string, int> m; // 或者unordered_map
m["apple"] = 5;
m.erase("apple");

m.insert(std::make_pair("orange", 7));
std::map<std::string, int>::iterator it = myMap.find("banana");
if (it != myMap.end()) {
// 找到元素
}


std::set<int> us; // 或者unordered_set
us.insert(1);
us.insert(2);
us.erase(1);
auto it = us.find(2);
if (it != us.end()) {
// 找到元素
}
```

## List
```cpp
std::list<int> lst;
lst.push_back(1);
lst.push_front(2);
lst.pop_back();
lst.pop_front();
lst.insert(lst.begin(), 3); // 在list begin前插入元素3
lst.erase(lst.begin());
std::cout << "Size of list: " << lst.size() << std::endl;
lst.empty();

```


# Common-used Function

## sort

```cpp

reverse(v.begin(), v.end()) // 倒序
排序算法：std::sort()
二分查找算法：std::binary_search()
字符串处理：std::string及其相关方法，如find()、substr()等
sort(a.begin(),a.end(),less<int>()); // 降序排列

sort(vals.begin(), vals.end(),[](int a,int b){return abs(a)<abs(b);});

static bool cmp(vector<int>& a, vector<int>& b){//static关键字不能丢
      if(a[0] == b[0])
          return a[1] < b[1];//身高相同k小的排前面
      return a[0] > b[0];//身高高的排前面
  }
  vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
      sort(people.begin(), people.end(), cmp);
      ...
  }

```

## math related Function
```cpp
std::max(1, 2);
std::max({1, 2, 3, 4});
std::pair<int, int> minmax = std::minmax({1, 2, 3, 4});
std::vector<int>::iterator result = std::max_element(v.begin(), v.end());

std::swap(a, b);
double pow(double base, double exponent) // 指数
double sqrt(double x) // 开根号


std::abs() // 绝对值

```

## algorithm search
```cpp
    std::vector<int> vec = {1, 3, 5, 7, 9, 11, 13, 15, 17, 19};
    int target = 11;
    // 使用binary_search搜索
    bool found = std::binary_search(vec.begin(), vec.end(), target);
    if (found) {
        std::cout << "找到了目标元素 "  << std::endl;
    }
```

## String related function
```cpp
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
