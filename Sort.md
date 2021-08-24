
## 冒泡

优化点1： 无交换操作时，提前结束

优化点2： 最后一次交换点后无交换，可作为下一次遍历的终点

+ 稳定排序

```c++
void sort(vector<int>& nums) {
    int len = nums.size();
    int ordered = len - 1;
    int lastIdx = 0;

    for (int i = 0; i < len - 1; i++) { // 只剩下一个元素时无需冒泡
        bool sorted = true; // 初始假设排序            
        for (int j = 0; j < ordered; j++) { // [0, lastIdx) 为新的冒泡区间, lastIdx为最后一次交换点
            if (nums[j] > nums[j + 1]) {
                swap(nums[j], nums[j + 1]);
                lastIdx = j;
                sorted = false; // 有交换则表示未完成排序
            }
        }
        ordered = lastIdx;
        if (sorted) {
            break;
        }
    }

    return;
}
```

## 选择

+ 不稳定排序

```c++
void sort(vector<int>& nums) {
    int len = nums.size();
    for (int i = 0; i < len; i++) {               
        for (int j = i + 1; j < len; j++) { // 选择区间(i, len)中最小值，与nums[i]交换
            if (nums[i] > nums[j]) {
                swap(nums[i], nums[j]);
            }
        }
    }

    return;
}
```

## 插入

+ 稳定排序

优化点1 ： 当出现无需交换时，即可终止冒泡

优化点2： 将冒泡交换操作，换成较大元素由移

优化点3： 插入位置通过二分查找减少比较次数。移动次数无法优化

```c++
void sortColors(vector<int>& nums) {
    int len = nums.size();

    for (int i = 1; i < len; i++) {
        for (int j = i; j > 0; j--) {
            if (nums[j - 1] > nums[j]) { // 区间[0, i) 为有序区间， nums[i] 从右往左依次冒泡至合适位置（实际可二分找右边界）
                swap(nums[j - 1], nums[j]);
            } else {
                break;
            }
        }
    }

    return;
}
```



## Heapsort

+ parentPos = (child - 1) / 2 -》 lastparent = length / 2 - 1
+ 从heap数组尾部开始堆化，使得各个子树能依次冒泡到顶部
+ 最差最好都是O(NlogN)的时间复杂度，O(1)的空间复杂度，若递归实现则需要O(logN)的空间复杂度
+ **不稳定**排序（相同元素的原始相对顺序在排序后会变化）
+ 对于降序排列的数据使用堆排序升序排序时，由于数组已经是大顶堆，所以比较次数变少且没有交换
+ 一般比快排和归并排序慢， 优势仅在已预排序的数组中。 



```c++
void heapSort(vector<int> heap, int parentPos)
{
    // build a heap
    for (int i = heap.size() / 2 - 1; i >= 0; i--) {
        heapify(heap, heap.size(), i); // 从尾部父节点开始，每个子树都堆化为最大堆，直到遍历到根节点
    }
    
    // sort  不停将尾部元素与堆顶交换， 直到剩下最后一个
 	for (int i = heap.size() - 1; i > 0; i--) {
        std::swap(heap[0], heap[i]);
        heapify(heap, i, 0); // heap范围[0, i) 区域[i, size())为已升序区
    }
}

// 当前堆只有根节点不稳定将其堆化
// 校验当前根节点是否大于子节点, 若不大于则交换，再校验子节点， 最终得到以parentPos为根节点的最大堆， O(logn)
void heapify(vector<int> &heap, int end, int parentPos) {
  while (true) {
    int leftChildPos = parentPos * 2 + 1;
    int rightChildPos = parentPos * 2 + 2;
 	int largestPos = parentPos;
      
    if (leftChildPos < end && heap[leftChildPos] > heap[largestPos]) { // 这里要去最大值，因此与largestPos比较
      largestPos = leftChildPos;
    }
    if (rightChildPos < end && heap[rightChildPos] > heap[largestPos]) {
      largestPos = rightChildPos;
    }

    // If it's the parent, we're done
    if (largestPos == parentPos) {
      break;
    }

    std::swap(heap[largestPos], heap[parentPos]);
    parentPos = largestPos; // 被交换的子节点位置开始检查堆条件
  }
	
```

> [Heapsort]: Heapsort–Algorithm,SourceCode,TimeComplexity
>
> 

## 快排

优化点1： 选择首个元素作为基准点，在顺序或逆序时，基准点为最大值或最小值，每次分治无法做到二分以缩小规模，为最坏情况n^2复杂度

```c++
void sortColors(vector<int>& nums) {
    quickSort(nums, 0, nums.size() - 1);
    return;
}

void quickSort(vector<int>& nums, int start, int end) {
    if (start >= end) { // 有效区间[start, end]
        return;
    }

    // 分区
    int mid = partion(nums, start, end);
    // 左右分区各自排序
    quickSort(nums, start, mid - 1);
    quickSort(nums, mid + 1, end);
    return;
}

int partion(vector<int>& nums, int start, int end)
{
    int left = start;
    int midNum = nums[start];

    // 首个不动， left指向小区的最后一个元素
    for (int i = start + 1; i <= end; i++) {
        if (nums[i] < midNum) {
            left++;
            swap(nums[i], nums[left]);
        }
    }

    swap(nums[start], nums[left]);
    return left; // 交换后left指向小区下一个元素，即中间点
}

// 非递归版本---》 递归实际是深搜，通过栈转换，
void sortColors(vector<int>& nums) {
    stack<Pos> st;
    st.emplace(0, nums.size() - 1);

    while (!st.empty()) {
        auto region = st.top();
        st.pop();

        int mid = partion(nums, region.first, region.second);
        if (mid - 1 > region.first) {
            st.emplace(region.first, mid - 1);
        }

        if (mid + 1 < region.second) {
            st.emplace(mid + 1, region.second);
        }
    }
}
```



## 桶排序





#### 算法可视化

https://visualgo.net/zh/sorting
