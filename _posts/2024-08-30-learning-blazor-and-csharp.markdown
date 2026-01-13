---
layout: post
title:  "My first 3 weeks learning C# and Blazor"
date:   2024-08-30 09:30:57 +0100
categories: blazor c# introduction
---

After a few years working in product managemenent roles and more 'generalist roles' (founding an <a href="https://www.sync-savings.com/" target="_blank">embedded finance startup</a>), I am excited to be moving back into a full-time software engineering role at <a href="https://www.howdeninsurance.co.uk/?bridging=off&int=2123&keyword=howden%20insurance&gclid=Cj0KCQjw28W2BhC7ARIsAPerrcITIywhGDjjxvcSGicSRtryxuikt4B_o7A3Ymztfsm3566uwokdpM4aAnmOEALw_wcB" target="_blank">Howden</a>. I'm looking forward to using the skills I have gained from these roles that are 'adjacent' to software engineering (perhaps another blog post on this sometime...) in my new role.

While I have remained close to the code and written *some* code in the past few years (mainly building prototypes etc.), I haven't been coding full time for a while. Plus, I haven't worked with C# or <a href="https://learn.microsoft.com/en-us/aspnet/core/blazor/?view=aspnetcore-8.0" target="_blank">Blazor</a> before, so I was going to have to sharpen up my programming skills *and* learn a new language/framework at the same time. 

So, here are my first thoughts on C# and Blazor from an intensive 3 weeks of learning before starting my new role, and how these technologies compare to others I have worked with, in my opinion.

Please feel free to message me on <a href="https://www.linkedin.com/in/sam-ollason-1b404593/" target="_blank">LinkedIn</a> with your thoughts, if I have misunderstood something, or if you have a different point of view 😀.

*(All views expressed are my own).*

# My background

To set the scene: I've been in full-time software dev roles for over 6 years, starting in front-end web development roles (React, <a href="https://angularjs.org/" target="_blank">AngularJS</a> and also on the backend with JavaScript/TypeScript). I have also worked with C++.

So I have experience learning new front-end frameworks and working with statically typed languages.

# How I approached learning 

In the last few weeks I went through <a href="https://www.udemy.com/course/blazor-deep-dive-from-beginner-to-advanced/learn/lecture/42460350#overview" target="_blank">this Blazor course on Udemy</a> and this <a href="https://academy.zerotomastery.io/courses/enrolled/2025735" target="_blank">C# course on ZTM</a>. I've taken notes using Notion and used <a href="https://zorbi.com/" target="_blank">Zorbi</a> to automatically generate revision flashcard from Notion that I run through each morning. I have also been doing lots of the simpler katas on <a href="https://www.codewars.com/dashboard" target="_blank">Codewars</a> each morning to get my muscle memory used to the syntax and foundations of C# (where brackets need to go, filtering arrays, splitting strings etc.).

There is probably another blog post I could write reflecting on the difference between making time to learn *new material* and *reinforcing existing knowledge* in such a fast-moving industry like software engineering.

# Structure

What immediately struck me was how structured C# is as a language, and .NET is as a framework/toolkit. This felt quite different to working with JavaScript which has some strange quirks and edges. For instance, JavaScript's equality operators have lots of gotchas, working with null always feels weird and I've never quite found a library I'm happy with to work with dates.

The best analogy of how they *feel* to me is that JavaScript is a lifeform that has evolved in a petri dish thats been left out (like <a href="https://www.acs.org/education/whatischemistry/landmarks/flemingpenicillin.html#:~:text=In%201928%2C%20at%20St.%20Mary's,number%20of%20deaths%20from%20infection." target="_blank">penicillin</a>), whereas C# is a robot built in a factory in a systemtic and structured way. Neither is better or worse - just different. And I think it makes sense given JavaScript was famously initially created in 10 days to add interactivity to web pages and then grew into what is is today.

Also, I really like C#'s <a href="https://learn.microsoft.com/en-us/dotnet/csharp/linq/" target="_blank">Linq</a> as a centralised and consistent way of working with enumerable data structures and I have found that when doing my daily Codewars practices that I reach for these as my first port of call for filtering, appending etc.

# Bouncing up against the walls

I always find it interesting to find the limitations of a front-end framework and how this is handled. I remember working with AngularJS as it was evolving in 2015/16 and components struggled to contain reusable logic gracefully, and so '<a href="https://dev.to/omnoms/angularjs-component-directives-306" target="_blank">component directives</a>' were introduced ...but still had lots of workarounds (and I seem to remember what felt like hacks). React has Refs and introduced 'Context' to avoid props drilling and, of course, React introduced <a href="https://react.dev/reference/react/hooks" target="_blank">Hooks</a> to overcome the challenges of sharing stateful logic between components.

With C#, so far, the main thing I have found is hooking into JavaScript to run alerts/confirmation pop-ups. I understand why this is necessary, but it does feel a bit 'unclean' having to rely on JavaScript to overcoming (what feels like) a C# shortcoming. The purist in me would want to only work in C# if possible. I haven't yet made up my mind whether this feels like a code smell or not.

# Scaffolding

I was pleasantly suprised by how easy it is to configure and manage Blazor apps. I remember the days of having to manually configure Webpack for front-end projects which was a complete nightmare. I know things like Vite and NextJS have moved things forwards but working with these JavaScript bundlers always feel a bit like hardwork. Perhaps it's partly that C# and Blazor is all Microsoft instead of a created by the community/third parties. I think I prefer a clearer and more idiomatic way of doing things instead of a huge array of choice, at least at the moment.

# What I am excited to explore next

I've enjoyed the journey so far - I quite like the humbling experience of feeling like a novice, despite feeling uncomfortable a lot of the time in doing so.

* I am looking forward to getting more into <a href="https://bunit.dev/" target="_blank">bUnit</a> for testing Blazor UIs
* I've gotten really interested in <a href="https://craftbettersoftware.com/p/how-i-write-1000s-tests-with-little" target="_blank">property testing</a> as a way of improving unit test coverage and want to play around with <a href="https://www.production-ready.de/2023/06/10/property-based-testing-in-csharp-en.html" target="_blank">FsCheck</a> a bit more
* I'm keen to start looking more into <a href="https://docs.reqnroll.net/latest/quickstart/index.html" target="_blank">Reqnroll</a> as a way of generating '<a href="https://www.youtube.com/watch?v=knB4jBafR_M" target="_blank">executable specifications</a>' for Blazor apps