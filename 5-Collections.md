# C# Book - Chapter 5 : Collections
## Collections
### Arrays
#### Intro
An array is a simple type of collection ( collection means a group of objects) supported intrinsically by CLR.

An array is a *type*. You can use it as a regular *type*. (even as a generic type parameter `T`)

- Array lengh is fix during its lifetime, and is not changable after creation.
- All array elements **must** be of the same type.
- Use brackets `[]` to define or access the elements of an array.
- Decleration syntax: `<Type name>[] <array name>;`
- Creation (memory allocation) syntax:
  1.  `<array name> = new <Type name>[<number of the elements>];`
  2.  `<array name> = new <Type name>[]{ element1, element2, ... };`
  3.  `<array name> = { element1, element2, ... };`

- You can access the items (elements) of an array by index.
  - Index is zero-based, meaning to access the first item you should use: `array_name[0]`

```csharp
// Declare a single-dimensional array
int[] array1;

// Create an array of 5 ints and fill with the default values
 array1 = new int[5];

// Declare and create array element values.
int[] array2 = new int[] { 2, 3, 5, 7 };

// Alternative way to create .
int[] array3 = {  2, 3, 5, 7 };

//Access the items
Console.Writeline( array3[3] ); // prints 7
```

- Hint: .NET docs: "All types, predefined and user-defined, reference types and value types, inherit directly or indirectly from `Object`". This means that you can have an array with `string` and int elements together by using the `object` type:

```csharp
object[] arr = new object[2];
arr[0] = new string("C#");
arr[1] = 4;
Console.WriteLine(arr[0].GetType()); //-> System.String
Console.WriteLine(arr[1].GetType()); //-> System.Int32
```
- Array type is **always** a reference type. An array has an independent entity from its items.
- You can retrieve the number of array elements by `Length` or `LongLength` properties.
- You can use brackets after any expression that returns an array to access its elements:

```csharp
Console.WriteLine(ZeroArray(3)[1]);    //->0
Console.WriteLine(ZeroArray(4).Length);//-> 4

// creat an array with length of i filled with zero
int[] ZeroArray(int i)
{
    int[] ar = new int[i];
    return ar;
}
```

- CLR will throw `IndexOutOfRangeException` if the index is **negative** or `index >= array Length`.
- An array can access, initialize, read or overwrite its items. But modifying an item depends on the item's capabilities. For example, you can overwrite an *immutable* item with a new one, but you cannot change the contents of this item.

#### Array Initialization
There are many ways to initialize an array, as we talked about some of them in [Intro](#intro). There are several other ways that I have categorized as follows:
- you can omit the number of elements while initializing
- you can use C# type inference(`var`)
- you can use short-hand way `new[]`.
- initializing an array as a method argument is like initializing an array
- You can also initialize an array item by item by using Index
  - Non-initialized items get initialized with the `default` value of the type used to construct the array

```csharp
//Multiple ways to initialize an array
var Seasons1 = new[] { "Spring", "Summer", "Fall", "Winter" };
var Seasons2 = new string[] { "Spring", "Summer", "Fall", "Winter" };
var Seasons3 = new string[4] { "Spring", "Summer", "Fall", "Winter" };
string[] Seasons4 = new string[] { "Spring", "Summer", "Fall", "Winter" };
string[] Seasons5 = new string[4] { "Spring", "Summer", "Fall", "Winter" };

// To initialize item by item by indexing
// item_1 is intentionally commented out to show that
// it's not mandatory to initialize all items
var Seasons6 = new string[4];
Seasons6[0] = "Spring";
//Seasons6[1]= "Summer";
Seasons6[2] = "Fall";
Seasons6[3] = "Winter";


//How to initialize an array as a method argument
PrintLength(new[] { "Spring", "Summer", "Fall", "Winter" });
PrintLength(new string[] { "Spring", "Summer", "Fall", "Winter" });
PrintLength(new string[4] { "Spring", "Summer", "Fall", "Winter" });

void PrintLength(string[] strArr)
{
  Console.WriteLine(strArr.Length);
};
```

#### Method parameter with Variable number of arguments
Sometimes you may want a method with variable numbers of *arguments*; You may have seen this functionality in `Console.Writeline` while passing multiple arguments like: 

`Console.WriteLine("{0}, {1}, {2}, {3}, {4}", 1, 2, 3, 4, 5);`

To achieve this functionality, you should define the **last** parameter of a method as an **array** and use the **`params`** keyword.

Now your method can accept several comma-separated arguments. 
The below code shows how to pass a variable number of arguments. You can Pass them within the array form too.

```csharp
PrintNumberOfParams(101, 102, 103); //prints 3
PrintNumberOfParams(new object[]{101, 102, 103}); //prints 3

void PrintNumberOfParams(params object[] strArr)
{
    Console.WriteLine(strArr.Length);
}

```

**Hint:** You can also overload the method with a known number of variables. From the point of memory allocation, it is better for the system to allocate memory fewer times. By defining an array, instead of multiple parameters, the system assigns memory one more time, and GC must free it at the end.


---
***To be continued ...***

`#cs_internship` `#csharp` `#step4`
