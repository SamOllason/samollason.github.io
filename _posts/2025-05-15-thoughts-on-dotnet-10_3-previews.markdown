---
layout: post
title:  "Thoughts on .NET 10.3 preview"
date:   2025-05-15 10:30:57 +0100
categories: dotnet c# blazor
---

I've been working with .NET at [Howden](https://www.howdengroup.com/uk-en) for about 6 months now and, after getting my head around whats available 'now' in .NET I have been looking to the future with what will be coming up in .NET 10 later in 2025, in particular those parts that are relevant for folks working with Blazor.

You can read about other previews here: [2025-25-03-thoughts-on-dotnet-10_1-and_10_2-previews]

*(All views expressed are my own).*

If I have misunderstood something or you disagree with me I would love to chat 😃 #everydayaschoolday

Full announcement: [.NET 10 Preview 3 is now available! - .NET Blog](https://devblogs.microsoft.com/dotnet/dotnet-10-preview-3/)


# Blazor and ASP.NET Core

## Declarative model for persisting component state 💪

**Who will this benefit:** This is useful for any server-rendered components (i.e. any component that isn't Interactive WebAssembly [render mode](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/render-modes?view=aspnetcore-9.0).

**What is this all about:** Persisting component state means saving some data during prerendering and restoring it when the app becomes interactive (in the browser), so there is no need to refetch is and the user experience is sharper.

**What's the current approach:** Currently, there is a lot of boilerplate needed to support this and it was quite manual and involved being careful with serialisation:
1.	Inject PersistentComponentState into your component.
2.	Register a callback to save state during prerendering.
3.	Read the state back when the component is interactive.

With this new feature, you can just decorate a property with the [SupplyParameterFromPersistentComponentState] attribute.

```

// On the server, Forecasts is loaded and persisted.
// On the client, Blazor restores Forecasts automatically—no extra code needed.
@code {
    [SupplyParameterFromPersistentComponentState]
    public WeatherForecast[]? Forecasts { get; set; }

    protected override async Task OnInitializedAsync()
    {
        if (Forecasts is null)
        {
            Forecasts = await WeatherService.GetForecastAsync();
        }https://learn.microsoft.com/en-us/credentials/certifications/azure-fundamentals/?practice-assessment-type=certification
    }
}
```

## Validation support in minimal APIs 🛡️


**Who will this benefit:** anyone that has APIs defined using minimal APIs.

**What is this all about**: Minimal APIs are a way to define HTTP API endpoints with very little boilerplate. You typically write them directly in your Program.cs or a similar startup file, using methods like app.MapGet, app.MapPost, etc. They are concise and great for small services or microservices.

```
    app.MapGet("/hello", () => "Hello World!");
```

Controllers (classes that inherit from ControllerBase or Controller) are more traditional. They are decorated methods with attributes like [HttpGet], [HttpPost], etc. This approach is more structured and is often used for larger applications.

```
    [ApiController]
    [Route("[controller]")]
    public class HelloController : ControllerBase
    {
        [HttpGet]
        public string Get() => "Hello World!";
    }
```   
 I personally prefer minimal APIs because I find them more lightweight with less boilerplate code. Minimal APIs also feels more like just writing more C# code and feels more 'organic' to me. I also like them because of how similar they feel to defining endpoints in Express (popular NodeJS server) which was where I first started.

**Current approach:** validation logic _inside_ of the api body with business logic, not as a decorator.

```
using System.ComponentModel.DataAnnotations;

public class Person
{
    [Required]
    public string Name { get; set; } = default!;
    [Range(0, 120)]
    public int Age { get; set; }
}

app.MapPost("/people", (Person person) =>
{
    var validationResults = new List<ValidationResult>();
    var context = new ValidationContext(person);
    if (!Validator.TryValidateObject(person, context, validationResults, true))
    {
        return Results.ValidationProblem(validationResults
            .ToDictionary(
                v => v.MemberNames.FirstOrDefault() ?? "",
                v => new[] { v.ErrorMessage ?? "" }
            ));
    }

    return Results.Ok(person);
});
```

New approach:

```
app.MapPost("/products",
    ([EvenNumber(ErrorMessage = "Product ID must be even")] int productId, [Required] string name)
        => TypedResults.Ok(productId))
    .DisableValidation();

```

Validation support looks useful and is something I will bear in mind when we need consider adding more endpoints. This certainly makes it more attractive to use them with our app.


More info:
[core/release-notes/10.0/preview/preview3/aspnetcore.md at main · dotnet/core · GitHub](https://github.com/dotnet/core/blob/main/release-notes/10.0/preview/preview3/aspnetcore.md#validation-support-in-minimal-apis)


# #C 14 new features in 10.3

## Null-conditional assignment 🤫

This assigns a value to a property or field only if the containing instance exists. This is a neat way to reduce boilerplate and I'm already used to using nifty shortcuts for working with nullable _properties_ with a more compact syntax (e.g. [null conditionals](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-)) so making use of this syntax for nullable _objects_ seems like a logical extension. **I like this!**

More here: [core/release-notes/10.0/preview/preview3/csharp.md at main · dotnet/core · GitHub](https://github.com/dotnet/core/blob/main/release-notes/10.0/preview/preview3/csharp.md#null-conditional-assignment)


## Extension members ⛓️

**Whats this all about:** extension methods are a way of extending a C# class - basically defining methods in a separate file the main class definition. This is useful for extending third-party libraries (I have never actually had to do this) and also to make unit testing functionality easier (which is a pattern I have seen a fair bit).

**What the current approach is:** the end result is still the same, but the new extension blocks make code easier to read and mentally group things together:

```
public static class Extensions
{
    extension(IEnumerable<int> source)  // new!
    {
        public IEnumerable<int> WhereGreaterThan(int threshold)
            => source.Where(x => x > threshold);

        public bool IsEmpty
            => !source.Any();
    }
}

var list = new List<int> { 1, 2, 3, 4, 5 };
var large = list.WhereGreaterThan(3);
if (large.IsEmpty)
{
    Console.WriteLine("No large numbers");
}
else
{
    Console.WriteLine("Found large numbers");
}
```

**Benefits of the new approach** I think this makes extension functionality eaisier to manage, **but a word of caution:** I can see this potentially getting a bit out of control; my instinct is that the 'supporting' methods in a block should be small, lean and as simple as possible to avoid the extension methods becoming bloated and larger than the class they are meant to extend. E.g. the examples online are for little helpers like `IsEmpty()` which I think is appropriate, but I would want to keen an eye on this. Perhaps one for an internal C# style guide to define when a method in an extension block becomes 'too big' 🤔

# Conclusion

Thanks for reading. I hope you found this interesting and/or useful! I certainly find this a useful way to learn and reflect on whats coming up, so that me and my team are prepared.