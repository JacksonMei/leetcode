# 随机函数

rand() 函数，头文件<cstdlib>， 返回`[0, RAND_MAX]`范围内的随机数
```
int rand();
RAND_MAX 大小视依赖库决定，保证至少32767（2^15 - 1）
  
v2 = rand() % 100 + 1;     // v2 in the range 1 to 100
  int index = rand() % (size - i) + i; [i, size)
```
[leetcode384] https://leetcode.com/problems/shuffle-an-array
