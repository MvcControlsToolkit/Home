# Asp.net Core Mvc Controls Toolkit Home
This is the home repository for the Asp.net Core version of the [Mvc Controls 
Toolkit](http://mvccontrolstoolkit.codeplex.com/). Here you will find all features, some examples, and the future Roadmap.

## First public release 
The first public release will be Asp.net Core Rc2 compatible, and will be available in a few days: we are moving rc1 bits 
to rc2 and we are preparing examples and live demos. 
This release will contain the following features:


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
                  
3. MvcControlsToolkit.Core, provides some enhancements of the standared Mvc engine.Namely:
    * Developer may specify a resoucre file containing custom default messages for all Validation attributes:
    ```C#
     services.AddMvcControlsToolkit(m => { m.CustomMessagesResourceType = typeof(Resources.ErrorMessages); }); 
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
   In this resource file use may specify also messages for type wrong format (see next points). Keys are:
    ```
    ClientFieldMustBeDate, ClientFieldMustBeDateTime, ClientFieldMustBeInteger, ClientFieldMustBeMonth, 
    ClientFieldMustBeNumber, ClientFieldMustBePositiveInteger, ClientFieldMustBeTime, ClientFieldMustBeWeek, ColorAttribute,
    EmailAddressAttribute, UrlAttribute
    ``` 
    Pls notice that the DynamicRangeAttribute admits several message templates separated by "|". First one is selected when both static minimum and maximum
   are provided, second one when only max is provided, and third when only min is provided. Fourth message is used for dynamic limits violations
   * input tag helpers automatically render the right Html5 input corresponding to the .net type, and to data annotations. On the client 
   side browser support for Html5 inputs is detected, and automatic fallback with bootstrap widgets is provided thanks to the [mvcct-enhancer](https://github.com/MvcControlsToolkit/mvcct-enhancer)
   and to the [bootstrap-html5-fallback](https://github.com/MvcControlsToolkit/bootstrap-html5-fallback). bootstrap-html5-fallback may be replaced with another 
   mvcct-enhancer module that uses different widgets.Mapping between .Net types+data attributes are as follows: Week and Month are rendered with "week" and "month" inputs, 
   all numeric types are rendered with "number", and with "range" iif DataType annotation specifies "Range" data type.
   Datetimes are rendered with "datetime-local", and with "date" if DataType attribute specifies "Date". "time" input is chosen if
   property is a TimeSpan and if it is decorated with the "Time" DataTipe annotation. Strings are rendered with "color" 
   if they decorated with a "Color" DataType.   Input type "password", "search", "url", "email" are chosen iff the 
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
    
    Second release should come together with Asp.net core RTM at the end of june,
    and should include a templated server grid with both immediate and batch updates. Filtering/ordering will be based on OData 
    url syntax with the options to use "simple syntax" (prop1=val1&prop2=val2...) for filters. 
    
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
     
    
    
  
    
    
   
     