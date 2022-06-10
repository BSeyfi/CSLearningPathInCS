## Tuples Types in C#
You can package multiple data elements with tuples. It has concise syntax, and it is possible to name the data members.
To define a tuple, you should specify types and fields in parentheses separated by commas. Naming of fields is optional.


```csharp
(double, int) t1 = (4.5, 3);
Console.WriteLine($"Tuple with elements {t1.Item1} and {t1.Item2}.");
// Output:
// Tuple with elements 4.5 and 3.

(double Sum, int Count) t2 = (4.5, 3);
Console.WriteLine($"Sum of {t2.Count} elements is {t2.Sum}.");
// Output:
// Sum of 3 elements is 4.5.
```

It is possible to define a tuple with an arbitrary large number of elements:

```csharp
var t = (1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15}
Console.WriteLine(t.Item15);  // output: 15
```

### Use cases of tuples
- 1️⃣ You can return several elements packaged in one tuple instance from the method.
- 2️⃣ It is also possible to *deconstruct* it into separate variables.
- You can use tuples instead of anonymous types.
- Tuples are loosely coupled and best suited within private and internal methods. So it's better to use a class or a structure while working with public ones.

```csharp
var (minimum, maximum) = MinMax(); // 2️⃣ 👈  deconstruction
Console.WriteLine($"int type minimum number is {minimum} , and the maximum is {maximum}");

(int min, int max) MinMax() // 1️⃣ 👈  return
{
    return (int.MinValue, int.MaxValue);
}
```

### Tuple field names
- It is possible to specify the names of tuple fields. 1️⃣ 2️⃣

```csharp
var t = (Sum: 4.5, Count: 3);// 1️⃣ 👈
Console.WriteLine($"Sum of {t.Count} elements is {t.Sum}.");

(double Sum, int Count) d = (4.5, 3);// 2️⃣ 👈
Console.WriteLine($"Sum of {d.Count} elements is {d.Sum}.");
```

- During initialization, if possible, the compiler can assign the names of the variables provided to the tuple field names. It is known as *tuple projection initializers*. 3️⃣

``` csharp
var sum = 4.5;
var count = 3;
var t = (sum, count); // 3️⃣ 👈
Console.WriteLine($"Sum of {t.count} elements is {t.sum}.");
```

- You can always use default field names (`Item1`, `Item2`,...) even if they are explicitly named. 4️⃣

```csharp
var a = 1;
var t = (a, b: 2, 3);
Console.WriteLine($"The 1st element is {t.Item1} (same as {t.a})."); // 4️⃣ 👈
Console.WriteLine($"The 2nd element is {t.Item2} (same as {t.b})."); // 4️⃣ 👈
Console.WriteLine($"The 3rd element is {t.Item3}."); // 4️⃣ 👈
```

- **HINT**: Any **non-default** field name will **not be accessible at run-time**.

### Tuple assignment and deconstruction
- If both have the same number of elements and respectively have the same type of elements (or implicitly convertible), you can assign one tuple to another. 1️⃣
  - Only values are assigned. Field names are ignored. 2️⃣

```csharp
(int, double) t1 = (17, 3.14);
(double First, double Second) t2 = (0.0, 1.0);
t2 = t1; // 1️⃣ 👈
Console.WriteLine($"{nameof(t2)}: {t2.First} and {t2.Second}"); // 2️⃣ 👈
// Output:
// t2: 17 and 3.14
```

- You can use `=` to *deconstruct* a tuple as follows:
  - To declare the type of variables explicitly 1️⃣
  - Using the `var` keyword to infer types 2️⃣
  - Using existing variables 3️⃣

```csharp
var t = ("post office", 3.6);
(string destination, double distance) = t; // 1️⃣ 👈
Console.WriteLine($"Distance to {destination} is {distance} kilometers.");
-------------------------------------------------------------------------
var t = ("post office", 3.6);
var (destination, distance) = t;// 2️⃣ 👈
Console.WriteLine($"Distance to {destination} is {distance} kilometers.");
-------------------------------------------------------------------------
var destination = string.Empty;
var distance = 0.0;

var t = ("post office", 3.6);
(destination, distance) = t; //3️⃣ 👈
Console.WriteLine($"Distance to {destination} is {distance} kilometers.");
// Output:
// Distance to post office is 3.6 kilometers.
```

### Tuple equality
You can use `==` or `!=` to check equality or non-equality.
You use these operators if both tuples have the same number of elements and respectively have the same type of elements (or implicitly convertible).

```csharp
(int a, byte b) left = (5, 10);
(long c, int d) right = (5, 10);
Console.WriteLine(left == right);  //  True
Console.WriteLine(left != right);  //  False
```

### Tuples as out parameters
It's possible to use a tuple as an `out` parameter

```csharp
var limitsLookup = new Dictionary<int, (int Min, int Max)>()
{
    [2] = (4, 10),
    [4] = (10, 20),
    [6] = (0, 23)
};

if (limitsLookup.TryGetValue(4, out (int Min, int Max) limits))
{
    Console.WriteLine($"Found limits: min is {limits.Min}, max is {limits.Max}");
}
// Output:
// Found limits: min is 10, max is 20
```

### Tuples vs System.Tuple

-|System.ValueTuple (Tuples) |  System.Tuple
--|---|--
**kind of the type**|value type  | reference type  
**mutability**|mutable  |  immutable
**data members are:**|fields  |  properties
