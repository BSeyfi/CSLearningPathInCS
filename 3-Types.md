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
  - At least **one** parameter of the type of the defining type (sample 3) is needed.
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
### Events
- You can define *events* for `struct` or `class`.
- Event model is a way to broadcast notifications to the subscribed objects.
- With event subscription, the event source object couples to the event sink object.
- Events are related to `delagte`s. So we will talk more about them in later chapters. The author of the book has talked a little about events here because an event is, after all, a type.
---
### Nested Types
- They're used to declare a type inside the scope of another.
- They're useful for implementing a better encapsulation.
- Their default access is `private`. Although you can give them permissive access(like `public`), they're not designed to be used in this way, and you may violate the *Code quality rule: Nested types should not be visible.*
- **Nested types:**
  - Should not be visible(accessible) outside of the containing(parent/outer) type.
  - Don't use them for logical grouping.
  - Don't use them to avoid name collision.
    - (`namespace` is for this job.)
- To refer to the outer type, you can initialize the inner/nested type with the outer/containing type instance. (refer to code below)
```csharp
public class Container //Outer Type
{
    public Container()
    {
        //this, here, refers to the defining type== Container class instance
        var nested = new Nested(this);
    }
    public class Nested // Inner Type
    {
        private Container parent;
        public Nested(Container parent)
        {
            //this, here, points to the defining type== Nested class instance
            this.parent = parent;
        }
    }
}
```

---
## Interfaces
### What are *interfaces* in C#?
*Interface* is a type to declare some services without any implementation\*. It can be used by different types with different implementation.

\* Starting with C# 8.0, interfaces can define *defualt implementation*. (more details later)
### How can we use *interfaces* ?
A `class` or a `struct` can use interfaces by declaring them after the type name, following with a colon. The type must implement all members of interfaces.
```csharp
public interface IService
{
    public void M();//Just declaration with no implementation
}

public class ClassX : IService
{
    /* Class body*/

    // Implementation of interface members
    public void M()
    {
        //body
    }
}

//=====To access interface member defined in a type=====
var classX = new ClassX();
classX.M(); // How to call the member of interface from a class instance
```
**Note:** A type can implement multiple interfaces by using a comma(`,`) separating interfaces name.

```csharp
public class ClassX : IService1, IService2, IService3
{
    /* Class body*/
}
```
### What can be declared as an interface member?
- **Before C# 8:**
  - Methods
  - Properties
  - Indexers
  - Events
- **Added after C# 8:**
  - Constants
  - Operators
  - Static constructor
  - Nested types
  - Static fields, methods, properties, indexers, and events
  - Member declarations with the explicit implementation
  - Explicit access modifiers (the default access is `public`)
- **Beginning with C# 11**
  - `static abstract` can usable to all types except fields.

### Some important notes in using interfaces:
  - You can't declare non-static fields in interfaces.
  - You can't change the *access modifier* when implementing.
  - You can add a `get` or `set` property implementation if it's not declared in the interface
    ```csharp
    interface IZ
    {
        public string Name { get; } // set is not declared
    }
    class ID : IZ
    {
        private string _name = "";
        public string Name
        {
            get => _name;
            private set => _name = value;// as set is not declared in interface,
            //we can add it with arbitrary access modifier
        }
    }
    ```

  - The `private` members of an interface must have an implementation.
  - Any member of an interface can have a *default implementation*.
  - To use a member of an interface, you should implement it in your type(like `class`) regardless of whether the member has been implemented in the `interface`(has a *default implementation*) or not.
  - All not-implemented members of an interface MUST be implemented in the target type.
  - To implement an `interface` member in a type(like `class`, `struct`,...), both members must have the same signature (same member name and same parameter types)
  - If two interfaces used in a type have the same member, you should implement them explicitly by the interface name preceding the member:
    - Syntax <*interface name*>.<*member name*>
    - To call the *explicit member* you should cast the instance to the related interface or use the `as` keyword like the following example:

  ```csharp
  interface IY
  {
      public void MyName();
  }
  interface IZ
  {
      public void MyName();
  }

  class MultipleImp : IY, IZ
  {
      void IY.MyName() //explicit implementation
      {
          Console.WriteLine("I'm the IY.MyName implementation");
      }
      void IZ.MyName() //explicit implementation
      {
          Console.WriteLine("I'm the IZ.MyName implementation");
      }
  }

  //=====RUN=====
  var multImp=new  MultipleImp();

  (multImp as IY).MyName(); //can not call directly. use the `as` keyword or cast
  (multImp as IZ).MyName(); //can not call directly. use the `as` keyword or cast
  ```

  Output:

  ```
  I'm the IY.MyName implementation
  I'm the IZ.MyName implementation
  ```

---
##  Enums
### What is the use of *enum* type?
- *Enum* (short for enumeration) is a type to define *named values*.
- Enumerations are for defining named values (flags, etc.) and combining them.
- Enumerations bring us code readability. Searching in codes and modifying flags gets easier with enumerations.

### How to define an `enum` type?

Enumerations utilize integral *value type*  **constants** for the underlying layer.
- to define an `enum` type, you can list some names:

```csharp
//===Defining an enum===
public enum FourElements
{
    Fire , //compiler may give it value == 0
    Air  , // also == 1
    Water, // also == 2
    Earth  // also == 3
}

//===An example of using an enum===
FourElements element = FourElements.Fire;

if (element == FourElements.Fire)
    Console.WriteLine("The elemnt is Fire");
```

- members of an enumeration are not mutually exclusive by default, so to define them as flags, you should give them mutually exclusive values manually:

```csharp
public enum FourElements
{
    Fire  = 1 << 1, // = 0b0000_0001
    Air   = 1 << 2, // = 0b0000_0010
    Water = 1 << 3, // = 0b0000_0100
    Earth = 1 << 4, // = 0b0000_1000  
}
```

- underlying value type for an enumeration is `int`. As `int` has 32-bits, to define a flag set with more than 32 flags, you should use `long` as the underlying type. Define the underlying data type by using a colon as follow:

```csharp
public enum ALotNumberOfFlags : long
{/*body*/}
```
- `enum` type members are practically constants.
- you can combine them by bitwise-OR operator `|`. Exclusion is also possible in addition to any arithmetic or logical operation.
- For checking flags' presence in an enum variable, use the `HasFlag` method :

```csharp
public enum Colors
{
    Black   = 0 ,
    Red     = 1 ,          //0b0000_0001
    Green   = 2 ,          //0b0000_0010
    Blue    = 4 ,          //0b0000_0100

    NoColor = 128 ,        //0b1000_0000

    Yellow  = Green | Red, //0b0011

}
Colors weirdYellow  =  Colors.NoColor | Colors.Yellow;
Colors normalYellow =  Colors.Yellow;

Console.WriteLine(weirdYellow.HasFlag(Colors.Red));  //-->True
Console.WriteLine(normalYellow.HasFlag(Colors.Red)); //-->True

```

```py
<enumX_instance>.HasFlag(<enumX_member>)
is equaivallent to
(<enumX_instance> & <enumX_member> ) == <enumX_member>
```

- `[System.Flags]` attribute provides functionalities dedicated to flags. By employing this attribute before defining the `enum,` compiler will treat enum as genuine flags. Decomposition into `string` gets possible.
Use it when you want to perform logical operations on mutually exclusive flags.
  - Nothing wrong will happen if you do not use this attribute. Its principal purpose is to decompose an `enum` member into strings.

```csharp
[System.Flags]
public enum ColorsF
{
    Red     = 1 ,          //0b0000_0001
    Green   = 2 ,          //0b0000_0010
    Blue    = 4 ,          //0b0000_0100
}

public enum Colors
{
    Red     = 1 ,          //0b0000_0001
    Green   = 2 ,          //0b0000_0010
    Blue    = 4 ,          //0b0000_0100
}

//===Run Sample===
Colors  Yellow   =  Colors.Green  | Colors.Red;
ColorsF YellowF  =  ColorsF.Green | ColorsF.Red;

Console.WriteLine(Yellow.ToString());
Console.WriteLine(YellowF.ToString());
```

Output:

```
3
Red, Green
```
---
## Other Types
### Delegates:
Will be discussed later in chapter 9.
### Pointers:
Will be discussed later in chapter 21.
### Special Classes:
There  are few classes with special handling in certain circumstances like:
  - exception classes will be discussed later in chapter 8.
  - attribute classes that will be discussed later in chapter 15.

### Anonymous Types
#### What is the use of *anonymous types*?
- You can encapsulate a read-only set of properties easily with *anonymous types* without defining the type.
- They are handy when working with LINQ queries.
- Two *anonymous types* have **same type** if they specify a sequence of properties that meet the following conditions:
  1.  properties in the same order have the same name
  2.  properties in the same order have the same type
  3.  both  have the same number of properties

#### How to define an anonymous type?
After the `new` keyword, write your property(s) with their value(s) within a curly braces`{}`. As the compiler infers the type of properties we can use the `var` keyword to defining a variable as an anonymous type:

```csharp
//defining an anonymous type
var animal      = new   { Name = "Sheep", Feet = 4 };
//defining an array of anonymous types
var animalArray = new[] { new { Name = "Sheep" , Feet = 4 },
                          new { Name = "Swan"  , Feet = 2 }  };


//===Run===
Console.WriteLine( animal.Name  );
Console.WriteLine(    animal    );
Console.WriteLine(animalArray[1]);
```

Output:

```
Sheep
{ Name = Sheep, Feet = 4 }
{ Name = Swan, Feet = 2 }
```

#### Important Notes About Anonymous Type:
- `null` is not supported
- It is derived directly from the `object` class.
- To pass it as an argument, the corresponding parameter type can be `object`.

- It is recommended to use *named struct* or class to store a query result or pass an anonymous type outside the method boundaries.

---
## Partial Types and Methods
### Partial Types
- You can define a type in multiple files using the `partial` keyword.
- It's supported for `class`, `struct`, `interface` and `record` types.
- *Partial types* may contain *partial method*s.
- It can be useful while working on:
  1. large projects
  2. automatically generated codes (like Windows forms designer)

File_1.cs contains the first part of `PartialClass`

```csharp
namespace PartialTypes
{
    public partial class PartialClass
    {
        public partial void PM(); //PM has declared here
    }
}
```

File_2.cs contains the second part of `PartialClass`

```csharp
namespace PartialTypes
{
    public partial class PartialClass
    {
        int index;
        public void M()
        {/*body*/}
        public partial void PM() // PM has defined here
        {/*body*/}
    }
}
```

### Partial Methods
*Partial methods* allow us to declare a method in one file and implement it in the other one.
- It enables developers to declare method hooks with no implementation defined. Developers can decide later to add the implementation or not.**\***
- Declared but not implemented methods will be discarded from the compilation process.
- Both declaration and implementation definition signatures **MUST** be the same.
- To define a method as partial, use the `partial` keyword.


**\*** If **all** the **following conditions** are met, you are eligible to omit the Implementation of a partial method, **otherwise you can not** :
- has no access modifier (`public`, `private` and so on)
- returns `void`
- has no parameter with `out` modifier
- has not defined with  `virtual`, `override`, `sealed`, `new`, or `extern` modifiers



`partial` is **not applicable** to:
- constructors
- finalizers
- overloaded operators
- property declarations
- event declarations

---
***To be continued ...***

`#cs_internship` `#csharp` `#step2`
