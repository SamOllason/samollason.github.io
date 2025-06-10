---
layout: post
title:  "Thoughts on .NET 10.4 preview"
date:   2025-06-10 12:13:41+0100
categories: dotnet c# blazor
---

I've been working with .NET at [Howden](https://www.howdengroup.com/uk-en) for about 9 months now and, after getting my head around whats available 'now' in .NET I have been looking to the future with what's will be coming up in .NET 10 later in 2025, in particular those parts that are relevant for folks working with Blazor.

You can read about other previews here: [2025-25-03-thoughts-on-dotnet-10_1-and_10_2-previews] and [2025-05-15-thoughts-on-dotnet-10_3-previews]. 

*(All views expressed are my own).*

# Overview

.NET 10 will be released in late 2025, but the previews are already being released (10.4 was released in May).

 Below are my thoughts on what's been announced and some of the **points that are of more interest to us in Howden Engineering Group**. You can read my [Thoughts on .Net 10.1 and 10.2 (previews) - Overview](https://dev.azure.com/howdenuki/Development%20Centre%20of%20Expertise/_wiki/wikis/Development%20Centre%20of%20Excellence/924/Thoughts-on-.Net-10.1-and-10.2-(previews)?anchor=overview) and [Thoughts on .NET 10.3 (preview) - Overview](https://dev.azure.com/howdenuki/Development%20Centre%20of%20Expertise/_wiki/wikis/Development%20Centre%20of%20Excellence/1173/Thoughts-on-.NET-10.3-(preview))

If I have misunderstood something or you disagree with me I would love to chat 😃 #everydayaschoolday
Full announcements: [.NET 10 Preview 4 is now available! - .NET Blog](https://devblogs.microsoft.com/dotnet/dotnet-10-preview-4/)


# A more 'meta' point

This is more of a comment on the .NET release notes structure, but I often find myself naturally asking 'what does this apply to?' and 'what previously existed?' when looking at new features to contextualise them. I often turn to the CoPilot app to give me some more background. It would be useful if some of this information was included in the release notes from Microsoft themselves, IMO.

E.g. this was particularly useful:

```
tell me more about integration testing with Kestrel in .NET 10.4

what types of .NET application is this relevant to?

https://github.com/dotnet/core/blob/main/release-notes/10.0/preview/preview4/aspnetcore.md#use-webapplicationfactory-with-kestrel-for-integration-testing
```

# Blazor WebAssembley runtime diagnostics

In .NET 10 Preview 4, Microsoft are introducing enhanced runtime diagnostics for Blazor WebAssembly apps, which include:

* Performance traces: You can now collect detailed performance profiling data.
* Memory dumps: Capture memory snapshots for in-depth analysis.
* Runtime metrics: Monitor runtime behavior such as memory usage.

These diagnostics are now built into the WebAssembly runtime, rather than relying solely on browser developer tools or external profiling setups. I haven't yet explored these but I am keen to see what additional insights we can gain about our application to give our users a better experience.

# Use WebApplicationFactory with Kestrel for integration testing

There is a new and improved way to create integration tests which allows for more realistic tests, which should bring more confidence.

This is particularly for applications have endpoints exposed from Blazor WASM hybrid apps.

Previously, using WebApplicationFactory to setup integration tests used an in-memory HTTP server called TestServer, but in 10.4 WebApplicationFactory can now be setup to use Kestrel which is the actual web server used in production by a .NET app.

Using Kestrel instead of TestServer in tests enables:

* End-to-end testing of real HTTP features (e.g., HTTPS, HTTP/2, WebSockets).
* Middleware and pipeline validation under real server conditions.
* Testing of custom server configurations (e.g., ports, certificates, limits).

Copied from some examples online, here is an example setup to give you a flavour of how this could work in your integration test setup:

```
var factory = new WebApplicationFactory<Program>()
    .WithWebHostBuilder(builder =>
    {
        builder.UseKestrel(); // Use real Kestrel server
        builder.ConfigureKestrel(options =>
        {
            options.ListenLocalhost(5001); // Customize as needed
        });
    });


factory.CreateClient() ...
```

(More here)[https://github.com/dotnet/core/blob/main/release-notes/10.0/preview/preview4/aspnetcore.md#use-webapplicationfactory-with-kestrel-for-integration-testing]