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
// max int
math.MaxInt32

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
```
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
``` 
// Sort a slice of strings in ascending order
strs := []string{"banana", "apple", "orange", "grape"}
sort.Strings(strs)
fmt.Println(strs) // Output: [apple banana grape orange]

```

#### sort.Slice (对各类slice排序，需提供比较函数)
```
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


# Typical Code Snip

### copy


### binary Search
```
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

