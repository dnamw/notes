# readonly & const

## 相同点

```csharp
var cls = new MyClass();
 
//初始化后都无法再（直接）修改值

MyClass.ConstValue = 20; //error
cls.ReadonlyValue = 20; //error

class MyClass
{
    public const int ConstValue = 10;
    public readonly int ReadonlyValue = 10;
}
```

## 不同点

### 初始化位置

`readonly`可以在构造函数中初始化

`const`必须在声明时初始化

```csharp
class MyClass
{
    public const int ConstValue = 10;
    public readonly int ReadonlyValue = 10;
    public MyClass()
    {
        ReadonlyValue = 20;
        ReadonlyValue = 30;
    }
}
```

### 使用方式

`readonly`只能用于声明类的字段

`const`还可以用于声明局部变量

```csharp
class MyClass
{
    public readonly int Field;
    public const int ConstValue = 10;
    void Foo()
    {
        const int a = 30; //OK
        readonly int b = 20; //error
    }
}
```

### 本质

`readonly`只是语法，表面变量一旦初始化便不能再修改，除非在构造函数中修改或通过反射方式修改

```csharp
var obj = new MyClass();
var fieldInfo = obj.GetType().GetField("Value");
fieldInfo.SetValue(obj, 20);

class MyClass
{
    public readonly int Value = 10;
}
```

`const`本质上是字面常量，是一个类的静态成员

### 初始化要求

`readonly`初始化不要求一定是编译时常量

```csharp
class MyClass
{
    public readonly int Value = 10;
    public readonly string Empty = string.Empty;
    public readonly DateTime Today = DateTime.Today;
    public readonly DateTime Tomorrow = DateTime.Today.AddDays(1);
}
```

`const`初始化要求一定是编译时常量

```csharp
class MyClass
{
    public const int Value = 20;
    public const int Value2 = Value * 2;
    public const string ClassName = nameof(MyClass);
}
```

## 新增语法特性

### readonly struct

readonly struct中的所有成员必须在初始化后不能被修改

```csharp
readonly struct MyCircle
{
    public readonly int Field = 10;
    
    public string ClassName {get;init;} = nameof(MyCircle);
    
    public MyCircle()
    {
        
    }
}
```

### readonly members

`readonly`表示`member`不会对本地的子段值进行修改，类似于C++中的`const method`

`readonly members`只能用于`struct`类型而不能用于`class`类型

```csharp
struct MyCircle
{
    public double X{get;}
    public double Y{get;init;}
    public double Radius{get;init;}

    public readonly void Display()
    {
        Console.WriteLine(this.ToString());
    }
    public override readonly string ToString() => $"{X},{Y},{Radius}";
}
```

```csharp
struct ShapeCollection
{
    private int field;
    private Dictionary<string, int>dict = new();
    
    public ShapeCollection() {}
    
    public int Field
    {
        readonly get => field;
        set => field = value;
    }
    public readonly int Dict
    {
        get => dict["shape"];
        set => dict["shape"] = value;
    }
}
```

### const string + string interpolation

```csharp
class MyClass
{
    public const int X = 10;
    
    public const string A = "hello";
    public const string B = "world";
    public const string C = $"{A}, {B}";
}
```
