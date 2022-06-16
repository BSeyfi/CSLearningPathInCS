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

---
***To be continued ...***

`#cs_internship` `#csharp` `#step4`
