# Documentation For CRX Extension
This page gives a Brief about the steps and explanation of code involved in development of the extension
## Contents
  1. [Start Up](#start-up)
  2. [Manifest.json](#manifestjson)
  3. [Angular.json](#angularjson)
  4. [Content Scripts](#content-scripts)
  5. [Assests](#assets)
  6. [inject.js](#injectjs)
  ### Extras
  + [Angular Application Structure](#angular-application-structure)
<hr>

## Start Up
  + Extension gives the page extra functionality. Every extension can be run as an independent application and can be developed with different technologies.
  + An extension can be a plain html page or an Angular application 
  + So every FrontEnd application built which is standalone can be an extension
  + An extension mainly requires a file which tells the browser about the extension , what does it need and what it does. It is the [Manifest.json](#manifestjson) file.
<hr>

## Manifest.json
  + Manifest file of the extension is a JSON file which has the details of the extension such as name,version,description,files,action...
  + An Example of Manifest File is

  ```
  {
    "manifest_version": 2,
    "name": "Extension Name",
    "description": "Description goes here...",
    "version": "1.0",
    "browser_action": {
      "default_icon": "url of icon",
      "default_popup": "name of the page"
    },
    "icons": {
      "16": "assets/icon.png",
      "48": "assets/icon.png",
      "128": "assets/icon.png"
    }
  }
  ```
  + The file format and documentation of an chrome extension can be found [here](https://developer.chrome.com/extensions/manifest)
  <hr>

  ## Angular.json
  + In angular 6 this file is changed to angular.json , previously angular-cli.json(<5.x).
  + This file has complete configuration of the application that are required.
  + Also files that are needed to be included in distrubtion can also be mentioned in the JSON so that after build the files would be accessible by the application.
  + The information such as what are the scripts and sheets that are used by application are also needed to be mentioned in the file so that when the application runs the files will be automatically injected into the page.
  + An Example of Angular CLI Looks like this:
  ```
  {
    "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
    "project": {
      "name": "example-app"
    },
    "apps": [
      {
        "root": "src",
        "outDir": "dist",
        "assets": [
          "assets",
          "favicon.ico"
        ],
        "index": "index.html",
        "main": "main.ts",
        "polyfills": "polyfills.ts",
        "test": "test.ts",
        "tsconfig": "tsconfig.app.json",
        "testTsconfig": "tsconfig.spec.json",
        "prefix": "bc",
        "styles": [
          "styles.css"
        ],
        "scripts": [],
        "environmentSource": "environments/environment.ts",
        "environments": {
          "dev": "environments/environment.ts",
          "prod": "environments/environment.prod.ts"
        }
      }
    ],
    "e2e": {
      "protractor": {
        "config": "./protractor.conf.js"
      }
    },
    "lint": [
      {
        "project": "src/tsconfig.app.json"
      },
      {
        "project": "src/tsconfig.spec.json"
      },
      {
        "project": "e2e/tsconfig.e2e.json"
      }
    ],
    "test": {
      "karma": {
        "config": "./karma.conf.js"
      }
    },
    "defaults": {
      "styleExt": "css",
      "component": {}
    }
}
  ```
  <hr>

  ## Content Scripts
  + [Content scripts](https://developer.chrome.com/extensions/content_scripts) are files that run in the context of web pages. By using the standard Document Object Model (DOM), they are able to read details of the web pages the browser visits, make changes to them and pass information to their parent extension.
  + Content scripts can access Chrome APIs used by their parent extension by exchanging messages and access information by making cross-site XMLHttpRequests to parent sites. They can also access the URL of an extension's file with `chrome.runtime.getURL()` and use the result the same as other URLs.
  + Content Scripts have 3 fields :

  Name | Type | Description
  --- | ----- | ------
   matches |	array of strings (Required)|	 Specifies which pages this    content script will be injected into. See Match Patterns for more details on the syntax of these strings and Match patterns and globs for information on how to exclude URLs
   css	| array of strings (Optional) | The list of CSS files to be     injected into matching pages. These are injected  in the order they appear in this array, before   any DOM is constructed or displayed for the page.
   js |	array of strings Optional. | The list of JavaScript files to be injected into matching pages. These are injected in the order they appear in this array.

   + [URL Patterns](https://developer.chrome.com/apps/match_patterns) tell to which page the content scripts are to be injected. If the url pattern is matched then only the content scripts would be injected.
   + CSS field is about which style sheets are to be injected into page
   + JS field is about what scripts are to injected into the page

  ### Explanation for the content script in the CRX Extension

  ```
    "content_scripts": [{
      "matches": [
        "<all_urls>"
      ],
      "css": [
        "assets/inject/inject.css"
      ]
    },
    {
      "matches": [
        "<all_urls>"
      ],
      "js": [
        "jquery.min.js",
        "assets/inject/inject.js"
      ]
    }
  ]

  ```
  + In this extension the matches feild is pointed to all URL's i.e the content scripts would be injected to all pages
  + The stylesheet injected is [inject.css](#injectcss) which would be present in the dist folder which is added as [asset](#assets) 
  + There are two scripts injected in this extension the same way as css by adding as an [asset](#assets)
    - jQuery (jquery.min.css)
    - [inject.js](#injectjs)
  <hr>

## Assets
  + Assets are the files which are addedor mentioned in [angular.json](#angularjson) so that those files would be included in dist folder after the build
  + Dependecies of application or resource files can be added as assets
  + The assets of the extension are
  ```
  "assets": [
   "src/favicon.ico",
   "src/assets",
   "src/manifest.json",
   "src/jquery.min.js"
  ]
  ```
  + Folders can be added so that all the files of the folder would be added as assets
  + <b>But the assets which are to be added should be from the source folder only</b>
  <hr>

## Inject.js
  + The inject.js file in the extension help to inject the angular application into the webpage 
  ```
  chrome.extension.sendMessage({}, function (response) {
  var readyStateCheckInterval = setInterval(function () {
    if (document.readyState === "complete") {
      clearInterval(readyStateCheckInterval);
      console.log("Hello. This message was sent from scripts/inject.js");
      $('<base href="/">').appendTo('head');
      $.get(chrome.extension.getURL("index.html"), function (data) {
        $(data).appendTo('body');   
        $('<script type="text/javascript" src="' + chrome.extension.getUR("runtime.js") + '"></script>').appendTo("body");
        $('<script type="text/javascript" src="' + chrome.extension.getURL("polyfills.js") + '"></script>').appendTo("body");
        $('<script type="text/javascript" src="' + chrome.extension.getURL("styles.js") + '"></script>').appendTo("body");
        $('<script type="text/javascript" src="' + chrome.extension.getURL("vendor.js") + '"></script>').appendTo("body");
        $('<script type="text/javascript" src="' + chrome.extension.getURL("main.js") + '"></script>').appendTo("body");
      });
    }
  }, 10);
});

  ```
  + All the scripts which contains the Script logic after the build will be included by use `chrome.extension.getURL()`
  + First the `index.html` will be obtained then the DOM would be appended to the webpage through `$(data).appendTo('body')`
  + So the functional application would be injected into the webpage 
<hr>

## Angular Application Structure

```
├── README.md
├── e2e
│   ├── app.e2e-spec.ts
│   ├── app.po.ts
│   └── tsconfig.e2e.json
├── karma.conf.js
├── package.json
├── protractor.conf.js
├── src
│   ├── app
│   │   ├── app.component.css
│   │   ├── app.component.html
│   │   ├── app.component.spec.ts
│   │   ├── app.component.ts
│   │   └── app.module.ts
│   ├── assets
│   ├── environments
│   │   ├── environment.prod.ts
│   │   └── environment.ts
│   ├── favicon.ico
│   ├── index.html
│   ├── main.ts
│   ├── polyfills.ts
│   ├── styles.css
│   ├── test.ts
│   ├── tsconfig.app.json
│   ├── tsconfig.spec.json
│   └── typings.d.ts
├── tsconfig.json
└── tslint.json
```
