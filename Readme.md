# WebapiCore-seed [![Build Status](https://travis-ci.org/MakingSense/WebApiCore-Seed.svg?branch=master)](https://travis-ci.org/MakingSense/WebApiCore-Seed)

## Prequisites
 * .Net core sdk
    * [Instructions for the latest sdk](https://github.com/dotnet/core/blob/master/release-notes/download-archives/2.1.200-sdk-download.md)
 * A text processor / gui like
    * [Visual Studio](https://www.visualstudio.com/es/downloads/)
    * [Visual studio code](https://code.visualstudio.com/Download)

    [Net core tools download page](https://www.microsoft.com/net/download/windows)

## Getting started

* ### For windows users with visual studio
    1. Open `WebApiCoreSeed.sln` located on the folder where the repository was downloaded

    2. If the WebApiCoreSeed.WebApi project is not selected as startup, just right click it and then click on `Set as StartUp Project` 

        ![set as startup](https://i.imgur.com/fTbU51p.gif)

    3. Now you just have to run it, pressing `F5` or the run button the top, if you use iisexpress configuration your app will be attached to the port `:4992`, if you use the excecutable, the port used will be `:4993`

        ![Run it](https://i.imgur.com/8TuB31V.gif)

    4. Now you just can open your favorite browser and navigate to `localhost:$Port/swagger` to see all the configured endpoints

* ### For users using vs code / console
    1. Open the folder in vs code

    2. Make sure you are in WebApiCore-Seed folder

    3. Run `dotnet restore` on the integrated terminal, to install the dependencies of the project

    4. Go to `WebApiCoreSeed.WebApi` folder using `cd` command

    5. Run `dotnet run` and wait, this would host the application on the :4993 port

    6. Finnaly you can open a browser and navigate `localhost:4993/swagger` to check all the availdable endpoints

## Next steps 

* ### Creating a new resource

    1. Let's asume that you want a `NewValue` resource, So your first step is give it a model, inside `WebApiCoreSeed.Data/Models` folder, create a new file called `NewValue.cs`

    2. Inside the file add the namespace 
        ``` csharp
        namespace WebApiCoreSeed.Data.Models
        ```

    3. Then you may want to create the class, make sure that inherits from `BaseEntity`

    4. When you're done adding properties add the reference to `System.ComponentModel.DataAnnotations` so you can add the [attributes](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations(v=vs.110).aspx) for a prevalidation 

    5. 
        If you want this resource in the database just add a property into `WebApiCoreSeedContext.cs` like 
        ``` csharp
        public DbSet<NewValue> NewValues { get; set; }
        ```
        and then register the table name into the method `OnModelCreating` (note that the property is plural but the table is singular)

        ``` csharp
        modelBuilder.Entity<NewValue>().ToTable("NewValue");
        ```
        finally if you want some initial data add it into `DatabaseSeed.cs` inside the `Initialize` method and before the `EnsureCreated()` but after the `SaveChanges()`

    6. 
        Then you're gonna need a place to drop the bussines/domain logic. Inside `WebApiCoreSeed.Domain/Service` add a `NewValueService.cs` and into `WebApiCoreSeed.Domain/Service/Contracts` add a `INewValueService.cs`. 

        Inside `INewValueService.cs` add the namespace `WebApiCoreSeed.Domain.Services.Interfaces` and define all the operations that NewValue performs inside an interface.

        Add the namespace `WebApiCoreSeed.Domain.Services` into `NewValueService.cs`, implement all the logic defined on the interface, if you need access to the data base just create a readonly field and "inject it" from the constructor like this

        ``` csharp
        private readonly WebApiCoreSeedContext _dbContext;

        public NewValueService(WebApiCoreSeedContext dbContext)
        {
            _dbContext = dbContext;
        }
        ```

    7. 


* ### Using a persistent database

* ### Deploying


## What uses

| Technology  | Description |
| :---------: | ----------- |
| [Asp .Net Web Api Core](https://docs.microsoft.com/en-us/aspnet/core/?view=aspnetcore-2.0) | Core framework |
| [Entity Framework](https://docs.microsoft.com/en-us/ef/core/) | Data access library |
| [Net Core DI](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/dependency-injection?view=aspnetcore-2.0) | Integrated dependecy injection library |
| [Xunit](https://xunit.github.io/) | Unit testing |
| [SwaggerUI](https://swagger.io/swagger-ui/) | Ui that document and exposes the api endpoints |
| [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle) | Documentation generator for swagger |
    
## Architecture
* DDD classic
    * Domain Services.
    * Inversion of control using conventions .
    * Autommaping for custom views decoupled from domain.
  
![demo](http://www.methodsandtools.com/archive/onion17.jpg)
