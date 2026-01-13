---
layout: post
title:  "Thoughts on .NET 10.1 and 10.2 previews"
date:   2025-03-25 10:30:57 +0100
categories: dotnet c# blazor
---

I've been working with .NET at <a href="https://www.howdengroup.com/uk-en" target="_blank">Howden</a> for about 6 months now and, after getting my head around whats available 'now' in .NET I have been looking to the future with what will be coming up in .NET 10 later in 2025.

I've been following the preview releases (10.1 and 10.2 are here!). Below are my thoughts on what's been announced and some of the points that are of more interest to me in my team working with Blazor hybrid WASM web apps.

If I have misunderstood something or you disagree with me I would love to chat 😃 #everydayaschoolday

*(All views expressed are my own).*

## Overall thoughts on .NET 10 so far 🥅

A lack of clarity on the direction, mission and aims. 

I'm not completely clear on what the vision is for .NET 10 to be honest. I'm only 6 months into my journey with .NET, so perhaps my expectations are not quite right. But I was expecting a clearer indication of what this release is aiming for - in the same way that new versions of the iPhone have a clear headline message (even if people feel that hardware innovation is becoming harder...). There are some general improvements across the .NET ecosystem and there are some new AI capabilities in the work if you are building your own AI agents, but I haven't seen a clear headline that helps me see the direction where .NET is heading.

Anyway, let's look at the relevant features in .NET 10.1 and 10.2 previews and _my opinionated comments (in italics)._

## C# 14 🦭

1. **Field backed properties** - using the new `field` keyword means the compiler will automatically generate the backing field. _A nice bit of syntactic sugar, but it's a minor improvement. It also means newer classes could look a bit different to older style classes and reduce readability across the codebase._
2. **Implicit 'Span' conversions** - Spans are wrappers around buffers. _Not relevant to what I the projects I am working on, as this is a bit too low level._
3. **Unbound generic support for nameof** - `nameof(List<>)` now evaluates to `List`, but before wouldn't have worked. _Not sure of the situation where you would need this offhand, but useful to know._
4. **Types no longer required with parameter modifiers**. When using e.g. the `out` param modifier in a lambda you previously had to specify the type of variable, but not any longer.

 Before:
    `TryParse<int> parse1 = (text, int out result) => Int32.TryParse(text, out result);`

in C# 14 you won't need to specify that result is an int:  `TryParse<int> parse1 = (text, out result) => Int32.TryParse(text, out result);`

_I think this is useful for readability when there is a verbose type that is returned, as long as the variable is suitably named so the reader can infer that the variable is probably a number/string/date etc when scanning the lambda._

5. **Partial constructors** - similar to how classes can already be defined over multiple files, constructors can now be as well. _I'm not sure of the benefit of this; if you need to split your constructor over multiple files because the constructor is too large then this is a sign your constructor is too big, perhaps? It seems like a marginal improvement and I think this could lead to more confusion. Happy to be proved wrong._

## Blazor🔥

1. **New ReconnectModal available out of the box** - _useful for folks who are working with server-rendered Blazor projects._
2. **NavigateTo method now doesnt scroll to top of page when same page nav** - _again, more relevant to server-rendered Blazor projects._
3. **QuickGrid styling improvements** - I don't touch QuickGrid much so I would have to look in more whether this is relevant to our use cases in my team.
4. **NavLink.Matchall tweaks** - NavLink is a Blazor components that sets the 'active' CSS class when a link matches the URL. In .NET 10, links will retain the `active` class if the URL _path_ matches but the query string or fragment change. _This may not affect you, and I don't believe it will affect us given the way we use NavLinks in our projects_

## Conclusion

Thanks for reading. I hope you found this interesting and/or useful! I certainly find this a useful way to learn and reflect on whats coming up, so that me and my team are prepared.

 ##Read more

* <a href="https://devblogs.microsoft.com/dotnet/dotnet-10-preview-1/#%F0%9F%93%A2-.net-10-discussions" target="_blank">.NET 10 Preview 1 is now available! - .NET Blog</a>
<a href="https://learn.microsoft.com/en-gb/dotnet/csharp/whats-new/csharp-14" target="_blank">What's new in C# 14 | Microsoft Learn</a>
* Subscribe to the .NET newsletter to get updates: <a href="https://devblogs.microsoft.com/dotnet/newsletter/" target="_blank">Newsletter - .NET Blog</a>
  
