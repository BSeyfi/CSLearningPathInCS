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


***To be continued ...*** 

`#cs_internship` `#csharp` `#step2`
