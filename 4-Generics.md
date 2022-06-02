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

#### Advantages of the Generics
1. Less code
2. Better maintenance
3. No duplication and its potential errors
4. ...

#### A few points about Generics
1. Term `T` is arbitrary. You can introduce whatever at the type signature level.
2. C# naming convention recommends to use `T` or a PascalCased identifier started with `T` to represent a generic type parameter.(like `TKey`,`TVal`, etc)

---
### Constraints
#### Whats are the Generic Type Constraints?
- Constraints are some validations we put on a generic type. They're like instructions or rules to define how to deal with a generic type.

- To create an instance of a generic type, you must use valid types  (which were declared by constraints); otherwise, you will get errors from the compiler.
- To declare a *constraint*, you can add a `where T:<constraint>` in front of the type signature.

```csharp
// How To add Constraints
public class ConstraintPR<T> where T: new()
{
    public T Instance;
    public ConstraintPR()
    {
      Instance=new T();
    }
}
```

Example above shows how to define the `new()` constraint. You can replace `new()` with any valid constraint.

- If we create an instance of the type `T` in the body of a generic type, we must use the `new()` constraint; Otherwise, the compiler won't let us.

#### Constraints Types:
There are a few types of constraints existed:

Constraint  |  Description
--|--
`where T : struct`  |  `T` must be a `struct`
`where T : class`<br>`where T : class?` | `T` must be a `class`<br>`T` must be a nullable `class`
`where T : notnull` |  `T` must be a non-nullable reference or value type
`where T : <base class name>`<br>`where T : <base class name>?`| `T` must be or derive from the `<base class name>` class. It **must be** non-nullable<br>`T` must be or derive from the `<base class name>` class. It **can be** nullable or non-nullable
`where T : <interface name>`<br>`where T : <interface name>?`  |  `T` must be or implement the `<base interface name>` interface. It can also be generic. Implementing type **must be** non-nullable<br>`T` must be or implement the `<base interface name>` interface. It can also be generic. Implementing type **can be** nullable or non-nullable
`where T : unmanaged` |  `T` must be a non-nullable *unmanaged type* which implies struct constraints. It **can not** be combined with `struct` or `new()` constraints
`where T : default`  |  To resolve ambiguity while **overriding** a **method** or defining an explicit interface implementation. Implying the base method is not constrained by `struct` or `class` constraints.
`where T : new()` |  To be able to create an instance object of the type `T`inside the generic type.
`where T : U` |  `T` must be or derive from the type argument `U`. If `U` is non-nullable then `T` **must** be non-nullable .

- The `class`, `struct`, `unmanaged`, `notnull`, and `default` constraints
  1.  cannot be combined or duplicated
  2.  must be specified **first** in the constraints list.

- `new()` constraint must be specified **last**

- While using `where T : class` be cautious about `==` and `!=` operators. If you apply them to the generic argument types, they compare the references and **not values.**
  - If you're intended to compare values, it's recommended to apply `where T : IEquatable<T>` or `where T : IComparable<T>` constraints.

##### Defining Multiple constraints
You can define multiple constraints on a single parameter or constraints for several parameters.
1. Use one `where` clause for each parameter
2. Separate multiple constraints for a single parameter with comma(`,`)
3. If a generic type derives from another type, declare it by using a colon before the first `where` clause if existed.

You can see an example in the following code:

```csharp
public class Test<T, U> : BaseClass, ISampleInterface
    where T : SpecificClass, new()
    where U : unmanaged
{/*body*/}
```

#####  Unbounded type parameters
Means there is no constraint used for that parameter.

For an unbounded type parameter:
  1.  You **can not** use `==` or `!=` operators.
  2.  You can convert them from or to `System.Object`. You can also explicitly convert it to an interface type.
  3. It is comparable to `null`. Comparing a value type argument to `null` returns `false`.

##### Type Parameter as Constraint `where T : U`
1.  It is useful when a member method has to constraint its type parameter to the defining type parameter. For example in the following code `U` is constrained to `T`.

```csharp
public class List<T>
{
    public void Add<U>(List<U> items) where U : T
    {/* body */}
}
```

2.  It's rare but useful to enforce the inheritance relationship between type parameters. Like:

```csharp
public class Sample<T, U, V, W> where T : U { }
```






---
***To be continued ...***

`#cs_internship` `#csharp` `#step3`