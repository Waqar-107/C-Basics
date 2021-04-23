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
  Shaping data: we can retrieve data in different shapes.

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

- Method syntax (also known as fluent syntax) uses extension methods included in the Enumerable or Queryable static class, similar to how we would call the extension method of any class. Where method accepts a predicate delegate

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
  (Student s,int wengAge) => s.Age >= wengage;

  // multiple statements and local variable
  (s, wengAge) =>
  {
    int x = 10000;
    Console.WriteLine("Lambda expression with multiple statements in the body");

    Return s.Age >= wengAge;
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

  OrderBy extension method has two overloads. First overload of OrderBy extension method accepts the Func delegate type parameter. So we need to pass the lambda expression for the field based on which we want to sort the collection.

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

  ```csharp
  var selectResult = studentList.Select(s => new { Name = s.StudentName ,
                                                 Age = s.Age  });
  ```

- The quantifier operators evaluate elements of the sequence on some condition and return a boolean value to indicate that some or all elements satisfy the condition.

  1. All

  ```csharp
  bool areAllStudentsTeenAger = studentList.All(s => s.Age > 12 && s.Age < 20);
  ```

  2. Any

  ```csharp
  bool isAnyStudentTeenAger = studentList.Any(s => s.age > 12 && s.age < 20);
  ```

  3. Contains

  ```csharp
  bool result = studentList.Contains(std);

  // with a custom comparator
  bool result = studentList.Contains(std, new StudentComparer());
  ```

- Average

  ```csharp
  var avgAge = studentList.Average(s => s.Age);
  ```

- The Count operator returns the number of elements in the collection or number of elements that have satisfied the given condition.

  ```csharp
  var totalStudents = studentList.Count();
  var adultStudents = studentList.Count(s => s.Age >= 18);
  ```

- The Max() method returns the largest numeric element from a collection.

  ```csharp
  var largest = intList.Max();

  var largestEvenElements = intList.Max(i => {
  		                        if(i%2 == 0)
  			                        return i;

  		                        return 0;
  	                        });

  // use custom comparator in class to get the max
  public class Student : IComparable<Student>
  {
      public int StudentID { get; set; }
      public string StudentName { get; set; }
      public int Age { get; set; }
      public int StandardID { get; set; }

      public int CompareTo(Student other)
      {
          if (this.StudentName.Length >= other.StudentName.Length)
              return 1;

          return 0;
      }
  }
  ```

- The Sum() method calculates the sum of numeric items in the collection.

  ```csharp
  var sumOfEvenElements = intList.Sum(i => {
  		                    if(i%2 == 0)
  			                    return i;

  		                    return 0;
  	                        });
  ```

- Element operators return a particular element from a sequence (collection).

  The ElementAt() method returns an element from the specified index from a given collection. If the specified index is out of the range of a collection then it will throw an Index out of range exception. Please note that index is a zero based index.

  The ElementAtOrDefault() method also returns an element from the specified index from a collaction and if the specified index is out of range of a collection then it will return a default value of the data type instead of throwing an error.

- `First()`, `FirstOrDefault()`, `Last()`, `LastOrDefault()` works like ElementAt().

- `Single()` and `SingleOrDefault()` works like the preceedings but gives error if there's more than one elment to return.

- There is only one equality operator: SequenceEqual. The SequenceEqual method checks whether the number of elements, value of each element and order of elements in two collections are equal or not.

  ```csharp
  bool isEqual = strList1.SequenceEqual(strList2);
  ```

  The SequenceEqual extension method checks the references of two objects to determine whether two sequences are equal or not. This may give wrong result.To compare the values of two collection of complex type (reference type or object), we need to implement IEqualityComperar<T> interface as shown below.

  ```csharp
  class StudentComparer : IEqualityComparer<Student>
  {
      public bool Equals(Student x, Student y)
      {
          if (x.StudentID == y.StudentID && x.StudentName.ToLower() == y.StudentName.ToLower())
              return true;

          return false;
      }

      public int GetHashCode(Student obj)
      {
          return obj.GetHashCode();
      }
  }
  ```

- The Concat() method appends two sequences of the same type and returns a new sequence (collection).

  ```csharp
  var collection3 = collection1.Concat(collection2);
  ```

- The DefaultIfEmpty() method returns a new collection with the default value if the given collection on which DefaultIfEmpty() is invoked is empty.

  Another overload method of DefaultIfEmpty() takes a value parameter that should be replaced with default value.

  ```csharp
  var newList1 = emptyList.DefaultIfEmpty();
  var newList2 = emptyList.DefaultIfEmpty("None");
  ```

- The Range() method returns a collection of IEnumerable<T> type with specified number of elements and sequential values starting from the first element.

  ```csharp
  // returns from 10 to 20
  var intCollection = Enumerable.Range(10, 10);
  ```

- The Repeat() method generates a collection of IEnumerable<T> type with specified number of elements and each element contains same specified value.

  ```csharp
  // returns 10 five times
  var intCollection = Enumerable.Repeat<int>(10, 5);
  ```

- The Distinct extension method returns a new collection of unique elements from the given collection.

  ```csharp
  var distinctList1 = strList.Distinct();
  ```

  The Distinct extension method doesn't compare values of complex type objects. we need to implement IEqualityComparer<T> interface in order to compare the values of complex types.

- `Except()` works in the same way.

- `Intersect()` works in the same way.

- `Union()` works in the same way.

- The Skip() method skips the specified number of element starting from first element and returns rest of the elements. `.Skip(val)`.

- `SkipWhile()` extension method in LINQ skip elements in the collection till the specified condition is true. It returns a new collection that includes all the remaining elements once the specified condition becomes false for any element.

- The Take() extension method returns the specified number of elements starting from the first element. `.Take(val)`.

- The TakeWhile() extension method returns elements from the given collection until the specified condition is true. If the first element itself doesn't satisfy the condition then returns an empty collection.
