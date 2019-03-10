# Routing in MVC
+ ASP.NET MVC routing is a pattern matching system that is responsible for mapping incoming browser requests to specified MVC controller actions. 
+ When the ASP.NET MVC application launches then the application registers one or more patterns with the framework's route table to tell the routing engine what to do with any requests that matches those patterns. 
+ When the routing engine receives a request at runtime, it matches that request's URL against the URL patterns registered with it and gives the response according to a pattern match.<space><space>
![Routing Image](http://csharpcorner.mindcrackerinc.netdna-cdn.com/UploadFile/3d39b4/routing-in-mvc/Images/MVC.jpg)

## Properties of Route

+ ASP.NET MVC routes are responsible for determining which controller method to execute for a given URL. 
+ A URL consists of the following properties:
    - ***Route Name***: A route is a URL pattern that is mapped to a handler. A handler can be a controller in the MVC application that processes the request. A route name may be used as a specific reference to a given route.
    - ***URL Pattern***: A URL pattern can contain literal values and variable placeholders (referred to as URL parameters). The literals and placeholders are located in segments of the URL that are delimited by the slash (/) character.
+ When a request is made, the URL is parsed into segments and placeholders, and the variable values are provided to the request handler. 
+ This process is similar to the way the data in query strings is parsed and passed to the request handler. 
+ In both cases variable information is included in the URL and passed to the handler in the form of key-value pairs. 
+ For query strings both the keys and the values are in the URL. 
+ For routes, the keys are the placeholder names defined in the URL pattern, and only the values are in the URL.
    - ***Defaults***: When you define a route, you can assign a default value for a parameter. The defaults is an object that contains default route values.
    - ***Constraints***: A set of constraints to apply against the URL pattern to more narrowly define the URL that it matches.
## Understand the Default Route
+ The default ASP.NET MVC project templates add a generic route that uses the following URL convention to break the URL for a given request into three named segments.
```
url: "{controller}/{action}/{id}"
```