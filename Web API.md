# Web API

- In computer programming, an application programming interface (API) is a set of subroutine definitions, protocols, and tools for building software and applications.

  Web API as the name suggests, is an API over the web which can be accessed using HTTP protocol. It is a concept and not a technology.

- Creating a project has the following steps-

  1.  ASP.Net Core Web Application.
  2.  Web Application (Model-View-Controller).

- Create controllers in the `Controllers` folder. Add `Web API 2 Controller with read/write actions`. The `GET`, `POST` etc. will be genrated. We can also add more routes.

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

- The name of a controller class **must** end with `Controller` and it must be derived from `System.Web.Http.ApiController` class.

- All the public methods of the controller are called action methods.

- Action method name can be the same as HTTP verb name or it can start with HTTP verb with any suffix (case in-sensitive) or we can apply Http verb attributes to method.

- Return type of an action method can be any primitive or complex type. Learn more about it.

- Web API supports code based configuration. It cannot be configured in web.config file. We use the `WebAPIConfig.cs` to configure it. `Global.asax.cs` also has some configs.

- Web API configuration process starts when the application starts. It calls GlobalConfiguration.Configure(WebApiConfig.Register) in the Application_Start method. The Configure() method requires the callback method where Web API has been configured in code. By default this is the static WebApiConfig.Register() method.

- Web API supports two types of routing:

  1.  Convention-based Routing
  2.  Attribute Routing

- WebAPIConfig.cs -

  ```csharp
  public static class WebApiConfig
  {
  		public static void Register(HttpConfiguration config)
  		{
  				// Enable attribute routing
  				config.MapHttpAttributeRoutes();

  				// Add default route using convention-based routing
  				config.Routes.MapHttpRoute(
  						name: "DefaultApi",
  						routeTemplate: "api/{controller}/{id}",
  						defaults: new { id = RouteParameter.Optional }
  				);
  		}
  }
  ```

  We can add custom route mapping too.

  ```csharp
  config.Routes.MapHttpRoute(
            name: "School",
            routeTemplate: "api/myschool/{id}",
            defaults: new { controller="school", id = RouteParameter.Optional }
            constraints: new { id ="/d+" }
        );
  ```

- Web API binds action method parameters either with URL's query string or with request body depending on the parameter type. By default, if parameter type is of .NET primitive type such as int, bool, double, string, GUID, DateTime, decimal or any other type that can be converted from string type then it sets the value of a parameter from the query string. And if the parameter type is complex type then Web API tries to get the value from request body by default.

- Query string parameter names must match with the name of an action method parameter. However, they can be in different order.

- In a `POST` method, by default the primitives are taken from the query parameters and the complex type variables from the request body.

  **We can change this defaault behavior using FromUri and FromBody**

- FromUri -

  ```csharp
  public class StudentController : ApiController
  {
  		public Student Post([FromUri]Student stud)
  		{

  		}
  }
  ```

  The query parameter names must be equal the property name

- FromBody -

  ```csharp
  public class StudentController : ApiController
  {
  		public Student Post([FromBody]string name)
  		{

  		}
  }
  ```

  **FromBody attribute can be applied on only one primitive parameter of an action method. It cannot be applied on multiple primitive parameters of the same action method.**

- The Web API action method can have following return types.

  1.  Void - sends `204` or `no content` status.
  2.  Primitive type or Complex type
  3.  HttpResponseMessage
  4.  IHttpActionResul

- Creating custom results,

  ```csharp
  public class TextResult : IHttpActionResult
  {
  		string _value;
  		HttpRequestMessage _request;

  		public TextResult(string value, HttpRequestMessage request)
  		{
  				_value = value;
  				_request = request;
  		}

  		public Task<HttpResponseMessage> ExecuteAsync(CancellationToken cancellationToken)
  		{
  				var response = new HttpResponseMessage()
  				{
  						Content = new StringContent(_value),
  						RequestMessage = _request
  				};
  				return Task.FromResult(response);
  		}
  }
  ```

- In HTTP request, MIME type is specified in the request header using Accept and Content-Type attribute.

- Web API includes filters to add extra logic before or after action method executes. Filters can be used to provide cross-cutting features such as logging, exception handling, performance measurement, authentication and authorization.

  Every filter attribute class must implement IFilter interface included in `System.Web.Http.Filters` namespace.
