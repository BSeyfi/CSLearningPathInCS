### C# Generics vs. C++ Templates
Both Generics in C# and Templates in C++ are similar concepts, but there is some differences too.

#### Main differences
**C# Generics** have simpler approach but less functionality than **C++ templates**. **C# Generic** type substitutions occur at runtime.

#### Differences in detail
Feature  |Description   |C# Generics |C++ Templates
--|---|---|--
 flexibility |  e.g. calling arithmetic operators directly is not possiple in C# generics |less flexible   |more  flexible
  non-type template parameters| like `template C<int i> {}`   |❌   |   ✅
  *explicit specialization*| a custom implementation of a template for a specific type  |  ❌ | ✅
 *partial specialization* |custom implementation for a subset of the type arguments  | ❌  | ✅
  type parameter can be used as the base class for the generic type| ie `G<T> : T`  | ❌ | ✅
  type parameters can have default types| -  | ❌  |✅
  generic type parameter can itself be a generic| hint: constructed types can be used as generics in c# | ❌ | ✅

In C#, the only allowed language constructs are those defined by constriants, so possible runtime errors are very limited.
But in C++ you can use expressions with more flexibility and with less limitation, but it is possible to encounter with runtime errors.
