---
tags: xamarin, C#, prompt, popup, mobile-development
---

# Prompting the user

I want to add a way for a view model to await a user prompt. Something like:

´´´csharp
var tag = await PromptService.Prompt\<ITagViewModel\>();
´´´

I've got [some](https://github.com/HoussemDellai/Xamarin-Forms-Popup-Demo) [avenues](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/pop-ups) to approach this challenge:

I added [AsyncCommand](https://docs.microsoft.com/en-gb/xamarin/community-toolkit/objectmodel/asynccommand?WT.mc_id=mobile-13724-bramin) from the [Xamarin CommunityToolkit](https://github.com/xamarin/XamarinCommunityToolkit?WT.mc_id=xamarin-c9-jamont), which looks really good.

Question I need answered:

- What is a DbContext?
- Can I cache fetched data in the DataStore, using EntityFramework?
- What is a Scoped instance?
