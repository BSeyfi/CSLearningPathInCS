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

##### The `notnull` Constraint
To specifying a value-type or a reference-type **must** be a non-nullable type, you can use the `notnull` constraint.

- By violating this constraint, compiler will just generate a warning instead of an error.
- The `notnull` constraint is effective in a *nullable context*. It is **not** effective in a *nullable oblivious context*.

##### The `class` Constraint
In a *nullable context* type argument **must** be a **non-nullable** reference type; **otherwise** compiler generates warning.

##### The `default` Constraint
This constraint is only applicable to overridden or explicitly implemented methods. We use it when the type parameter is neither known as a reference type nor as a value type.

```csharp
public class Parent
{
    public virtual void M<T>(T? item) where T : struct { }
    public virtual void M<T>(T? item) { } // 👈
}

public class Child : Parent
{
    public override void M<T>(T? item) where T : struct { }
    public override void M<T>(T? item) where T : default { } // 👈
}
```

##### The `unmanaged` constraint
What is an ***unmanaged type***? Any type of the followings is *unmanaged type*:
- `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`
- Any `enum` type
- Any `pointer` type (Will be explained in later chapters)
- Any user-defined `struct` that contains fields with the *unmanaged types* above.

So, the `unmanaged` constraint specifies that the `T` must be a *non-nullable* unmanaged type. It **can not** be combined with `struct` or `new()` constraints.

##### Delegate constraint
 Both `System.Delegate` and `System.MulticastDelegate` can be used as a base class constraint in a type-safe way, which were not allowed before  C# 7.3 release.

##### Enum constraint
The `System.Enum` class can be used as a constraint. It's type-safe and is useful to cache results.
For example, the following code, which is using not-efficient `reflection` method to retrieve an `enum` all valid values, can be used to cache and make a dictionary from this values. After a single time caching, the dictionary can be called without any performance implication. Cool!

```csharp
public static Dictionary<int, string> EnumNamedValues<T>()
  where T : System.Enum
{
    var result = new Dictionary<int, string>();
    var values = Enum.GetValues(typeof(T));

    foreach (int item in values)
        result.Add(item, Enum.GetName(typeof(T), item)!);
    return result;
}
```

---
### Zero-Like Values
#### What is the `default` value?
The CLR fills all the fields in a newly constructed object or `struct` with zero or zero-like value. This process also occurs for elements of an array during initialization.

We call these zero-like values the default values.
You can see the default value for any type in the table below:

Type  |  Default Value
--|--
  reference types|  `null`
built-in numeric types  |  `0`
`bool`  | `false`  
`char`  |  `\0`
`enum`  |  `(<enum type name>)0` zero casts to the enum type name
`struct`  | Creates a value and assigns the default value to all its fields
nullable value type  |  `null` (in practice `HasValue` property gets `false` and the `Value` property gets undefined. )

#### How can we use the `default` value in generics? And why?
In a generic type, it sometimes can be beneficial to reset a value to its initial value.

But in most cases, there is no way to assign a literal to a Generic type or generic parameter directly. In this case, we use the `default` operator or the `default` literal.

- `default` operator
  - `default(<type name>)` returns the default value of that type:

```csharp
public T M<T>()
{
    T t = default(T); // 👈
    return t;
}
```

- `default` literal
  - You can assign the `default` keyword to a value ( which returns the default of the corresponding type). It is equivalent to `default(<type name>)` You can use it almost everywhere instead of a real variable.
  You can see some examples of `default` literal usage:


```csharp
public GClass(int Length, T t = default) {/*body*/} // as a method optional parameter 👈
---
return default; // as a return value 👈
---
GClass<bool>(4, default); // as an argument while calling a method 👈
---
T t= default; // in a variable initialization or an assignment 👈
```

---
### Generic Methods
You can define a method in a generic format. The syntax is very similar to the generic type. Declare an ordinary method and add the type parameter(s) (`<T>` or similar generic type parameters) after the method name and before the parenthesis. You can add any constraint after the closing parenthesis like generic types.

```csharp
public static T GenericMethod<T>(T item) where T: struct
{/*Method Body*/}
```

#### How to define a *generic method* ?
1. You can define a generic method in a **non-generic** type:

```csharp
public class CX
{
    public T GenericM<T>(T item) where T: class
    {
        /*body of the mehtod*/
        return item;
    }
}
```

2. You can define a generic method in a **generic** type.
  - In this condition, it's recommended that the type parameter of the method and the containing type(`class` for example) are not the same; otherwise, the type parameter of the method will hide the outer scope type parameter and you receive a warning.

```csharp
public class CX<U>               // 👈 a generic class
{
    public T GenericM<T>(T item) // 👈 a generic method with a different type parameter name
    {
        /*body of the mehtod*/
        return item;
    }
    /*body of the class*/
}
```

#### Type Inference in the Generic methods
The compiler can **often** infer the type of the type argument of a generic method. So, you can omit the type parameter in the constructed generic type. Both method calls are the same in the following lines.

```csharp
GC.M<int>(a); // 👈
GC.M(a);      // 👈

public class GC
{
    public static void M<T>(T t) {/*body*/}
}
```

---
***To be continued ...***

`#cs_internship` `#csharp` `#step3`
