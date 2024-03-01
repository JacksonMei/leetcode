
## Mutable vs Immutable Objects
### Immutable objects
+ like integers, strings, and tuples, cannot be altered. When you attempt to change an immutable object, what actually happens is the creation of a new object in memory.
+ if you try to modify an immutable variable (like an integer) defined in an outer scope within a nested function, without declaring it as nonlocal, Python will create a new local variable in the nested function's scope.
### Mutable objects
+ such as lists, dictionaries, and sets, can be modified in place.
+ This means you can alter the content of a mutable object without changing its identity (its location in memory).
```
def findFrequentTreeSum(self, root: Optional[TreeNode]) -> List[int]:
        count = defaultdict(int)
        max_count = [0]  # Use a list for mutable integer

        def dfs(root):
            if not root:
                return 0
            
            left_sum = dfs(root.left)
            right_sum = dfs(root.right)
            tree_sum = left_sum + right_sum + root.val
            count[tree_sum] += 1
            
            # Update max_count[0] since max_count is a list;   or use interger and use "nolocal max_count" here
            max_count[0] = max(max_count[0], count[tree_sum])
            
            return tree_sum

        dfs(root)
        # Use max_count[0] to access the updated maximum count
        return [k for k, v in count.items() if v == max_count[0]]
```
## list
listA = [0] * n   => 
listAB = [[0]] * n  =>  n ref of [0]
```
a = [[1]] * 10
a[0][0] = 33
print(a) # [[33], [33], [33], [33], [33], [33], [33], [33], [33], [33]]
```

listAB = [[0] for _ in range(n)]
```
a = [[1] for _ in range(10)]
a[0][0] = 33
print(a) # [[33], [1], [1], [1], [1], [1], [1], [1], [1], [1]]
```

# find element in the list
i = listA.index(b) if b in listA else -1 # find b in listA and return the first index

## queue

from collections import deque

queue = deque([(start, 0)])
current_node, distance = queue.popleft()  # Dequeue the next node from the left side
queue.append((neighbor, distance + 1))   # add node to the right side of the queue


# Example 

## set
```
set = {w for w in word}
set.add(v)
set.remove(v)

A.difference(B)  # returns items present only in set A

v = setA.pop() # randomly remove an item from setA

setA.clear()

setB = setA.copy()

setA.issubset(setB) # return True if set A is subset of set B

setA.union(setB) # # return a new set from distinct element from all the sets

setA.update(B, C) # add B and C (B, C can be dict, list or set) to set A
{1, 3}.update('odd', {2, 4}, [5, 6], {'key': 1, 'lock' : 2}) => {1, 2, 3, 4, 5, 6, 'key', 'lock', 'd', 'o'}

```


## dict
```
dict = {w:v for w in word}
dict = defaultdict(int)

#### Counter
c = collections.Counter(A)
sorted(c) # sort the counter
list(c.elements()) # Return an iterator over elements repeating each as many times as its count. 
```

## str
```
"".join(list) => "".join(w for w in word)

 count = str.count('1')   # Counter(str)['1']
 return '1' * count #'11111'
```

## char
```
ord('a') = 97
chr(97) = 'a'
```

## math

math.log(a, base) # = log(a) / log(base)

math.log(a) # base default as e

math.log2(a) # base = 2

math.sqrt(x) # x >= 0

math.floor(x) # return the largest integer not greater than x.           -23.11 ->  -24.0

math.ceil(x) # return the smallest integer greater than or equal to x.   300.72 -> 300.0

round(number, ndigits) # returns a floating-point number rounded to the specified number of decimals.

round(10.7) # ndigits defaults to 0, -> 11

round(2.675, 2) -> 2.67   # most decimal fractions can't be represented exactly as a float.  2.67 == 2.674999999

print(round(Decimal('2.675'), 2))  # -> 2.68 using decimal.Decimal (passed float as string for precision)

math.inf # -math.inf defines a negative infinite integer

## algorithm
itertools.permutations(list) # return a tuple list

itertools.combinations([1, 2, 3], 2) 

any(cand[0] != '0' and bin(int(''.join(cand))).count('1') == 1 for cand in permutations(str(n)))

map(func, iter1, iter2) # map(lambda x, y: x + y, (x1, x2), (y1, y2))

#### bisec (using bisection algorithm on a sorted list)
```
bisect.bisect_left(a, x, lo=0, hi=len(a), *, key=None)
The returned insertion point i partitions the array a into two halves so that all(val < x for val in a[lo : i]) for the left side and all(val >= x for val in a[i : hi]) for the right side.


bisect.bisect_right/bisect.bisect(a, x, lo=0, hi=len(a), *, key=None)
The returned insertion point i partitions the array a into two halves so that all(val <= x for val in a[lo : i]) for the left side and all(val > x for val in a[i : hi]) for the right side.


bisect.insort_left  # use bisect.bisect_left to get an insertion point (O(log n), then it runs list.insert (O(n)) on `a` and insert `x` at the insertion point to maintain sort order
bisect.insort_right/bisect.insort
```

# heap
```
heap = []
heapq.heappush(heap, item)
item = heapq.heappop(heap) # return the smallest item
heapq.heapify(list1) # Transform list x into a heap, in-place, in linear time.
item = heapq.heapmerge(list1, list2, list3, , key=None, reverse=False) # Merge multiple sorted inputs into a single sorted output
```
