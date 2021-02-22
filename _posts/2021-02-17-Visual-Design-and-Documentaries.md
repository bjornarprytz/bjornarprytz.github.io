---
tags: xamarin, plapp
---

# Color palette and separation of concerns

I adjusted the color palette, and I chose [this](https://colorpalettes.net/color-palette-4291/) one for now. At least the application is somewhat readable (every color was 50% transparent before).

Also moved view models to a new project. It did wonders for writing both view models and views, because it means I _have to_ keep them independent of one another; ViewModels are unaware of the view, while the view only relies on the contract (interface) that's in Plapp.Core.

For charts I'm using [Microcharts](https://github.com/dotnet-ad/Microcharts). It was easy to get going, and we'll see how it performs, and how adjustable it is to my needs.

## Adam Curtis just released a new documentary series

and it is excellent.

>"Back in the 1960s, as the engineers began to build the first neural networks, what they had forgotten was that the system of thought they were creating inside the machines _did_ have its own history - that it had been borne out of a time when science had become deeply involved in questions of power and control in the British Empire, and that what lay behind the computer logic was the aim of simplifying human thought, which would finally allow you to colonize the last free outpost - the human mind." - _[Can't get you out of my head](https://thoughtmaybe.com/cant-get-you-out-of-my-head/)_
