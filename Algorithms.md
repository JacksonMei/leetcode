## Dijsktra 最短路径算法


```cpp
// https://leetcode.com/problems/network-delay-time/
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<bool> visited(n + 1, false);
        vector<int> dis(n + 1, INT_MAX);
        unordered_map<int, vector<pair<int, int>>> edges;

        for (auto time : times) {
            edges[time[0]].emplace_back(time[1], time[2]);
        }
        // 最小堆（默认less,最大堆）， 自动排序pair<int, int>
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> que;
        que.emplace(0, k);
        dis[k] = 0;

        while (!que.empty()) {
            auto [val, cur] = que.top(); // 优先队列，找出队列中到源点最近的节点！！！
            que.pop();
            if (!visited[cur]) {
                visited[cur] = true;

                for (auto &[next, val] : edges[cur]) {
                    if (dis[next] > dis[cur] + val) {
                        dis[next] = dis[cur] + val;
                        que.emplace(dis[next], next);
                    }
                }
            }
        }

        auto result = *max_element(dis.begin() + 1, dis.end());
        return result == INT_MAX ? -1 : result;
    }
};
```

## Divide and Conquer



23. Merge k Sorted Lists

    题目输入有k个有序链表的数组，要求合并所有链表。如果将从左到右， 累计合并链表，则每个节点需要与其他k个点比较，时间复杂度O(kN) (N为总节点数)

    **分治法**，先分为k/2组， 每组两个链表合并，再将合并链表分为k/2/2组，再合并，循环直至结束。每个节点只需与其他节点比较logk次，时间复杂度O(Nlogk)

```c++
// non-recursive implementation
{
    int interval = 2;
    while (interval < 2 * lists.size()) {
        for (int i = 0; i < lists.size(); i += interval) {
            auto next = i + interval / 2 >= lists.size() ? NULL : lists[i + interval / 2]; // 相邻项可能为空
            lists[i] = merge(lists[i], next); // 合并两个链表返回新链表头结点
        }
        interval *= 2;
    }
}

// recursive implementation
ListNode* mergeGroup(vector<ListNode*>& lists, int left, int right) {
    if (left == right) { // 剩余1个时的处理
        return lists[left];
    } else if (left == right - 1) { // 剩余2个时的处理
        return merge(lists[left], lists[right]);
    } else {
        // 分治为左右两个区后，合并两区链表
        ListNode* ll = mergeGroup(lists, left, (left + right) / 2);
        ListNode* rr = mergeGroup(lists, (left + right) / 2 + 1, right);
        return merge(ll, rr);
    }
}
```

## 二分搜索模板

```c++
// 查找target值
int binary_search(int[] nums, int target) {
    int left = 0, right = nums.length - 1; 
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1; 
        } else if(nums[mid] == target) {
            // 直接返回
            return mid;
        }
    }
    // 直接返回
    return -1;
}

// 查找target左边界，可能不存在
int left_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定左侧边界
            right = mid - 1;
        }
    }
    // 最后要检查 left 越界的情况
    if (left >= nums.length || nums[left] != target)
        return -1;
    return left;
}

// 查找target右边界，可能不存在
int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2; // 不要用 >> 1，防止右移变负数!!!!
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定右侧边界
            left = mid + 1;
        }
    }
    // 最后要检查 right 越界的情况
    if (right < 0 || nums[right] != target)
        return -1;
    return right;
}

作者：labuladong
链接：https://leetcode-cn.com/problems/binary-search/solution/er-fen-cha-zhao-xiang-jie-by-labuladong/

// 利用stl lower_bound和upper_boud实现
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int l = distance(nums.begin(), lower_bound(nums.begin(), nums.end(), target)); // 第一次>=target
        int r = distance(nums.begin(), upper_bound(nums.begin(), nums.end(), target)); // 第一次>target, [l,r)区间
        // cout << l << "-" << r << endl;
        l = (l >= nums.size() || nums[l] != target) ? -1 : l;
        r = (r == 0 || nums[r - 1] != target) ? -1 : r - 1; // (1)都比target大时，r==0 (2)都比target小时，r==size() (3) 存在时需r-1
        return {l, r};
    }
};
```



## A* 启发式搜索算法

+ 本质上是在广度优先搜索的基础上改进的
+ 在下一个节点的选择上，广度优先搜索是从子节点中顺序选择一个，而A*算法，则是提供一个value price机制，评估所有待选节点的F指标，选择F最大或最小的节点作为子节点。
+ 求最短路径的场景下，指标F用于评价与目标点的距离值（路径和，路径长等），一般选取为F = G + H， 其中G为起点实际遍历到当前节点的路径长，H一般设计为不考虑障碍的情况下当前节点到目的点的距离值。
+ H指标的设计是算法的灵魂，用于给漫无目的搜索过程指明一个最可能的方向。





## LRU

有限容量缓存机制，提供get/put方法要求O(1)，容量满时优先销毁非最近读写的记录。

```c++
struct Node {
    int key;
    int value;
}
unordered_map<int, list<int>::iterator> kmap; // key值到 list迭代器的映射
list<Node> vlist; // 双向链表保存key值对应的Node信息(包含key值)
// get方法中，获取记录后，要将对应记录从vlist中移至链表头，同时更新kmap中迭代器
// put方法中，在记录不存在且满容量时，应删除vlist中尾部节点，及节点中key值在kmap中的记录。再在vlist头添加新纪录
```



## LFU

有限容量缓存机制，提供get/put方法要求O(1)，容量满时优先销毁读写最低频率的记录，相同频率时先销毁最老记录

```c++
struct Node {
    int key;
    int value;
    int freq;
}
unordered_map<int, list<int>::iterator> kmap; // key值到 list迭代器的映射
unordered_map<int, list<Node> vlist; // 双向链表保存freq值对应的Node信息
// get方法中，获取记录后，要将对应记录从vlist[freq]中删除(vlist[freq]为空且freq==minfreq时，minfreq++)，添加到vlist[freq + 1]中，同时更新kmap中迭代器
// put方法中，在记录不存在且满容量时，应删除vlist[minfreq]中尾部节点，及节点中key值在kmap中的记录。再minfreq = 1，在vlist[minfreq]头添加新纪录
```

另外一种是利用set 对freq和time排序
