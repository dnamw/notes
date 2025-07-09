# Switch用法

## case

`case`后可跟任何编译时常量

1. `232`,`10/5`,`2+3`等数字
2. `enum`类型，且被标记`[Flags]`的`enum`也是合法的
3. `nameof`,`"he"+"ll"+"o"`等编译时能确定的字符串
4. `const`变量

## goto case

```csharp
int × = 10, y = 5;
Console.WriteLine(Calc(x,y,"add"));
Console.WriteLine(Calc(x,y,"sub"));
int Calc(int a,int b,string op)
{
    switch (op)
    {
        case "add"
            return a b;
        case "sub"
            b = -b;
            goto case "add";
        default:
            return 0;
    }
}
```

## case语句的结束

可使用`break`,`return`,`goto`,`continue`,`yield break`五种方式

```csharp
IEnumerable<int> GetValues()
{
    foreach(var val in Enumerable.Range(1,10))
    {
        switch(val)
        {
            case 5:
                continue;
            case 9:
                yield break;
            default:
                yield return val;
                break;
        }
    }
}
```

## 使用when语句

```csharp
string item = "apple";
int amount = 5;
switch (item)
{
    case "apple" when amount >= 10:
        Console.WriteLine("Lots of apples");
        break;
    case "apple" when amount < 10:
        Console.WriteLine("several apples");
        break;
    case "banana":
        Console.WriteLine("some bananas");
        break;
}
```

## 模式匹配

```csharp
object x = 10;
switch (x)
{
    case int n when n is 0 or 1 or >= 10:
        Console.WriteLine($"Number: {n}");
        break;
    case bool flag:
        Console.WriteLine($"FLag: {flag}")
        break;
    case string s:
        Console.WriteLine(s);
        break;
}
```

## 新语法

```csharp
Weekday GetWeekday(int n) => n switch
{
    0 => Weekday.Sunday,
    1 => Weekday.Monday,
    2 => Weekday.Tuesday,
    3 => Weekday.Wednesday,
    4 => Weekday.Thursday,
    5 => Weekday.Friday,
    6 => Weekday.Saturday,
    _ => throw new ArgumentException("Invalid n", nameof(n))
};

enum Weekday
{
    Sunday,
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday
}
```
