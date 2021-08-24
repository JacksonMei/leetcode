# Linked List

## categories 

+ singly-linked list

+ doubly-linked list

+ circular-linked list

## algorithm pattern 

remove one or some node except header （双指针）

```c++
Node* pre = head;
Node* cur = head->next;

while (cur) {
    if (cur->val == val) { // 连续满足某种删除cur条件
        pre->next = cur->next;
    } else {
        pre = cur;
    }
    
    cur = cur->next;
}
```

reverse (pre  cur -> next)   [left (start ... end) right]

或用栈 先入后出

```c++
Node* pre = NULL;
Node* cur = start; // 反转整个链表时start=head,right=NULL
while (cur != right) {
    Node* tmp = next;
    cur->next = pre;
    // cur->last = next 双向链表
    pre = cur;
    cur = tmp;
}

if (left) {
    left->next = pre;
}
start->next = right;
```





# Bitmask

```c++
Test kth bit: s & (1 << k);
Set kth bit: s |= (1 << k);
Turn off kth bit: s &= ~(1 << k);
Toggle kth bit: s ^= (1 << k);
Multiple by 2n: s << n;
Divide by 2n: s >> n;
Intersection: s & t;
Union: s | t;
Set Subtraction: s & ~t;
Extract lowest set bit: s & (-s); 保留最低为1的bit位
Extract lowest unset bit: ~s & (s + 1); 保留最低为0的bit位
    s & (s - 1) 清除最低为1的bit位
Swap Values: x ^= y; y ^= x; x ^= y; // 即由于 (x ^ y) ^ y = x ^ (y ^ y) = x ^ 0 = x 自己与自己异或为0，与0异或不变
```

+ 偶数个数的某个数字异或会抵消。两数异或后为1的bit位，必然来自其中一个数
+ **K进制的数的每一位，累加K次后就为0（无进位）**

#### K进制转换

```c++
vector<int> getKNumFrom10(int value, int k)
{
    vector<int> numK;	
    while (value) {
        numK.push_back(value % k);
        value /= k;
    }
    return numK;
}

 get10NumFromK(vector<int> numK, int k)
{	
    int value = 0;
    for (int i = numK.size() - 1; i >= 0; i--) {
        value = value * K + num;   
    }
    return ret;
}
```

#### 大数据处理

+ 布隆过滤器： 大数据量或频繁查询场景下，判断一个数是否不存在
  + 每种数占用1个bit（比正常少8倍）
  + 通过多个独立hash函数，每个数的每个hash函数输出一个目标bit位置，并将其置1
  + 有查询请求时，每个hash函数的目标bit位须全部为1，才表示该数可能存在（存在重叠），否则一定不存在。

+ 一致性哈希

+ 数据估算
  + 2^32 =4G = 40亿， 4G整数（4Bytes）需16GB，对应一条hash记录k-v 暂用8bytes
  + 对于大量数据小内存场景下，如**需统计数据次数（如最多次数，TOPK， 中位数，出现次数为2）则将数据按hash值分布划分为多个子区域**（如[0-2^30), [2^30, 2^31）..). 分别用小内存统计各个子区域并提取子问题的目标值， 综合各个子问题的解获取最终解。（此类问题要求可通过各个子区域问题得到最终解）
  + 统计次数问题时，竟可能使用bit位表征数据次数，如1个bit位表示次数是否大于0，两个bit位表示次数是否超过2等
