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
