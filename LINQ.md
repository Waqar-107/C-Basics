# LINQ Basics

- LINQ (Language Integrated Query) is uniform query syntax in C# and VB.NET to retrieve data from different sources and formats.

- LINQ is a structured query syntax built in C# and VB.NET to retrieve data from different types of data sources such as collections, ADO.Net DataSet, XML Docs, web service and MS SQL Server and other databases.

- LINQ queries return results as objects.

- Advantages of LINQ

  **Familiar language:** Developers don’t have to learn a new query language for each type of data source or data format.

  **Less coding:** It reduces the amount of code to be written as compared with a more traditional approach.

  **Readable code:** LINQ makes the code more readable so other developers can easily understand and maintain it.

  **Standardized way of querying multiple data sources:** The same LINQ syntax can be used to query multiple data sources.

  **Compile time safety of queries:** It provides type checking of objects at compile time.
  IntelliSense Support: LINQ provides IntelliSense for generic collections.
  Shaping data: You can retrieve data in different shapes.

- There are APIs available to access third party data; for example, LINQ to Amazon provides the ability to use LINQ with Amazon web services to search for books and other items. This can be achieved by implementing the IQueryable interface for Amazon.

- Use `System.Linq` namespace to use LINQ.

- The static Enumerable class includes extension methods for classes that implements the `IEnumerable<T>` interface.

- `IEnumerable<T>` type of collections are in-memory collection like List, Dictionary, SortedList, Queue, HashSet, LinkedList.

- The static Queryable class includes extension methods for classes that implements the `IQueryable<T>` interface.

- There are two basic ways to write a LINQ query to IEnumerable collection or IQueryable data sources.

  1. Query Syntax or Query Expression Syntax - similar to SQL
  2. Method Syntax or Method Extension Syntax or Fluent

- Query Syntax

  ```csharp
  from <range variable> in <IEnumerable<T> or IQueryable<T> Collection>

  <Standard Query Operators> <lambda expression>

  <select or groupBy operator> <result formation>
  ```

  Example,

  ```csharp
  // string collection
  IList<string> stringList = new List<string>() {
      "C# Tutorials",
      "VB.NET Tutorials",
      "Learn C++",
      "MVC Tutorials" ,
      "Java"
  };

  // LINQ Query Syntax
  var result = from s in stringList
              where s.Contains("Tutorials")
              select s;
  ```

- LINQ query syntax always ends with a Select or Group clause.

- Method syntax (also known as fluent syntax) uses extension methods included in the Enumerable or Queryable static class, similar to how you would call the extension method of any class. Where method accepts a predicate delegate

  ```csharp
  // string collection
  IList<string> stringList = new List<string>() {
      "C# Tutorials",
      "VB.NET Tutorials",
      "Learn C++",
      "MVC Tutorials" ,
      "Java"
  };

  // LINQ Query Syntax
  var result = stringList.Where(s => s.Contains("Tutorials"));
  ```

- The compiler converts query syntax into method syntax at compile time.

- The lambda expression is a shorter way of representing anonymous method using some special syntax.

  ```csharp
  s => s.Age > 12 && s.Age < 20

  // multiple params
  (Student s,int youngAge) => s.Age >= youngage;

  // multiple statements and local variable
  (s, youngAge) =>
  {
    int x = 10000;
    Console.WriteLine("Lambda expression with multiple statements in the body");

    Return s.Age >= youngAge;
  }
  ```

- The Where operator (Linq extension method) filters the collection based on a given criteria expression and returns a new collection. The Where extension method has following two overloads. Both overload methods accepts a Func delegate type parameter. One overload required `Func<TSource,bool>` input parameter and second overload method required `Func<TSource, int, bool>` input parameter where int is for index

```csharp
// query syntax
var filteredResult = from s in studentList
                     where s.Age > 12 && s.Age < 20
                     select s.StudentName;

// Func with delegate
Func<Student,bool> isTeenAger = delegate(Student s) {
                                    return s.Age > 12 && s.Age < 20;
                                };

var filteredResult = from s in studentList
                     where isTeenAger(s)
                     select s;

// method syntax with lambda
var filteredResult = studentList.Where(s => s.Age > 12 && s.Age < 20);

// overloaded one, the one with the index
var filteredResult = studentList.Where((s, i) => {
            if(i % 2 ==  0) // if it is even element
                return true;

        return false;
    });
```

- The OfType operator filters the collection based on the ability to cast an element in a collection to a specified type.
  ```csharp
  var stringResult = mixedList.OfType<string>();
  ```
- OrderBy sorts the values of a collection in ascending or descending order. It sorts the collection in ascending order by default because ascending keyword is optional here. Use descending keyword to sort collection in descending order.

  ```csharp
  var orderByResult = from s in studentList
                      orderby s.StudentName
                      select s;
  ```

  OrderBy extension method has two overloads. First overload of OrderBy extension method accepts the Func delegate type parameter. So you need to pass the lambda expression for the field based on which you want to sort the collection.

  The second overload method of OrderBy accepts object of IComparer along with Func delegate type to use custom comparison for sorting.

  ```csharp
  var studentsInAscOrder = studentList.OrderBy(s => s.StudentName);
  var studentsInDescOrder = studentList.OrderByDescending(s => s.StudentName);
  ```

- The ThenBy and ThenByDescending extension methods are used for sorting on multiple fields.

  ```csharp
  var thenByResult = studentList.OrderBy(s => s.StudentName).ThenBy(s => s.Age);
  ```

- The grouping operators do the same thing as the GroupBy clause of SQL query. The grouping operators create a group of elements based on the given key. This group is contained in a special type of collection that implements an `IGrouping<TKey,TSource>` interface where TKey is a key value, on which the group has been formed and TSource is the collection of elements that matches with the grouping key value.

  ```csharp
  var groupedResult = studentList.GroupBy(s => s.Age);
  ```

- ToLookup is the same as GroupBy; the only difference is GroupBy execution is deferred, whereas ToLookup execution is immediate. Also, ToLookup is only applicable in Method syntax. **ToLookup is not supported in the query syntax.**

  ```csharp
  var lookupResult = studentList.ToLookup(s => s.age);
  ```

- The Join operator operates on two collections, inner collection & outer collection. It returns a new collection that contains elements from both the collections which satisfies specified expression. It is the same as inner join of SQL.

  The first overload method takes five input parameters (except the first 'this' parameter): 1) outer 2) inner 3) outerKeySelector 4) innerKeySelector 5) resultSelector.

  ```csharp
  var innerJoin = studentList.Join(// outer sequence
                      standardList,  // inner sequence
                      student => student.StandardID,    // outerKeySelector
                      standard => standard.StandardID,  // innerKeySelector
                      (student, standard) => new  // result selector
                                    {
                                        StudentName = student.StudentName,
                                        StandardName = standard.StandardName
                                    });
  ```

  The GroupJoin operator performs the same task as Join operator except that GroupJoin returns a result in group based on specified group key. The GroupJoin operator joins two sequences based on key and groups the result by matching key and then returns the collection of grouped result and key.

  ```csharp
  var groupJoin = standardList.GroupJoin(studentList,  //inner sequence
                                std => std.StandardID, //outerKeySelector
                                s => s.StandardID,     //innerKeySelector
                                (std, studentsGroup) => new // resultSelector
                                {
                                    Students = studentsGroup,
                                    StandarFulldName = std.StandardName
                                });
  ```

- The Select operator always returns an IEnumerable collection which contains elements based on a transformation function. It is similar to the Select clause of SQL that produces a flat result set.
