# C# Book - Chapter 6 : Inheritance
## Inheritance
### Inheritance Intro

![Type Inheritance in C#](resources/class-inheritence.png)

*Inheritance* is a significant attribute of *Object-Oriented Programming*. A child type can reuse, extend or modify the parent type's behavior.

- **Classe**s and **interface**s **support** inheritances 
- Other type categories (**structs, delegates, and enums**) **do not support** inheritance.
- All types in the .NET implicitly inherit from `System.Object` (Or a type derived from it).

Any instance of a more-derived type *is a* derived and a base type. C# recognizes this relationship. This relationship is called *is-a* relationship.
All members of a child class are derived from its parent except the following members:
1.  *static Constructor*s
2.  *Instance Constructors*s
3.  *Finalizers*
Member access modifiers may prevent access to base class members in the derived class.

Classes in C# support only single inheritance, which means they can be inherited from one class directly.
A *class* or an *interface* can inherit from many interfaces directly.

### Inheritance and Conversions
**Upcast**: As a child *is a* parent or *grand*parent, we can *upcast* a type to its parent like this code:

```csharp
string str= new string("CS_Internship");
//upcasting implicitly
object obj = str;
```

**Downcast**: We can downcast a parent reference to a child reference in these ways:

1.  *Explicit Downcast*:

```csharp
var child = new Child();
//upcasting implicitly
Parent parentRefersToChild = child;

//explicit downcasting
var d = (Child)parentRefersToChild;

class Child : Parent { }
class Parent { }
```

Trying to *downcast* a `parentRefersToChild` instance to a `Child` type will throw an `InvalidCastException` if the `parentRefersToChild` not refers to `Child` or a type derived from `Child`.

2.  Down casting by use of `as` keyword: 

```csharp
var child = new Child();
//upcasting implicitly
Parent parentRefersToChild = child;

//explicit downcasting
var d = parentRefersToChild as Child;

class Child : Parent { }
class Parent { }
```

Using the `as` keyword is the same as *explicit down-cast* without the risk of exception; If the conversion fails, the `d` variable will get `null` without throwing any exception.

3. Check the type with the `is` keyword:

- You can check if a variable is of type of the `Child` or its parents type with the `is` keyword.
- `GetType()` method returns the exact type of the instance. You can see the similarity and differences between the two:

```csharp
var child = new Child();
//upcasting implicitly
Parent parentRefersToChild = child;

Console.WriteLine(parentRefersToChild is Child);      //✅prints True
Console.WriteLine(parentRefersToChild is Parent);     //✅prints True
Console.WriteLine(parentRefersToChild is GrandParent);//✅prints True

Console.WriteLine(parentRefersToChild.GetType() == typeof(Child));      //✅prints True
Console.WriteLine(parentRefersToChild.GetType() == typeof(Parent));     //⛔️prints False
Console.WriteLine(parentRefersToChild.GetType() == typeof(GrandParent));//⛔️prints False


class Child : Parent { }
class Parent : GrandParent { }
class GrandParent { }
```

### Interface Inheritance
A class or an interface can inherit from one or more interfaces. A class must implement all parent interfaces and all great-grandparents interfaces. 

```csharp
interface IBase1
{
  void Base1Method();
}

interface IBase2
{
  void Base2Method();
}
interface IBoth : IBase1, IBase2
{
  void BothMethod3();
}

//A class that implements an interface must also implement the interface's ancestors.
class ClassX : IBoth
{
  public void Base1Method() { }
  public void Base2Method() { }
  public void BothMethod3() { }
}
```

### Inheritance in Generics   
If you derive a **non-generic** type from a generic Type, you **must** provide the required type arguments.

If you derive a **generic** type from a generic Type, you **can** use type parameters in the parent.
  - It's **not possible** to derive from a type parameter directly.
  - It is **possible** to derive from a generic type with a mixed version of type parameters and type arguments(like `IBase<string, T>`)

```csharp
public class GenericBase1<T> { }
public class GenericBase2<TKey, TValue> { }
public class NonGenericDerived : GenericBase1<string> { }
public class GenericDerived<T> : GenericBase1<T> { }
public class MixedDerived<T> : GenericBase2<string, T> { }
```

---
***To be continued ...***

`#cs_internship` `#csharp` `#step4`
