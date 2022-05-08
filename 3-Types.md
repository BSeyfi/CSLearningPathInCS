# C# Book - Chapter 3 : Types
## Members
### Properties
- *auto property* syntax

  ```csharp
  public string FirstName { get; set; }
  ```
- **Initializing** a property is **possible**

  ```csharp
  public string FirstName { get; set; } = string.Empty;
  ```
  - You can initialize the property in the declaration or constructor.

    ```csharp
    public List<int> points { get; set;} = new List<int>();
    ```
  
  - If you also delete the `set` *accessor*, you will have a read-only property that you can not reassign after construction is completed.

    ```csharp
    public List<int> points { get; } = new List<int>();
    ```

- `value` *contextual keyword* is used as the input parameter for `set` *accessor*.
  
  ```csharp
  //_num is a field
  set { _num = value; }
  ```
- *expression-bodied* members can be used to implement single expression properties
  ```csharp
  set => firstName = value;
  ```
- `set` or `get` *accessors* access modifier can be restrictive but not permissive than property access modifier:
  
  ```csharp
  public int N  { get; private set;} //    is valid
  private int N { get; public  set;} //    is not valid
  ```
  
- Properties are just like methods. This means that you can't modify the return value of a property of  value type.

---

### Indexers
- Provide an access mechanism to objects similar to array access by brackets.`[]`
- You can treat indexers like properties, methods that accept arguments of any type.
- Use `this[` *arg(s)* `]` to access indexers. 
- Indexers are overloadable.
  
  ```csharp
  private int[]  _v = new int[10];
  private int[,] _m = new int[10,10];
  
  //Type 1️⃣ syntactic sugar but not get
  public int this[int index] => _v[index];

  //Type 2️⃣ - like properties
  public int this[int index]
  {   
      get => _v[index];
      set => _v[index] = value;
  }
  
  //Type 3️⃣ - multi-dimensional
  public int this[int d1, int d2]
  {
      get => _m[d1, d2];
      set => _m[d1, d2] = value;
  }
  
  //Type 4️⃣ - any type of argument is possible
  public int this[string s]
  {
      get => s;
  }  
  
  //Type 5️⃣ - indexers are like properties. Doing a conditional evaluation for example
  public int? this[int i]
  {
      get => (i < 0 || i >= _v.Length) ? null : _v[i];
  }
  ```
---
### Opertatos
- You can modify the operator function in types by *operator overloading*.
- Most of the operators are overloadable, even `true` or `false` operators.
- These operators MUST be overloaded in pairs:
  - `==` and `!=` pair
  -  `<` and `>` pair
  -  `<=` and `>=` pair
- These operators are not overloadable.
  - `^x, x = y, x.y, x?.y, c ? t : f, x ?? y, x ??= y, x..y, x->y, =>, f(x), as, await, checked, unchecked, default, delegate, is, nameof, new, sizeof, stackalloc, switch, typeof, with`
- Operator overloading **MUST**:
  - Be `public static`
  - At least **one** parameter of the type of the defining type
the defining type (sample 3)
    - Order of types matter ie. `Tyep1+Type2` is different from `Type2+Type1`
    - You can overload both
```csharp
//1- unary operator overloading sample: 
public static Counter operator ++(Counter x) { /*body*/ }

//2- binary operator overloading sample: 
//evaluates x+y both of the Counter type.
public static Counter operator +(Counter x, Counter y)
{
    return new Counter(x.Count + y.Count);
}

//3- at least one parameter MUST be of the type of the defining class
//(here Counter is the defining class) <Counter instance> + <an int number>
public static Counter operator +(Counter x, int y) { /*body*/ }
```
- To define a type conversion operator (casting), we use `explicit` or `implicit` keywords with the type to be cast as the operator method name. 
  - `implicit` should be used when there is a standard built-in *type promotion* available(e.g. int->long,...)
    - If no standard type promotion available you may encounter exceptions or weird behavior
    - Our first choice is to use `explicit`. We use `implicit` just when it makes sense 
```csharp
// is used to resolve: (int)counter
public static explicit operator int(Counter x)
{
    return x.Count;
}

// is used to resolve implicit conversions like:
// <int or a type with a built-in conversion
//      available of int such as long, double,... > + counter
public static explicit operator int(Counter x)
{
    return x.Count;
}
```
- `true` and `false` operators are also overloadable. Their main purpose is to define the condition that is not true neither false. 
  - After the `nullable` feature is added to C#, overloading these operators may not be reasonable.
  - You can overload `implicit` cast to `bool` to cover any reasonable condition about booleans.
    - you can combine `?` with the type in to define a `nullable` conversion operator
```csharp
// implicit cast to nullable bool
public static implicit operator bool?(Counter x)
{
    if (x.x > 0)
        return true;
    if (x.x < 0)
        return false;
    return null;
}

//Example above without nullable support
public static implicit operator bool(Counter x)
{
    if (x.x >= 0)
        return true;
    return false;
}

//to overload true or false
public static bool operator true(Counter x)
{
    //body with return type of bool
}
```
---
***To be continued ...*** 

`#cs_internship` `#csharp` `#step2`