![Asp.net Core Mvc Controls Toolkit tag](https://raw.githubusercontent.com/MvcControlsToolkit/Home/master/MvcControlsToolkitCore.PNG)

# Asp.net Core Mvc Controls Toolkit Home
This is the home repository for the Asp.net Core version of the [Mvc Controls 
Toolkit](http://mvccontrolstoolkit.codeplex.com/). **The first controls suite completely based on TagHelpers!** Here you will find all features, some examples, and the future Roadmap. 
[Here](https://github.com/MvcControlsToolkit) you may find the list of all repositories included in the Mvc Controls Toolkit project. 

## Current version: 1.1.5 RTM
This version is compatible with Asp.net Core 1.1.0 Mvc

See [installation instructions](http://documentation.aspnetcore.mvc-controls.com/QuickStart/Installation)

See [live examples](http://examples.aspnetcore.mvc-controls.com/).

See [full documentation](http://documentation.aspnetcore.mvc-controls.com/)

See [versions history](http://documentation.aspnetcore.mvc-controls.com/Home/ReleasesHistory).

Download [example project](https://github.com/MvcControlsToolkit/Home/releases/download/1.1.5/ControlsExamples.zip) 
and follow instructions in INSTRUCTIONS.txt

Installation instructions are updated to the last 1.1.5 release.
 
## Roadmap
    
### Changes in the next to come 1.2 version

1. OData 4.0 compatible Filtering, sorting, and grouping capabilities you may apply easily to grids or to your custom pages.
2. When DateTimeOffset is used instead of DateTime for date+time editing, automatic conversion to the browser Time zone is performed. On the server side model binder "catches" date, time and client time zone offset in the DateTimeOffset.
3. Added a not  trivial default "updating" behavior (current default behavior simply does nothing). Namely, during ajax updates buttons that might interfere with the ongoing operation are automatically disabled, and the updating part of the screen changes its opacity to give a vsual feedback of the update process. As for previous versions default behavior may be changed.
4. Added antiforgery protection to ServerCrudController Delete
5. When automatically copying ViewModel/DTO Month and Week properties from/to DB Models with CRUDRepository methods, automatic conversion is performed.
6. DefaultCRUDRepository, and Projection operator will tranform models into viewmodles also in nested collection with no need to copy all properties: writing Select(m => new MyNestedViewModel{}) will suffice.
7. Added support for interfaces in both business and asp.net core tools. Business tools may return and process interfaces instead of ViewModels, and model binder will bind and validate interfaces.
8. Improved Detail Form default Edit and display templates by adding bootstrap clearfix to all ViewPort line boundaries. This way, the form layout will not break also when using custom column templates with higher heights. 

### Other features to come:

* json file with specs of classes to be compiled from C# to TypeScript. Some C# data annotations will be translated into MetaData JavaScript objects, so client side frameworks like angular or knockout may use the same C# classes, with the same validation rules.  Compilation may come in different "flavors" in such a way to adapt the code to various client frameworks.
* TagHelpers providers for all most common client side frameworks.
* TypeScript version of IQueryables, DContexts, and DbSets, with changes tracking cababilities and with the possibility 
to synchronize entities with various data sources (both local, and remote). 
* Several more complex controls, like TreeViews, enahnced with native Drag/Drop capabilities (or polyfills). 
* Advanced interaction protocols and widgets based on native drag and drop. 
     
    
    
  
    
    
   
     
