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

- Organize namespaces with a clearly defined structure
  ```csharp
  // Examples
  namespace Company.Product.Module.SubModule
  namespace Product.Module.Component
  namespace Product.Layer.Module.Group
  ```
- Vertically align curly brackets.

- Declare all member variables at the top of a class, with **static variables at the very top**.

- Place comments in a separate line, not with the same line of code. Begin with an uppercase letter. End with a period. Insert one space between `//` and the start of comment.

- Use string interpolation to concatenate short strings, as shown in the following code.
  ```csharp
  string displayName = $"{nameList[n].LastName}, {nameList[n].FirstName}";
  ```
- Interpolated string works like js. in js we use,

  ```js
  `${var name}`
  ```

  in c# we will use,

  ```csharp
  $"hello world {var_name}"
  ```

- Basic code structure

  ```csharp
  using namespace_name1;
  using namespace_name2;
  ...
  using namespace_nameN;

  namespace applicationName
  {
    class ClassName
    {
      static void Main(string[] args)
      {

      }
    }
  }
  ```

- Variables are three types.

  1. values types: value can be assigned. e.g: primitive types.
  2. reference types: saves the location of memory where the variabe is. e.g: `object`, `string`, `dynamic` are reference.
  3. pointer types: same as c++.

- The Object Type is the ultimate base class for all data types in C# Common Type System (CTS). When a value type is converted to object type, it is called boxing and on the other hand, when an object type is converted to a value type, it is called unboxing.

- We can save anyting in `dynamic` type variables.

  ```csharp
  dynamic <variable_name> = value;
  ```

- Variable declaration.
  ```csharp
  <data_type> <variable_list>;
  ```
- Constants can't be altered later
  ```csharp
  const <data_type> <constant_name> = value;
  ```
- Operators are same as c++.

- Decision making same as c++.

  ```csharp
  // switch
  switch(x)
  {
      case 1:
          Console.WriteLine("one");
          break;
      case 2:
          Console.WriteLine("two");
          break;
      default:
          Console.WriteLine("> 2");
          break;
  }
  ```

- Loops are same as c++.

  ```csharp
  int[] arr3 = { 1, 2, 3, 4, 5 };

  foreach(int e in arr3)
  {
      Console.WriteLine(e);
  }
  ```

- An assembly is a collection of types and resources that are built to work together and form a logical unit of functionality. Assemblies take the form of executable (.exe) or dynamic link library (.dll) files, and are the building blocks of .NET applications.

- Encapsulation is defined 'as the process of enclosing one or more items within a physical or logical package'. Encapsulation is implemented by using access specifiers.

  1. public - the type or member can be accessed by any other code in the same assembly or another assembly that references it.
  2. private - the type or member can be accessed only by code in the same class or struct.
  3. protected - The type or member can be accessed only by code in the same class, or in a class that is derived from that class.
  4. internal - The type or member can be accessed by any code in the same assembly, but not from another assembly.
  5. protected internal - The type or member can be accessed by any code in the assembly in which it's declared, or from within a derived class in another assembly.
  6. private protected - The type or member can be accessed only within its declaring assembly, by code in the same class or in a type that is derived from that class.

- Methods.

  ```csharp
  <Access Specifier> <Return Type> <Method Name>(Parameter List) {
    Method Body
  }
  ```

  parameters can be of 3 types.

  1. value:

  ```csharp
  public void swap(int x, int y) {}

  swap(x, y)
  ```

  2. reference:

  ```csharp
  public void swap(ref int x, ref int y) {
    int temp;

    temp = x; /* save the value of x */
    x = y;    /* put y into x */
    y = temp; /* put temp into y */
  }

  int a = 100;
  int b = 200;

  swap(ref a, ref b);
  ```

  3. output: A return statement can be used for returning only one value from a function. However, using output parameters, you can return two values from a function.

  ```csharp
  void getValue(out int x ) {
      int temp = 5;
      x = temp;
  }

  int a = 100;
  getValue(out a);
  ```

- C# provides a special data types, the nullable types, to which you can assign normal range of values as well as null values. `< data_type> ? <variable_name> = null;`

  ```csharp
  int? num1 = null;
  int? num2 = 11;

  double? num3 = new double?();
  ```

- The null coalescing operator is used with the nullable value types and reference types. `first ?? second`. If first is null then the second number is returned.

  ```csharp
  num1 = num2 ?? 5.3;
  ```

- Array can be declared by, `datatype[] arrayName;`.

  Declaration and initialization examples,

  ```csharp
  int[] arr = new int[4] { 1, 2, 5, 6};

  int[] arr2;
  arr2 = new int[10];

  int[] arr3 = { 1, 2, 3, 4, 5 };
  ```

  Multi-dimensional can be declared as `a[,]`, `a[,,]`. Can be declared using `comma` and accessed using `comma`.

  ```csharp
  int[,] a = new int[3, 4] {
      {0, 1, 2, 3} ,
      {4, 5, 6, 7} ,
      {8, 9, 10, 11}
  };

  // this will print 6
  Console.WriteLine(a[1, 2]);
  ```

- A `jagged array` is an array of arrays.

  ```csharp
  int[][] arr = new int[5][];

  for(int i = 0; i < 5; i++)
  {
      arr[i] = new int[i + 1];
      for (int j = 0; j <= i; j++)
          arr[i][j] = j + 1;
  }

  for(int i = 0; i < 5; i++)
  {
      for(int j = 0; j < arr[i].Length; j++)
          Console.Write(arr[i][j] + " ");

      Console.WriteLine();
  }

  // this will print
  // 1
  // 1 2
  // 1 2 3
  // 1 2 3 4
  // 1 2 3 4 5
  ```

- Array can be passed in functions too.

  ```csharp
  public static void foo(int[,] arr)
  ```

- We can pass array of params in a function.

  ```csharp
  public static int AddElements(params int[] arr) {}

  int sum = AddElements(1, 2, 3, 4, 6, 7, 0);
  ```

- Strings can be handled like c++. Also, can be created from array of chars.

  ```csharp
  char[] letters = { 'H', 'e', 'l', 'l', 'o' };
  string s = new string(letters);

  Console.WriteLine(letters);
  ```

  Some useful methods of string are as following,

  1. comparing:

     ```csharp
     string s1 = "aab";
     string s2 = "aba";

     // a.CompareTo(b)
     // returns -1 when a < b
     // returns 0 when a == b
     // returns 1 when a > b
     Console.WriteLine(s1.CompareTo(s2));
     ```

  2. contains: `a.Contains(b)`
  3. sub-string: `a.Substring(startIndex, length)`
  4. joining: `String.Join("\n", string_array);`

- Structures in c# are a bit different than c++. Can be defined using `struct`.

  ```csharp
  struct Books
  {
    // variable declarations
    // constructor if any
    // methods
  };

  // declare struct variable
  Books book1;

  // can be accessed like
  book1.name = "";
  ```

  1. There is no default constructor. If we declare one then **we must initialize all the variables inside**.
  2. Can't implement any interface.
  3. Can't be extended.
  4. structs are value types unlike classes which are reference types.

- An enumeration is a set of named integer constants. An enumerated type is declared using the enum keyword.

  ```csharp
  enum <enum_name> {
    enumeration list
  };

  enum Days {sun, mon, tues, wed, thurs, fri, sat};

  // sun will have value 0, mon will have 1, and so on
  ```

- Class

  ```csharp
  <access specifier> class  class_name {
   // member variables
   <access specifier> <data type> variable1;
   <access specifier> <data type> variable2;
   ...
   <access specifier> <data type> variableN;
   // member methods
   <access specifier> <return type> method1(parameter_list) {
      // method body
   }
   <access specifier> <return type> method2(parameter_list) {
      // method body
   }
   ...
   <access specifier> <return type> methodN(parameter_list) {
      // method body
   }
  }
  ```

  **If not specified, the access specifier of the class is internal and if the specifier of the member variables are not defined then they are private**

  constructor of the classes can be of two types,

  1. default, without any parameters
  2. parameterized, with params

  There can be more than one construtor with different params. When initialized, the one with the matching params is called.

  A destructor is a special member function of a class that is executed whenever an object of its class goes out of scope. A destructor has exactly the same name as that of the class with a prefixed tilde (~) and it can neither return a value nor can it take any parameters.

  Destructor can be very useful for releasing memory resources before exiting the program. Destructors cannot be inherited or overloaded.

  Static variables inside a class means, no matter how many objects we create that variable will be the same across all.

  Static functions can be called without creating an object, like `ClassName.FunctionName()`. Static functions can use only the static variables.

  **C# does not allow multiple inheritance, but we can use interface to meet this need**

- The word polymorphism means having many forms. In object-oriented programming paradigm, polymorphism is often expressed as 'one interface, multiple functions'. Can be static and dynamic. `Function Overloading` and `Operator Overloading` belongs to static and `abstract class and virtual functions` belong to dynamic.

- Operator can be overloaded for classes.

  ```csharp
  class Box {
    private double length;   // Length of a box
    private double breadth;  // Breadth of a box
    private double height;   // Height of a box

    public double getVolume() {
        return length * breadth * height;
    }
    public void setLength( double len ) {
        length = len;
    }
    public void setBreadth( double bre ) {
        breadth = bre;
    }
    public void setHeight( double hei ) {
        height = hei;
    }

    // Overload + operator to add two Box objects.
    public static Box operator+ (Box b, Box c) {
        Box box = new Box();
        box.length = b.length + c.length;
        box.breadth = b.breadth + c.breadth;
        box.height = b.height + c.height;
        return box;
    }
  }
  ```

  So, the syntax is,

  ```csharp
  public static return_type_which_is_the_class operator"operator sign without the quotes"(){}
  ```
