---
tags: xamarin, C#
---

# UI in C\#

I'm done with the broad strokes of rewriting the UI. There are still details, like icons, to figure out.
So far it's a blast writing extension methods and getting compile-time feedback.

## Icons

1. Started [here](https://montemagno.com/using-font-icons-in-xamarin-forms-goodbye-images-hello-fonts/) to find out how to embed font files (.otf/ttf) in my project, and reference them in code.
2. Ended up using [Material Design](https://google.github.io/material-design-icons/) instead of [FontAwesome](https://fontawesome.com/) becauase it's free and there are way more icons. There seems to be a lot more support and documentation around Material Design.

## Colors

Next up is setting up a working pallette of colors, and applying it correctly to the styles.
