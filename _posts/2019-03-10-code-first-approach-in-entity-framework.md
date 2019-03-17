---
title: "Code First Approach in Entity Framework"
categories:
  - Programming
tags:
  - CSharp
  - ASP.NET
  - MVC
---

# Steps in Code First Approach For Entity Framework

- Step:1 [Install Entity Frame Work](#install-entity-framework)
- Step:2 [Add A Context To The Project](#adding-a-context-to-project)
- Step:3 [Enable Migrations](#enable-migrations)
- Step:4 [Add Migrations](#add-migrations)
- Step:5 [Adding Tables](#adding-tables)
- Step:6 [Create DbContext instance in Controllers](#creating-dbcontext-in-a-class)
- Step:7 [Operations Using Context](#operations-using-the-context)

## Install Entity Framework

- Approach through NUGET Manager UI - Search For EntityFramework and EntityFrameWorkCore and Install - Search for EntityFramework.SqlServerCompact - Install packages
- Approach through NUGET Manager Console - For installing EntityFramework and EntityFrameWorkCore
  `Install-Package EntityFramework -Version 6.2.0 Install-Package Microsoft.EntityFrameworkCore -Version 2.0.1` - For installing EntityFramework.SqlServerCompact
  `Install-Package EntityFramework.SqlServerCompact -Version 6.2.0`

## Adding A Context To Project

- A Context is nessecary so that migrations for the database can be done
- So before enabling the migrations we must create a context for the database
- It can be done in the following way
  `using System.Data.Entity; namespace <Namespace> { public class <ClassName> : DbContext { public <ClassName>() { } } }`
- System.Data.Entity Consist of DbContext Class so it should be inherited
- This file will be recognized during enabling the migrations

## Enable Migrations

- Enable Migrations using command :
  `Enable-Migrations`

## Add Migrations

- If any changes to the schema or data are to be made they can only be done through Migrations
- To add migrations use command :
  `Add-Migration <MigrationName>`

## Adding Tables

- As this is code first flow we should not add tables manually any modification to the database should be done though code
- [In the context file created](#adding-a-context-to-project) a `<DbSet>` object is created with models which will be used in the project any number of tables can be created.
- a table can be created in context using
  `public DbSet<Contact> Contacts{ get; set; }`
- The `Contact` Model consist of
  ```
  using System.ComponentModel.DataAnnotations;
  namespace AddressBook.Framework
  {
  public class Contact
  {
  [Required]
  public string NAME { get; set; }

      			[Required]
      			public string EMAIL { get; set; }

      			[Required]
      			public string PHONE { get; set; }

      			public string LANDLINE { get; set; }

      			public string WEBSITE { get; set; }

      			public string ADDRESS { get; set; }

      			[Key]
      			public int Id { get; set; }
      		}
      	}
      	```

- See About [Data Annotations](https://docs.microsoft.com/en-us/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) which are used above variables for validation

## Using Queries

- After [adding migration](#add-migrations) a file having the migration name as suffix will be created in the Migrations folder of the project
- Open the file it will consist of two methods in the `Up()`method we can write the sql queries which are to be executed in the following way
  `Sql(<Query>);`

- After writing the query excute the command the save changes to database
  `Update-database`
- Example :
  `namespace AddressBook.Framework.Migrations { using System; using System.Data.Entity.Migrations; public partial class PopulateDatabase : DbMigration { public override void Up() { Sql("INSERT INTO CONTACTS VALUES('Trinath Reddy Inturi','trinathreddyinturi@gmail.com','+91 8143420021','040 8522 8544','www.github.com/trinath','Hyderabad')"); Sql("INSERT INTO CONTACTS VALUES('Tony Stark','tonystark@avengers.com','+91 900009956','15 85985 85695','www.avengers/ironman','NewYork')"); } public override void Down() { } } }`

## Creating DbContext In a Class

- In the service layer an instance of [DbContext](#adding-a-context-to-project) must be created inorder to perform operations on the database
- This object after its usage must be disposed by overring `Dispose()` method
- Syntax :
  `private <ClassNameDbContext> <instanceName>; public ClassNameConstructor() { <instanceName> = new <ClassNameDbContext>(); } protected override void Dispose(bool disposing) { <instanceName>.Dispose(); }`
- An example of the Creation
  `using System.Linq; using System.Collections.Generic; namespace AddressBook.Framework { public class Manager : ContactDbContext { private ContactDbContext _context; public Manager() { _context = new ContactDbContext(); } protected override void Dispose(bool disposing) { _context.Dispose(); } }`

## Operations Using The Context

- Getting all the data in a Table
- Syntax :
  `<instanceName>.<TableName>`
- Example :
  `_context.Contacts`
- Getting required row using LINQ with Lambda Expressions
- Syntax :
  `<instanceName>.<TableName>.SingleOrDefault(<obj> => <obj>.<prop>== <prop>);`
- Example :
  ```
  \_context.Contacts.SingleOrDefault(contact => contact.Id == id);
