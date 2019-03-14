---
title: "Life Cycles in ASP.NET MVC"
categories:
  - Programming
tags:
  - MVC
  - ASP.NET
---
# States in MVC
1. [Application State](#application-state)
2. [Session State](#session-state)
3. [Request](#request)

## Application State

+ Application state is one of the ways available to store some data on the server and faster than accessing the database.
+ The data stored on the Application state is available to all the users(Sessions) accessing the application. 
+ So application state is very useful to store small data that is used across the application and same for all the users. 
+ Also we can say they are global variables available across the users.
+ As I am saying small data, we should not store heavy data in ApplicationState because it is stored on the server and can cause performance overhead if it is heavy.

  Example showing that Application Message is stored:-
  ```
  void Application_Start(object sender, EventArgs e)
  { 
  Application["Message"] = "Welcome to my Website"; 
  }
  ```
## Session State

+ Session state, in the context of .NET, is a method keep track of the a user session during a series of HTTP requests. 
+ Session state allows a developer to store data about a user as he/she navigates through ASP.NET web pages in a .NET web application.
  ### What is SessionState
  -  Session State is the attribute of controller class which is used to control or manage the default session behavior.
  -  To use session state attribute we need to use System.Web.SessionState namespace.
  -  The following are the behavior of the Session State attribute 
        + Default
        + Disabled
        + Required
        + ReadOnly
   - Example of Default State
      ```
        [SessionState(SessionStateBehavior.Default)]    
        public class HomeController : Controller    
        {    
            // Action methods     
        
        } 
       ```
 ## Request
 + It is the sequence of events that happen every time an HTTP request is handled by our application.
 + The entry point for every MVC application begins with routing. After the ASP.NET platform has received a request, it figures out how it should be handled through the URL Routing Module.
 + Modules are .NET components that can hook into the application life cycle and add functionality. The routing module is responsible for matching the incoming URL to routes that we define in our application.
 + All routes have an associated route handler with them and this is the entry point to the MVC framework.
