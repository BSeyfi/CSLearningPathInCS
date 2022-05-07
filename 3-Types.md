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
  


***To be continued ...*** 

`#cs_internship` `#csharp` `#step2`
