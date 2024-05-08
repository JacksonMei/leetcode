# Data Structure
## Array

``` Golang
// init an array with size = 10 and default value = 0
nums := make([10]int)

// transverse the array
for i := 0; i < len(nums); i++ {
}

// 迭代数组元素
for _, num := range arr {
    fmt.Println(num)
}

// init a 2d array
visited := make([5][10]int)
```


## Slice
``` Golang

```

## String
``` Golang
s1 := "hello world"
s2 := 'This is a
world
'

// string lower letter.
s=strings.ToLower(arr)

// string -> int
strconv.Atoi(res) // like ”2“ -> 2
// int -> string
strconv.Itoa(res)


// 字符串连接
str1 := "Hello"
str2 := "World"
result := str1 + " " + str2
fmt.Println(result) // 输出：Hello World

// 字符串查找
str := "Hello, World"
index := strings.Index(str, "World") // 找到首次出现World索引位置，找不到则返回-1
fmt.Println(index) // 输出：7

indexes := strings.IndexAll("Hello, World, World", "World") 
fmt.Println(indexes) // 输出：[7 14]

// 字符串替换
newStr := strings.Replace(str, "World", "Go", -1) // World是被替换str, Go 是换成的新str,  -1是替换次数（为负数则无数次）
fmt.Println(newStr) // 输出：Hello, Go

```


## Map
```Golang
// 创建一个哈希表
hashMap := make(map[int]string)

// 插入键值对
hashMap[1] = "value1"

// 获取值
value := hashMap[1] // 输出："value1"

// 检查键是否存在
if _, ok := hashMap[3]; ok {
    fmt.Println("Key exists")
} else {
    fmt.Println("Key does not exist")
}

```
## List

## Heap

最小堆实现 算法题https://leetcode.com/problems/kth-largest-element-in-an-array/
```
import (
	"container/heap"
	"fmt"
)
type IntHeap []int


func (h IntHeap) Len() int { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x any) { // define interface from "container/heap"
     *h = append(*h, x.(int))
}

func (h *IntHeap) Pop() any { // define interface from "container/heap"
    // old := *h
	// n := len(old)
	// x := old[n-1]
	// *h = old[0 : n-1]
	// return x
    result := (*h)[len(*h)-1] // 使用 := 声明并赋值一个新变量 result
    *h = (*h)[:len(*h)-1]     // 对 *h 进行更新，删除最后一个元素
    return result
}

func findKthLargest(nums []int, k int) int {
    h := &IntHeap{}
    heap.Init(h)

    for _, num := range nums {
        heap.Push(h, num)
        if h.Len() > k { // min-heap 顶部是最小的值，每次都弹出了最小值，所以留下的是最大K个值。
            heap.Pop(h)
        }
    }

    return heap.Pop(h).(int)
}
```
## Tree
```Golang
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

root := &TreeNode{Val: 1}
root.Left = &TreeNode{Val: 2}  // Corrected: Use simple assignment instead of :=
root.Right = &TreeNode{Val: 3} // Corrected: Use simple assignment instead of :=

func preorderTraversal(root *TreeNode) []int {
    if root == nil {
        return nil
    }

    result := []int{root.Val}
    result = append(result, preorderTraversal(root.Left)...)
    result = append(result, preorderTraversal(root.Right)...)
    return result
}

```



# Common-used Function
## Standard Lib
### Sort
### sort.Ints
```Golang
nums := []int{4, 2, 7, 1, 5}
sort.Ints(nums)
fmt.Println(nums) // Output: [1 2 4 5 7]
```

#### sort.Strings
``` Golang
// Sort a slice of strings in ascending order
strs := []string{"banana", "apple", "orange", "grape"}
sort.Strings(strs)
fmt.Println(strs) // Output: [apple banana grape orange]

```

#### sort.Slice (对各类slice排序，需提供比较函数)
```Golang
// 对整数切片进行排序
nums := []int{4, 2, 7, 1, 5}
sort.Slice(nums, func(i, j int) bool {
    return nums[i] < nums[j]
})
// 对字符串切片进行排序
strs := []string{"banana", "apple", "orange", "grape"}
sort.Slice(strs, func(i, j int) bool {
    return strs[i] < strs[j]
})

```

### math
```Golang
// max int
math.MaxInt32
math.MinInt32
math.MaxInt64
math.MinInt64

//  输入输出及返回都是float64
math.Abs(-5) // 返回 5
math.Floor(3.14) // 返回 3
math.Ceil(3.14)  // 返回 4
math.Round(3.14) // 返回 3, 四舍五入
math.Pow(2, 3) // 返回 2 的 3 次方，即 8，
math.Sqrt(9) // 返回 3

math.Exp(1)  // 返回 e 的 1 次幂，即 e， 指数
math.Log(10) // 返回 10 的自然对数，对数

math.Max(2, 5) // 返回较大的数，即 5
math.Min(2, 5) // 返回较小的数，即 2

```

# Typical Code Snip

### copy


### binary Search
```Golang
func binarySearch(nums []int, target int) int {
    left, right := 0, len(nums)-1 // Fix: Initialize right pointer to len(nums)-1
    for left <= right {
        mid := left + (right-left)/2
        if nums[mid] == target {
            return mid
        } else if nums[mid] < target { // Fix: Adjust condition for updating left and right pointers
            left = mid + 1
        } else {
            right = mid - 1
        }
    }

    return -1
}

```

### Type convertion
```Golang
// int <-> float
var x int = 10
var y float64 = float64(x) // 将 int 转换为 float64
num := int(y)

// int -> str
num := 10
str := strconv.Itoa(num)

// str -> int
str := "10"
num, err := strconv.Atoi(str)
if err != nil {
    // 处理错误
}



// 指针转换，需确保类型兼容
var x int = 10
var ptr *int = &x
var ptr64 *int64 = (*int64)(ptr) // 将 *int 指针转换为 *int64 指针

// 接口转换
var x interface{} = 10
var y int = x.(int) // 将 interface{} 转换为 int


```
