# override 与 new 覆盖基类方法

## 基本语法

### override

```cs
var person = new Person();
person.Intro(); // I'm a person
var student = new Student();
student.Intro(); // I'm a student
var teacher = new Teacher();
teacher.Intro(); // I'm a person
class Person
{
    public virtual void Intro() => Console.WriteLine("I'm a person");
}

class Student : Person
{
    public override void Intro() => Console.WriteLine("I'm a student");
}

class Teacher : Person
{
    public override void Intro()
    {
        base.Intro();
    }
}
```

### new

```cs
var person = new Person();
person.Intro(); // I'm a person
var student = new Student();
student.Intro(); // I'm a student
var teacher = new Teacher();
teacher.Intro(); // I'm a person
class Person
{
    public void Intro() => Console.WriteLine("I'm a person");
}

class Student : Person
{
    public new void Intro() => Console.WriteLine("I'm a student");
}

class Teacher : Person
{
    public new void Intro()
    {
        base.Intro();
    }
}
```

## 区别

`new`是覆盖，`override`是重写

```cs
BaseClass c = new DerivedClass();
c.F1(); // BaseClass F1
c.F2(); // DerivedClass F2

class BaseClass
{
    public void F1() => Console.WriteLine("BaseClass F1");
    public virtual void F2() => Console.WriteLine("BaseClass F2");
}

class DerivedClass :BaseClass
{
    public new void F1() => Console.WriteLine("DerivedClass F1");
    public override void F2() => Console.WriteLine("DerivedClass F2");
}

```

## 如何选择

1. 被`virtual`标记的方法就意味着这个方法有很大可能性会在子类中被覆写，并提供更具体且有意义的实现。基类中的`virtual`方法通常只给出最基本且很可能不完整的实现，通常只是用来辅助子类的覆写。一旦子类给出了更好的实现，那么基类的方法并不应该希望被调用。
2. 基类的普通方法在设计时并不打算让子类进行覆写，因为这些方法通常已经是完整且有意义的。只有当子类希望对某个基类方法进行进一步定制时，才会考虑用`new`这种方式来显式隐藏基类的方法。但基类的方法仍然是有意义且可以被调用的。
