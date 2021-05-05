# Web API

- Creating a project has the following steps-

  1.  ASP.Net Core Web Application.
  2.  Web Application (Model-View-Controller).

- Create controllers in the `Controllers` folder. Add `MVC 5 Controller with read/write actions`. The `GET`, `POST` etc. will be genrated. We can also add more routes.

  NOTE: API of these files start with `api/controllerName` and then we can use whatever we want.

- Create models in the `Models` folder. Just like classes.

- Documentation is auto generated. We just have to edit some config and write descriptions for the doc.

  1.  `solution manager` -> double click on `properties` -> `build` -> scroll down and find `output path` -> check the `xml documentation file` -> change the folder name to save the doc e.g: `~App_Data/Documentation.xml` -> hit save.
  2.  `solution manager` -> `Areas` -> `Help Page` -> `App_Start` -> `HelpPageConfig.cs` -> uncomment the line
      `config.SetDocumentationProvider(new XmlDocumentationProvider(HttpContext.Current.Server.MapPath("~/App_Data/Documentation.xml")));` which is typically on line `37`.

          Note that I have changed the name of file. It's different from the one we save in the properties.

  3.  use `///` in your codes and comment will be auto generated that we can edit later. This will be parsed to docs.

- For custom routes, use these-

  ```csharp
  [Route("full url")]
  [HttpGet] // or anyother protocol ;)
  ```

- We can pass query parameters in the `GET` like `api/People/TestingGet/{name}/{age:int}`. The variables in the curly braces are passed as parameter. **Note that we do not require to write type `string` explicitely like any other variables.**

  We access them in the function like this-

  ```csharp
  public ret_type function_name(string name, int age){}
  ```
