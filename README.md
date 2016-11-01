# Asp.net Core Mvc Controls Toolkit Home
This is the home repository for the Asp.net Core version of the [Mvc Controls 
Toolkit](http://mvccontrolstoolkit.codeplex.com/). Here you will find all features, some examples, and the future Roadmap. 
[Here](https://github.com/MvcControlsToolkit) you may find the list of all repositories included in the Mvc Controls Toolkit project. 

## Current version: 1.1.4 RTM
This version is compatible with Asp.net Core 1.0.1 Mvc
See [versions history](https://github.com/MvcControlsToolkit/MvcControlsToolkit.Core/wiki/Versions-history).

See [live examples](http://examples.aspnetcore.mvc-controls.com/).

Installation instructions are updated to the last 1.1.4 release.

Documentation for the newly 1.1.4 features will be provided in a short time in a dedicated documentation web site.

## Announcements
2016-11-01 - The new 1.1.4 version is ready, try: EDITABLE GRIDS, PAGERS, DETAIL VIEWS AND AUTOCOMPLETE! GRID, CONTROLLERS, AND RELATED DB REPOSITORIES MAY BE CREATED WITH MINIMAL CODE, RELYING ON DEFAUL TEMPLATES, NAME CONVENTIONS AND EASY OVERRIDES.

## Installation
1. Create an Ap.net core Mvc 1.0.1 project and install the `MvcControlsToolkit.ControlsCore` **Nuget** package 1.1.4.
2. Install the following Bower packages: `jquery-validation-unobtrusive-extensions`: >= 1.0.2 to handle 
    multilanguage enhanced client validation, `bootstrap-html5-fallback` >= 1.0.2 to have bootstrap widget 
    fallback of all Html5 inputs, and finally  `mvcct-controls` >= 1.0.2 to have the needed client side JavaScript support for all advanced Mvc Controls Toolkit controls.
3. Install the following **npm** packages in the same order as given below. **Important:** As a default Asp.net Core 
    1.0.1 Mvc project template do not contain a "package.json" file, so no npm folder appears next to the Bower folder.
    In order to create one, please right click on the project root folder and select "add new item" then select "package.json".
    1. `cldr-data` >= 29.0.1. It will install the culture database used by the globalization system. Installation 
    might last a few minutes. Pls, don't worry this big cultures database is not be deployed when the web application is published. 
    Neither it is added to source control if you add "node_modules" to the "gitignore" file. 
    2. `globalize` >= 1.1.1. It installs all modules needed by the globalize library.
    3. `mvcct-templates` >= 1.1.2. This is an Mvc Controls toolkit specific package that will scaffold 
    some file in your project.Installation might last a few minutes. After installation 
    Mvc Controls Toolkit tag helpers, and namespaces will be added to the _ViewImports.cshtml view, 
    new task will be added to your gulpfile.js (within two files under a "tasks" folder), 
    an [mvcct-enhancer](https://github.com/MvcControlsToolkit/mvcct-enhancer) options and startup file will be added in `wwwroot/startupjs/startup.js` (see below),
    and some utility PartialViews will be placed in the Shared folder to handle Globalization, Validation, and Html5 fallback scripts,
4. If not already there a gulpfile.js file appears in the project root. Right click on the project gulpfile.js in order to show the task runner, then run the following tasks:
    1. Open `tasks/globalize.tasks.js`, and verify if the selected languages/cultures cover your needs, 
       otherwise add more. Then run the `move:gdata` task that will copy the selected languages data under `wwwroot/globalize`.
    2. Verify if the globalize modules mentioned in the same file cover your needs, otherwise add more, 
    and then run the `min:globalize` that minimze the selected modules and plaze the in the unique `wwwroot/globalize/globalize.min`
    file.
    3. Open `wwwroot/startupjs/startup.js`. This file contains the overall options module that should be used by all 
     Javascrit modules handled by the `mvcct-enhancer` module. Here you may force the usage of a widget fallback for any html5 input type also 
     if browser supports it. It is enough to uncomment the relative section of otpion object and to set `force: true`. 
     Otherwise bootstrap widget fallback will be used just in case browser doesn't support the associated html5 input.
     In this file you may also change the widget options (for more information see the [bootstrap-html5-fallback documentation](https://github.com/MvcControlsToolkit/bootstrap-html5-fallback)).
     The mvcct-enahence module makes available both client validation and html5 input fallback also on ajax received or 
     dynamically created content. For more information see the [mvcct-enhancer documentation](https://github.com/MvcControlsToolkit/mvcct-enhancer). 

     Once you have finished with options, minimize the `startup.js` file by invoking the `min:startup gulp` task.
5. Add the following code in the css area of you Layout page:
    ```C#
    @{ await Html.RenderPartialAsync("_FallbackCss"); }
    ```
    and the following code at the end of the body:
    ```
    @{ await Html.RenderPartialAsync("_GlobalizationScriptsPartial"); }
    @{ await Html.RenderPartialAsync("_EnhancementFallbackPartial"); }
    @RenderSection("scripts", required: false)
    <script src="~/startupjs/startup.min.js"></script>
    ```
    Validation script may be added to a page by adding:
    ```
    @{ await Html.RenderPartialAsync("_ValidationScriptsPartial"); }
    ```

    in the srcipt area of the View:
    ```
    @section Scripts {
        @{ await Html.RenderPartialAsync("_ValidationScriptsPartial"); }
    }
    ```
    Anyway an example layout page with the above code inserted in the right points is included in the Shared folder.
6. In the Startup.cs file,  in the ConfigureServices methos, just after the instruction: `services.AddMvc();`, place:
    ```C#
    services.AddMvcControlsToolkitControls();
    ```
    or 

    ```C#
    services.AddMvcControlsToolkitControls(m => { m.CustomMessagesResourceType = typeof(ErrorMessages); });
    ```
    If you want to provide a resorce file containing custom messages for all 
    validation attributes (see point 3) in the documentation section for more details

    Then in the Configure method just before the `app.UseMvc(routes =>......` instruction, place:
    ```C#
    app.UseMvcControlsToolkit();
    ```
    

DONE! you have finished! 

Please don't forget to configure [application culture](https://docs.asp.net/en/latest/fundamentals/localization.html) coherently with the languages specified in `tasks/globalize.tasks.js`. 
Otherwise if the request set a current that is not among the one specified there Javascript code will break!

I strongly suggest  to test all above steps in the [example provided with the distribution](https://github.com/MvcControlsToolkit/Home/releases/download/1.0.0/Example.zip). 
Plese notice that the example contains migrations, so you need to go in the web project folder, in the package manager consolle,
and to run the command `dotnet ef database update`.

## Example
Download the [example provided with the distribution](https://github.com/MvcControlsToolkit/Home/releases/download/1.0.0/Example.zip), 
and apply all steps described above (you don't need to change stratup.js options, globalize modules, or supported languages). 
You don't have to apply step 6 since Startup.cs class is already configured. Also culture settings have been already 
put in place properly.
After that, since the project already contains migrations you need to go in the web project folder, in the package manager consolle,
and to run the command `dotnet ef database update` that updates/creates the database. Connection string in `appsettings.json` should work, 
but in case of problems consider changing it.

When the application starts the Welcome message is an example of how the Options/Preferences framework works. 
You may change it by providing the following parameters in the query string:
* 'Welcome.Message="your messages"
* 'Welcome.AddDate="True if you want date be added to the message, otherwise False>" 

Once captured by the Options request provider the above information is stored by all 
other providers that are configured to store data. In this case by the cookies provider, and by the claims provider.
The first one works when you are not logged, otherwise its behavior is overriden by the claims provider that stores the info 
as a user claim in the database. Fom more inforrmation about the Options/Preferences provider please refer to point 1) 
in the documentation below.

The `GlobalizationTest` menu item goes to a page showing how the MvcControlsToolkit automatically choose the right Html5 input
for each data type, automatic fallback to bootstrap widgets is provided in case browser doesn't support that input type.
Moreover, you may force the usage of bootstrap widgets by acting on the options object in the `wwwroot/startupjs/startup.js` 
file as explained in the installation guide.

Validation range attributes are applied to all date/numeric inputs. `min` and `max` of the Html5 input are automatic set 
using this validation infos, so for instance dates outside the allowed range in date pickers are not available at all.

The `DynamicRangeTest` page shows how the `DynamicRangeAttribute` forcse an input to be within the limits specified by other inputs.
In this example the attribute has been set to "Propagate" i.e. to automatically correct (if possible) out-of-the-allowed-range values.
Each time either the input or its bounds are changed a test is done, and if it fails a correction is attempted on the value.

The `StoreTest` page shows how the `store-model` tag helper works by storing a whole object into an hidden fleld and the rebuilding it 
when the page is posted. You need to use the inspect browser command to see the content of the hidden field in the page, 
then post the page, and verify how the model arrives back to the controller, that injects it again in the `store-model` tag helper.

Finally, before going to the `Business DB Test` page, please put a breakpoint in the `Index` action method of the `BusinessTestController`, 
since that method has no View associated with it and has been built just with the intent of being followed in the debugger.
A table is cleared, then it is populated with new fresh data, and changes saved. Then all entities are detached 
from the context with:
```C#
foreach (var item in population) ctx.Entry(item).State = EntityState.Detached; 
```
In order to simulate a new we request. Then data are retrieved again:
```C#
var original = await ctx.TestModels.Project().To\<Models.TestViewModel\>().ToArrayAsync();
```
Data are projected in the ViewModel with  MvcControlsToolkit.Project().To<Models.TestViewModel>() thus avoiding manual copying of all fields.
Then a new copy of the ViewModels is created with the help of the MvcControlsToolkit object copier. 
One copy simulates original data stored in a `store-model` tag, and the other one simulates data modified by the user.

Insertion, Deletions, and Modifications are detected by building an MvcControlsToolkit `ChangeSet` object:
```C#
var changes = ChangeSet.Create(original, changed, m => m.Id);
```

Then modifications are passed automatically to the database by invoking the `UpdateDatabase` method.

Now data are retriebed again and a custom projection in the ViewModels is used:
```C#
.Project().To<Models.TestViewModel>(m => new Models.TestViewModel {
                FieldBC=m.FieldB+" "+m.FieldC
            })
```
`FieldBC` is built as specified, while all other properies are copied with a same name convention.
For more infos on the features described in the `Business DB Test` page please read point 1) in the documentation below.






## Basic Features and Basic Documentation 
The first  Asp.net Core Rc2 compatible public release of the tookit, is out! in a few day a full documentation web site, documentation, examples and live demos!
In the meantime the main features documentation is summarized below.
This first release contains the following features:


1. A [business module](https://github.com/MvcControlsToolkit/MvcControlsToolkit.Core/tree/master/src/MvcControlsToolkit.Core.Business) that has no reference to asp.net dlls, containing:
    * The Week and Month structures, that store week of the year and month of the year infos. They are converted and parsed from 
strings in the same ISO format used by the Html5 `week`  and `Month` input types. They have all operations a DateTime has, 
and may be converted into Datetimes by simply assigning them to a Datetime var.
    * The `DynamicRangeAttribute`. A dynamic version of the Range Attribute that accepts an overall constant max, an overall constant min and a list of dynamic mins and maxs whose values are taken
    from other model properties. Each min, max in the dynamic lists may contain a static constant that is added to the dynamic value. Below an example of usage:
    ```C#
    [DynamicRange(typeof(float), SMaximum = "20", SMinimum = "10")]
    [DynamicRange(typeof(float), DynamicMaximum = "MyMax+!1  MyOtherMax+!2", DynamicMinimum = "MyMin-!1")]
    ```
    The first instance specifies overall Maxs and Mins, while the second instance specifies 2 dynamic maxs and a dynamic min. 
    Each of them has an expression summed to it. The DynamicRangeATrribute works properly with all numeric types and with 
    all Date/Time types (Datetime, TimeSpam, Week, Month)
    
    * A `RequiredFielsAttribute`. A class level annotation that accepts a comma separated list of property names, and declares they are required.
    Each property may be also an IEnumerable, in which case it is considered invalid when it is either null or empty.
    * LinQ productivity tools, more specifically:
    
     1) a projection operator(in MvcControlsToolkit.Core.LinQ) to project LinQ queries into ViewModels without being forced to copy manually all properties. It works
    Like: 
    ```C#
    db.MyTable.Where(...)...Project().To<MyViewModel>(m => new MyViewModel{FullName = m.Name+' ' +m.Surname})
    ```
    That is, you need to specify just the exceptions from normal property copying, or just `To<MyViewModel>()` if there are no exceptions. 
    One level nested objects are accessed automatically by concatenating the main object and child object property names in the ViewModel.
    So, for instance, if I am retrieving `OrderItem` objects that contain the father order into a property named `Order` you can get the `Number` property of the Order object,
    by adding a `OrderNumber` property to your ViewModel. The projection operator is implemented efficiently and rely on 2 caches 
    to speed up LinQ expressions automatic creation.
    
    2) A `ChangeSet` object (MvcControlsToolkit.Core.Business.Utilities) to help moving changes performed in the user interface into the database. You may create a new `ChangeSet` by calling
    the static `Create` property of the `ChangeSet` class:
     ```C#
     ChangeSet<T, K> Create<T>(IEnumerable<T> oldValues, 
        IEnumerable<T> newValues, 
        Expression<Func<T, K>> keyExpression,
        bool verifyChangedProperties = true)
     ```
     Inserted, Modified, Unmodified, and Deleted objects are detected by comparing the ViewModel lists before,and after modifications. The `oldValues` list
     may be saved by adding an old values section to your View ViewModel and saving it with the help of the `<save-model/>` tag helper (see below the MvcControlsToolkit.Core dll) 
     `keyExpression` specifies the property that works as a principal key. The principal key may contain a null value in the case of newly added objects.
     If `veriFyChangedProperies` is `true` old and new values of properties are checked to verify if ViewModels that are not Deleted, nor
     Inserted are Modified or Unmodified, otherwise all that ViewModels are assumed to be Modified. IEnumerable properties possibly containing child objects,
     are not taken into account during this processing, so possible children entities must be processed either manually or by using other `ChangeSet` objects.
     Once created a `ChangeSet` object you may either handle manually the Inserted, Modified, Unmodified, 
     and Deleted lists or call its `UpdateDatabase` method.
        ```C#
        public async Task<List<M>> UpdateDatabase(DbSet<M> table, DbContext ctx, 
            Expression<Func<M, bool>> accessExpression=null,
            bool saveChanges = false, boo retrieveChanged = true)
        ```
        Where: `M` is the database class (different from the ViewModel the ChangeSet is based on). 
        `accessExpression` is an expression used to verify "access rights" to modify or delete each entity. 
        For instance, if we are modifying "task" items we need to verify that the logged user is the owner of the task with an 
        `accessExpression` like: `m => m.OwnerId == loggedUserId`. If `saveChanges` is `true` `DbContext` changes are saved 
        automatically after the update, and the possibly newly created keys of the inserted objects are copied into the 
        associated ViewModels of the Inserted list. If `retrieveChanged` is `true` (the default) the changed objects are retireved 
        from the database and changed poperties in the associated ViewModels are copied into them, otherwise new instances 
        of `M` are created, attached to the context as "changed" and Changed ViewMdels are copied into them. `retrieveChanged = false` 
        may speed up the update when ViewModels contain substantially all properies of `M`, otherwise we are forced to set `retrieveChanged = true`.
        The method returns the newly created database objects. This list is useful to get the newly created keys in case we set `saveChanges = false` so
        they cant be copied automatically in the original ViewModels.
        The `ChangeSet` object may be used also when we need to Insert, Delete, or Save a sigle object. Namely, 
        for a single insert just put a single object in the `newValues` list and set the `oldValues` list to null. 
        For a single delete just do the contrary. For a single changed object, if the old copy is available 
        put the old copy in the `oldValues` list and the modified copy in the `newValues` list, otherwise put the same object
        in both lists and set `verifyChangedProperties = false`.
         
    3) The `MvcControlsToolkit.Core.Business.Utilities.ObjectCopier<M, T>` object to copy properties 
    with the same name form an instance of `M` to an instanceof `T`. Once created an instance of `ObjectCopier<M, T>` 
    may be used in several copy operatons. It is worth saving instances of `ObjectCopier<M, T>`, 
    since during its creation it performs costly reflection operations.
2.  [An option Module](https://github.com/MvcControlsToolkit/MvcControlsToolkit.Core/tree/master/src/MvcControlsToolkit.Core.Options) that uses several providers to fill an hierarchical options dictionary. 
It has the purpose of adapting request processing to the current environment: browser capabilities, logged user, explicitely specified preferences, and overall application settings.
The option dictionary is used to fill option classes that are injected wherever Asp.net Core accepts injection. 
Dictinary entries are filled the first time they are requested, in order to avoid uneuseful processing. 
Providers may be also two-ways i.e they may detect changes in the options dictionary and update back their sources. 
Built-in providers are:
    * RequestProvider. That extracts options from url and posted forms. Each param or each form field provides a value with the same name of the parameter/form field.
    * CookieProvider. That extracts options from a cookie. Two-ways, if some other provider updates the option  the cookie is updated.
    Information is stored in the cookie in jSon format as a Key/Value pairs array: `[{Key: "myKey", Value="myValue"}....]`.
    * ClaimsProvider that extracts options from user claims. Two-ways, if some other provider updates the option claims are updated
    * EntityFrameworkProvider. It extracts options from a DBSet related with the logged user. Two-ways, if some other provider update the option database is updated.
    * ApplicationConfigurationProvider that extracts options from application configuration data.
    * RequestJsonProvider. It extracts infos  from a single form field/param in the same format used by the CookyProvider.
    
    Below an example of usage:
    ```C#
    services.AddPreferences()
                .AddPreferencesClass<WelcomeMessage>("UI.Strings.Welcome")
                .AddPreferencesProvider(new ApplicationConfigurationProvider("UI", Configuration)
                {
                    SourcePrefix = "CustomUI",
                })
                .AddPreferencesProvider(new ClaimsProvider<ApplicationUser>("UI.Strings.Welcome"){
                    Priority=2,
                    AutoCreate=true,
                    SourcePrefix= "http://www.mvc-controls.com/Welcome"
                })
                .AddPreferencesProvider(new RequestProvider("UI.Strings.Welcome")
                {
                    Priority = 10,
                    SourcePrefix = "Welcome"
                }) ;
     ```
         
   The first line declares one of the classes that may be injected. Its property are filled with the entries
   contained in the options dictionary under the path: `"UI.Strings.Welcome"`. Match is name based and is herarchical:
   nested properties are inserted into child objects that are created automatically. If a property is decorated with the 
   `OptionNameAttribute` the name specified there is used instead of the property name.
   The first provider takes all infos contained in the application configuration under  `"CustomUI"` and use them
   to fill all options dictionary entries under `"UI"`. It is the one with the worst priority (since default priority is 0).
   The second provider takes and store(AutoCreate=true) information from user claims whose claim names start with `"http://www.mvc-controls.com/Welcome"` and insert them
   in the options dictionary under `"UI.Strings.Welcome"`. From the point of view of path nesting `/` are converted into dots. It has an average priority
   (Priority=2). The last provide takesinfos from all url parameters whose names starts with `"Welcome"`
   (ie also params like Welcome.nested are included, too) and put them under `"UI.Strings.Welcome"`.
                  
3. [MvcControlsToolkit.Core](https://github.com/MvcControlsToolkit/MvcControlsToolkit.Core/tree/master/src/MvcControlsToolkit.Core), provides some enhancements of the standared Mvc engine.Namely:
    * Developer may specify a resoucre file containing custom default messages for all Validation attributes:
    ```C#
     services.AddMvcControlsToolkit(m => { m.CustomMessagesResourceType = typeof(ErrorMessages); }); 
     ```
     ```
     <data name="DynamicRangeAttribute" xml:space="preserve">
    <value>{0} must be between {1} and {2}|{0} must be greater than {1}|{0} must be less than {2}|{0} is not in the required range</value>
  </data>
   <data name="Mail" xml:space="preserve">
    <value>Wrong EMail format</value>
   </data>
   <data name="RangeAttribute" xml:space="preserve">
    <value>value is not in the required range</value>
   </data>
   ```
   This resoutce file must be placed in the root of the web application, in the root namespace that will 
   have the same name of the WebSite dll. Otherwise it will not be found.
   In this resource file use may specify also messages for type wrong format (see next points). Keys are:
    
    ClientFieldMustBeDate, ClientFieldMustBeDateTime, ClientFieldMustBeInteger, ClientFieldMustBeMonth, 
    ClientFieldMustBeNumber, ClientFieldMustBePositiveInteger, ClientFieldMustBeTime, 
    ClientFieldMustBeWeek, ColorAttribute,
    EmailAddressAttribute, UrlAttribute
    
    Pls notice that the DynamicRangeAttribute admits several message templates separated by "|". First one is selected when both static minimum and maximum
   are provided, second one when only max is provided, and third when only min is provided. Fourth message is used for dynamic limits violations
   * input tag helpers automatically render the right Html5 input corresponding to the .net type, and to data annotations. On the client 
   side browser support for Html5 inputs is detected, and automatic fallback with bootstrap widgets is provided thanks to the [mvcct-enhancer](https://github.com/MvcControlsToolkit/mvcct-enhancer)
   and to the [bootstrap-html5-fallback](https://github.com/MvcControlsToolkit/bootstrap-html5-fallback). bootstrap-html5-fallback may be replaced with another 
   mvcct-enhancer module that uses different widgets.Mapping between .Net types+data attributes are as follows: Week and Month are rendered with "week" and "month" inputs, 
   all numeric types are rendered with "number", and with "range" iif DataType annotation specifies "Range" data type.
   Datetimes are rendered with "datetime-local", and with "date" if DataType attribute specifies "Date". "time" input is chosen if
   property is a TimeSpan and if it is decorated with the "Time" DataTipe annotation. Strings are rendered with "color" 
   if they decorated with a "Color" DataType.   Input type "password", "search", "url", "email", "tel" are chosen iff the 
   relative DataTypes are specified.
   * Validation attributes for Emails, and Urls are automatically created when a this DataTypes are used to decorate a property.
   * Client Validation logics has been enhanced to include the newly defined validation attributes and to provide support for 
   not-english locales, thanks to the [javascript unobtrusive validation extensions](https://github.com/MvcControlsToolkit/Unobtrusive.Extensions).
    Not english culture handling is needed just in case of falled back Html5 inputs since native Html5 inputs uses an international format.
   When using the `DynamicRangeAttribute with the `Propagate =true` client side validation rules before signalling the error attempt to correct the values so that it fits in the allowed range.
   Moreover, client side validation of all simple data types is performed: integer, floats, and positive integers of any size; Dates, Weeks, Months, Detetimes, Times, and Colors.
   * Model binder is able to obtain information on the browser Html5 input support through the client side [mvcct-enhancer](https://github.com/MvcControlsToolkit/mvcct-enhancer) 
   that sends to the server this information,and through the option module that adds it to the options dictionary. 
   This way it may select the right culture to parse numbers and dates received from the client: 
   international for true Html5 inputs,and current culture fo falled back inputs
   * `<store-model/>` tag helper to save a whole object in a Json or encripted format into an hidden filed. The saved model is 
   automatically restored by the model binder as if it were a simple property (int, string, etc.). The model to save is specified with the `asp-for` 
   property. If the `encrypted` poperty is `true` the model is saved with an encrypted format. This helper is useful to store old unmodified 
   values in order to perform changes tracking (see  `MvcControlsToolkit.Core.Business` dll above)
   * In line transformations may be registred and invoked during View rendering. When this is done the "reistration id" of the 
   transformation is added to all input names, so that during model binding the model binder may apply the "second half of the transformation".
   In line transformations adds general transformations to the rendering/model binding pipeline that adapt 
   data received by the client to data needed by a controller. For instance, if we have a list of items that we edit 
   in our page, we might need directly the list of changes on the receiving controller. That is 3 lists: newly inserted items, deleted items, 
   and modifed items. In this case we may define a two stage trandformation as follows: 
        *  "First part". This is performed when the transformation is invoked in the view. The list is duplicated. The first copy is 
    rendered in json format into an hidden input, to store old values, and the other is used to render an insert/delete/modify grid.
        * "Second part" this is applied by the model binder when it receives the couple of lists. By comparing primary keys and proprety values
    of the items in the two lists, it is possible to figure out which entities were inserted (the ones with no principal key)
    which entities were deleted (the one that are in the first list but not in the second) and which entities 
    changed(the ones in both lists whose property values changed).
    Transformations are implementations of the following interface:
   
        ```C#
        namespace MvcControlsToolkit.Core.Views
            {
                public interface IBindingTransformation
                {

                }
                public interface IBindingTransformation<S, I, D>: IBindingTransformation
                {
                    I Transform(S x);
                    D InverseTransform(I x);
                }
            }
         ```
            
    Transform is the first transformation and InverseTransform the second one.They are registred with the call `TransformationsRegister.Add(Type x)`
    that accepts the transformation type as its argument, and are renderend in various ways in the views. 
    We will give more details when we will propose actual controls relying on specific transformations.
    
## Roadmap
    
Next version will include a templated server ajax-grid with both immediate updates,and paging. Filtering/ordering will be based on OData 
url syntax with the options to use "simple syntax" (prop1=val1&prop2=val2...) for filters, and will be available after the fist Asp.net core 
1.0.0. compatible version of the OData packages.
    
Other features to come:

* json file with specs of classes to be compiled from C# to TypeScript. Some C# data annotations will be translated into TypeScript MetaData annotations(in particular validation attributes) 
so client side frameworks like angular or knockout may use the same C# classes, with the same validation rules.  Compilation may come in different "flavors"
in such a way to adapt the code to various client frameworks.
* TagHelpers translation providers that translate some "building blocks" TagHelpers to adapt them to 
various scenarios (client controls based on angular, client controls based on knockout.js, server controls). 
This way the same view may be used with different techniques
* TypeScript version of IQueryables, DContexts, and DbSets, with changes tracking cababilities and with the possibility 
to synchronize entities with various data sources (both local, and remote). Some of these feature will be available 
only on the commercial version.
* Several complex controls, like Grids, TreeViews, enahnced with native Drag/Drop capabilities (or polyfills). 
 Some features will be available just on the commercial version.
* Advanced interaction protocols and widgets based on native drag and drop (commercial version only). 
     
    
    
  
    
    
   
     
