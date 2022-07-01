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

#### Searching and Sorting an Array
You can search for a specific item in an array or sort items in a specified order.

-  `Array.IndexOf` method searches for the first occurrence of an item in an array and returns the index number.
- You can also search for a value that does not exist in the array or with an incompatible type. In this condition, `Array.IndexOf` returns `-1` without throwing any error.

```csharp
int searchItem = 3;
int[] sampleArray = new[] { 1, 2, 3, 4, 5 };
int searchItemIndex = Array.IndexOf(sampleArray, searchItem); // searchItemIndex == 2
searchItemIndex = Array.IndexOf(sampleArray, 10);     // searchItemIndex == -1
searchItemIndex = Array.IndexOf(sampleArray, "Word"); // searchItemIndex == -1
```

There are two main methods to search for an item in an array as below:
- `Array.IndexOf` starts to search for an item from the lowest index ( == 0 ) toward the highest index and stops with the first match
- `Array.LastIndexOf` starts to search for an item from the highest index (end of an array) backward to the lowest index and stops with the first match.

```csharp
int[] sampleArray = new[] { 5, 3, 4, 10, 5, 3, 2 };
int searchItemIndex = Array.IndexOf(sampleArray, 3); // searchItemIndex == 1
searchItemIndex = Array.LastIndexOf(sampleArray, 3); // searchItemIndex == 5
```

Searching methods have many overloads. For example, you can search from the `startIndex` index for the `count` next items using:
  1. Syntax:  `int Array.IndexOf<T>(T[] array, T value, int startIndex, int count)`
  2. You can search in a slice of an array by using this overloaded method.

###### `Array.Find` Method and alternatives
You can search for an item(s) (not for indexes) by introducing a customized searching method to the `Array.Find` method or its alternatives.
Suppose we want to find items larger than `2`. Let's see how we can do it:

```csharp
int[] sampleArray = new[] { 5, 3, 4, 10, 1, 2 };

//by passing a delegate 
Console.WriteLine(Array.Find(sampleArray, IsLargerThan2)); // output: 5

// by using lambda expressions
Console.WriteLine(Array.Find(sampleArray, val => val > 2));// output: 5

bool IsLargerThan2(int val)
{
  return val > 2;
}
```

- `Array.FindLast`: similar to `Array.Find` but searches in reverse order.
- `Array.FindIndex`: searches for the item and returns the index of the first occurrence.
- `Array.FindLastIndex`: similar to `Array.FindIndex` but searches in reverse order.
- `Array.FindAll`: searches and returns all occurrence 

**A few words about search performance:**

Above methods, search for elements one by one in order. It's well for small arrays, but for super large Arrays, this may produce some performance issues, particularly for searches with more complex comparisons.

One approach is to change the search method. For example, `Array.BinarySearch` is super fast. But to use this method, your array must be pre-sorted. And sorting an Array may consume a lot of CPU time. You should do a tradeoff and choose which approach is suitable for the current problem.
Besides the CPU usage, there are many other factors you can bring into account, like CPU cache in use, RAM, IO, etc.

###### Sorting an Array
By using `Array.Sort`, you can sort items of an array ascending.
You can pass a delegate or a lambda expression to do a customized sort. It is also possible to sort values by introducing a key array. You can sort either the whole array or a slice of it.

```csharp
int[] sampleArray = new int[] { 5, 3, 4, 10, 1, 2 };
//Sort ascending
Array.Sort(sampleArray); // sampleArray == {1, 2, 3, 4, 5, 10}

sampleArray = new int[] { 5, 3, 4, 10, 1, 2 };
//Sort descending by changing the i1, i2 comparison order
Array.Sort(sampleArray, 
            (a, b) => b.CompareTo(a));// sampleArray == {10, 5, 4, 3, 2, 1}

sampleArray = new int[] { 5, 3, 4, 10, 1, 2 };
//Sort from index=2 upto 3 number of items
Array.Sort(sampleArray, 2,3);// sampleArray == {5, 3, 1, 4, 10, 2}
```

#### Multi-dimensional Arrays
All arrays we talked about up to here were *single-dimension* arrays. Single dimensional Array means that all items of an array are in a single row. On the other hand, a multi-dimension Array has two or more dimensions, and you need two or more index numbers to access each item.

There are two types of multi-dimensional array types in C#:
1- Jagged arrays
2- Rectangular arrays

##### Jagged arrays
You can embed arrays in arrays. An array of arrays! Suppose you want to define an array that contains several arrays of `int`. Inner arrays can have different lengths. To create a two-dimensions jagged array, for example, you should use two pairs of brackets `[]` side by side like `[][]`. The first bracket pair is for the most outer array that contains internal arrays. The second one goes one step inward (here, Array of `int`s).

```csharp
int[][] jaggedArray = new int[3][]{
  new int[]{1,2,3},
  new int[]{4,5},
  new int[]{7,8,9,10,20,30}
};
```

C# does not limit the number of dimensions, but CLR does. When you go further than 1000 dimensions, you may encounter performance issues before reaching the maximum number of dimensions limit ( For example, `[][][]` has three dimensions. Every pair of bracket makes one dimension. )

##### Rectangular arrays
A rectangular array contains an equal number of columns in each row. This concept is expandable to higher *rank*s. All ranks are the same. You can define a rectangular array by using commas inside a pair of brackets `[,].` The main syntax is `[rank0, rank1, rank2, ... ]`. In a two-dimensional array, `rank0` is equivalent to the *rows* and `rank1` is equivalent to *columns*.
- You should not use the `new` keyword inside the array because a rectangular array is a large array, not an array of arrays.
- `Length` indicates the number of all elements. (in the below example, it is 3x4=12)
- Use `GetLength` to now the size of each rank(dimension) at runtime. `<array name>.GetLength(<rank number>)`
```csharp
int[,] mArray = new int[3,4]{
  {1, 2, 3, 4},
  {4, 5, 6, 7},
  {7, 8, 9, 10}
};
```

#### Copying and Resizing Arrays
- You can use the `Array.Copy` static method to copy a chunk of items from the source array to the target array with the specified number of items.
  - You can be more specific by defining the start index to copy in source and target arrays.
  - Source and target arrays can be the same.
  - `Array.Copy` uses a temporary array to do the copy job so it is overlap safe.

- There is also a non-static `CopyTo` method available because `Array` implements `ICollection` interface.

```csharp
int[] A = new[] { 10, 20, 30, 40, 50 };

int[] B = new int[A.Length * 2]; // B == {0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
int[] C = new int[A.Length * 2]; // C == {0, 0, 0, 0, 0, 0, 0, 0, 0, 0}
int[] D = new int[A.Length * 2]; // D == {0, 0, 0, 0, 0, 0, 0, 0, 0, 0}

// Copies A.Length numbers of items of A to B 
Array.Copy(A, B, A.Length); //B == {10, 20, 30, 40, 50, 0, 0, 0, 0, 0}

// Copies A to C from index 3 of C
A.CopyTo(C, 3); //C == {0, 0, 0, 10, 20, 30, 40, 50, 0, 0}

// Copy from A(index=1) to D(from index=2) for 2 numbers of items.
Array.Copy(A,1,D,2,3); //D == {0, 0, 20, 30, 40, 0, 0, 0, 0, 0} 
```

- `Array.Resize` virtually resizes a single-dimensional array. As you know, the Arrays' length is fixed. This method creates a new Array, copies items into the new Array, and returns a new array accordingly.

- `Array.Reverse` reverses the order of items in an array. You can reverse the order of items in an Array within a specified range. You can also use this method for *jagged arrays*.

- `Array.Clear` resets itmes to their defualt values(`defualt(T)`).

```csharp
int[] A = new[] { 10, 20, 30, 40, 50 };
int[] B = new[] { 10, 20, 30, 40, 50 };

// Resize A to an arbitrary length
Array.Resize(ref A, 9); //A == {10, 20, 30, 40, 50, 0, 0, 0, 0}
Array.Resize(ref A, 3); //A == {10, 20, 30}

//Reverse the order of items of A
Array.Reverse(A); // A == {30, 20, 10}

//Reverse the order of 3 numbers of items of B started from index=1 
Array.Reverse(B, 1, 3); //B == {10, 40, 30, 20, 50}

// Resets the items with the defualt value
Array.Clear(A); // A == {0, 0, 0}
```

### List<T>
You can create a variable-length sequence of items with the `List<T>` generic class. List<T> is defined in `System.Collections.Generic` namespace. 

**What can you do with the `List` class?**
- Add or remove items freely (variable size)
- Get or set items by index number (like arrays)
- Search, sort, and more

**How to define and use a `List`?**
Its definition syntax is very similar to arrays. Their methods are different but with similar concepts. 
It's also possible to initialize while declaring a *List* or initialize one by one later.

```csharp
//Some ways to define and initialize List
// lst1, lst2, and lst3 are equivallent

//Compact Initialization Form
var lst1 = new List<int> { 10, 20, 30, 40 };
List<int> lst2 = new() { 10, 20, 30, 40 };

//Verbose definition form
var lst3 = new List<int>();
lst3.Add(10);
lst3.Add(20);
lst3.Add(30);
lst3.Add(40);
```

**Some points in using `List<T>`:**
- You can not pass an argument with type `List<T>` to a parameter with type `T[]` (an array of type `T`). They are not the same type.
  - Both List and Array implement various interfaces. You can enable any parameter to accept both list and array by defining the type as one of them, like `IList<T>`.
  - Be cautious when accessing a collection by indexer. Because the indexer is a property and returns a temporary copy of the item instead of the item itself. So if the item is a mutable value type, you may fall into trouble.

```csharp
var arr = new int[] { 10, 20, 30, 40, 50 };
var lst = new List<int> { 10, 20, 30, 40, 50 };

Console.WriteLine(CountArrayOrList(arr)); // prints 5
Console.WriteLine(CountArrayOrList(lst)); // prints 5

//Any type that implements IList can be used as the argument 👍
int CountArrayOrList(IList<int> collection)
{
  return collection.Count();
}
```

#### Some Important Methods Of Lists
- `Add`: appends a new item to the list. The size of the list automatically increases by one.
- `AddRange`: to add several items
- `Insert`: to insert an item in a specific index
- `InsertRange`: to insert several items in a specific index
- `Remove`: to remove the first occurrence of an item if there exists any
- `RemoveAt`: to remove the item with a specific index number
- `RemoveRange`: to remove a range of indexes
- `Count`: returns the number of items in a list

```csharp
List<int> A = new(); // A == {}

//Add an item
A.Add(10); // A == {10}

//Add several items
A.AddRange(new[] { 20, 30 }); // A == {10, 20, 30}

//Insert an item in specific index
A.Insert(1, 15); // A == {10, 15, 20, 30}

//Insert several items at specific index
A.InsertRange(1, new[] { 12, 14 }); // A == {10, 12, 14, 15, 20, 30}

//To remove the first occurrence of an item
A.Remove(500); // Nothing changes because 500 not exists in A
A.Remove(12); // A = {10, 14, 15, 20, 30}

//To remove item with index==3
A.RemoveAt(3); // A = {10, 14, 15, 30}

//To remove from index 0 for 2 numbers of items
A.RemoveRange(0, 2); // A = {15, 30}

System.Console.WriteLine(A.Count()); // prints 2
```

There is no need to define size while creating a list. A list automatically changes the underlying array size and capacity for storing the items. However, you can pre-define the initial capacity.
Capacity and size are not the same.
- `Capacity`: The list's underlying array size means how many items the list can fit without auto-resizing
- `Count`: Returns the number of items that exist in the list

```csharp
var B = new List<int>(20); // Creates Capacity == 20 but Count == 0
B[19] = 1000; // ❌ Runtime error because no item is accessible

var C = new List<int>(new int[20]); //initialize with an array of length == 20 so now 20 items are accessible
C[19] = 1000; // ✅ Works!
```

---
***To be continued ...***

`#cs_internship` `#csharp` `#step4`
