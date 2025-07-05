# LINQ

## 基本概念

语言集成查询 Language-Interacted Query

## 常见用途

* .NET 原生集合
* SQL 数据库
* XML 文档
* JSON 文档

## 常见功能

* 排序、筛选、选择
* 分组、聚合、合并
* 最大值、最小值、求和、求平均、求数量

## 两种形式

* 查询表达式
* 链式表达式

### 查询表达式

```cs
var lst = new List<int>{1,3,5,7,9,2,4,6,8,0};
var res = 
    from n in lst
    where n & 2 == 0 && n >= 4
    orderby n
    select n;
```

### 链式表达式

```cs
var lst = new List<int>{1,3,5,7,9,2,4,6,8,0};
var res = lst
    .Where(n => n % 2 == 0 && n >= 4)
    .OrderBy(n => n);
```

## 延迟执行（defer）与消耗（exhaust）

### 延迟执行

```cs
var lst = new List<int>{1,2,3,4,5};
var query = lst.Select(n => n * n);
query.Dump(); //1 4 9 16 25
lst.Add(6);
query.Dump(); //1 4 9 16 25 36 
```

### 消耗

* 遍历 `foreach`
* 转换 `ToList()`,`ToArray()`,`ToDictionary()`
* 计算 `Count()`,`Min()`,`Max()`,`Sum()`
* 查询 `Take()`,`First()`,`Last()`

## 并行查询

```cs
var arr = Enumerable
    .Range(1,10)
    .ToArray()
    .AsParallel()
    .AsOrdered()
    .Select(n => 
    {
        Thread.Sleep(500);
        return n * n;
    })
    .AsSequential()
    .Dump();
```

## 实战

### 公共元素

```cs
var arr1 = new int[]{1,2,3,4,5,6};
var arr2 = new int[]{4,5,6,7,8,9};
var res = arr1.Intersect(arr2);
```

### 分组+匿名类统计数字出现频率

```cs
var rnd = new Random(1334);
var arr = Enumerable.Range(0, 200).Select(_ = rnd.Next(20))
var res = 
    from n in arr
    group n by n into g
    select new {g.Key,Count = g.Count()};
```

```cs
var rnd = new Random(1334);
var arr = Enumerable.Range(0, 200).Select(_ = rnd.Next(20))
var res = arr
    .GroupBy(n => n)
    .Select(g => new {g.Key,Count = g.Count()});
```

### 展平

```cs
var mat = new int[][]{
new[]{1,2,3,4},
new[]{5,6,7,8},
new[]{9,10,11,12}};
var res = 
    from row in mat
    from n in row
    select n;
res.Dump();
```

```cs
var mat = new int[][]{
new[]{1,2,3,4},
new[]{5,6,7,8},
new[]{9,10,11,12}};
var res = mat.SelectMany(n => n);
res.Dump();
```

### 笛卡尔积

```cs
var products = 
    from i in Enumerable.Range(0,5)
    from j in Enumerable.Range(0,4)
    from k in Enumerable.Range(0,3)
    select $"{i},{j},{k}";
products.Dump();
```

### 统计字母频率

```cs
var words = new string[] {"tom","jerry","spike","tyke","butch"};
var query = 
    from w in words
    from c in w
    group c by c into g
    select new {g.Key,Count = g.Count()} into a
    orderby a.Count descending
    select a;
query.Dump();
```

```cs
var words = new string[] {"tom","jerry","spike","tyke","butch"};
var query = words
    .SelectMany(c => c)
    .GroupBy(c => c)
    .Select(g => new {g.Key,Count = g.Count()})
    .OrderByDescending(g => g.Count);
query.Dump();
```

### 批量下载文件

```cs
var urls = new string[]{
"http://www.example.com/pic1.jpg",
"http://www.example.com/pic2.jpg",
"http://www.example.com/pic3.jpg"
};

var tasks = urls.Select(url => DownLoadAsync(url, url.Split('/').Last()));
await Task.WhenAll(tasks);

async Task DownloadAsync(string url, string fileName)
{
    await Task.Delay(1000);
    $"{fileName} downloaded.".Dump();
}
```

## 常见错误

1. 不知道有`First()`,`Last()`等方法
2. 不知道有`Average()`方法
3. 不知道`Count()`,`First()`,`Min()`,`Sum()`可以传参
4. 不知道`Max()`与`MaxBy()`的区别
5. 不知道各种`Default`
6. 滥用`ToList()`
7. 滥用`Count()`
8. 滥用`OrderBy()`而不用`Sort()`
9. 不知道`First()`与`Single()`的区别
