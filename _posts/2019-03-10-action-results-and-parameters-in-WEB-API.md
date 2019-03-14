---
title: "Action Results and Parameters in WEB API (ASP.NET)"
categories: 
    - Programming
tags:
    - CSharp
    - ASP.NET
    - MVC
---

# Action Result

| Type | Helper Method() |
|-------------------|---------------------------------------------|
|ViewResult | View() |
|PartialResult | PartialView() |
|ContentResult | Content() |
|RedirectResult | Redirect() |
| RedirectToRouteResult | RedirectToAction() |
|JsonResult | Json() |
|FileResult | File() |
|HttpNotFoundResult | HttpNotFound() |
|EmptyResult | - |

# Action Parameters
+ Action parameters are sent through url
+ They are Embedded in the URL
+ Eg: /Home/GetValue/1
  - Home is the controller
  - GetValue is the Action in Controller
  - 1 is the id Passed
