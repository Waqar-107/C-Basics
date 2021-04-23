# Entity Framework Core

- An ORM.

- Supports two development approaches

  1. Code-First
  2. Database-First

- EF Core mainly targets the code-first approach and provides little support for the database-first approach.

- In the code-first approach, EF Core API creates the database and tables using migration based on the conventions and configuration provided in your domain classes. This approach is useful in Domain Driven Design (DDD).

- In the database-first approach, EF Core API creates the domain and context classes based on your existing database using EF Core commands. This has limited support in EF Core as it does not support visual designer or wizard.

- Install `Microsoft.EntityFrameworkCore.SqlServer` from `Tools -> NuGet Package Manager -> Manage NuGet Packages For Solution`.

- Install `Microsoft.EntityFrameworkCore.Tools`.

- In the datavase-first approach we need to do reverse engineering using the `Scaffold-DbContext` command. This reverse engineering command creates entity and context classes (by deriving DbContext) based on the schema of the existing database.

- A connection string includes three parts: DB Server, database name and security info.

- The DbContext class is an integral part of Entity Framework. An instance of DbContext represents a session with the database which can be used to query and save instances of your entities to a database. DbContext is a combination of the Unit Of Work and Repository patterns.

  This context class typically includes DbSet<TEntity> properties for each entity in the model

  ```csharp
  public class SchoolContext : DbContext
  {
      public SchoolContext() {}

      protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder) {}

      protected override void OnModelCreating(ModelBuilder modelBuilder) {}

      //entities
      public DbSet<Student> Students { get; set; }
      public DbSet<Course> Courses { get; set; }
  }
  ```

  We must create an instance of SchoolContext to connect to the database and save or retrieve Student or Course data.

  The `OnConfiguring()` method allows us to select and configure the data source to be used with a context using `DbContextOptionsBuilder`.

  The `OnModelCreating()` method allows us to configure the model using ModelBuilder Fluent API.

- Creating first application

  ```csharp
  using System;
  using EFCoreTutorials;
  using Microsoft.EntityFrameworkCore;
  using Models;

  namespace Models
  {
      public class Student
      {
          public int StudentId { get; set; }
          public string Name { get; set; }
      }

      public class Course
      {
          public int CourseId { get; set; }
          public string CourseName { get; set; }
      }
  }

  namespace EFCoreTutorials
  {
      public class SchoolContext : DbContext
      {
          public DbSet<Student> Students { get; set; }
          public DbSet<Course> Courses { get; set; }

          protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
          {
              optionsBuilder.UseSqlServer(@"Server=(localdb)\\mssqllocaldb;Database=SchoolDB;Trusted_Connection=True;");
          }
      }
  }

  namespace HelloEFCore
  {
      class Program
      {
          static void Main(string[] args)
          {
            using (var context = new SchoolContext())
            {
                var std = new Student()
                {
                    Name = "Bill"
                };

                context.Students.Add(std);
                context.SaveChanges();
            }
          }
      }
  }
  ```

  use `add-migration CreateSchoolDB` to create a migration to create database called `SchoolDB`.

  use `update-database –verbose` to create the database.

- Querying data,

  ```csharp
  var context = new SchoolContext();

  var studentWithGrade = context.Students.Where(s => s.FirstName == "Bill")
                          .Include(s => s.Grade)
                          .Include(s => s.StudentCourses)
                          .FirstOrDefault();
  ```

  `Include` are converted to join

  `ThenInclude()` can be used after `Include`. This will join the the table usinng tables joined on `Include` phase.

- `contextVariableName.ModelName.Add(ModelVariable)` to Add data.

- Updating,

  ```csharp
  var std = context.Students.First<Student>();
    std.FirstName = "Steve";
    context.SaveChanges();
  ```

- Deleting

  ```csharp
  var std = context.Students.First<Student>();
  context.Students.Remove(std);

  // or
  // context.Remove<Student>(std);

  context.SaveChanges();
  ```

- We use `context.SaveChanges();` after insert/update/delete.

- Conventions are default rules using which Entity Framework builds a model based on your domain (entity) classes.

- EF Core will create database tables for all `DbSet<TEntity>` properties in a context class with the same name as the property. It will also create tables for entities which are not included as DbSet properties but are reachable through reference properties in other DbSet entities.

- EF Core creates null columns for all reference data type and nullable primitive type properties e.g. string, Nullable<int>, decimal?.

- EF Core creates NotNull columns in the database for all primary key properties, and primitive type properties e.g. int, float, decimal, DateTime etc..

- EF Core API will create a foreign key column for each reference navigation property in an entity with one of the following naming patterns.

  `<Reference Navigation Property Name>Id`
  `<Reference Navigation Property Name><Principal Primary Key Property Name>`

- One-to-Many Relationship Conventions in Entity Framework Core:
  here many students can get the same grade, so one grade many students, one to many relationship.

  1.  convention 1

  ```csharp
  public class Student
  {
  		public int Id { get; set; }
  		public string Name { get; set; }

  		public Grade Grade { get; set; }
  }

  public class Grade
  {
  		public int GradeId { get; set; }
  		public string GradeName { get; set; }
  		public string Section { get; set; }
  }
  ```

  2. convention 2

  ```csharp
  public class Student
  {
  		public int StudentId { get; set; }
  		public string StudentName { get; set; }
  }

  public class Grade
  {
  		public int GradeId { get; set; }
  		public string GradeName { get; set; }
  		public string Section { get; set; }

  		public ICollection<Student> Students { get; set; }
  }
  ```

  3.  convention 3 = c1 + c3

  ```csharp
  public class Student
  {
  		public int Id { get; set; }
  		public string Name { get; set; }

  		public Grade Grade { get; set; }
  }

  public class Grade
  {
  		public int GradeID { get; set; }
  		public string GradeName { get; set; }

  		public ICollection<Student> Students { get; set; }
  }
  ```

  4.  convention 4

  ```csharp
  public class Student
  {
  		public int Id { get; set; }
  		public string Name { get; set; }

  		public int GradeId { get; set; }	// defining foreign key explicitly
  		public Grade Grade { get; set; }
  }

  public class Grade
  {
  		public int GradeId { get; set; }
  		public string GradeName { get; set; }

  		public ICollection<Student> Students { get; set; }
  }
  ```

- One-to-One Relationship Conventions in Entity Framework Core:

  In EF Core, a one-to-one relationship requires a reference navigation property at both sides.

  ```csharp
  public class Student
  {
  		public int Id { get; set; }
  		public string Name { get; set; }

  		public StudentAddress Address { get; set; }
  }

  public class StudentAddress
  {
  		public int StudentAddressId { get; set; }
  		public string Address { get; set; }
  		public string City { get; set; }
  		public string State { get; set; }
  		public string Country { get; set; }

  		public int StudentId { get; set; }
  		public Student Student { get; set; }
  }
  ```

- There are two ways to configure domain classes in EF Core

  1.  By using Data Annotation Attributes
  2.  By using Fluent API

  Data annotation attributes are not dedicated to Entity Framework, as they are also used in ASP.NET MVC. This is why these attributes are included in separate namespace System.ComponentModel.DataAnnotations.

- Entity Framework Fluent API is used to configure domain classes to override conventions. EF Fluent API is based on a Fluent API design pattern

  In Entity Framework Core, the ModelBuilder class acts as a Fluent API. By using it, we can configure many different things, as it provides more configuration options than data annotation attributes.

  ```csharp
  public class SchoolDBContext: DbContext
  {
  		public DbSet<Student> Students { get; set; }

  		protected override void OnModelCreating(ModelBuilder modelBuilder)
  		{
  				//Write Fluent API configurations here

  				//Property Configurations
  				modelBuilder.Entity<Student>()
  								.Property(s => s.StudentId)
  								.HasColumnName("Id")
  								.HasDefaultValue(0)
  								.IsRequired();
  		}
  }
  ```

- Configure One-to-Many Relationships using Fluent API in Entity Framework Core:

  ```csharp
  public class Student
  {
  		public int Id { get; set; }
  		public string Name { get; set; }

  		public int CurrentGradeId { get; set; }
  		public Grade Grade { get; set; }
  }

  public class Grade
  {
  		public int GradeId { get; set; }
  		public string GradeName { get; set; }
  		public string Section { get; set; }

  		public ICollection<Student> Students { get; set; }
  }

  public class SchoolContext : DbContext
  {
  		protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
  		{
  				optionsBuilder.UseSqlServer("Server=(localdb)\\mssqllocaldb;Database=NewSchoolDBTrusted_Connection=True;");
  		}

  		protected override void OnModelCreating(ModelBuilder modelBuilder)
  		{
  				modelBuilder.Entity<Student>()
  						.HasOne<Grade>(s => s.Grade)
  						.WithMany(g => g.Students)
  						.HasForeignKey(s => s.CurrentGradeId);
  		}

  		public DbSet<Grade> Grades { get; set; }
  		public DbSet<Student> Students { get; set; }
  }
  ```

  `OnDelete(DeleteBehavior.Cascade)` can be used to delete on cascade.

- Configure One-to-One Relationships using Fluent API in Entity Framework Core:

  ```csharp
  public class Student
  {
  		public int Id { get; set; }
  		public string Name { get; set; }

  		public StudentAddress Address { get; set; }
  }

  public class StudentAddress
  {
  		public int StudentAddressId { get; set; }
  		public string Address { get; set; }
  		public string City { get; set; }
  		public string State { get; set; }
  		public string Country { get; set; }

  		public int AddressOfStudentId { get; set; }
  		public Student Student { get; set; }
  }

  public class SchoolContext : DbContext
  {
  		protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
  		{
  				optionsBuilder.UseSqlServer("Server=.\\SQLEXPRESS;Database=EFCore-SchoolDB;Trusted_Connection=True");
  		}

  		protected override void OnModelCreating(ModelBuilder modelBuilder)
  		{
  				modelBuilder.Entity<Student>()
  						.HasOne<StudentAddress>(s => s.Address)
  						.WithOne(ad => ad.Student)
  						.HasForeignKey<StudentAddress>(ad => ad.AddressOfStudentId);
  		}

  		public DbSet<Student> Students { get; set; }
  		public DbSet<StudentAddress> StudentAddresses { get; set; }
  }
  ```

- Configure Many-to-Many Relationships in Entity Framework Core:

  ```csharp
  public class Student
  {
  		public int StudentId { get; set; }
  		public string Name { get; set; }
  }

  public class Course
  {
  		public int CourseId { get; set; }
  		public string CourseName { get; set; }
  		public string Description { get; set; }
  }
  ```

  The steps for configuring many-to-many relationships would the following:

  1.  Define a new joining entity class which includes the foreign key property and the reference navigation property for each entity.
  2.  Define a one-to-many relationship between other two entities and the joining entity, by including a collection navigation property in entities at both sides (Student and Course, in this case).
  3.  Configure both the foreign keys in the joining entity as a composite key using Fluent API.

  ```csharp
  public class StudentCourse
  {
  		public int StudentId { get; set; }
  		public Student Student { get; set; }

  		public int CourseId { get; set; }
  		public Course Course { get; set; }
  }

  public class Student
  {
  		public int StudentId { get; set; }
  		public string Name { get; set; }

  		public IList<StudentCourse> StudentCourses { get; set; }
  }

  public class Course
  {
  		public int CourseId { get; set; }
  		public string CourseName { get; set; }
  		public string Description { get; set; }

  		public IList<StudentCourse> StudentCourses { get; set; }
  }

  modelBuilder.Entity<StudentCourse>().HasKey(sc => new { sc.SId, sc.CId });

  // when the key names differ
  modelBuilder.Entity<StudentCourse>()
  		.HasOne<Student>(sc => sc.Student)
  		.WithMany(s => s.StudentCourses)
  		.HasForeignKey(sc => sc.SId);


  modelBuilder.Entity<StudentCourse>()
  		.HasOne<Course>(sc => sc.Course)
  		.WithMany(s => s.StudentCourses)
  		.HasForeignKey(sc => sc.CId);
  ```

- The following steps must be performed in order to insert, update or delete records into the DB table using Entity Framework Core in disconnected scenario:

  Attach an entity to DbContext with an appropriate EntityState e.g. Added, Modified, or Deleted
  Call SaveChanges() method

  ```csharp
  //Disconnected entity
  var std = new Student(){ Name = "Bill" };

  using (var context = new SchoolContext())
  {
  		//1. Attach an entity to context with Added EntityState
  		context.Add<Student>(std);

  		//or the followings are also valid
  		// context.Students.Add(std);
  		// context.Entry<Student>(std).State = EntityState.Added;
  		// context.Attach<Student>(std);

  		//2. Calling SaveChanges to insert a new record into Students table
  		context.SaveChanges();
  }
  ```
