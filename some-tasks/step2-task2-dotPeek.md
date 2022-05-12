### What is the *dotPeek* tool?
- *dotPeek* is a free tool by Jetbrains designed to decompile .NET assemblies to C# or IL code.
- It supports six file types which `dll` and `exe` are the most popular ones.
- It has many features like:
  - Assembly decompiler
  - Source code viewer
  - IL code viewer
  - Assembly viewer
  - Search and navigate
  - Hierarchy diagram tool
  - and more.

### How to install?
- You can find an installer on [Jetbrains](https://www.jetbrains.com/decompiler/download/#section=web-installer)
- If you are using a 32-bit Windows, you can find an x86 version under the **portable version** tab. [direct link](https://www.jetbrains.com/decompiler/download/download-thanks.html?platform=windows32)

### Looking for more info?
- You can find more info on Jetbrains [here](https://www.jetbrains.com/help/decompiler/dotPeek_Introduction.html).
- Official overview on Youtube: [dotPeek - .NET decompiler and assembly browser](https://www.youtube.com/watch?v=msJVDzrHS2g)

### An example you can try as a first step on *dotPeek*:
#### How to remove comments from the built assembly?
- It's preferred to keep comments and docs when compiling our codes into assemblies, but if you want to set them aside, you can use the following settings: [source on StackOverflow](https://stackoverflow.com/questions/62758703/how-to-permanently-remove-source-code-reference-in-a-net-core-application)
  - `dotnet run  property:DebugType=None property:DebugSymbols=false`
- You can see how an assembly can keep an exact source code in addition to the compiled version if you don't use the above settings.

### Next steps on *dotPeek*:
- In special situations or for learning purposes, you may need to use a decompiler.
- In some cases, viewing the decompiled C# or even IL version could be beneficial.
