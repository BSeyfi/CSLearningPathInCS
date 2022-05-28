# C# Book - Chapter 4 : Generics
## Generic Types
### Intro

If you decide to use a type while it is unknown (not specified right now), you can use Generics. We also use generic to define a type that is  not specific to a particular data type.
Let me give an example.

Here we have  a simple class:

```csharp
// Class for int
public class CNormalInt
{
    public CNormalInt(int number, string name)
    {
        Name = name;
        Number = number;
    }
    public string Name { get; }
    public int Number { get; }
}
```

As you see, it's so simple. Receives two arguments, one `int` and the other is `string` and stores them.
Very well! But what happens if I like to have a similar class  with a `double` number or a `float` one? I have to create two more classes. The complete code will be something like:

```csharp
// Class for int
public class CNormalInt
{
    public CNormalInt(int number, string name)
    {
        Name = name;
        Number = number;
    }
    public string Name { get; }
    public int Number { get; }
}

// Class for double
public class CNormalDouble
{
    public CNormalDouble(double number, string name)
    {
        Name = name;
        Number = number;
    }
    public string Name { get; }
    public double Number { get; }
}

// Class for float
public class CNormalFloat
{
    public CNormalFloat(float number, string name)
    {
        Name = name;
        Number = number;
    }
    public string Name { get; }
    public float Number { get; }
}
```

This code is good, but is there a smarter way? Is there any way to not copy-paste a single code several times for changing a single type name?
Yes! ***Generic Types*** is the answer. Lets directly explain on the code:

```csharp
// Defining a Generic Class
public class CGeneric<T>
{
    public CGeneric(T number, string name)
    {
        Name = name;
        Number = number;
    }
    public string Name { get; }
    public T Number { get; }
}
```

At first glance it may seem scary or strange, but it's simple. Lets explain:

There is a letter `T` in three places:
  1. at class definition line in a pair of an *angle brackets* `<>`.
  2. as a parameter type in the construction `method`.
  3. as the type of the `Number` property

As you noticed, `T` is used in place of a type. This type is unknown to us right now, but it is same everywhere the `T` is used in the class.
To use the `T`, we first should declare that we want to use it. Compiler understands that with using `<T>`. We use it for a single time in the defining type. Then we can use it.

But wait! How can we use a generic class? Does it directly usable? No. But it is easy. We must to construct our class based on a real type( instead of `T`) . Lets use the aforementioned generic class:

```csharp
var cNormalInt = new CGeneric<int>(10, "is int");
var cNormalFloat = new CGeneric<float>(10.1f, "is float");
var cNormalDouble = new CGeneric<double>(10.00000000001d, "is double");

Console.WriteLine($"{cNormalInt.Number} - {cNormalInt.Name}");
Console.WriteLine($"{cNormalFloat.Number} - {cNormalFloat.Name}");
Console.WriteLine($"{cNormalDouble.Number} - {cNormalDouble.Name}");
```

We have to specify the original type we want to use instead of the `T`. As you see, we made three instances from a single generic class based on different types(`int`, `float`, `double` here). Each object instance is constructed from a **distinct class**! Yes, you're right. They use identical Generic class, but they are three different classes construced on distinct types. We made completely different classes. After constructing classes based on real types, we call them *constructed generic class*.

### Advantages of the Generics
1. Less code
2. Better maintenance
3. No duplication and its potential errors
4. ...

### A few points about Generics
1. Term `T` is arbitrary. You can introduce whatever at the type signature level.
2. C# naming convention recommends to use `T` or a PascalCased identifier started with `T` to represent a generic type parameter.(like `TKey`,`TVal`, etc)

---
***To be continued ...***

`#cs_internship` `#csharp` `#step3`
