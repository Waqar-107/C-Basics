# NHibernate

- From Nuget package manager `install-package NHibernate`

- Create

  ```csharp
  using (var session = sefact.OpenSession())
  {
   	using (var tx = session.BeginTransaction())
  	{
  			var student1 = new Student {
  				ID = 1,
  				FirstMidName = "Allan",
  				LastName = "Bommer"
  			};

  			session.Save(student1);
  			tx.Commit();
  	}
  }
  ```

- Read

  ```csharp
  using (var session = sefact.OpenSession())
  {
  	using (var tx = session.BeginTransaction()) {
  			var students = session.CreateCriteria<Student>().List<Student>();

  			foreach (var student in students) {
  				Console.WriteLine("{0} \t{1} \t{2}",
  						student.ID,student.FirstName, student.LastName);
  			}

  			tx.Commit();
  	}
  }
  ```

- Update: fetch -> change -> save

  ```csharp
  using (var session = sefact.OpenSession())
  {
  	using (var tx = session.BeginTransaction())
  	{
  			var stdnt = session.Get<Student>(1);
  			stdnt.LastName = "Donald";
  			session.Update(stdnt);

  			tx.Commit();
  	}
  }
  ```

- Delete: fetch -> delete

  ```csharp
  using (var session = sefact.OpenSession())
  {
  	using (var tx = session.BeginTransaction())
  	{
  			var stdnt = session.Get<Student>(1);
  			session.Delete(stdnt);
  			tx.Commit();
  	}
  }
  ```

- `x.LogSqlInConsole = true;` logs the query on console.

- `install-package NHibernateProfiler` - this can also be used to see what the sql is doing.

- Intelisense for XML -

  1.  select an xml file.
  2.  in the toolbox select XML->schema.
  3.  hit add at the right corner of the box popped up.
  4.  browse to the c:/user/name/.nuget/packages/nhibernate/version_No/
  5.  select the two xsd files.

- We can define `BatchSize` in the config that means total number of updates that go out in a single round trip. For this we need to have `GUID` as id instead of native id because otherwise NHibernate won't be able to distinguish the new records created and thus will depend on the database for the informations that will not enable us to reduce the batch size.

  ```csharp
  // change this
  <id name = "ID">
  		<generator class = "native"/>
  </id>

  // into this
  <id name = "ID">
  		<generator class = "guid.comb"/>
  </id>
  ```

- Mapping. Suppose we have a `Location` object in `Student` class that describes the address. But in the database the variables of `Location` are in separate column of the `Student` table. e.g: House no, road no. city, post office etc. Now when we create a Student object, we create a Location object for it too. But how will the database interpret this? Mapping comes to the rescue in this case.

  In the XML file for `Student`, we do the following,

  ```csharp
  <component name = "Address">
  	<property name = "Street"/>
  	<property name = "City"/>
  	<property name = "Province"/>
  	<property name = "Country"/>
  </component>
  ```
