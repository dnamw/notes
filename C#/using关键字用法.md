# Using用法

## Using Namespace

## Using Static

```csharp
using static System.Math;
```

## Global Using & Implicit

```csharp
global using System;
```

在`.csproj`中可配置`<ImplicitUsings>enable</ImplicitUsings>`

## Using Alias

```csharp
using Sys = System;
using M = System.Math;
```

## Using IDisposable

```csharp
using System.Text;

using var stream = new FileStream("",FileMode.Open);
using var reader = new StreamReader(stream, Encoding.UTF8);
reader.ReadToEnd();
```

`HttpClient`类不建议使用`using`
