# NHibernate

- From Nuget package manager `install-package NHibernate`

- The application code uses the NHibernate ISession and IQuery APIs for persistence operations and only has to manage database transactions, ideally using the NHibernate ITransaction API.The application code uses the NHibernate ISession and IQuery APIs for persistence operations and only has to manage database transactions, ideally using the NHibernate ITransaction API.

- SessionFactory compiles all the metadata necessary for initializing NHibernate. SessionFactory can be used to build sessions, which are roughly analogous to database connections.

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

- One-to_Many relation,

  ```csharp
  // a customer can have many orders
  // Orders is the classname, order is the table name in db
  <set name = "Orders" table = "`Order`">
  		<key column = "CustomerId"/>
  		<one-to-many class = "Order"/>
  </set>
  ```

  We can use `Bag` if we need a multi-set.

- There are different options for cascading, which are as follows −

  1.  **none** − which is the default and it means no cascading.

  2.  **all** − which is going to cascade saves, updates, and deletes.

  3.  **save-update** − it will cascade, saves and updates.

  4.  **delete** − it will cascade deletes.

  5.  **all-delete-orphan** − it is a special one which is quite frequently used and is the same as All Except, if it finds Delete-orphan rows, it will delete those as well.

- Inverse Relationships,

  Let’s say you have a bidirectional associations using a single foreign key.

  1.  From a relational standpoint, you have got one foreign key, and it represents both customer to order and orders to customer.

  2.  From the OO model, you have unidirectional associations using these references.

  3.  There is nothing that says that two unidirectional associations represent the same bidirectional association in the database.

  4.  The problem here is that NHibernate doesn't have enough information to know that customer.orders and order.customer represent the same relationship in the database.

  5.  We need to provide inverse equals true as a hint, it is because the unidirectional associations are using the same data.

  6.  If we try to save these relationships that have 2 references to them, NHibernate will try to update that reference twice.

  7.  It will actually do an extra roundtrip to the database, and it will also have 2 updates to that foreign key.

  8.  The inverse equals true tells NHibernate which side of the relationship to ignore.

  9.  When you apply it to the collection side and NHibernate will always update the foreign key from the other side, from the child object side.

  10. Then we only have one update to that foreign key and we don't have additional updates to that data.

  11. This allows us to prevent these duplicate updates to the foreign key and it also helps us to prevent foreign key violations.

- Load/Get
  `Get` returns an object or null. `Load` returns the object or throws `ObjectNotFoundException`

  - Load can optimize database round trips much more efficiently.
  - Load actually returns a proxy object and doesn't need to access the database right when you issue that Load call.

  ```csharp
  var customer1 = session.Get<Customer>(id1);

  var customer1 = session.Load<Customer>(id1);
  ```

- There are two types of syntax while using LINQ −

  1. Query Chaining Syntax
  2. Query Comprehension Syntax

  ```csharp
  var customer = session.Query<Customer>().Where(c => c.FirstName == "Laverne");

  var customer = (from c in session.Query<Customer>()
                  where c.FirstName == "Laverne" select c).First();
  ```

- Hibernate Query Language

  ```csharp
  var customers = session.CreateQuery("select c from Customer c where c.FirstName = 'Laverne'");
  ```

  **it's actually the name of the class that we are selecting from.**

- Criteria API lets you build a query by manipulating criteria objects at runtime.

  1. This approach lets you specify constraints dynamically without direct string manipulations, but it doesn’t lose much of the flexibility or power of HQL.

  2. On the other hand, queries expressed as criteria are often less readable than queries expressed in HQL.

  ```csharp
  var customers = session.CreateCriteria<Customer>()
                         .Add(Restrictions.Like("FirstName", "H%"));
  ```

- QueryOver Queries

  It is a new syntax which is more like LINQ using the method chain syntax.

  ```csharp
  var customers = session.QueryOver<Customer>()
                         .Where(x => x.FirstName == "Laverne");
  ```

- Executing rae queries
  ```csharp
  IQuery sqlQuery = session.CreateSQLQuery("SELECT * FROM \
                                            CUSTOMER").AddEntity(typeof(Customer));
  var customers = sqlQuery.List<Customer>();
  ```

# More on Mappings

- hibernate-mapping

  ```xml
  <hibernate-mapping
    schema="name of a db schema - optional"
    catalog="name of a db catalog - optional"
    default-cascade="default cascade style - default is none"
    auto-import="Specifies whether we can use unqualified class names of classes in this mapping in the query language - true|false - defaults to true"
    assembly="optional"
    namespace="optional"
    default-access="field|property|field.camelcase... - The strategy NHibernate should use for accessing a property value. It can be a custom implementation of IPropertyAccessor."
    default-lazy="true|false - defaults to true"
  />
  ```

- If you have `two persistent classes with the same (unqualified) name`, you should set auto-import="false". NHibernate will throw an exception if you attempt to assign two classes to the same "imported" name.

- We may declare a persistent class using the class element

  ```xml
  <class
    name="className"                                  (1)
    table="tableName"                                 (2)
    discriminator-value="discriminatorValue"          (3)
    mutable="true|false"                              (4)
    schema="owner"                                    (5)
    catalog="catalog"                                 (6)
    proxy="proxyInterface"                            (7)
    dynamic-update="true|false"                       (8)
    dynamic-insert="true|false"                       (9)
    select-before-update="true|false"                 (10)
    polymorphism="implicit|explicit"                  (11)
    where="arbitrary sql where condition"             (12)
    persister="persisterClass"                        (13)
    batch-size="N"                                    (14)
    optimistic-lock="none|version|dirty|all"          (15)
    lazy="true|false"                                 (16)
    abstract="true|false"                             (17)
    entity-name="entityName"                          (18)
    check="arbitrary sql check condition"             (19)
    subselect="SQL expression"                        (20)
    schema-action="none|drop|update|export|validate|all|commaSeparatedValues"  (21)
    rowid="unused"                                    (22)
    node="unused"                                     (23)
  />
  ```

  for more detail: https://nhibernate.info/doc/nhibernate-reference/mapping.html

- It is perfectly acceptable for the named persistent class to be an interface. You would then declare implementing classes of that interface using the <subclass> element.

- Due to an HQL parser limitation inner classes can not be used in queries in NHibernate, unless assigning to them an entity-name without the + character.
