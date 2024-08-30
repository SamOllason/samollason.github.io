---
layout: post
title:  "My first 2 weeks of Blazor and C#"
date:   2024-08-30 09:30:57 +0100
categories: blazor c# introduction
---

After a few years working in product managemenent roles and more 'generalist roles' (founding an [embedded finance startup](https://www.sync-savings.com/)), I am excited to be moving back into a full-time software engineering role at [Howden](https://www.howdeninsurance.co.uk/?bridging=off&int=2123&keyword=howden%20insurance&gclid=Cj0KCQjw28W2BhC7ARIsAPerrcITIywhGDjjxvcSGicSRtryxuikt4B_o7A3Ymztfsm3566uwokdpM4aAnmOEALw_wcB). I'm looking forward to using the skills I have gained from these roles that are 'adjacent' to software engineering (perhaps another blog post on this sometime...) in my new role.

While I have remained close to the code and written *some* code in the past few years (mainly building prototypes etc.), I havent been coding full time for a while. Plus, I haven't worked with C# or Blazor before, so I was going to have to sharpen up my programming skills *and* learn a new language/framework at the same time. 

So, here are my first thoughts on C# and Blazor from an intensive 3 weeks of learning before starting my new role, and how these technologies compare to others I have worked with, in my opinion.

Please feel free to message me on [LinkedIn](https://www.linkedin.com/in/sam-ollason-1b404593/) with your thoughts, if I have misunderstood something, or if you have a different point of view 😀.

*(All views expressed are my own).*

# My background

To set the scene: I've been in full-time software dev roles for over 6 years, starting in front-end web development roles (React, [AngularJS](https://angularjs.org/) and also on the backend with JavaScript/TypeScript. I have also worked with C++.

So I have experience learning new front-end frameworks and working with statically typed languages.

# How I approached learning 

In the last few weeks I went through [this Blazor course on Udemy](https://www.udemy.com/course/blazor-deep-dive-from-beginner-to-advanced/learn/lecture/42460350#overview) and this [C# course on ZTM](https://academy.zerotomastery.io/courses/enrolled/2025735). I've taken notes using Notion and used [Zorbi](https://zorbi.com/) to automatically generate revision flashcard from Notion that I run through each morning. I have also been doing lots of the simpler katas on [Codewars](https://www.codewars.com/dashboard) each morning to get muscle memory used to the syntax and foundations of C# (where brackets need to go, filtering arrays, splitting strings etc.).

There is probably another blog post I could write reflecting on the difference between making time to learn *new material* and *reinforicing knowledge* in such a fast-moving industry like software engineering.

# Structure

What immediately struck me was how structured C# is as a language, and .NET is as a framework/toolkit. This felt quite different to working with JavaScript which has some strange quirks and a edges. For instance, JavaScript's equality operators have lots of gotchas, working with null always feels weird and I've never quite found a library I'm happy with to work with dates.

The best analogy of how they *feel* to me is that JavaScript is a lifeform that has evolved in a petri dish thats been left out (like [penicillin](https://www.acs.org/education/whatischemistry/landmarks/flemingpenicillin.html#:~:text=In%201928%2C%20at%20St.%20Mary's,number%20of%20deaths%20from%20infection.)), whereas C# is a systematically robuilt buit in the lab in a very systemtic and structured way. Neither is better or worse - just different. And I think that's fair given JavaScript was famously initially created in 10 days to add interactivity to web pages and then grew into what is is today.

I really like [Linq](https://learn.microsoft.com/en-us/dotnet/csharp/linq/) as a centralised and consistent way of working with enumerable data structures and I have found that when doing my daily Codewars practices that I reach for these as my first port of call for filtering, appending etc.

# Bouncing up against the walls

I always find it interesting to find the limitations of a front-end framework and how this is handled. I remember working with AngularJS as it was evolving in 2015/16 and components struggled to contain logic gracefully, and so '[component directives](https://dev.to/omnoms/angularjs-component-directives-306)' were introduced ...but still had lots of workarounds (and I seem to remember what felt like hacks). React has Refs and introduced 'Context' to avoid props drilling and, of course, React introduced [Hooks](https://react.dev/reference/react/hooks) to overcome the challenges of sharing stateful logic between components.

With C# so far the main thing I have found is hooking into JavaScript to run alerts/confirmation pop-ups. I understand why this is necessary, but it does feel a bit 'unclean' having to rely on JavaScript to overcoming a C# shortcoming. The purist in me would want to only work in C# if possible. I haven't yet made up my mind whether this is a code smell or not to be honest.

# Scaffolding

I was pleasantly suprised by how easy it is to configure and manage Blazor apps. I remember the days of having to manually configure Webpack for front-end projects which was a complete nightmare. I know things like Vite and NextJS have moved things forward but working with these JavaScript bundlers always feel a bit like hardwork. Perhaps it's partly that C# and Blazor is all Microsoft instead of a created by the community/third parties. I think I prefer a clearer more idiomatic way of doing things instead of a huge array of choice, at least at the moment.

# What I am excited to explore next

I've enjoyed the journey so far (it's quite enjoyable being a novice at a new skill and experiencing the satisfaction of learning!).

* I am looking forward to getting more unto [bUnit](https://bunit.dev/) for testing Blazor UIs
* I've gotten really interested in [property testing](https://craftbettersoftware.com/p/how-i-write-1000s-tests-with-little) as a way of coverage and want to play around with [FsCheck](https://www.production-ready.de/2023/06/10/property-based-testing-in-csharp-en.html) a bit more
* I'm keen to start looking more into [Reqnroll](https://docs.reqnroll.net/latest/quickstart/index.html) as a way of generating executable specifications for Blazor apps