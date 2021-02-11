---
tags: xamarin, c#, depencency-injection, configuration
---

# The Fundamentals

Before I go too far into UI, I want to set up a config file, so I don't have to put resource references in source code, [this guide](https://www.xamarinhelp.com/configuration-files-xamarin-forms/) looks promising and straight forward. In the comments of that blog post, there was a link to a [nuget package](https://github.com/maximrub/Xamarin) that looks useful. Holy crap it was over complicated for my use case.

---

Try this next: [Loading a config file](https://johnthiriet.com/xamarin-loading-a-configuration-file/)

Another thing is to set up logging. Just something simple that I can change out later if I want. It's important to use dependecy injection for what it's worth.

1. Config
2. Logging
3. Remove Database depenceny on file system (inject StreamProvider in constructor)

---

So these three are done (though I haven't tested the Logging yet). It took a bit of refactoring, but now I think I can really focus on the View, because the ViewModels are fairly easy to program / add new ones.
