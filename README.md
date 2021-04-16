# C#-Basics

- `.Net` framework has two components,

  1. CLR - Common Language Runtime.
  2. Class Library.

- `C` or `C++` code is compiled into machine code which is language dependent. That means when we compile it a particular machine, it won't run in any machine with different OS or config. To overcome this, `C#` borrowed concepts from `Java`. In Java, the code is coverted into bytecodes then the `JVM` runs it. Similarly in C#, the code is converted into `**IL Code**` which is machine independent. Then we use `CLR` to run it. CLR is sitting in the memory and translating the IL Code. This process is called `Just in Time Compilation` aka `JIT`.

- A .Net application consists of classes.

- A `Namespace` is used for organizing classes. There are graphics namespace, database namespace etc.

- The namespaces are maintained by `Assembley` which can be either an `EXE` or a `DLL`. DLL stands for `Dynamically Linked Library`.

- Variables with `const` needs to be initialized when declared.

- Float type needs `f` at the end of it. Otherwise it should be declared as Double`

- Decimal type needs `m` as suffix.

- Primitive types
  |C# Type|.Net Type|Bytes|
  |-------|---------|-----|
  |byte|Byte|1|
  |short|Int16|2|
  |int|Int32|4|
  |long|Int64|8|
  |float|Single|4|
  |double|Double|8|
  |decimal|Decimal|16|
  |char|Char|2|
  |bool|Boolean|1|
