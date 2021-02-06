---
tags: xamarin, c#, depencency-injection, configuration
---

# The Fundamentals

Before I go too far into UI, I want to set up a config file, so I don't have to put resource references in source code, [this guide](https://www.xamarinhelp.com/configuration-files-xamarin-forms/) looks promising and straight forward.

Another thing is to set up logging. Just something simple that I can change out later if I want. It's important to use dependecy injection for what it's worth.

1. Config
2. Logging
3. Remove Database depenceny on file system (inject StreamProvider in constructor)
