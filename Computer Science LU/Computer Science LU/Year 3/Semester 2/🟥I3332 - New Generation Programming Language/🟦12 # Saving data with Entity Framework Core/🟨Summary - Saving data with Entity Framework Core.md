*important bits are ==highlighted==*

*Future me: nothing is important apparently lmao (im being sarcastic if you cant tell)*

---

- EF Core is an object-relational mapper (ORM) that lets you interact with a database by manipulating standard POCO classes, called entities, in your application. This can reduce the amount of SQL and database knowledge you need to have to be productive.
- EF Core maps entity classes to tables, properties on the entity to columns in the tables, and instances of entity objects to rows in these tables. Even if you use EF Core to avoid working with a database directly, you need to keep this mapping in mind.
- EF Core uses a database-provider model that lets you change the underlying database without changing any of your object manipulation code. EF Core has database providers for Microsoft SQL Server, SQLite, PostgreSQL, MySQL, and many others.
- EF Core is cross-platform and has good performance for an ORM, but has a different feature set to EF 6.x. Nevertheless, EF Core is recommended for all new applications over EF 6.x.
- EF Core stores an internal representation of the entities in your application and how they map to the database, based on the `DbSet<T>` properties on your application’s `DbContext`. EF Core builds a model based on the entity classes themselves and any other entities they reference.
- You add EF Core to your app by adding a NuGet database provider package. You should also install the Design packages for EF Core. This works in conjunction with the .NET Core tools to generate and apply migrations to a database.
- EF Core includes many conventions for how entities are defined, such as primary keys and foreign keys. You can customize how entities are defined either declaratively, using `DataAnotations`, or using a fluent API.
- Your application uses a `DbContext` to interact with EF Core and the database. You register it with a DI container using `AddDbContext<T>`, defining the database provider and providing a connection string. This makes your `DbContext` available in the DI container throughout your app.
- EF Core uses migrations to track changes to your entity definitions. They’re used to ensure your entity definitions, EF Core’s internal model, and the database schema all match.
- After changing an entity, you can create a migration either using the .NET Core tool or using Visual Studio PowerShell cmdlets.
- To create a new migration with the .NET CLI, run `dotnet ef migrations add NAME` in your project folder, where NAME is the name you want to give the migration. This compares your current `DbContext` snapshot to the previous version and generates the necessary SQL statements to update your database.
- You can apply the migration to the database using dotnet ef database update. This will create the database if it doesn’t already exist and apply any outstanding migrations.
- EF Core doesn’t interact with the database when it creates migrations, only when you explicitly update the database, so you can still create migrations when you’re offline.
- You can add entities to an EF Core database by creating a new entity, e, calling `_context.Add(e)` on an instance of your application’s data context, `_context`, and calling `_context.SaveChangesAsync()`. This generates the necessary SQL INSERT statements to add the new rows to the database.
- You can load records from a database using the `DbSet<T>` properties on your app’s `DbContext`. These expose the `IQueryable` interface, so you can use LINQ statements to filter and transform the data in the database before it’s returned.
- Updating an entity consists of three steps: read the entity from the database, modify the entity, and save the changes to the database. EF Core will keep track of which properties have changed so that it can optimize the SQL it generates.
- You can delete entities in EF Core using the Remove method, but you should consider carefully whether you need this functionality. Often a soft delete technique using an `IsDeleted` flag on entities is safer and easier to implement.
- This chapter only covers a subset of the issues you must consider when using EF Core in your application. Before using it in a production app, you should consider, among other things: the data types generated for fields, validation, how to handle concurrency, the seeding of initial data, handling migrations on a running application, and handling migrations in a web-farm scenario.