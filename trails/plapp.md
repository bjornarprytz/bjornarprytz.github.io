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

### C\# 9

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

## Todo

- Look into EFCore [database migrations](https://docs.microsoft.com/en-us/ef/core/managing-schemas/migrations/?tabs=dotnet-core-cli). Do I need it?
- Look into refactoring `ToViewModel`/`ToModel` code
- Fix the LoadingScreen (ILoadingViewModel)
- Visualize DataSeries
  - The topic view should be able to adjust the time span, which will affect all dataseries
  - Alternatives
    - [Microcharts](https://github.com/dotnet-ad/Microcharts)
    - [OxyPlot](https://github.com/oxyplot/oxyplot)
- Refactor ViewModels so ObservableCollections are updated only when needed
- When adding new Entities, how can I get its Id when records are immutable?

### Ideas

- Add static properties to a data series, and time stamps for when they change
  - E.g. A plant is standing in a windown along the north wall. Time passes, data is gathered. The plant is moved to a window on the south side. When looking at the time series, it will be observable where the plant was.
