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
{1, 3}.update('odd', {2, 4}, [5, 6], {'key': 1, 'lock' : 2}) => {1, 2, 3, 4, 5, 6, 'key', 'o', 'd', 'd'}

```


## dict
dict = {w:v for w in word}
dict = defaultdict(int)

## str
"".join(list) => "".join(w for w in word)

## char
###char <=> int 
ord('a') = 97
chr(97) = 'a'
