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

- When we assign values out of range to a variable it overflows. if we keep a code chukn inside `checked{}` then the code gives `Overflow Exception`
  ```csharp
  checked {
    byte num = 255;
    num++:
  }
  ```
- Using `var` as variable type will enable c# to detect the variable type on its own.

- `cw` then hit `tab` to get `Console.writeLine()`

- We can have placeholder while printing
  ```csharp
  Console.writeLine("{0} {1}", var1, var2);
  ```
- Type conversion can be of three types.
  1. Implicit - assigning variable of one type to another. possible if the new variables range is greater than the old. else there remains a chance of data loss which the compiler sensed and stops us from compiling.
  2. Explicit - casting.
     ```csharp
     float f = 3.14f;
     int i = (int)f;
     ```
  3. Conversion between non-compatible types.

# Coding Standards

- Use `PascalCasing` for `class` and `method` names.
- Use `camelCasing` for `local variable` and `method arguments`.
- Do not use `Screaming Caps` for consts as they will grab too much attention.

  ```csharp
  // Correct
  public static const string ShippingType = "DropShip";

  // Avoid
  public static const string SHIPPINGTYPE = "DropShip";
  ```

- Avoid using abbreviations like `emp` instead of `employee`, `grp` instead of `group`. Use abbreviations for `Id`, `Xml`, `Ftp`, `Uri`.

- Use `PascalCasing` for abbreviations 3 characters or more. 2 char are both upper.

- Do not use `_` in identifiers. **Exception: private static variables**.

  ```csharp
  // Correct
  public DateTime clientAppointment;
  public TimeSpan timeLeft;

  // Avoid
  public DateTime client_Appointment;
  public TimeSpan time_Left;

  // Exception
  private DateTime _registrationDate;
  ```

- Use implicit type var for local variable declarations. Exception: primitive types (int, string, double, etc) use predefined names

  ```csharp
  var stream = File.Create(path);
  var customers = new Dictionary();

  // Exceptions
  int index = 100;
  string timeSheet;
  bool isCompleted;
  ```

- Prefix `interfaces` with the letter `I`. Interface names are noun (phrases) or adjectives.

- Name source files according to their main classes. Exception: file names with partial classes reflect their source or purpose, e.g. designer, generated, etc.

  ```csharp
  // Located in Task.cs
  public partial class Task
  {
      //...
  }

  // Located in Task.generated.cs
  public partial class Task
  {
    //...
  }
  ```
