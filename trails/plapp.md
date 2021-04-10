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

## Todo

- Fix the EFCore database persistence
  - Update tracked entities
- Look into EFCore [database migrations](https://docs.microsoft.com/en-us/ef/core/managing-schemas/migrations/?tabs=dotnet-core-cli). Do I need it?