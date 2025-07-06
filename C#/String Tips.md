# String Tips

[基本操作汇总](https://learn.microsoft.com/zh-cn/dotnet/standard/base-types/basic-string-operations)
[String类](https://learn.microsoft.com/zh-cn/dotnet/api/system.string?view=net-9.0)

## 常用方法的重载

### `Slipt()`

1. 传入整数表示最多分割成多少份
2. 传入`StringSplitOptions`来删除空字符串/删除空白字符

### `IndexOf()`

1. 传入`startIndex`和`count`指定从哪里开始搜索以及最多搜索几个字符
2. 传入`StringComparison`指定忽略大小写等

## 换行符相关

1. 使用`Environment.NewLine`以支持跨平台
2. 使用`ReplaceLineEndings()`来统一换行符

## 路径分隔符

使用`Path.Combine()`

## 字符串的构造函数

例如：翻转字符串

```cs
var message = "Hello World!";
var arr = message.ToCharArray();
// arr = arr.Reverse().ToArray();
Array.Reverse(arr);
var res = new string(arr);
```

## 字符串包含

使用`Contains()`而不是`IndexOf()>=0`

## 字符串比较

1. 使用`Equals()`而不是`CompareTo()==0`
2. 使用`StringComparison`以支持忽略大小写等

## 插值与格式化

使用字符串插值而非`String.Format()`，`StringBuilder.AppendFormat()`等

## StringBuilder类

[StringBuilder类](https://learn.microsoft.com/zh-cn/dotnet/api/system.text.stringbuilder?view=net-9.0)

1. 支持`Remove()`，`Insert()`，`Clear()`，`Replace()`等方法
2. `ToString()`有重载，可得到子字符串
