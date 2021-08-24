
#### 可整合数组

数组在排序后相邻元素的差值等于1----》 等效于数组` max-min + 1 == arr.size()`， 如果包含重复元素，则大于等于`arr.size()`



#### 和为指定值K的最长子数组问题

+ 数组arr[0...n]元素均为正数时，使用双指针,arr[i...j]累加和和K的关系如下

  ```c++
  if (sum < k) {
      j++;
      sum += arr[j];
  } else if (sum > k) {
      i++;
      sum -= arr[i];
  } else {
      maxlen = max(maxlen, j - i);
      sum -= arr[i++];
  }
  ```

+ 数组元素有正有负有0时，sum[i + 1...j] = sum[j] - sum[i]， 子数组长度为j - i, 因此将sum数组存在map中，对于每个sum[i], 查找是否存在k - sum[i]

  ```c++
  unordered_map<int, int> smap; smap[0] = -1; // 设置一个元素都没有时sum=0, 位置为-1，保证arr[0...j] 的和等于k时，k-sum==0, 得到长度=j - ()
  int maxlen = 0;
  for (int i = 0, sum = 0; i < arr.size(); i++) {
      sum += arr[i];
      if (smap.count(sum - k)) {
          maxlen = max(maxlen, i - smap[k - arr[i]]);
      } 
      if (!smap.count(sum)){ // 只保存首次出现的位置，记录某个值的最左边值，从而保证最长
          smap[sum] = i;
      }
  }
  return maxlen;
  ```

+ 数组元素有正有负有0， 且要求取累加和小于等于K的最长子数组

  sum[i + 1...j] = sum[j] - sum[i] <= k, 即问题转换为，求取大于等于sum[j] - k 值在sum数组中出现的最左位置。

  ```c++
  // 数组sum， k， 可利用元素左边最大值数组及二分算法来求取
  harr[0] = 0; //  sum[j] - sum[i], 这里类似加了sum[-1], 使得sum[0...j]有效
  for (int i = 0, sum = 0; i < arr.size(); i++) {
      sum += arr[i];
      harr[i + 1] = max(harr[i], sum);
  }
  for (int i = 0, sum = 0; i < arr.size(); i++) {
      sum += arr[i];
      int pre = findLessIndex(harr, sum - k);
      int len = (pre != -1) ? i - (pre - 1) : 0;
   	maxlen = max(maxlen, len);
  }
  // 二分查找，找大于等于k的最左数
  void findLessIndex(vector<int> &harr, int k)
  {
      int left = 0;
      int right = harr.size() - 1;
      int mid = 0;
    while (left <= right) {
          mid = left + (right - left) / 2;
        if (harr[mid] >= k) {
              res = mid;
              right = mid - 1;
          } else {
              left = mid + 1;
          }
      }
      
      return res;
  }
  
  ```
  
  
  
  

#### BFPRT算法

#### 矩阵转圈打印/矩阵旋转

子矩阵边界由左上和右下两个顶点决定, 按圈层打印

```c++
 // 顶点(topRow, topCloumn)与 (downRow, downCloumn)
while (topRow <= downRow && topCloumn <= downCloumn) {
    printEdge(arr, topRow++, topCloumn++, downRow--, downCloumn--) // 每次收缩打印一圈，直至左上顶点与右下顶点重合
}
// printEdge 中要考虑只有一行和只有一列的特殊情况
```

#### 需排序的最短子数组长度

arr[0...n], 其中若arr[i...j]部分无序，需求取j - i + 1的最大值，那么实际上将满足

```c++
arr[i - 1] < min{arr[i...j]}  && arr[j + 1] > max{arr[i...j]} 
// 同时arr[0...i - 1]和arr[j + 1, n]升序,因此a[i] 是从右到左遍历最后一个大于min的，同理a[j]是从左到右遍历最后一个小于max的。
```



#### 找出长度为N的无序数组中出现次数大于K/N的数

+ 长度 N的无序数组，找次数超过K次的数， 关键思路是： **循环删除K个不同的数直至数组剩下数种类不足K个**。
+ 当K/N = 1/2 时，可以理解到，每次删除两个不同的数，那么出现次数超过一半的数必然最后会剩下， K/N同理。

###### 实现

每次删除K个不同的数，实现思路是，维护map数组cand[0...k-1], 记录k个不同数出现的次数。 遍历每个数，凑齐K个不同时，则全部次数减1，最终的map中一定包含次数超过k的数。

```c++
unordered_map<int, int> cand; // 时间O(N), 空间O(k)

auto allMinusOne = [](map<int, int>& cand) {
    for (auto it = cand.begin(); it != cand.end();) {
        it->second--;
        if (!it->second) {
            it = cand.erase(it);
        } else {
            it++;
        }
    }
}

// 维持map中最多k个候选，到达K个时，全部删一遍， 遍历到最后剩下的k个数中必然有目标值
for (int i = 0; i != arr.size(); i++) {
    cand[arr[i]]++;
    if (cand.size() == k) {
        allMinusOne(cand);
    }
}

// 从cand数组中 找出次数超过K次的值（可能存在多个）
map<int, int> cand2;
vector<int> real;
for (int i = 0; i != arr.size(); i++) {
    if (!cand.count(arr[i])) {       
        continue;
    }
    cand2[arr[i]]++;
 	if (cand2[arr[i]] > k) {       
        real.emplace_back(arr[i]);
    }
}

//-----------------------------
// 找出次数超过一半的数
for (int i = 0， times = 0; i != arr.size(); i++) {
    if (times == 0) {
        cand = arr[i];
        times = 1;
    } else if (cand  == arr[i]) {
        times++;
    } else {
        times--;
    }
}
// 校验cand是否超过一半
```

