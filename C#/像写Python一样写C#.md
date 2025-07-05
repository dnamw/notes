# 像写 Python 一样写 C Sharp

## 初始化数组与字符串

### 一维数组

```python
arr1 = [3] * 10
print(arr1)
arr2 = [x * 2 for x in range(10)]
print(arr2)
```

```cs
Enumerable.Repeat(3,10).Dump();
Enumerable.Range(0,10).Select(x => x * 2).ToArray().Dump();
```

### 二维数组

```python
mat = [[3] * 5 for _ in range(5)]
print(mat)
```

```cs
var mat = Enumerable.Range(0,5).Select(_ => Enumerable.Repeat(3,5).ToArray()).ToArray();
mat.Dump();
```

### 字符串

```python
print('-' * 20)
print('-=' * 20)
```

```cs
var s = new string('-',20);
s.Dump();
s = string.Join("",Enumerable.Repeat("-=",20));
s.Dump();
```

## 切片

```python
arr = list(range(20))
print(arr[2:6])
```

```cs
var arr = Enumerable.Range(0,20).ToArray();
arr[2..6].Dump();
```

## Enumerate & Zip

### Enumerate

```python
s = 'hello'
for i,c in enumerate(s,start = 1):
    print(i,c)
```

```cs
var s = "hello";
foreach(var (i,c) in Enumerate(s))
{
    $"{i} {c}".Dump();
}

IEnumerable<(int,T)> Enumerate<T>(IEnumerable<T> items, int start = 0)
{
    foreach(var item in items)
    {
        yield return (start++, item);
    }
}
```

### Zip

```python
s = 'hello'
a = list(range(5))
for i,c in zip(a, s):
    print(i,c)
```

```cs
var s = "hello";
var a = new int[] {0,1,2,3,4};
foreach(var pair in a.Zip(s))
{
    pair.Dump();
}
```

## Reduce等

```python
from functools import reduce
arr = [1,5,2,6,3,7,8,4,9]
print(reduce(lambda x,y:x+y,arr))
print(list(map(str,arr)))
print(list(filter(lambda x:x%2==0,arr)))
```

```cs
var arr = new int[]{1,5,2,6,3,7,8,4,9};
arr.Aggregate((x,y)=>x+y).Dump();
arr.Select(x=>x.ToString()).Dump();
arr.Where(x=>x%2==0).Dump();
```

## eval

```python
print(eval('(1+2)*3)')
```

```cs
var table = new DataTable();
Convert.ToInt32(table.Compute("(1+2)*3)","")).Dump();
```
