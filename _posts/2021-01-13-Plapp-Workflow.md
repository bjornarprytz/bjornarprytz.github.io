# Setting up a nice workflow

I've been trying to get work done with VS2019 and Xamarin, but it's proving difficult to debug. If something goes wrong in the view (xaml), all I get back is "Specified cast is invalid", and I have to guess where the error is. I've resorted to commenting out code, running again, etc. in order to work out where the bug is.

Using [this guide](https://www.mfractor.com/blogs/news/migrating-listview-to-collectionview-in-xamarin-forms-user-interface) I found and fixed the error swiftly. Even though I spent a lot of time finding that guide, I found many useful guides along the way.

In order to avoid too much of that in the future I have to improve my workflow:

1. Get Hot Reload up and running with Xamarin.
2. Fix a bug where the view doesn't load until after I open the app in split view (on the phone I'm debugging on).
3. Possibly (re)write the view in C\#, which Xamarin supports, to hopefully catch errors earlier.

Making a mobile app has so far been a lot of reading and troubleshooting. Once I get the workflow good and comfortablbe, I'm looking forward to focus on the UI :)
