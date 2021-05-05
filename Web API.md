# Web API

- Documentation is auto generated. We just have to edit some config and write descriptions for the doc.

  1.  `solution manager` -> double click on `properties` -> `build` -> scroll down and find `output path` -> check the `xml documentation file` -> change the folder name to save the doc e.g: `~App_Data/Documentation.xml` -> hit save.
  2.  `solution manager` -> `Areas` -> `Help Page` -> `App_Start` -> `HelpPageConfig.cs` -> uncomment the line
      `config.SetDocumentationProvider(new XmlDocumentationProvider(HttpContext.Current.Server.MapPath("~/App_Data/Documentation.xml")));` which is typically on line `37`.

          Note that I have changed the name of file. It's different from the one we save in the properties.

  3.  use `///` in your codes and comment will be auto generated that we can edit later. This will be parsed to docs.
