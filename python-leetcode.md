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
```
dict = {w:v for w in word}
dict = defaultdict(int)
```

## str
```
"".join(list) => "".join(w for w in word)
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

