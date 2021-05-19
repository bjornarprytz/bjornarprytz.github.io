---
layout: post
tags: plapp, xamarin, c#, dot-net
---

# Goal

Create an app which lets the user log data series on different topics. The topics can have a title, a description and an image. The data series are based on user-defined tags.

[repo](https://github.com/bjornarprytz/plapp)

## Previous posts

- [workflow](../_posts/2021-01-13-Plapp-Workflow.md)
- [UI](../_posts/2021-01-27-Writing-UI-in-C-sharp.md)
- [More UI](../_posts/2021-02-03-C-sharp-UI-Continued.md)
- [Config](../_posts/2021-02-10-Config-and-Dependency-Injection.md)
- [Visual](../_posts/2021-02-17-Visual-Design-and-Documentaries.md)
- [Prompts](../_posts/2021-02-24-Popups-and-Prompts.md)

## Fixing the database

There's somthing wrong with the way I'm storing records in the database. Somehow data is getting lost / not persisted properly.

This is the exception that's being thrown every time I'm updating an entity (e.g. Topic):

> The instance of entity type 'Topic' cannot be tracked because another instance with the same key value for {'Id'} is already being tracked. When attaching existing entities, ensure that only one entity instance with a given key value is attached. Consider using 'DbContextOptionsBuilder.EnableSensitiveDataLogging' to see the conflicting key values.

[This article](https://medium.com/@yostane/data-persistence-in-xamarin-using-entity-framework-core-e3a58bdee9d1) goes through the basics of EF Core with Xamarin. It's made me aware of [migrations](https://medium.com/@yostane/entity-framework-core-and-sqlite-database-migration-using-vs2017-macos-28812c64e7ef), but I'll look into that later.

### Tests

Wrote a bunch of tests to avoid having to boot the Android Emulator for every little change. It looks like I've also fixed the Entity Update bug.

### C# 9

Upgraded to C\# 9, and made the database entities `records`, which could make the `ToViewModel`/`ToModel` functions refactorable!

### Eager Loading

There are [various ways](https://docs.microsoft.com/en-us/ef/core/querying/related-data/) to handle the loading of related entities. I'm using [eager Loading](https://docs.microsoft.com/en-us/ef/core/querying/related-data/eager) in some places (load tags when loading data series). It looks like there are many considerations to take in that article, so if I want to do more advanced stuff with this, I should read up on the caveats.

### Disposing Context and Dependency Injection

This error message pops up when I try to add a DataSeries:

> System.ObjectDisposedException: 'Cannot access a disposed context instance. A common cause of this error is disposing a context instance that was resolved from dependency injection and then later trying to use the same context instance elsewhere in your application. This may occur if you are calling 'Dispose' on the context instance, or wrapping it in a using statement. If you are using dependency injection, you should let the dependency injection container take care of disposing context instances.
Object name: 'PlappDbContext'.'

The issue was (as the exception states) that I was disposing the context twice. To fix it, I simply removed the `using` statement outside `GetContextAsync()`:

```csharp
public async Task<IEnumerable<Tag>> FetchTagsAsync(CancellationToken cancellationToken = default)
{
    /*using*/ var context = await GetContextAsync(cancellationToken);
    
    return await context.Tags
        .AsNoTracking()
        .ToListAsync(cancellationToken);
}
```

### Concurrency Issues on the DbContext

When clicking the AddDataSeries Button:

> System.InvalidOperationException: 'A second operation was started on this context before a previous operation completed. This is usually caused by different threads concurrently using the same instance of DbContext. For more information on how to avoid threading issues with DbContext, see https://go.microsoft.com/fwlink/?linkid=2097913.'

### Refactored the `record`s back to `class`

I really wanted to use `record`, but this wasn't the use case for it.

### Refactoring `Plapp.Persist`

When I started watching this [video series](https://www.youtube.com/watch?v=7pkmqrrjAAQ&list=PLA8ZIAm2I03jSfo18F7Y65XusYzDusYu5&index=2), I realised that I've been making the DataStore complicated. I can really cut down a lot of the boiler plate and use generics for my data access. This is a huge relief, and I can't wait to get it done!

The refactoring is now done, but it seems I'll be retracing some of my steps ("Fixing the database", "Disposing Context and Dependency Injection" above). Could `.AsNoTracking()` be the answer?

## Adding Data Points

Stuff that needs to be done:

- CreateDataPointsViewModel
  - [x] Keep track of user input
  - [x] Undo actions
  - [ ] Persist the data
- Create ViewModel implementations for the various data types
  - [x] Decimal
  - [ ] Integer
  - [ ] Bool
  - [ ] Check
- DataPoint Form
  - [x] UI for data point creation
  - [ ] Depending on data type
- Make sure the dataseries is updated when the points are added

## Targeting Windows in Test projects

I can't really explain why, but it's neccessary to add `-windows` to the target framework for test projects (Ref: Error NETSDK1136).

```xml
<TargetFramework>net5.0-windows</TargetFramework>
```

## Adding AutoMapper

Having gone back and forth with the data mapping functions over the course of this project, I've finally decided to try AutoMapper. Starting [here](https://docs.automapper.org/en/stable/Getting-started.html).

## Todo

- Look into EFCore [database migrations](https://docs.microsoft.com/en-us/ef/core/managing-schemas/migrations/?tabs=dotnet-core-cli). Do I need it?
- Fix the LoadingScreen (ILoadingViewModel)
- Visualize DataSeries
  - The topic view should be able to adjust the time span, which will affect all dataseries
  - Alternatives
    - [Microcharts](https://github.com/dotnet-ad/Microcharts)
    - [OxyPlot](https://github.com/oxyplot/oxyplot)
- Add a mechanism where 'new' DataPoints are created in the context of their data series (i.e. with data type + id filled in)

- Sketch UI Layout

- Add Localization ([gettext](https://www.gnu.org/software/gettext/) | [localization for xamarin.forms](https://developers.localizejs.com/docs/how-to-use-localize-to-translate-your-xamarin-mobile-application))

- Transition to MAUI ([blog post](https://devblogs.microsoft.com/dotnet/introducing-net-multi-platform-app-ui/) | [repo](https://www.google.com/search?client=firefox-b-d&q=github+maui))
  - [MVU](https://thomasbandt.com/model-view-update) | ([example](https://devblogs.microsoft.com/xamarin/fabulous-functional-app-development/)) and [F#](https://fsharp.org/learn/).

- User confirmation on delete (create a ViewModel which can be bound to a button?)
  - ConfirmActionViewModel ()
  - Topic
  - DataSeries
  - DataPoints
  - Tag (may need extra confirmation because it's latteraly connected)

- Tests for the ViewModels
  - Prerequisites: Figure out the business logic
- Test infrastructure
  - Navigation
  - ViewFactory

- Use `GetRequiredService` instead of `GetService` on the `ServiceProvider`

- Refactor [Config](https://andrewlock.net/how-to-use-the-ioptions-pattern-for-configuration-in-asp-net-core-rc2/)
  - Add whole Connection String to config (not just the filename)

### Bugs

- When topics are updated, a new copy of it is created instead
  - Likely cause: `TopicViewModel.Id` is not updated when saved
- Fix DataSeries not being Saved on `Topic.OnHide()`
- Some tests are non-deterministic
  - Likely because they test a function which has `Task.Run(...)` in it and the test does not await.
- Exception thrown when adding a second tag:
  - > Microsoft.EntityFrameworkCore.DbUpdateException: 'An error occurred while updating the entries. See the inner exception for details.'
    - Note: There was no inner exception to be accessed
  - I may be able to unconver this in the test project

- Investigate if event subscription ([example](https://github.com/bjornarprytz/Plapp/blob/master/Plapp.ViewModels/ViewModels/BaseTaskViewModel.cs)) can cause a memory leak.

### Ideas

- Add static properties to a data series, and time stamps for when they change
  - E.g. A plant is standing in a windown along the north wall. Time passes, data is gathered. The plant is moved to a window on the south side. When looking at the time series, it will be observable where the plant was.

### Low Priority

- Tidy Construction Extensions
